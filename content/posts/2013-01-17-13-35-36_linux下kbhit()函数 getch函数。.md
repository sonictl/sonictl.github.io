---
layout: post
title:  "linux下kbhit()函数 getch函数。"
date: 2013-01-17 13:35:36 +0800
tags: 
slug: p20130117133536
---

# linux下kbhit()函数 getch函数。





### 怎样在C语言执行循环程序时按键盘任意键停止程序，再按再继续执行？ 参考http://zhidao.baidu.com/question/272086855.html  在[linux](https://so.csdn.net/so/search?q=linux&spm=1001.2101.3001.7020)下C语言写了一个while循环，怎么实现按任意键退出。如何编写程序？ 参考：http://zhidao.baidu.com/question/461121586.html



  



 对于上面的问题，都用到 
 ### linux下的getch函数与kbhit函数。



 参考了http://kpld8888.wordpress.com/2007/03/07/linux%E4%B8%8B%E7%9A%84getch%E5%87%BD%E6%95%B0%E4%B8%8Ekbhit%E5%87%BD%E6%95%B0-2/ 
 

 和：http://zhidao.baidu.com/question/461121586.html 
 

  



 但是我照搬上面两条都不能很好的编译，总是有错误。奉上我的文件： 
 

 将下列三个文件放在工程目录下，build project即可。 
 

  



 main.c: 
 


```
1. #include "kbhit.h"
2. #include <stdio.h>
3. #include <stdlib.h>
4. #include "accelerometer.h" //contains the middle level macros, state variable
5. // definition and declaration for the
6. //middle layer (class layer) functions
7. 
8. 
9. 
10. void delay(int a){
11. int i,j,delay;
12. for(i = 0 ;  i < 1170*(200/a); i++){
13. delay = 0;
14. for(j = 0; j < 100; j++){
15. delay = delay+1;
16. }
17. }
18. }
19. 
20. int main()
21. {
22. 
23. float x,y,z;       // store x,y,z acceleration
24. 
25. acc_t state;
26. 
27. int i = 0;
28. int Hz,N;
29. 
30. 
31. // initialize using ADXL345 at 50 HZ
32. if (acc_Init(&state,ADXL345,50,"/dev/input/event3") < 0) {
33. 
34. printf("Cannot Init \n");
35. return (-1);
36. }
37. 
38. // No Auto Sleep1:
39. if (acc_autosleepSet(&state,NO_AUTOSLEEP) < 0) {
40. 
41. printf("Failed to set AutoSleep condition\n");
42. }
43. // No Auto Sleep2:
44. system("echo 0 > /sys/bus/i2c/devices/0-0053/autosleep");
45. 
46. //--------------Initialize finished. --------------//
47. 
48. printf("input the number of data, Hz: ");
49. scanf("%d,%d", &N, &Hz);
50. 
51. while(i < N){
52. if (acc_xyzGet(&state,&x,&y,&z) < 0) {  // get acceleration
53. 
54. printf("Failed to read acceleration values via non blocking read\n");
55. }
56. else {
57. 
58. printf("%f,%f,%f\n",x-0.573,y,z); //x correction
59. }
60. delay(Hz);
61. i++;
62. if( kbhit()>0 ){
63. break;
64. }
65. }
66. 
67. 
68. 
69. if (acc_Release(&state) < 0) {
70. 
71. printf("Failed to Release Accelerometer Nuggets\n");
72. 
73. return (-1);
74. }
75. else{
76. printf("Accelerometer Released! \n");
77. }
78. 
79. return 0;
80. }
81. 
82. /* 	if (acc_xyzGet(&state,&x,&y,&z) < 0) {  // get acceleration
83. 
84. printf("Failed to read acceleration values via non blocking read\n");
85. }
86. else {
87. 
88. printf("Accelerations:::  X=%f,  Y=%f,  Z=%f\n",x,y,z);
89. }
90. 
91. if (acc_xyzEventGet(&state,&x,&y,&z) < 0) {  // get acceleration
92. 
93. printf("Failed to read acceleration values via event get\n");
94. }
95. else {
96. 
97. printf("Accelerations:::  X=%f,  Y=%f,  Z=%f\n",x,y,z);
98. 
99. }
100. 
101. if (acc_xyzEventGet(&state,&x,&y,&z) < 0) {  // get acceleration
102. 
103. printf("Failed to read acceleration values via event get\n");
104. }
105. else {
106. 
107. printf("Accelerations:::  X=%f,  Y=%f,  Z=%f\n",x,y,z);
108. 
109. }
110. 
111. if (acc_freefallEventGet(&state) < 0) {
112. 
113. printf("Failed to wait for Freefall \n");
114. }
115. else {
116. 
117. printf ("Freefall!!\n");
118. }
119. 
120. 
121. */

```

  

  



 kbhit.c 
 


