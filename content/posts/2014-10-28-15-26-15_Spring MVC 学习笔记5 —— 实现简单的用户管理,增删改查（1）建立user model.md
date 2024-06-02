---
layout: post
title:  "Spring MVC 学习笔记5 —— 实现简单的用户管理,增删改查（1）建立user model"
date: 2014-10-28 15:26:15 +0800
tags: 
slug: p20141028152615
---

# Spring MVC 学习笔记5 —— 实现简单的用户管理,增删改查（1）建立user model





在上一节的基础上，增加 user [model](https://so.csdn.net/so/search?q=model&spm=1001.2101.3001.7020), 并在对应的class代码中，利用Eclipase提供的Source - Generate Getter and Setters/  Generate Construction Using Fields...


  
 


  
 


1. 新建Package：edu.bit.model, 新建Class：" User "




```
1. package edu.bit.model;
2. 
3. public class User {
4. private String username;
5. private String password;
6. private String nickname;
7. private String email;
8. 
9. <span style="white-space:pre">	</span>//写到这里，Resouce - Getter and Setter/ Construction using fields，生成下面：
10. 
11. public User(){
12. 
13. }
14. 
15. public String getUsername() {
16. return username;
17. }
18. 
19. public void setUsername(String username) {
20. this.username = username;
21. }
22. 
23. public String getPassword() {
24. return password;
25. }
26. 
27. public void setPassword(String password) {
28. this.password = password;
29. }
30. 
31. public String getNickname() {
32. return nickname;
33. }
34. 
35. public void setNickname(String nickname) {
36. this.nickname = nickname;
37. }
38. 
39. public String getEmail() {
40. return email;
41. }
42. 
43. public void setEmail(String email) {
44. this.email = email;
45. }
46. 
47. public User(String username, String password, String nickname, String email) {
48. super();
49. this.username = username;
50. this.password = password;
51. this.nickname = nickname;
52. this.email = email;
53. }
54. }

```

  


2. Resouce - Getter and Setter/ Construction using fields


才得到上面的代码。


  
 


3. 创建一个UserController来得到映射。  
 


        在Package“edu.bit.controller”中，new - class - name="UserController" - finish


  
 


4. 文件UserController.java，写代码：


@Controller, 


@RequestMapping("/users")


  
 



  


  
 




