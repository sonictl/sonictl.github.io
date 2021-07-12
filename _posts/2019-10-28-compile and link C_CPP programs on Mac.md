---
layout: post
title: 'compile and link C/CPP programs on Mac'
date: 2019-10-28 10:32:00 +0800
category: from_cnblogs
slug: p20191028103200
---
ref: https://stackoverflow.com/questions/29987716/cannot-use-gsl-library-on-macos-ld-symbols-not-found-for-architecture-x86-6

>
Indeed, as @trojanfoe and @bergercookie said you have to compile your file and then link it to the library. As explained in compile and link, for that particular example:
First compile the file:
 `gcc -Wall -I/usr/local/include -c example.c -o example.o`
second, link it to the library:
 `gcc -L/usr/local/lib example.o -lgsl -o example`
where of course, /usr/local/lib should be replaced for the path where you have gsl installed.
The `example` file is excutable.
EDIT: in macOS, from Yosemite the default location for the installation is /opt/local/lib (and /opt/local/include)