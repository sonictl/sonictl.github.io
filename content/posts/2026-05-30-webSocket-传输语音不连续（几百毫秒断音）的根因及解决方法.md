---
layout: post
title:  "webSocket 传输语音不连续（几百毫秒断音）的根因及解决方法"
date: 2026-05-30 19:03:00 +0800
categories: Technology
slug: p20260530191618
---

## webSocket 传输语音不连续（几百毫秒断音）的根因及解决方法

一般有 **4 个关键问题**，它们共同导致了接收方语音出现几百毫秒的断音：

---

### 🔴 问题 1：环形缓冲区欠载（最严重）

**位置**: `audio-worklet.js` 第 60 行

```js
buffer: new Float32Array(this._frameSamples * 8), // 8帧缓冲 (~480ms)
```

- **8 帧 = 480ms 缓冲**，但解码器输出是异步的，网络抖动 + 编解码延迟很容易超过 480ms
- 更关键的是：`_frameSamples` 在 `config` 消息更新后才重新计算，但 **初始创建 peerBuffer 时使用的是旧的 `_frameSamples` 值**。如果 config 消息在第一个 pcm 数据到达之后才到达，缓冲区大小会算错
- **欠载后直接重置为未就绪状态**（第 155 行），导致重新预缓冲，造成连续断音

### 🔴 问题 2：解码器输出直接推入 Worklet，无缓冲预填

**位置**: `client.js` 第 260-270 行

```js
output: (audioData) => {
    // 解码完成 → 立即发送 PCM 到 Worklet
    workletNode.port.postMessage({ type: 'pcm', peerId: peerId, data: pcmForPlayback });
}
```

- 解码器输出回调是异步的，可能多个帧同时到达
- **没有做预填缓冲**（pre-buffering），Worklet 刚启动时立即欠载
- **没有做帧序号检测**，乱序包直接解码可能导致错误 PCM

### 🔴 问题 3：无丢包隐藏（PLC）

- 当网络丢包或延迟抖动导致帧丢失时，Worklet 直接输出静音
- 没有重复上一帧或做简单的 PLC

### 🔴 问题 4：解码器创建时机问题

**位置**: `client.js` 第 340-345 行

```js
let decoder = peerDecoders.get(senderId);
if (!decoder) {
    decoder = createPeerDecoder(senderId);
    peerDecoders.set(senderId, decoder);
}
```

- 解码器在收到第一个音频包时才创建，此时 **解码器可能还没完成配置**（`decoder.state !== 'configured'`），导致前几帧被丢弃
- 没有解码器预热机制

---

## 解决方案实施计划

按以下优先级修改代码：

### 第一步：修复环形缓冲区（audio-worklet.js）
- 将缓冲区从 8 帧增加到 **20 帧**（1200ms）
- 添加 **预填机制**：缓冲区填满 50% 后才开始播放
- 添加 **PLC（丢包隐藏）**：欠载时重复最后一帧，而不是输出静音
- 修复缓冲区大小计算时机问题

### 第二步：添加解码预填缓冲（client.js）
- 解码器启动后，先缓存 3-4 帧再开始送入 Worklet
- 避免启动瞬间欠载

### 第三步：添加帧序号检测和乱序处理（client.js）
- 在 `handleAudioPacket` 中检测帧序号
- 丢弃重复帧，记录丢包
- 对乱序帧做简单重排

### 第四步：修复解码器创建时机（client.js）
- 在 peer 加入时就创建解码器并预热
- 避免首帧丢失

---

**总结**：问题核心都是**环形缓冲区太小 + 无预填 + 无 PLC**。修复方案将直接解决这 4 个问题，预计可以消除 90% 以上的断音现象。

