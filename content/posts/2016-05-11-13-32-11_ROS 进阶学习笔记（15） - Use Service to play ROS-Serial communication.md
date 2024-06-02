---
layout: post
title:  "ROS 进阶学习笔记（15） - Use Service to play ROS-Serial communication"
date: 2016-05-11 13:32:11 +0800
tags: 
slug: p20160511133211
---

# ROS 进阶学习笔记（15） - Use Service to play ROS-Serial communication






### Use Service to play ROS-Serial communication

 Continue for this blog : 


[ROS 进阶学习笔记（12） - Communication with ROS through USART Serial Port](http://blog.csdn.net/sonictl/article/details/50623662)


In that blog above, we discussed ROS-Serial communication through r2serial\_driver node (C++). There is a problem that the r2serial made too much cpu expenses. Like below:


  
 


----------


 **Kevin** mentioned a question: "Interesting, but a lot of my serial stuff is send a message and receive a response back. Instead of separate publish/subscribes. have you tried a service? That seems like it would work nicely for this."


I also noticed a problem: MY Linux's System RAM and CPU seems to be much costed by r2serial\_driver when it is running.  
 


*This time*, we'll see how to use  [Kevin's service](https://github.com/walchko/serial_node) to play ROS serial communication.  
 


### Use Service to play ROS-Serial communication


=============  
 


### -- Check out and compile this package --


Use this commands to check out the rosbuild package (rather than catkin package), androsmakethem:



```
exbot@my_robot:~/rosbuild_ws$ cd sandbox/
exbot@my_robot:~/rosbuild_ws/sandbox$ git clone https://github.com/walchko/serial_node
exbot@my_robot:~/rosbuild_ws/sandbox$ cd serial_node/
exbot@my_robot:~/rosbuild_ws/sandbox/serial_node$ rosmake
...
...
[ rosmake ] /home/exbot/.ros/rosmake/rosmake_output-20160511-173021             

exbot@my_robot:~/rosbuild_ws/sandbox/serial_node$ rospack find serial_node
/home/exbot/rosbuild_ws/sandbox/serial_node
exbot@my_robot:~/rosbuild_ws/sandbox/serial_node$roscore && rosrun serial_node serial_node 0 /dev/ttyUSB0 9600 true

[ INFO] [1462959095.157208573]: Service uc0_serial on /dev/ttyUSB0 @ 9600 baud


```


We can see the expense of serial\_node by command "top": [ The expenses has been very small. ]  
 


![](https://img-my.csdn.net/uploads/201605/11/1462962131_7774.jpg)  
 


====================================================================================


### -- How to use this service for our works --


1.  Understand what is ros service:


* Here, I recommend the newer of ROS to read four links carefully and understand them as deep as you can:
* http://wiki.ros.org/ROS/Tutorials/UnderstandingServicesParams#Using\_rosservice  
 Above, you need to understand the service\_node need a request and will give back a response.  
   
 http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv#Using\_srv  
 Above, you need to understand the .srv file defines the data type of req. and rep.  
   
 http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29  
 Above, you need to understand the key points of a source code for an ROS Service node.  
   
 http://wiki.ros.org/ROS/Tutorials/ExaminingServiceClient  
 Above, describes the source code for client to examine if the service node works fine.


  
 


2.  Read the source code of "serial\_node\_service.cpp" and use "rosservice call" command to test it


          run the service by:



```
        > $ rosrun seal_node serial_node 0 /dev/ttyUSB0 9600 true

```

          call this service by (open another terminal):  
 



```
        > $ rosservice call /uc0_serial d 5 300

```

          If you send "**3C 6D 05 73 6F 6E 69 63 3E**" from your micro controller to your usb-serial port of ROS running on machine, you will see the info as below:  
 


          ![](https://img-blog.csdn.net/20160519110335618?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


            3C  -- ASCII Code for the "<"  
             6D  -- ASCII Code for the "m"  
             05  -- number 5  #This is strange, but the servier is designed like this.  
             73  -- ASCII Code for the "s"  
             6F  -- ASCII Code for the "o"  
             6E  -- ASCII Code for the "n"  
             69  -- ASCII Code for the "i"  
             63  -- ASCII Code for the "c"  
             3E  -- ASCII Code for the ">"


  
 


3.  I modified the code for the serial\_node\_service.cpp file before running the rosservice call command above：




```
1. /*
2. * Copyright (c) 2010, Willow Garage, Inc.
3. * All rights reserved.
4. *
5. * Redistribution and use in source and binary forms, with or without
6. * modification, are permitted provided that the following conditions are met:
7. *
8. *     * Redistributions of source code must retain the above copyright
9. *       notice, this list of conditions and the following disclaimer.
10. *     * Redistributions in binary form must reproduce the above copyright
11. *       notice, this list of conditions and the following disclaimer in the
12. *       documentation and/or other materials provided with the distribution.
13. *     * Neither the name of the Willow Garage, Inc. nor the names of its
14. *       contributors may be used to endorse or promote products derived from
15. *       this software without specific prior written permission.
16. *
17. * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
18. * AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
19. * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
20. * ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE
21. * LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
22. * CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
23. * SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
24. * INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
25. * CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
26. * ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
27. * POSSIBILITY OF SUCH DAMAGE.
28. */
29. 
30. /***********************************************
31. *
32. * rosrun serial_node <uC> <port> <baud>
33. *
34. * uC   = 0, 1, 2, ... what number micro controller
35. * port = serial port, /dev/cu.usbserial
36. * baud = baud rate, 115200
37. *
38. ***********************************************/
39. 
40. #include "ros/ros.h"
41. #include "std_msgs/String.h"
42. #include <pthread.h>
43. #include <sys/types.h>
44. #include <sys/stat.h>
45. #include <sys/ioctl.h>
46. #include <sys/time.h>
47. #include <fcntl.h>
48. #include <termios.h>
49. #include <stdio.h>
50. #include <stdlib.h>
51. #include <queue> // C++ FIFO
52. #include <iostream>
53. 
54. #include "serial_node/serial.h"
55. 
56. 
57. class Serial {
58. public:
59. Serial(ros::NodeHandle& n, int i=0){
60. char sname[30];
61. sprintf(sname, "uc%d_serial",i);
62. name = sname;
63. service = n.advertiseService(name, &Serial::callback, this);
64. debug = false;
65. read_error = read_good = 0;
66. }
67. 
68. ~Serial(){
69. ::close(fd);
70. ROS_INFO("%s stopping", name.c_str());
71. }
72. 
73. inline void setDebug(bool b=true){ debug = b; }
74. 
75. inline void flush(void){ //discard data that was not read
76. tcflush (fd, TCIFLUSH);
77. }
78. 
79. bool callback(serial_node::serial::Request& req,
80. serial_node::serial::Response& res);
81. 
82. int read(char *buf, const int numbytes, const int trys=3);
83. bool readMsg(std::string&, int size, const int trys=3);
84. //int read2(char *buf, const int numbytes, const int trys=3);
85. int write(const char* buf, const int numbytes, const int trys=3);
86. bool open(char *port, int baud);
87. unsigned int available();
88. 
89. std::string name;
90. 
91. protected:
92. unsigned short checksum(char* buffer, int cnt);
93. ros::ServiceServer service;
94. int fd;   //serial port file pointer
95. bool debug;
96. unsigned long read_error;
97. unsigned long read_good;
98. };
99. 
100. // check this!!
101. unsigned short Serial::checksum(char* buffer, int cnt){
102. unsigned int sum = 0; // 32 bit int
103. 
104. for(int i=0;i<cnt;i+=2) sum += *((unsigned short*)buffer++);
105. sum = (sum & 0xFFFF) + (sum >> 16);
106. 
107. return(~sum);
108. }
109. 
110. //Initialize serial port, return file descriptor
111. bool Serial::open(char *port, int baud){
112. char msg[70];
113. struct termios options;
114. 
115. fd = ::open( port, O_RDWR | O_NOCTTY | O_NDELAY /*| O_NONBLOCK*/ ); //may not need the nodelay
116. if( fd <= 0 ){
117. // [FIXME 20120322] errno not defined in linux
118. sprintf(msg,"ERROR: Unable to open port %s:%d - %s",port,baud,strerror(errno));
119. perror(msg);
120. return false;
121. }
122. 
123. fcntl(fd, F_SETFL, FNDELAY); // return if nothing there to read
124. 
125. //get config from fd and put into options
126. tcgetattr (fd, &options);
127. 
128. // Enable the receiver and set local mode
129. options.c_cflag |= (CLOCAL | CREAD);
130. 
131. //give raw data path
132. cfmakeraw (&options);
133. 
134. //set baud
135. cfsetspeed(&options, baud);
136. 
137. //send options back to fd
138. tcsetattr (fd, TCSANOW, &options);
139. 
140. Serial::flush();
141. 
142. return true;
143. } //serialInit
144. 
145. #define __K_SERIAL_DEBUG__ 0
146. 
147. int Serial::read(char *buf, const int numbytes, const int trys){
148. int i, numread = 0, n = 0, numzeroes = 0;
149. 
150. while (numread < numbytes){
151. //printf("%d .\n", fd);
152. n = ::read (fd, (buf + numread), (numbytes - numread));
153. if (n < 0)
154. return -1;
155. else if (0 == n){
156. numzeroes++;
157. if (numzeroes > trys)
158. break;
159. }
160. else numread += n;
161. }
162. 
163. if (__K_SERIAL_DEBUG__){
164. printf ("Read:   ");
165. for (i = 0; i < numread; i++)
166. printf ("%d ", buf[i]);
167. printf ("\nRead %d of %d bytes\n", numread, numbytes);
168. }
169. 
170. //tcflush (fd, TCIFLUSH);			//discard data that was not read
171. return numread;
172. }
173. 
174. bool getChar(char& c, int fd, const int trys=20){
175. int n = 0;
176. int numzeroes = 0;
177. 
178. // wait until start char found
179. while (numzeroes++ < trys){
180. n = ::read (fd, &c, 1);
181. if(n == 1){
182. return true;
183. }
184. usleep(1000);
185. }
186. 
187. return false;
188. }
189. 
190. inline char makeReadable(char c){
191. return (c < 32 ? '.' : (c > 126 ? '.' : c));
192. }
193. 
194. void printMsg(std::string& msg){
195. std::cout<<"---[[ "<<msg[1]<<" ]]-----------------------------------"<<std::endl;
196. std::cout<<"Size data: "<<(int)msg[2]<<"\t"<<"Size Msg: "<<msg.size()<<std::endl;
197. std::cout<<"Start/End characters: "<<msg[0]<<" / "<<msg[msg.size()-1]<<std::endl;
198. int size = (int)msg[2];
199. if(size > 0){
200. std::cout<<"Raw:  ";
201. for(int i=0;i<size;i++) std::cout<<(int)msg[3+i]<<' ';
202. std::cout<<std::endl;
203. std::cout<<"ASCII: ";
204. for(int i=0;i<size;i++) std::cout<<makeReadable(msg[3+i])<<' ';
205. std::cout<<std::endl;
206. }
207. std::cout<<"-------------------------------------------"<<std::endl;
208. }
209. 
210. bool Serial::readMsg(std::string& ucString, int size, const int trys){
211. int numread = 0, n = 0, numzeroes = 0;
212. char c = 0;
213. char buf[128];
214. 
215. // wait until start char found
216. while (numzeroes++ < trys){
217. n = ::read (fd, &c, 1);
218. if(c == '<') break;
219. usleep(1000);
220. ROS_INFO("wait uC's: < ");
221. }
222. 
223. // get start char
224. while(c != '<'){
225. if(!getChar(c,fd)) return false;
226. }
227. buf[0] = c;
228. numread++;
229. ROS_INFO("Serial::readMsg: got start char <");
230. 
231. // get message char
232. if(!getChar(c,fd)) return false;
233. buf[1] = c;
234. numread++;
235. 
236. //get message size
237. if(!getChar(c,fd)) return false;
238. buf[2] = c;
239. numread++;
240. n = (int)c;
241. ROS_INFO("uC data size: %d",n);
242. 
243. // get data section
244. if(n){
245. for(int i=0;i<n;i++){
246. if(!getChar(c,fd)) return false;
247. buf[3+i] = c;
248. numread++;
249. }
250. }
251. 
252. // get end char
253. if(!getChar(c,fd)) return false;
254. if(c != '>') return false;
255. buf[numread] = c;
256. numread++;
257. ROS_INFO("got end char >");
258. 
259. ucString.assign(buf,numread);
260. 
261. if(debug) printMsg(ucString);
262. 
263. return true;
264. }
265. 
266. 
267. /**
268. * Determine the number of bytes in the input buffer without the need to
269. * read it.
270. */
271. unsigned int Serial::available(void){
272. int bytes = 0;
273. ioctl(fd, FIONREAD, &bytes);
274. 
275. //ROS_INFO("bytes avail: %d",bytes);
276. 
277. return (unsigned int)bytes;
278. }
279. 
280. /**
281. * Write a specific number of bytes from a buffer
282. * \note write() will attempt to write to the serial port several
283. *       times before failing
284. * \return number of bytes written
285. */
286. int Serial::write(const char* buf, const int numbytes, const int trys){
287. int i, numwritten = 0, n = 0, numzeroes = 0;
288. 
289. while (numwritten < numbytes){
290. n = ::write (fd, (buf + numwritten), (numbytes - numwritten));
291. 
292. if (n < 0) return -1;
293. 
294. else if (0 == n){
295. numzeroes++;
296. 
297. if (numzeroes > trys) break;
298. }
299. else numwritten += n;
300. }
301. 
302. if (__K_SERIAL_DEBUG__){
303. printf ("cwrite[%d]: ", numbytes);
304. for (i = 0; i < numbytes; i++) printf ("%d ", buf[i]);
305. printf ("\n");
306. }
307. 
308. return numwritten;
309. }
310. 
311. //Process ROS command message, send to uController
312. bool Serial::callback(serial_node::serial::Request& req,
313. serial_node::serial::Response& res){
314. 
315. Serial::flush();
316. 
317. ROS_INFO("%s command: %s", name.c_str(), req.str.c_str());
318. Serial::write(req.str.c_str(),req.str.size());
319. usleep(3000);
320. 
321. ROS_INFO("send: %s[%d]",req.str.c_str(),req.str.size());
322. 
323. // wait x msec
324. int miss = 0;
325. while( available() < req.size ){
326. if(miss++ < req.time) usleep(1000);
327. else { // resend
328. Serial::flush();  //Commented originally*********
329. Serial::write(req.str.c_str(),req.str.size());
330. miss = 0;
331. ROS_INFO("Resending: avail[%d] need[%d]",
332. Serial::available(), req.size);
333. // QoS
334. ++read_error;
335. usleep(1000); //commented originally*************
336. }
337. 
338. //if(!ros::ok()) return 0;
339. //ROS_INFO("loop: %d", available());
340. }
341. 
342. 
343. if(req.size > 0){
344. usleep(1000);
345. 
346. ROS_INFO("going to read");
347. int rcvBufSize = 200;
348. char ucResponse[rcvBufSize];
349. std::string ucString;
350. int num = (req.size > available() ? available() : req.size);
351. ROS_INFO("num= %d, going to readMsg.", num);
352. bool ok = Serial::readMsg(ucString,req.time);
353. ROS_INFO("got:%s",ucResponse);
354. ROS_INFO("done to read");
355. 
356. if (ok) {
357. ROS_INFO("%s response: %s", name.c_str(), ucResponse);
358. res.str = ucString;
359. 
360. // QoS
361. ++read_good;
362. }
363. else{
364. ROS_ERROR("Error: %s",req.str.c_str());
365. res.str = "error";
366. Serial::flush();
367. 
368. // QoS
369. ++read_error;
370. return false;
371. }
372. }
373. else res.str = "success";
374. 
375. 
376. Serial::flush();
377. return true;
378. 
379. } //ucCommandCallback
380. 
381. 
382. 
383. int main(int argc, char **argv){
384. char *port;    //port name
385. int baud;     //baud rate
386. int uC;
387. 
388. //Initialize ROS
389. ros::init(argc, argv, "serial_node");
390. ros::NodeHandle rosNode;
391. 
392. Serial serial(rosNode,0);
393. 
394. //Open and initialize the serial port to the uController
395. if (argc > 1) uC = atoi(argv[1]);
396. else {
397. ROS_ERROR("ucontroller index parameter invalid");
398. return 1;
399. }
400. 
401. 
402. //strcpy(port, DEFAULT_SERIALPORT);
403. if (argc > 2) port = argv[2];
404. else {
405. ROS_ERROR("need port defined");
406. return 1;
407. }
408. 
409. if (argc > 3) baud = atoi(argv[3]);
410. else {
411. ROS_ERROR("ucontroller baud rate parameter invalid");
412. return 1;
413. }
414. 
415. bool debug;
416. if (argc > 4) debug = (strcmp(argv[4],"true") ? false : true);
417. else debug = false;
418. serial.setDebug(debug);
419. 
420. //ROS_INFO("Service %s on %s @ %d baud",serial.name.c_str(), port, baud);
421. 
422. if (serial.open(port, baud) == false){
423. ROS_ERROR("unable to create a new serial port");
424. return 1;
425. }
426. else ROS_INFO("Service %s on %s @ %d baud",serial.name.c_str(), port, baud);
427. 
428. //Process ROS messages and send serial commands to uController
429. ros::spin();
430. 
431. return 0;
432. }
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  


### For conclusion:


1. There seems nothing to do about System Resouces Expenses and whether use service or not.
2. This service node expenses less than r2serial\_driver. The machanisms of calling serial port of this service\_node can be refered when sometime we need to write a less-expense usb\_serial driver in ROS.
3. The way using this serial\_node\_service is pretty weird since it requires HEX input from the uController.
4. I will not continue for this topic since this cost me too much time.


  
 


  
 


====================================================================================


====================================================================================


### -- The Catkin way to compile (problem) --



The Catkin way to compile the serial\_node service pack: (But I met a lot of errors...)



```
sonictl@my_robot_pc:~/catkin_ws/src$ git clone https://github.com/walchko/serial_node
Cloning into 'serial_node'...
remote: Counting objects: 95, done.
remote: Compressing objects: 100% (39/39), done.
remote: Total 95 (delta 47), reused 95 (delta 47), pack-reused 0
Unpacking objects: 100% (95/95), done.

sonictl@my_robot_pc2:~/catkin_ws/src$ cd serial_node/

sonictl@my_robot_pc2:~/catkin_ws/src/serial_node$ git checkout catkin
Branch catkin set up to track remote branch catkin from origin.
Switched to a new branch 'catkin'

sonictl@my_robot_pc2:~/catkin_ws/src/serial_node$ cd ~/catkin_ws/
sonictl@my_robot_pc2:~/catkin_ws$ catkin_make

```

But I met a lot of errors...


====================================================================================


====================================================================================


====================================================================================


  
 




