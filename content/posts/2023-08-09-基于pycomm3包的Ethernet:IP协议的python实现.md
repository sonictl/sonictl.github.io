---
layout: post
title:  "基于pycomm3包的Ethernet/IP协议的python实现"
date: 2023-08-09 16:04:26 +0800
categories: Tech
slug: p20230809160426
---



# 基于pycomm3包的Ethernet/IP协议的python实现


Ethernet/IP(EIP)协议的通信python实现，用pycomm3包进行支持。

Ethernet/IP 协议可能要求包含的功能：隐式报文通信、显示报文通信 (Explicit 讯息通信)、支持UCMM 及Class3

如果您需要创建Ethernet/IP协议的自定义实现，则通常需要研究Ethernet/IP规范、处理低级套接字通信、消息编码和解码、消息的组装和拆解以及各种其他特定于协议的细节。这是一项复杂的任务，需要对网络协议和编程有扎实的理解。在大多数情况下，建议使用现有的库或工具来节省时间并确保可靠的通信。

Ethernet/IP 协议的从头实现比较复杂，但是也有相关的库可以调用。比如 python 的 pycomm3。

pycomm3库抽象了许多Ethernet/IP协议的复杂性，但是了解协议的概念和结构对于有效地使用库并排除可能出现的任何问题仍然很重要。

Here's a basic example of how you might use it to communicate with an Ethernet/IP device:
  $ pip install pycomm3

```python
#====== python通信代码：=========

from pycomm3 import LogixDriver

# Create a LogixDriver instance
with LogixDriver('192.168.1.100') as plc:   # replace '192.168.1.100' with the actual IP address of your device

    # Read tag values
    tag_values = plc.read('Tag1', 'Tag2')   # adjust the tag names and values as needed

    # Write tag values
    plc.write('Tag1', 100)


```

以下这段代码将：
    - 定义Ethernet/IP设备的IP地址和端口。
    - 创建到设备的连接
    - 打开一个到设备的会话
    - 读取输入Tag的值并写入值到输出Tag
    - 最后，关闭会话和连接。

```pyrhon

import pycomm3

# Create a connection to the device
conn = pycomm3.Connection("192.168.1.100", 44818)  # Define the IP address and port of the Ethernet/IP device

# Open a session to the device
session = conn.open_session()

# Read the value of an input tag
tag_name = "Input1"
value = session.read_tag(tag_name)

# Write the value of an output tag
tag_name = "Output1"
value = 1
session.write_tag(tag_name, value)

# Close the session
session.close()

# Close the connection
conn.close()

```

pycomm3库提供了各种用于与Ethernet/IP设备通信的其他功能，例如读写多个标签，上传和下载文件，以及执行设备发现。有关更多信息，请参阅pycomm3文档:https://docs.pycomm3.dev/



## python implement for socket and TCP/IP

To implement a communication module with sockets via TCP/IP in Python, you can use the following steps:

1. Import the socket module.
2. Create a socket object.
3. Bind the socket to a port.
4. Listen for connections.
5. Accept a connection.
6. Receive data from the connection.
7. Send data to the connection.
8. Close the connection.

Here is an example of a Python code that implements a communication module with sockets via TCP/IP:

```python
import socket

# Create a socket object
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Bind the socket to a IP and port
port = 8080
IP = '192.168.1.100'
sock.bind((IP, port))

# Listen for connections
sock.listen(1)

# Accept a connection
conn, addr = sock.accept()

# Receive data from the connection
data = conn.recv(1024)

# Send data to the connection
conn.sendall('Hello, world!'.encode())

# Close the connection
conn.close()
```



Reference: [基于python socket实现TCP/UDP通信](https://juejin.cn/post/7035458621051764773)