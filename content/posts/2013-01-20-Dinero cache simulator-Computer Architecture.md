---
layout: post
title: 'Dinero cache simulator-Computer Architecture'
date: 2013-01-20 14:55:00 +0800
category: from_cnblogs
slug: p20130120145500
---


<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
【问题描述：】</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
PART A.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
The Dinero cache simulator is available on the COE Solaris system (machines alpha, beta). The</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
following information is for using dineroIII (which is the default dinero installed on COE Solaris</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
systems). You will also find dineroIV installed on both Solaris and Linux machines. Please use</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
dineroIII for this assignment. You are welcome to experiment with dineroIV.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Dinero allows you to configure a cache using command line arguments (see attached info on</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Dinero). The simulator takes as input an address trace. The Dinero input format &quot;din&quot; is an</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
ASCII file with one LABEL and one ADDRESS per line. The rest of the line is ignored so that it</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
can be used for comments.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
LABEL = 0 read data</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
= 1 write data</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
= 2 instruction fetch</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
= 3 escape record (treated as unknown access type)</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
= 4 escape record (causes cache flush)</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
0 &lt;= ADDRESS &lt;= ffffffff where the hexadecimal addresses are NOT preceded by &quot;0x.&quot;</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Here are some examples:</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
2 0 This is an instruction fetch at hex address 0.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
0 1000 This is a data read at hex address 1000.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
1 70f60888 This is a data write at hex address 70f60888.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Make sure to use lowercase.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
After creating a trace file (e.g., trace.txt), you run Dinero as follows:</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
dinero –i16KB –d32KB –b32 &lt; trace.txt</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
This will simulate a 16KB instruction cache and a 32KB data cache, each with a 32 byte block</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
size.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Your job is to generate reference streams in Dinero's din format (see description provided with</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
trace) and produce the following (where you are asked to generate an address stream, submit in</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
your report enough of the trace so that we can understand the pattern you are using – do not</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
submit the full trace since it will only waste paper):</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
1. Assume that you have a direct-mapped 16KB instruction cache (32B block). Generate an</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
address stream that will touch every cache line once, but no more than once.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
2. Assume the same instruction cache organization as in (1), but now the instruction cache is 2-</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
way set associative, with LRU replacement. The total cache space is still 16KB with a 32B</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
block, but now you have 1/2 the number of indices. Generate an address stream that touches</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
every cache index only 8 times, producing 5 misses and 3 hits, but only accesses 3 unique</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
addresses per index.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<div style="line-height:1.5">【Solution】</div>
<div style="line-height:1.5">2.</div>
<div style="line-height:1.5">
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 000|0 0000 001|0 0000</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 2020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 001|0 0000 001|0 0000</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 010|0 0000 001|0 0000</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 000|0 0000 001|0 0000</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 010|0 0000 001|0 0000</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 000|0 0000 001|0 0000</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 010|0 0000 001|0 0000</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 2020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 001|0 0000 001|0 0000</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
这是32位的物理地址，Cache 大小16KB = 2^14, 故物理地址的低14位</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
对Cache是唯一对应的，前提是direct association。 此时有32Byte = 2^5</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
的block大小，所以低5位是offset。从而有16K/32 = 2^9 = 2^(14-5) 个block,</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
也就是512个index. 这是direct的情况。如果是2set, 那就是256组，</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
256个index, 占用8位，每组2个block随机分配或者LRU(Least Recent Use)。</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
所以：1.对于direct associate: offset的5位若有不同不会造成miss</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
比如：</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000&nbsp;<wbr></div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 21 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0001</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 22 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0010</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000&nbsp;<wbr></div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 21 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0001</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 22 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0010</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;&nbsp;<wbr>$ dinero -i16K -d16K -b32 -a1 &lt; hw6_1_2d.txt&nbsp;<wbr></div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
预测结果: fetch: 6 &nbsp;<wbr>&nbsp;&nbsp;<wbr>miss: 1 &nbsp;<wbr>&nbsp;【符合】</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;2.对于direct associate: index的变化会有失效产生</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
比如：</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;(miss)1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 2020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|10 0000 001|0 0000 &nbsp;<wbr>&nbsp;(miss)2</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 01|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;(miss)3 与1的index相同，冲掉1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;(miss)4 与3的index相同，冲掉3</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 01|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;(miss)5 与4的index相同，冲掉4</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;(miss)6 与5的index相同，冲掉5</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 01|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;(miss)7 与6的index相同，冲掉6</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 2020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|10 0000 001|0 0000 &nbsp;<wbr>&nbsp;与2的index相同，高位也相同，hit</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>$ dinero -i16K -d16K -b32 -a1 &lt; hw6_1_2d.txt</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
仿真结果：fetch:8 &nbsp;<wbr>&nbsp;&nbsp;<wbr>miss:7</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;3.对于2set associate, 此时只有512/2 = 256 = 2^8 组，index的位数还是视为9：</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;1(miss) -&gt;index1-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 01|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;2(miss) -&gt;index1-2</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 8020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 10|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;3(miss) 冲掉-&gt;index1-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;4(miss) 冲掉-&gt;index1-2</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 8020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 10|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;5hit same with 3&lt;-index1-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;6hit same with 4&lt;-index1-2</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 8020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 10|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;7hit same with 5&lt;-index1-1&nbsp;<wbr></div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 01|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;8(miss) 冲掉-&gt;index1-2 &nbsp;<wbr></div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>$ dinero -i16K -d16K -b32 -a2 &lt; hw6_1_2d.txt</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
仿真结果：fetch:8 &nbsp;<wbr>&nbsp;&nbsp;<wbr>miss:5</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
思路：同样的地址stream, 如果 视为9 和 视为8 不一样，谁符合仿真结果，谁就正确。</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
所以要构造地址stream, 使 视为9 和 视为8 的人工预测结果不一样。</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;4.1对于2set associate, 此时只有512/2 = 256 = 2^8 组，index的位数还是视为9：</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;1(miss) -&gt;index1-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 2020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|10 0000 001|0 0000 &nbsp;<wbr>&nbsp;2(miss) -&gt;index2-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 01|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;3(miss) -&gt;index1-2</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;4hit same with 1&lt;- index1-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 01|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;5hit same with 3&lt;-index1-2</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;6hit same with 4&lt;-index1-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 01|00 0000 001|0 0000 &nbsp;<wbr>&nbsp;7hit same with 5&lt;-index1-2&nbsp;<wbr></div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 2020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 00|10 0000 001|0 0000 &nbsp;<wbr>&nbsp;8hit same with 2&lt;-index2-1&nbsp;<wbr></div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;4.2对于2set associate, 此时只有512/2 = 256 = 2^8 组，index的位数视为8：</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 000|0 0000 001|0 0000 &nbsp;<wbr>&nbsp;1(miss) -&gt;index1-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 2020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 001|0 0000 001|0 0000 &nbsp;<wbr>&nbsp;2(miss) -&gt;index1-2</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 010|0 0000 001|0 0000 &nbsp;<wbr>&nbsp;3(miss) 冲掉-&gt;index1-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 000|0 0000 001|0 0000 &nbsp;<wbr>&nbsp;4(miss) 冲掉-&gt;index1-2</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 010|0 0000 001|0 0000 &nbsp;<wbr>&nbsp;5hit same with 3&lt;-index1-1</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 20 &nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 000|0 0000 001|0 0000 &nbsp;<wbr>&nbsp;6hit same with 4&lt;-index1-2</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 4020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 010|0 0000 001|0 0000 &nbsp;<wbr>&nbsp;7hit same with 5&lt;-index1-1&nbsp;<wbr></div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
2 2020 &nbsp;<wbr>&nbsp;----- 0000 0000 0000 0000 001|0 0000 001|0 0000 &nbsp;<wbr>&nbsp;8(miss) 冲掉-&gt;index1-2&nbsp;<wbr></div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>$ dinero -i16K -d16K -b32 -a2 &lt; hw6_1_2d.txt</div>
<div style="font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
仿真结果：fetch:8 &nbsp;<wbr>&nbsp;&nbsp;<wbr>miss:5</div>
<div><br>
</div>
</div>
</div>
   