```
1. /*#include <termios.h>
2. #include <unistd.h>
3. #include <assert.h>
4. #include <string.h>
5. #include <stdlib.h>
6. #include <sys/ioctl.h>
7. #include <sys/time.h>
8. #include <sys/types.h>
9. #include "kbhit.h"
10. 
11. static struct termios initial_settings, new_settings;
12. static int peek_character = -1;
13. 
14. void init_keyboard()
15. {
16. tcgetattr(0,&initial_settings);
17. new_settings = initial_settings;
18. new_settings.c_lflag &= ~ICANON;
19. new_settings.c_lflag &= ~ECHO;
20. new_settings.c_lflag &= ~ISIG;
21. new_settings.c_cc[VMIN] = 1;
22. new_settings.c_cc[VTIME] = 0;
23. tcsetattr(0, TCSANOW, &new_settings);
24. }
25. 
26. void close_keyboard()
27. {
28. tcsetattr(0, TCSANOW, &initial_settings);
29. }
30. 
31. int kbhit()
32. {
33. unsigned char ch;
34. int nread;
35. 
36. if (peek_character != -1) return 1;
37. new_settings.c_cc[VMIN]=0;
38. tcsetattr(0, TCSANOW, &new_settings);
39. nread = read(0,&ch,1);
40. new_settings.c_cc[VMIN]=1;
41. tcsetattr(0, TCSANOW, &new_settings);
42. if(nread == 1)
43. {
44. peek_character = ch;
45. return 1;
46. }
47. return 0;
48. }
49. 
50. int readch()
51. {
52. char ch;
53. 
54. if(peek_character != -1)
55. {
56. ch = peek_character;
57. peek_character = -1;
58. return ch;
59. }
60. read(0,&ch,1);
61. return ch;
62. }
63. */
64. 
65. #include <termios.h>
66. #include <unistd.h>
67. #include <assert.h>
68. #include <string.h>
69. #include <stdlib.h>
70. #include <sys/ioctl.h>
71. #include <sys/time.h>
72. #include <sys/types.h>
73. #include "kbhit.h"
74. 
75. #undef TERMIOSECHO
76. #define TERMIOSFLUSH
77. 
78. /*
79. * kbhit() — a keyboard lookahead monitor
80. *
81. * returns the number of characters available to read
82. */
83. int kbhit ( void )
84. {
85. struct timeval tv;
86. struct termios old_termios, new_termios;
87. int            error;
88. int            count = 0;
89. tcgetattr( 0, &old_termios );
90. new_termios              = old_termios;
91. /*
92. * raw mode
93. */
94. new_termios.c_lflag     &= ~ICANON;
95. /*
96. * disable echoing the char as it is typed
97. */
98. new_termios.c_lflag     &= ~ECHO;
99. /*
100. * minimum chars to wait for
101. */
102. new_termios.c_cc[VMIN]   = 1;
103. /*
104. * minimum wait time, 1 * 0.10s
105. */
106. new_termios.c_cc[VTIME]  = 1;
107. error                    = tcsetattr( 0, TCSANOW, &new_termios );
108. tv.tv_sec                = 0;
109. tv.tv_usec               = 100;
110. /*
111. * insert a minimal delay
112. */
113. select( 1, NULL, NULL, NULL, &tv );
114. error                   += ioctl( 0, FIONREAD, &count );
115. error                   += tcsetattr( 0, TCSANOW, &old_termios );
116. return( error == 0 ? count : -1 );
117. }  /* end of kbhit */
118. /*————————————————*/
119. 

```

  
 kbhit.h 
 


```
1. #ifndef KBHITh
2. #define KBHITh
3. 
4. void init_keyboard(void);
5. void close_keyboard(void);
6. int kbhit(void);
7. int readch(void);
8. 
9. #endif

```

  

  





