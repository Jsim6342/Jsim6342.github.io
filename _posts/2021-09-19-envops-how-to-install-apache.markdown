---
layout: post
title:  "[서버구축] APM 서버 구축(아파치 소스코드 설치)"
subtitle:   "우분투에 아파치 소스코드 설치하기"
categories: envops
tags: envops apache
use_math: False
comments: False
---

## 개요
> `우분투에 아파치 소스코드 설치하는 방법`에 대한 정리글입니다.

- 목차
	- [APM이란?](#APM이란?) 
    - [의존성 패키지 설치](#의존성-패키지-설치)
    - [아파치 소스코드 설치](#아파치-소스코드-설치)


## APM이란?
---

* __서버 구성__  
APM에 대해서 말하기 전에 서버에 대해서 알아야 한다. 흔히, 클라이언트의 요청에 응답해주는 것을 서버라고 한다.  
서버를 세부적으로 보자면, 서버 프로그램, BL(Backend Language), DB로 볼 수 있다.  
클라이언트 요청을 맨 처음 서버 프로그램이 가공하여 BL의 비즈니스 로직을 타고, DB에 접근 후, 다시 클라이언트에게 요청해주는 흐름이다.  


* __APM이란?__  
그럼 APM은 무엇일까? APM은 Apache, PHP, MySQL의 약자이다. 앞서 말한 서버를 구성하는데 있어서, 서버 프로그램은 Apache, BL은 PHP, DB는 MySQL로 구성된 서버를 지칭한다.  
즉, APM 서버를 구축한다는 것은 Apache와 PHP, MySQL로 구성된 서버를 구축한다는 의미이다.  
이번 포스팅에서는 앞서 구축한 우분투 vm에 APM을 소스코드로 구축해보고자 한다.   


## 의존성 패키지 설치
---


* __필수 패키지 설치__  
Apache를 소스설치 하면서 없으면 에러가 발생할 수 있는 필수 패키지들을 미리 설치하도록 한다.  
[참고 사이트](https://inma06.tistory.com/62)
```
$ sudo su
$ apt-get install gcc
$ apt-get install --reinstall make
$ apt-get install libexpat1-dev
$ apt-get install g++
```


* __의존성 패키지 설치__  
Apache 다운로드에 필요한 APR(Apache Portable Runtime), PCRE(Perl Compatible Regular Expressions)와 같은 의존성 패키지를 설치한다.  

  1. apr 설치  
  apr과 apr-util을 설치해준다.  
  ```
  $ wget http://mirror.navercorp.com/apache//apr/apr-1.7.0.tar.gz #(apr주소)
  $ wget http://mirror.navercorp.com/apache//apr/apr-util-1.6.1.tar.gz #(apr-util주소)
  $ tar xvfz apr-1.7.0.tar.gz #이 소스로 apr 파일을 압축을 해제해준다.
  $ tar xvfz apr-util-1.6.1.tar.gz #이 소스로 apr-util 파일 압축을 해제해준다.
  ```
  wget: web get의 약자로 웹에 있는 파일을 다운로드 받을 때 사용하는 명령어  
  tar xvfz: tar.gz 형식의 압축된 파일을 압축해제 해주는 명령어  
    

  압축해제한 apr 파일을 각각 컴파일 및 설치해준다.  
  ```
  $ cd usr/local/apr-1.7.0
  $ ./configure --prefix=/usr/local/apr
  $ make
  $ make install
  ```
  ```
  $ cd usr/local/apr-1.7.0
  $ ./configure --prefix=/usr/local/apr
  $ make
  $ make install
  ```
  ./configure --prefix=/usr/local/apr: 어떤 파일을 /usr/local/apr 에 설치하겠다는 뜻  
  make: 소스를 컴파일한다는 뜻  
  make install: make를 통해 만들어진 설치 파일을 설치한다는 뜻  


  2. pcre 설치  
  apr과 같은 방식으로 pcre도 설치해준다.
  ```
  $ cd usr/local
  $ wget ftp://ftp.pcre.org/pub/pcre/pcre-8.43.tar.gz
  $ tar xvfz pcre-8.43.tar.gz
  $ cd usr/local/pcre-8.43
  $ ./configure --prefix=/usr/local/pcre
  $ make
  $ make install
  ```


## 아파치 소스코드 설치
---

* __아파치 설치__  
[아파치 다운로드 페이지](http://httpd.apache.org/download.cgi)에서 최신 버전 확인하고 링크를 복사.
```
$ cd /usr/local
$ wget https://dlcdn.apache.org//httpd/httpd-2.4.48.tar.gz
$ tar xvfz httpd-2.4.48.tar.gz
```


configure 스크립트를 이용해서 아파치 소스 트리를 구성한다.  
> tip. Bash 명령어를 한줄에 다 쓸 수 없을경우, 다음줄로 이어서 적을 때 첫 줄의 마지막에 \를 붙여주면 된다.
```
$ cd httpd-2.4.46
$ ./configure --prefix=/usr/local/apache2.4 \
--enable-module=so --enable-rewrite --enable-so \
--with-apr=/usr/local/apr \
--with-apr-util=/usr/local/apr-util \
--with-pcre=/usr/local/pcre \
--enable-mods-shared=all
$ make
$ make install
```


* __아파치 실행__  
```
$ sudo /usr/local/apache2.4/bin/httpd -k start
```
시작: -k start 또는 start. (-k start 는 httpd 가 죽으면 재시작한다는 뜻)  
종료: -k stop 또는 stop.  


* __아파치 동작 확인__  
실행한 아파치가 정상적으로 동작하는지 확인  
```
$ ps -ef|grep httpd|grep -v grep
$ sudo netstat -anp|grep httpd
$ sudo curl http://127.0.0.1
```
ps 는 preocess status 의 준말로, 현재 실행중인 프로세스의 목록을 보여준다.  
netstat 은 네트워크 연결상태, 라우팅테이블, 인터페이스 상태등을 보여준다.  
curl 은 command line 기반의 웹 요청도구이다. curl options url 형태로 사용한다.  


* __httpd 서비스 등록__  
ubuntu 부팅 시에 Apache가 자동으로 실행되도록 설정해준다.  
/usr/local/apache2.4/bin/apachectl 이라는 파일을 /etc/init.d/httpd 에 복사한다.  
```
$ cp /usr/local/apache2.4/bin/apachectl /etc/init.d/httpd
```
/etc/rc.d/: 부팅레벨별 부팅스크립트파일들이 존재하는 디렉토리.  
/etc/rc.d/init.d/: 시스템 초기화 파일들의 실제파일들이 존재함.  


우분투 서버 부팅시에 실행시킬 스크립트에 아파치 서비스를 등록해준다.  
```
$ update-rc.d httpd defaults
```


## 참고

- [https://minhyeok-rithm.tistory.com/entry/Install-Apache](https://minhyeok-rithm.tistory.com/entry/Install-Apache)  
- [https://resilient-923.tistory.com/100](https://resilient-923.tistory.com/100)
- [https://happylulurara.tistory.com/136](https://happylulurara.tistory.com/136)