<link rel='stylesheet' href='https://raw.github.com/hyonani/pcube/master/WebContent/css/jasonm23_10.css'/>

![](https://raw.github.com/hyonani/pcube/master/WebContent/images/Developer_Manual.jpg)

<h1> Chapter 1 </h1>
<h2> P-CUBE 개발환경 개요 </h2>
- <h3> 각 서버별 소프트웨어 용도 및 버전 </h3>

	1. WEB 서버의 운영체제는 RedHat AS4 버전을 사용했으며, 클라이언트로부터 HTTP 요청을 받아들이고 HTML 문서와 같은 웹페이지의 콘텐츠에 따라 HTTP에 반응하는 기능을 제공하는 소프트웨어는 Apache 2.0.54 버전을 사용하였습니다.  
	2. WAS 서버의 운영체제는 Web 서버와 같으며, 인터넷 상에서 HTTP를 통해 사용자 컴퓨터나 장치에 P-CUBE 어플리케이션을 실행해주는 미들웨어 소프트웨어는 Tomcat 6.0 버전, 빌드 환경을 위해서 JDK 1.6 버전을 사용하였습니다.  
	3. DB 서버의 운영체제는 Web 서버와 같으며, MySQL 5.5.23 버전을 사용해 시스템을 운영하는데 필요한 데이터를 저장하였습니다.  

- ### 각 서버별 정보 ###

	1. WEB / WAS 서버의 상세 설명은 Chapter 2 ~ 4 를 참고해 주십시오.  
	2. WAS / DB 서버에 접속하기 위한 Username 및 Password는 보안상의 이유로 이 문서에는 포함되지 않습니다.  

- ### P-CUBE 의 GitHub repository ###

	1. GitHub는 소스관리를 하기위한 분산형 버전관리 시스템 으로서 서버에 설치할 P-CUBE 파일을 만들기 위해 사용합니다.  
	2. P-CUBE 프로젝트의 GitHub 정보 및 소스 받는 방법은 Chapter6을 참고해 주십시오.  
  
# Chapter 2 #
## DB 서버 설정 ##
- ### MySQL 설치 ###

	1. [http://dev.mysql.com/downloads](http://dev.mysql.com/downloads) 에서 MySQL을 다운로드 받을 수 있습니다.  
	2. P-CUBE를 설치할 리눅스플랫폼에 맞는 MySQL community edition을 다운로드 해주십시오.  
	3. 이미 설치된 MySQL이 있다면 아래와 같이 삭제해 주십시오.  
		> $ **rpm -qa | -i mysql**  
		> mysql-5.0.22-2.1.0.1  
		> mysqlclient10-3.23.58-4.RHEL4.1  

		> $ **rpm -e mysql --nodeps**  
		> warning: /etc/my.cnf saved as /etc/my.cnf.rpmsave  
		> $ **rpm -e mysqlclient10**  
	4. MySQL 패키지 설치를 아래와 같이 해주십시오.  
		> $ **rpm -ivh Mysql-server-community-5.1.25-0.rhel5.i386.rpm MySQL-client-community-5.1.25-0.rhel5.i386.rpm**  

	5. MySQL서버의 보안 설정을 아래와 같이 해주십시오.  
		>**MySQL에서 아이템에 대한 일반적인 보안 설정**  
		> - root 패스워드 변경  
		> - anonymous 이용자 삭제  
		> - 테스트용 기본 데이터베이스 삭제  

	6. MySQL 설치 확인은 아래와 같습니다.  
		> $ **/usr/bin/mysql\_secure_installation**  

	7. root 이용자로 MySQL 데이터베이스 접속은 아래와 같습니다.  
		> $ **mysql -u root -p**  
		> Enter password:  
		> Welcome to the MySQL monitor. Commands end with ; or \\g.  
		> Your MSQL connection id is 13  
		> Server version: 5.1.25-rc-community MySQL Community Server (GPL)  

		> Type 'help;' or '\\h' for help. Type '\\c' to clear the buffer.  

		> mysql>  

	8. MySQL 시작, 종료는 아래와 같습니다.  
		> $ **service mysql status**  
		> MySQL running (12588) [ OK ]  
		> $ **service mysql stop**  
		> Shutting down MySQL. [ OK ]  
		> $ **service mysql start**  
		> Starting MySQL. [ OK ]  

	9. MySQL DB, 유저를 생성하고 권한을 설정합니다.  
		> mysql> **CREATE DATABASE cube DEFAULT CHARACTER SET utf8 COLLATE utf8\_general\_ci;**  
		> mysql> **use mysql;**  
		> **'USERNAME' 과 'PASSWORD' 의 자리에 각각 사용할 DB유저명과 password를 넣습니다.**  
		> mysql> **insert into user (host,user,password) values('localhost','USERNAME',password('PASSWORD'));**  
		> mysql> **insert into user (host,user,password) values('%','USERNAME',password('PASSWORD'));**  
		> mysql> **insert into db values('%','cube','USERNAME','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y');**  
		> mysql> **insert into db values('localhost','cube','USERNAME','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y','y');**  
		> mysql> **grant all privileges on *.* to 'USERNAME'@'%' identified by 'PASSWORD';**  
		
# Chapter 3 #
## WEB 서버 설정 ##
- ### Apache Httpd 서버 설치 ###

	1. [http://www.apache.org](http://www.apache.org) 에서 Apache Httpd 를 다운로드 받을 수 있습니다.  
	2. 다운로드 받은 파일을 아래와 같이 압축을 풀어주십시오.  
		> $ gunzip -d httpd-2\_0\_NN.tar.gz  
		> $ tar xvf httpd-2\_0\_NN.tar	

	3. 레드햇 리눅스에 Apache httpd 설치는 아래와 같습니다.  
		> $ up2date httpd  
	
	4. Apache httpd 시작은 아래와 같습니다.  
		> $ chkconfig httpd on  
		> $ /etc/init.d/httpd start  
  
# Chapter 4 #
## WAS 서버 설정 ##
- ### Tomcat 서버 설치 ###

	1. Tomcat을 실행하기 위해서는 Java Standard Edition (Java SE)가 필요합니다.  
    	[http://java.sun.com/javase/6/webnotes/version-6.html](http://java.sun.com/javase/6/webnotes/version-6.html) 에서 받을 수 있으며 설치 과정은 아래와 같습니다.  
		> $ mkdir -p /usr/java  
		> $ cd /usr/java  
		> $ chmod 700 /tmp/jdk-6u45-linux-x64.bin  
		> $ /tmp/jdk-6u45-linux-x64.bin  
		> $ export JAVA\_HOME=/usr/java/jdk1.6.0\_10  
		> $ export PATH=$JAVA\_HOME/bin:$PATH  
	2. [http://tomcat.apache.org/download-60.cgi](http://tomcat.apache.org/download-60.cgi) 에서 Tomcat을 받을 수 있으며, 다음과 같이 압축을 풀어주십시오.  
		> $ tar zxvf /tmp/apache-tomcat-6.0.37.tar.gz  
	3. 서버 시작과 정지는 아래와 같습니다.  
		> $ /bin/startup.sh  
		> $ /bin/shutdown.sh  

# Chapter 5 #
## Eclipse 설치 ##
- ### Eclipse ###

	1. [http://www.eclipse.org/downloads](http://www.eclipse.org/downloads) 에서 Eclipse를 받아서 압축을 풀어주십시오.  
	2. Eclipse 실행파일을 바로가기 아이콘을 만든 뒤, 속성에 아래와 같이 추가하여 주십시오.  
		> C:\eclipse\eclipse.exe -vm D:\java\j2sdk1.4.2\_09\bin\javaw -vmargs -Xms128M -Xmx256M  

		> **속성 설명**  
		> - C:\eclipse\eclipse.exe : Eclipse 실행파일 위치  
		> -  -vm D:\java\j2sdk1.4.2_09\bin\javaw : Eclipse 실행시 사용할JDK(eclipse3.1은1.4 이상의jdk필요)  
		> - vmargs -Xms128M -Xmx256M : Eclipse 메모리 설정  
	3. Eclipse 실행하면 다음과 같이 workspace 설정이 나타납니다. 프로젝트의 위치 설정 후, "OK“버튼을 클릭해 주십시오.  
	   image

# Chapter 6 #
## GitHub를 이용한 P-CUBE 프로젝트 체크아웃 ##
- ### P-CUBE 체크아웃 ###

	1.만들기  

# Chapter 7 #
## P-CUBE 프로젝트 WAS 서버에 업로드 ##
- ### P-CUBE 업로드 ###
	
	1. "cube" 프로젝트의 project-works/config/cube.cfg 파일설정을 해주십시오.  
		> **필수 관리정보 세팅**  
		> cube.dir : project-works안의 cube 폴더가 서버에 위치하는 경로입니다.  
		> cube.hostname : cube의 hostname입니다.  
		> cube.baseUrl : cube의 url주소입니다.  
		> cube.name : 기관에 맞게 수정합니다.  
		> db.url : 서버의 MySQL의 경로입니다.  
		> db.driver : DB driver입니다.  
		> db.username : DB의 유저명입니다.  
		> db.password : DB유저의 비밀번호입니다.  
		> handle.prefix : 시스템 핸들링에 적용되는 고유 키값입니다.  
		
		> **메일발송을 위한 서버정보 세팅**  
		> mail.server : 메일 서버명  
		> mail.server.username : 메일주소  
		> mail.server.password : 메일비밀번호  
		> mail.admin : 관리자 메일주소  

		> **google analytics 관련 세팅**  
		> xmlui.google.analytics.change.key : google analytics 키값입니다.  

		> **google chart 관련 세팅**  
		> google.chart.username : 구글차트 연동 메일주소입니다.  
		> google.chart.password : 구글차트 연동 메일비밀번호입니다.  
		> google.chart.instituteCode : 구글차트 연동 키값입니다.  
		
		> **NDSL openAPI 검색 세팅**  
		> ndsl.openapi.key : NDSL연동을 위한 키값입니다.  

		> **시스템 모니터링 세팅**  
		> cube.system.monitoring.use : 시스템모니터링의 사용여부입니다.  

		> **레피던트 세팅**  
		> rapidant.use.yn : 레피던트의 사용여부입니다.  

		> **주제분류 세팅**  
		> subject.category.yn : 주제분류의 사용여부입니다.  
		
		> **Mulgara 세팅**  
		> mulgara.use.yn : Mulgara 사용어부입니다.  
		
	2. "cube" 프로젝트의 project-works/config/log4j.properties 파일설정을 해주십시오.  
		> log4j.appender.A1.File : cube의 로그파일이 생성될 경로입니다.  
		> log4j.appender.A2.File : cube의 로그파일이 생성될 경로입니다.  
		> log4j.appender.A3.File : cube의 로그파일이 생성될 경로입니다.  
	
	3. FTP 프로그램을 실행하여 서버에 연결하여 주십시오.  
	
	4. "cube" 프로젝트의 project-works안의 cube폴더를 서버에 복사하여 넣어 주십시오.  
	
	5. "cube" 프로젝트의 project-works안의 solr폴더를 서버에 복사하여 넣어 주십시오.  

	6. "cube" 프로젝트의 project-works안의 share_lib폴더를 서버에 복사하여 넣어 주십시오.  
	
	7. "cube", "oai" 프로젝트의 resources/server/applicationContext.xml 파일의 **dataSource bean** 에 DB경로, username, password를 서버에 맞게 변경합니다.  
		><property name="url"\> <value\>jdbc:mysql://127.0.0.1/cube?characterEncoding=utf-8</value\> </property\>  
		><property name="username"\> <value\>USERNAME</value\> </property\>  
		><property name="password"\> <value\>PASSWORD</value\> </property\>  
	
	8. "cube", "oai" 프로젝트의 WebContent/WEB-INF/web.xml 파일의 **cube-config, cube.dir** 에 config file 경로, username, password를 서버에 맞게 변경합니다.  
		> <param-value\>/data/home/cube/config/cube.cfg</param-value\>  
		> <param-value\>/data/home/cube</param-value\>  
	
	9. "cube", "oai" 프로젝트 위에서 마우스 오른쪽 버튼을 클릭한 뒤, "Export"를 클릭해주십시오.  
	
	10. Export 목록에서 Web / WAR file를 선택한 뒤, "Next >"버튼을 클릭해주십시오.  
	
	11. 경로와 생성파일명을 지정한 후 "finish"버튼을 클릭해 주십시오.  
		> "cube"의 Export WAR file 이름은 cube.war로 저장해 주십시오.  
		> "oai"의 Export WAR file 이름은 oai.war로 저장해 주십시오.  

	12. FTP 프로그램을 실행하여 서버에 연결하여 주십시오.  
	
	13. Export된 “cube.war", "oai.war" 파일을 아래의 경로에 올려주십시오.  
		> /tomcat6/webapps  
	
	14. 서버의 tomcat6/conf/catalina/localhost 경로에 solr.xml 파일이 있는지 확인 후 없으면 생성합니다.  
		> **파일내용 (서버에 넣은 solr폴더 경로와 맞게 넣어주십시오.)**  
		> <Context docBase="/data/home/solr/solr-4.3.1.war" debug="0" crossContext="true" path="/solr"\>  
    	> <Environment name="solr/home" type="java.lang.String" value="/data/home/solr/cube" override="true" /\>  
 		> </Context\>  
 	
	15. 서버의 tomcat6/conf 경로에 catalina.properties 파일의 common.loader에 서버에 맞게 solr경로를 추가합니다.  
		> **작성된 텍스트 뒤에 경로에 맞게 첨부합니다.**  
		>,/home/solr/ext/*.jar  
	
	16. 서버의 tomcat6/webapps/cube/WEB-INF/classes 경로에서 jar파일을 생성합니다. 생성된 파일은 서버의 cube/lib 경로안에 넣습니다.  
		> **서버 커맨드 창에서 경로접근 후 아래와 같이 생성합니다.**  
		> $ jar cvf pcube.jar kr config spring  

	17. 서버의 tomcat6/webapps/cube/WEB-INF/lib 경로의 모든 라이브러리 파일을 복사하여 cube/lib 경로안에 넣습니다.  
	
	18. 서버의 tomcat6/lib 경로의 servlet-api.jar 파일을 복사하여 cube/lib 경로안에 복사하여 넣습니다.  
	
	19. 서버의 루트경로에 bash_profile 파일을 수정합니다.  
		> **LD\_LIBRARY\_PATH 는 서버경로에 맞게 넣습니다.**  
		> $ vi.bash\_profile  
		> $ export LD\_LIBRARY\_PATH=/data/home/share\_lib  
		> $ source .bash_profile  
 
	20. 서버 접속 프로그램으로 서버에 접속해 주십시오.  
	
	21. Tomcat 서버를 재실행하여 주십시오.  
		> $ /bin/startup.sh  
		> $ /bin/shutdown.sh  