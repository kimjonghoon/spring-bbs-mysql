SpringBbs
============

## A Bulletin Board Program with 
* Spring 6
* Spring Security 6
* MyBatis-Spring
* Bean Validation
* i18n

## Database Design (MySQL)

	mysql --user=root --password mysql
	
	create user 'java'@'%' identified by 'school';
	grant all privileges on *.* to 'java'@'%';
	
	create database javaskool;
	exit;
	
	mysql --user=java --password javaskool
	
	create table member (
	    email varchar(60) PRIMARY KEY,
	    passwd varchar(200) NOT NULL,
	    name varchar(20) NOT NULL,
	    mobile varchar(20)
	);
	
	create table authorities (
	    email VARCHAR(60) NOT NULL,
	    authority VARCHAR(20) NOT NULL,
	    CONSTRAINT fk_authorities FOREIGN KEY(email) REFERENCES member(email)
	);
	
	CREATE UNIQUE INDEX ix_authorities ON authorities(email,authority); 
	
	create table board (
	    boardcd varchar(20),
	    boardnm varchar(40) NOT NULL,
	    boardnm_ko varchar(40) NOT NULL,
	    constraint PK_BOARD PRIMARY KEY(boardcd)
	);
	
	create table article (
	    articleno int NOT NULL AUTO_INCREMENT,
	    boardcd varchar(20),
	    title varchar(200) NOT NULL,
	    content text NOT NULL,
	    email varchar(60),
	    hit bigint,
	    regdate datetime,
	    constraint PK_ARTICLE PRIMARY KEY(articleno),
	    constraint FK_ARTICLE FOREIGN KEY(boardcd) REFERENCES board(boardcd)
	);
	
	create table comments (
	    commentno int NOT NULL AUTO_INCREMENT,
	    articleno int,
	    email varchar(60),
	    memo varchar(4000) NOT NULL,
	    regdate datetime,
	    constraint PK_COMMENTS PRIMARY KEY(commentno)
	);
	
	create table attachfile (
	    attachfileno int NOT NULL AUTO_INCREMENT,
	    filename varchar(255) NOT NULL,
	    filetype varchar(255),
	    filesize bigint,
	    articleno int,
	    email varchar(60),
	    filekey varchar(255),
	    creation datetime,
	    constraint PK_ATTACHFILE PRIMARY KEY(attachfileno)
	);
	
	create table views (
	  no int primary key AUTO_INCREMENT,
	  articleNo int,
	  ip varchar(60),
	  yearMonthDayHour char(10),
	  unique key (articleNo, ip, yearMonthDayHour)
	);
	
	insert into board values ('chat','Chat','자유게시판');
	commit;

## Have to do
### 1.Modify the **UPLOAD_PATH** in WebContants.java
(e.g. /home/kim/upload/ => C:/Users/kim/upload/)

On Linux, the following additional work is required.

> sudo chown -R tomcat:tomcat /home/kim/upload/

> sudo chmod u=rwX,g=rwXs,o=rX /home/kim/upload

### 2.Modify the **fileName** in log4j.xml
(e.g. /home/kim/logs/A1.log => C:/Users/kim/logs/A1.log)
> &lt;File name="A1" fileName="**C:/Users/kim/logs/A1.log**" append="false"&gt;

On Linux, the following additional work is required.
#### 1 Change ownership to the directory where log files are to be created.

> sudo chown -R tomcat.tomcat logs/

#### 2 You may also need the following:

> sudo chmod 644 A1.log

## How to run

### 1. (Current Settings)
Edit ROOT.xml.(Context file of Tomcat's root application)

> **mvn compile war:inplace**

Start Tomcat and visit http://localhost:8080
(If dependencies change, run $ **mvn clean** first)

### 2.
if you want use jetty plugin, edit pom.xml and run $ **mvn jetty:run**. 

## for Admin Test
After sign up, add admin role as following.

	insert into authorities values ('User Email','ROLE_ADMIN');
