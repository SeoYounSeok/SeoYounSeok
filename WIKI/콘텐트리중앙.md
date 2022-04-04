## TA TEAM
### Shell Script DAY 1

<details>
<summary>이슈 발생 및 목표</summary>
<div markdown="1">

## 이슈 발생 및 목표

  - YYYYMMDD.log 파일에 출력
  - 30초에 1회씩 출력( 서윤석 )
    - date/time
    - cpu 사용률 상위 5개 프로세스 출력
    - date/time
  - 스크립트 실행 후 command 명령어를 수행( 백그라운드 수행 )
  - 터미털 종료 후에도 쉘이 종료되지 않아야함
  - *.out 파일이 남지 않아야 함.

</div>
</details>

<details>
<summary>소스 코드 및 주석 (+ 틀린 코드)</summary>
<div markdown="1">

## 소스 코드 및 주석

### seo.sh - 틀린 코드 (피드백 후 수정)

```
#!/bin/bash

NOW_DATE=$(date +"%Y%m%d")
LOG_DIR=/home/asmanager/logs/
LOG=./${NOW_DATE}.log # 날짜 별 저장

cd ${LOG_DIR} # 로그 파일들을 저장할 곳
while true;
do

        echo "SEOYOUNSEOK" >> ${LOG}
        date '+%Y-%m-%d/%H:%M:%S' >> ${LOG}

        # CPU 사용률 상위 5개 출력
        ps aux --sort=-pcpu | head -5 >> ${LOG}

        date '+%Y-%m-%d/%H:%M:%S' >> ${LOG}
sleep 30;
done
```

### seoInit.sh

```
#!/bin/bash

nohup bash seo.sh 1> /dev/null 2>&1 &
```

</div>
</details>

<details>
<summary>질의 응답</summary>
<div markdown="1">

## 처음 회의때 물어보셨던 질의

### Q $ 사용하여 백 그라운드 실행하게 되면, 터미널 종료후 프로세스 꺼지나요? <span style="color: red">YES</span>.

  ![image](/uploads/4998f81cda1eba87bca63da6536b0a90/image.png)
  ![image](/uploads/5f7d33986cea39fb6da3fa7fc650edec/image.png)

</div>
</details>

### Shell Script DAY2

<details>
<summary>이슈 발생 및 목표</summary>
<div markdown="1">

## 이슈 발생 및 목표
  - 프로세스 종료 쉘 만들어 봅시다
  - 3글자 이상의 문자열을 받아 해당 문자열이 들어가 있는 프로세스 종료
  - n개 이상의 프로세스를 모두 종료
  - 실행한 계정의 프로세스만 종료
  - 종료되는 프로세스 이름 및 pid를 표시

</div>
</details>

<details>
<summary>소스 코드 및 주석</summary>
<div markdown="1">

## 소스코드 및 주석

```
#!/bin/bash

echo -n "Please enter a string to delete :"
read answer

OWNER=$(whoami)
NAME=${ONE%e*}

if [ ${#answer} -ge 3 ] ; then

	ps aux | grep "${answer}" | awk '{print $1,$2,$12}' | grep -v "grep"| grep "${NAME}" | awk '{print $2,$3}'
	#ps aux | grep "${answer}" | awk '{print $2,$12}'
	#ps aux | awk '{print $1}' | grep '${NAME}'
	kill (ps aux | grep "${answer}" | awk '{print $1,$2,$12}'| grep -v "grep" | grep "${NAME}"| awk '{print $2,$3}')
else
	echo "Please enter more than 3 letters."
fi
```
</div>
</details>

<details>
<summary>최종 피드백 (+ 최종 코드 수정)</summary>
<div markdown="1">

```
* 로그 파일에 날짜가 수정되지 않을 것 같다 BY 장은종 차장님

  => 생각을 해보니, 만약 LOG 를 지정해주는 부분이 바깥쪽에 있다면, WHILE 문이 도는 동안
     똑같은 파일에만 저장할 것이다. 그러므로 날짜 로그를 찍어내는 파일부분을 안쪽으로 넣었다.
```

### seo.sh - 최종 코드

```
#!/bin/bash

LOG_DIR=/home/asmanager/logs/

cd ${LOG_DIR} # 로그 파일들을 저장할 곳
while true;
do
        NOW_DATE=$(date +"%Y%m%d")
        LOG=./${NOW_DATE}.log # 날짜 별 저장
        echo "SEOYOUNSEOK" >> ${LOG}
        date '+%Y-%m-%d/%H:%M:%S' >> ${LOG}

        # CPU 사용률 상위 5개 출력
        ps aux --sort=-pcpu | head -5 >> ${LOG}

        date '+%Y-%m-%d/%H:%M:%S' >> ${LOG}
sleep 30;
done
```

</div>
</details>



### JAVA + WAS(Wildfly) + APM(Scouter) + Spring5(5.1.13) MVC

#### Initial Goal

<details>
<summary>초기 목표 설정 &#128204</summary>
<div markdown="1">

### 1. Initial Goal
```
* 목표 설정

  1. 환경 설정
  - Java 8 이상 설치
    * 오라클 JDK 자바 SE 버전 상용화로 인한 openjdk 설치(Zulu를 이용한 설치)
  - WAS - wildfly 18 이상 설치
  - MariaDB 설치

  2. Spring5 MVC 프로젝트 생성 후  DB 데이터 출력
```
</div>
</details>

#### Setting Version
<details>
<summary>로컬 세팅 버전 및 서버 호환 버전 &#128204</summary>
<div markdown="1">

### 2. Setting

#### 서버(server)
##### JDK : zulu 8(openjdk 1.8.0_272) [다운로드](https://www.azul.com/downloads/zulu-community/?architecture=x86-64-bit&package=jdk)
##### Maven : apache-maven-3.6.3 [다운로드](https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz)
##### Wildfly : 18.0.0.Final [다운로드](https://www.wildfly.org/downloads/)
##### MariaDB [root 권한 가이드](https://bamdule.tistory.com/59)

#### 로컬(local)
##### JDK : zulu 8(openjdk 1.8.0_272) [다운로드](https://www.azul.com/downloads/zulu-community/?architecture=x86-64-bit&package=jdk)
##### Maven : apache-maven-3.6.3 [다운로드](https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz)
##### ~~Wildfly : 18, 21(Spring 충돌 실패) [이클립스 연동](https://niceman.tistory.com/110)~~
##### Tomcat : v8.5 [이클립스 연동](https://all-record.tistory.com/49)
##### MariaDB [root 권한 가이드](https://bamdule.tistory.com/59)

#### Git Tools
<image src="https://user-images.githubusercontent.com/43161245/99182676-abb19480-2779-11eb-8b8f-e8666dd13a19.png" width="300" height="150">
<image src="https://user-images.githubusercontent.com/43161245/99182738-267aaf80-277a-11eb-98b4-f8203e67ea65.png" width="400" height="150">

</div>
</details>

#### Base Installation and Setting

<details>
<summary>JDK 설치 및 설정</summary>
<div markdown="1">

```
# 3 STEP SETTING

  1 STEP : Download
  2 STEP : Unzip
  3 STEP : Modify (bash_profile)  
```

##### 2-1-1 JDK 설정 (2가지 방법)

```
# 설치 후 WinSCP (FTP) 를 활용하여 전송
```
![java-1](https://user-images.githubusercontent.com/43161245/99152874-f5519f00-26e7-11eb-9096-d76052af6e6f.PNG)
```
# 링크 주소 복사를 통한 command 활용
```
![java-2](https://user-images.githubusercontent.com/43161245/99152878-f682cc00-26e7-11eb-885b-f0d3b52be24f.jpg)

```
# 파일 다운로드
$ wget https://cdn.azul.com/zulu/bin/zulu8.50.0.51-ca-jdk8.0.275-linux_x64.tar.gz

# 해당 폴더로 이동 (필자의 폴더는 local/java)
$ cd /local/java

# 압축 해제
$ tar -zxvf zulu8.50.0.51-ca-jdk8.0.275-linux_x64.tar.gz

# bash_profile 수정
$ vi ~/.bash_profile
```
```
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs
JAVA_HOME=/home/asmanager/local/java/zulu8.50.0.51-ca-jdk8.0.275-linux_x64
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME
```

```
# 소스 코드 저장
$ source ~/.bash_profile

# 자바 버전 확인
$ java -version
```
</div>
</details>

<details>
<summary>Wildfly 설치 및 설정</summary>
<div markdown="1">

##### 2-1-2 Wildfly 설정 (2가지 방법)
```
# 설치 후 WinSCP (FTP) 를 활용하여 전송
```
![wildfly1](https://user-images.githubusercontent.com/43161245/99153480-17e5b700-26ec-11eb-87f6-70d5a1033a15.PNG)

```
# 링크 주소 복사를 통한 command 활용
```
![wildfly2](https://user-images.githubusercontent.com/43161245/99153481-187e4d80-26ec-11eb-901d-47b16916e638.jpg)
```
# 파일 다운로드
$ wget https://download.jboss.org/wildfly/18.0.0.Final/wildfly-18.0.0.Final.tar.gz

# 해당 폴더로 이동 (필자의 폴더는 local/wildfly)
$ cd /local/wildfly

# 압축 해제
$ tar -zxvf wildfly-18.0.0.Final.tar.gz

# standalone.conf 설정
$ vi /local/wildfly/wildfly-18.0.0.Final/standalone.conf
```
```
# standalone.conf 파일 안에 추가

JBOSS_HOME="home/asmanager/local/wildfly/wildfly-18.0.0.Final"
JAVA_HOME="/home/asmanager/local/java/zulu8.50.0.51-ca-jdk8.0.275-linux_x64"
```
```
# bash_profile 수정
$ vi ~/.bash_profile
```
```
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs
JAVA_HOME=/home/asmanager/local/java/zulu8.50.0.51-ca-jdk8.0.275-linux_x64
JBOSS_HOME=/home/asmanager/local/wildfly//wildfly-18.0.0.Final
PATH=$PATH:$JAVA_HOME/bin:$JBOSS_HOME/bin
export PATH JAVA_HOME JBOSS_HOME
```
```
# 소스 코드 저장
$ source ~/.bash_profile

# 해당 경로 이동 후 실행
$ cd local/wildfly/wildfly-18.0.0.Final/bin
$ ./standalone.sh
```
![wildfly 실행환경](https://user-images.githubusercontent.com/43161245/99153854-9e02fd00-26ee-11eb-90cb-1ab7edc2b3aa.PNG)

</div>
</details>


<details>
<summary>Maven 설치 및 설정</summary>
<div markdown="1">

##### 2-1-3 Maven 설정
```
# 링크 주소 복사를 통한 command 활용
```
![maven1](https://user-images.githubusercontent.com/43161245/99153921-37caaa00-26ef-11eb-91ce-2ac740fd1131.jpg)

```
# 파일 다운로드
$ wget https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz

# 해당 폴더로 이동 (필자의 폴더는 local/maven)
$ cd /local/maven

# 압축 해제
$ tar -zxvf /local/maven/apache-maven-3.6.3-bin.tar.gz

# bash_profile 수정
$ vi ~/.bash_profile
```
```
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs
JAVA_HOME=/home/asmanager/local/java/zulu8.50.0.51-ca-jdk8.0.275-linux_x64
JBOSS_HOME=/home/asmanager/local/wildfly//wildfly-18.0.0.Final
MAVEN_HOME=/home/asmanager/local/maven/apache-maven-3.6.3
PATH=$PATH:$JAVA_HOME/bin:$JBOSS_HOME/bin:$MAVEN_HOME/bin
export PATH JAVA_HOME JBOSS_HOME
export MAVEN_HOME
```
```
# 소스 코드 저장
$ source ~/.bash_profile

# Maven 버전 확인
$ mvn -version
```
![maven 버전](https://user-images.githubusercontent.com/43161245/99154051-364db180-26f0-11eb-9f59-ee831811b989.PNG)

</div>
</details>

<details>
<summary>MariaDB 설치 및 설정</summary>
<div markdown="1">

##### 2-1-4 MariaDB 설정 [출처 가이드](https://bamdule.tistory.com/59)
![image](https://user-images.githubusercontent.com/43161245/99154075-73b23f00-26f0-11eb-8cc0-1a8ad7d1cb6b.png)

```
# MariaDB yum repo 등록
$ vi /etc/yum.repos.d/MariaDB.repo
```
```
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.4/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```
```
# MariaDB Install
$ yum install MariaDB

# 설치 파일 확인
$ rpm -qa | grep MariaDB
MariaDB-compat-10.4.12-1.el7.centos.x86_64
MariaDB-client-10.4.12-1.el7.centos.x86_64
MariaDB-common-10.4.12-1.el7.centos.x86_64
MariaDB-server-10.4.12-1.el7.centos.x86_64

# MariaDB 버전 확인
$ mariadb --version
mariadb Ver 15.1 Distrib 10.4.12-MariaDB, for Linux (x86_64) using readline 5.1
```

```
# MariaDB 기동
$ systemctl start mariadb

# MariaDB 정지
$ systemctl stop mariadb
```
##### <span style="color:red">MariaDB root 암호 및 기본 보안설정 </span>
##### <span style="color:red">TIP : 외부 접근만 허용 </span>
```
$ mysql_secure_installation
```
```
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

You already have a root password set, so you can safely answer 'n'.

# root password 변경하시겠습니까? y
 Change the root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!

#  익명성 있는 유저를 삭제하시겠습니까? y
By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
... Success!

# 외부 접속을 막으시겠습니까? n
Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] n
... skipping.

# Test Database를 삭제하시겠습니까? y
By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

# 리로드 하시겠습니까? y
Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

```
# MariaDB 접속
$ mysql -u root -p

# MariaDB 버전 확인
MariaDB [(none)] > SELECT version();
```

![마리아 디비 버전확인 1번](https://user-images.githubusercontent.com/43161245/99367028-c2ccbf80-28fc-11eb-9812-e008bf620ba4.PNG)
![마리아 db 버전 2번](https://user-images.githubusercontent.com/43161245/99367025-c19b9280-28fc-11eb-85e8-3c6c6b3a7b14.PNG)

</div>
</details>

<details>
<summary>MariaDB 재설치 중 발생한 오류 &#10060</summary>
<div markdown="1">


### <span style="color:red">⚠️ MariaDB 재설치시 확인 되었던 오류  ❗️❗️</span>
```
* 사고쳤던 오류...

* 상황
  1. MariaDB 권한을 주는 도중에, 맨 처음부터 깔끔하게 진행해야겠다고 생각을 했다.
  2. MariaDB 삭제 후 재설치를 실행했다.
  3. 재설치 후, START를 했으나, 다음과 같은 오류가 발견됨.

  can't connect to local mysql server through socket '/var/lib/mysql/mysql.sock' (2)  
```
```
* 위의 오류 코드를 찾아보고 당연하듯이 블로그를 따라함.
* 권한이 mysql 로 작성이 되어있지 않아, 권한을 바꿔줘야 한다고 인지함.
* 아래 코드를 작성함

* 문제의 코드

  chown -R mysql. / var lib / mysql /

=> chmod [옵션] [소유자권한][그룹권한][그외사용자권한] [파일또는폴더]
=> 문제의 코드를 보면 중간중간 space 가 들어가 있음...
=> 각 폴더들의 권한이 mysql로 바뀌워서 문제가 발생함
```
```
# 결과

* 서버가 연결은 되나, 켜지지 않음.

* 결과

  1. 띄어쓰기(space) 를 사용해도 해당 파일만 적용되는 줄 알았으나, 스페이스 효과는 각각이라는 것을 확인했습니다.
  2. 장태영 과장님께 말씀드렸고, 과장님과 인프라개발팀에서 도와주셔서 해결해주셨습니다.
  3. 장은종 과장님께서 좋은 말씀과 조언을 해주셨고, 다음부터는 root 권한을 다시는 사용을 안할 것이라고 다짐했습니다.
```
</div>
</details>

#### Spring5 MVC Project

<details>
<summary>Spring5 MVC Project 작업</summary>
<div markdown="1">

#### 2-2 Spring5 MVC Project [출처 가이드](https://immose93.tistory.com/9?category=862380)
##### <span style="color:red"> [pom.xml 및 환경 설정, 쿼리 테스트를 위주로 작성하였습니다. ] </span>
##### 2-2-0 의존성 주입 [설정 사이트](https://mvnrepository.com/)
```
# pom.xml 에 추가
# 찾고 싶은 정보 검색 후 설정
```
![의존성 주입](https://user-images.githubusercontent.com/43161245/99182489-9e47da80-2778-11eb-9d5d-83477b0a7332.PNG)


![의존성 주입2](https://user-images.githubusercontent.com/43161245/99182490-9ee07100-2778-11eb-91bb-661eeb7c356f.PNG)


![의존성 주입3](https://user-images.githubusercontent.com/43161245/99182492-9f790780-2778-11eb-9655-b9bd268de39c.PNG)

##### 2-2-1 Spring5, JDK 1.8 버전 수정
- 자바 버전 : 1.8, 스프링 프레임워크 버전 5.1.13
![spring 설정1](https://user-images.githubusercontent.com/43161245/99182282-6c824400-2777-11eb-9a11-87c8182171aa.PNG)

- maven-compiler 버전: 3.7.0 (source : 1.8, target : 1.8)
![spring 설정2](https://user-images.githubusercontent.com/43161245/99182283-6d1ada80-2777-11eb-9c3a-ef132c3dcb5d.png)

##### 2-2-2 Spring + MariaDB, MyBatis 연동
```
# 1번
# WHAT? pom.xml 수정
# WHY ? MyBatis 이용한 MariaDB 접근을 통한 의존성

<!-- DB -->
<!-- Maria DB -->
<dependency>
	<groupId>org.mariadb.jdbc</groupId>
	<artifactId>mariadb-java-client</artifactId>
	<version>2.0.3</version>
</dependency>

<!-- DBCP 데이터베이스 풀 커넥션 -->
<dependency>
	<groupId>commons-dbcp</groupId>
	<artifactId>commons-dbcp</artifactId>
	<version>1.4</version>
</dependency>

<!-- Spring JDBC -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-jdbc</artifactId>
	<version>4.3.9.RELEASE</version>
</dependency>

<!-- Mybatis -->
<dependency>
	<groupId>org.mybatis</groupId>
	<artifactId>mybatis</artifactId>
	<version>3.4.4</version>
</dependency>

<dependency>
	<groupId>org.mybatis</groupId>
	<artifactId>mybatis-spring</artifactId>
	<version>1.3.1</version>
</dependency>

<!-- Mybatis log -->
<!-- https://mvnrepository.com/artifact/org.bgee.log4jdbc-log4j2/log4jdbc-log4j2-jdbc4.1 -->
<dependency>
	<groupId>org.bgee.log4jdbc-log4j2</groupId>
	<artifactId>log4jdbc-log4j2-jdbc4.1</artifactId>
	<version>1.16</version>
</dependency>
```
##### IP 주소, username, password setting 필요
```
# 2번
# WHAT? root-content.xml 수정
# WHY ? 웹 이외의 부분 세팅

<bean id="dataSource"
	class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	<property name="driverClassName"
		value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
	<property name="url"
		value="jdbc:log4jdbc:mariadb://서버 주소:3306/theater" />
	<property name="username" value="root" />
	<property name="password" value="비밀번호" />
</bean>

<bean id="sqlSessionFactory"
	class="org.mybatis.spring.SqlSessionFactoryBean">
	<property name="dataSource" ref="dataSource"></property>
	<property name="configLocation"
		value="classpath:/mybatis/mybatis-config.xml"></property>
	<property name="mapperLocations"
		value="classpath*:/mybatis/sql/*.xml"></property>
</bean>

<bean id="sqlSession"
	class="org.mybatis.spring.SqlSessionTemplate">
	<constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>
</bean>

<!-- <mybatis-spring:scan base-package="com.moses.dao" /> -->
<context:component-scan base-package="com.moses.dao"></context:component-scan>
<context:component-scan	base-package="com.moses.service"></context:component-scan>

```
```
# 3번
# WHAT? mybatis-config.xml 수정
# WHY? mybatis 설정 파일

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
	PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
	<typeAliases>
		<typeAlias type="com.moses.vo.MovieVO" alias="movieVO" />
	</typeAliases>
</configuration>
```
</div>
</details>

#### Scouter Connection

<details>
<summary>SCOUTER 설치 및 실행</summary>
<div markdown="1">

## Scouter
<image src="https://user-images.githubusercontent.com/43161245/99141830-5a7aa580-2692-11eb-8d78-c30332e28421.png" width="300" height="200">

### 1. Open Source Program Scouter?

```
* Scouter 란?

  - 오픈 소스 모니터링 도구로서 JAVA & WAS 에 대한 모니터링 및 DB Agent를 통해 오픈소스 DB모니터링 기능을 제공하는 프로그램입니다.
  - 오픈 소스 프로그램으로서, 기존 비용이 많이 들었던 APM (Application Performance Monitoring) 서비스를 무료로 이용 가능합니다.
  - CPU, MEMORY 등 서버 기본 자원부터 NETWORK 및 Heap usage, Connection pools 등 지속적인 모니터링이 필요한 자원을 효과적으로 관리할 수 있습니다.
```

### 2. Scouter 설치

##### 2-1 [Scouter Release Page](https://github.com/scouter-project/scouter/releases)

![문서작업파일](https://user-images.githubusercontent.com/43161245/99144544-172c3100-26aa-11eb-9d24-f069e786bf0c.PNG)

##### 2-2 scouter-all-2.10.0.tar.gr (Server Install)
- Scouter Colletor, Host, Agent등을 포함한 압축파일

##### 2-3 scouter.client.product-win32.wim32.x86_64.zip (Client Install)
- 각 OS별 Client 프로그램(필자는 Window 환경)
##### 2-4 Scouter Server 구성 및 실행

- 위의 홈페이지를 통하여, 최신 릴리즈 다운로드 진행
```
$ wget https://github.com/scouter-project/scouter/releases/download/v2.10.0/scouter-all-2.10.0.tar.gz
```
- 압축 파일 풀기 및 이동 후 실행

```
# 압축파일 풀기
$ tar -xvf scouter-all-2.10.0.tar.gz

# 경로 이동
$ cd ./local/scouter

# Scouter start
$ ./startup.sh
```

<image src="https://user-images.githubusercontent.com/43161245/99145174-d9caa200-26af-11eb-8fca-bab81c1cc29b.PNG">


### <span style="color:red">⚠️  JDK 환경 설정을 제대로 안한 경우 오류 ❗️❗️</span>
![문제의 사진](https://user-images.githubusercontent.com/43161245/99145465-768e3f00-26b2-11eb-9a60-e1b0717e46ea.PNG)
```

#Tip 스카우터 서버는 JDK 1.8 버전 이상이 필요

해결 방안
- java -version 을 확인해보세요.
  필자의 경우, 서버 복원시 경로를 재설정해서 해결해주었습니다.
```

##### 2-6 Host Agent 구성 및 실행
- Host Agent : OS의 CPU, memory, DISK 사용량 등 성능 정보 전송

```
$ vi /scouter 설치 경로/agent.host/conf/scouter.conf
```
```
# net_collector_ip : Agent에서 수집한 데이터를 보낼 서버의 ip address
# 필자의 경우 서버 ip 주소를 입력

# Server IP(default:127.0.0.1)
net_collector_ip = ip address 입력

# Port Setting(default:6100)
net_collector_udp_port=6100
net_collecotr_tcp_port=6100
```
```
# 실행

$ cd /scouter 설치 경로/agent.host
$ ./host.sh
````

##### 2-7 Java Agent 구성 및 실행
- Java Agent : 실시간 서비스 성능 정보, Heap Memory, Thread 등의 Java 성능 정보
```
$ vi /scouter 설치 경로/agent.java/conf/scouter.conf
```
```
# obj_name 과 ip address 변경

obj_name=(WAS-seo 하고 싶은 변수명)
net_collector_ip= (ip address)
net_collector_udp_port=6100
net_collector_tcp_port=6100
hook_method_patterns=sample.mybiz.*Biz.*,sample.service.*Service.*
trace_http_client_ip_header_key=X-Forwarded-For
profile_spring_controller_method_parameter_enabled=false
hook_exception_class_patterns=my.exception.TypedException
profile_fullstack_hooked_exception_enabled=true
hook_exception_handler_method_patterns=my.AbstractAPIController.fallbackHandler,my.ApiExceptionLoggingFilter.handleNotFoundErrorResponse
hook_exception_hanlder_exclude_class_patterns=exception.BizException
```

##### 2-8 Scouter Client 실행

![scouter 실행1](https://user-images.githubusercontent.com/43161245/99146493-08e71080-26bc-11eb-8c5a-ada49a801ef1.PNG)
![scouter 실행2](https://user-images.githubusercontent.com/43161245/99146495-097fa700-26bc-11eb-9d16-334e6e7dba78.PNG)

##### 2-9 Wildfly & Scouter 연동 후 실행( ./standalone.sh )
```
$ vi /wildfly 설치 경로/bin/standalone.sh
```
```
# standalone.sh 하위 내용 추가

#Scouter
export JAVA_OPTS=" $JAVA_OPTS -Djboss.modules.system.pkgs=org.jboss.byteman,scouter"
export JAVA_OPTS=" $JAVA_OPTS -Dscouter.config=/app/apm/scouter/agent.java/conf/scouter.conf"
export JAVA_OPTS=" $JAVA_OPTS -javaagent:/app/apm/scouter/agent.java/scouter.agent.jar"
```

</div>
</details>
<details>
<summary>SCOUTER 연결 중 발생한 오류 &#10060</summary>
<div markdown="1">


### <span style="color:red">⚠️ Scouter 연결 후 발생한 오류  ❗️❗️</span>
```
ERROR [io.undertow.request] (default task-1) ut005023:
exception handling request to /springtest/: java.lang.noclassdeffounderror: scouter/agent/trace/tracemain
.
.
.
.
...
```
```
* 오류 확인 시: scouter 확인

* 해결방안 (standalone.sh 하위 내용 추가 scouter 추가)

# 변경 전

#Scouter
export JAVA_OPTS=" $JAVA_OPTS -Djboss.modules.system.pkgs=org.jboss.byteman"
export JAVA_OPTS=" $JAVA_OPTS -Dscouter.config=/app/apm/scouter/agent.java/conf/scouter.conf"
export JAVA_OPTS=" $JAVA_OPTS -javaagent:/app/apm/scouter/agent.java/scouter.agent.jar"

# 변경  후

#Scouter
export JAVA_OPTS=" $JAVA_OPTS -Djboss.modules.system.pkgs=org.jboss.byteman,scouter"
export JAVA_OPTS=" $JAVA_OPTS -Dscouter.config=/app/apm/scouter/agent.java/conf/scouter.conf"
export JAVA_OPTS=" $JAVA_OPTS -javaagent:/app/apm/scouter/agent.java/scouter.agent.jar"
```

```
# 실행
$ ./standalone.sh
```

![결과 화면1](https://user-images.githubusercontent.com/43161245/99146036-a724a780-26b7-11eb-80d5-37bce980d546.PNG)
##### 2-9-1 Scouter 실행 결과 - 개인
![결과 화면2](https://user-images.githubusercontent.com/43161245/99145806-0d5bfb00-26b5-11eb-907e-18315421f2c8.PNG)

![결과 화면3](https://user-images.githubusercontent.com/43161245/99145807-0e8d2800-26b5-11eb-8530-e1ea7ab1122f.PNG)

##### 2-9-1 Scouter 실행 결과 - 인턴 (진우씨 계정 로그인)
##### <span style="color:red">차장님과 함께 과부하 결과 확인</span>

![결과 화면4](https://user-images.githubusercontent.com/43161245/99880668-4289bf80-2c58-11eb-8dfc-d5542a93a0d3.PNG)

</div>
</details>

#### Jenkins Connetion

<details>
<summary>JENKINS 설치 및 실행</summary>
<div markdown="1">

## JENKINS

### 1. What's Jenkins?
![image](https://user-images.githubusercontent.com/43161245/99146022-7a709000-26b7-11eb-943a-4550ddd55382.png)

```
* Jenkins 란?

  - 지속적으로 빌드, 테스트 및 배포 등을 통합을 자동화 해주는 툴
  - 소프트웨어 개발 시 지속적 통합 서비스를 제공하는 툴
```

### 2. Jenkins 설치 및 실행<br /><br />

#### 2-1 [Jenkins 설치 홈페이지]([https://www.jenkins.io/download/)
![젠킨스 설치1](https://user-images.githubusercontent.com/43161245/99150829-90437c80-26da-11eb-9c3b-14c8579a10c0.PNG)

![젠킨스 설치2](https://user-images.githubusercontent.com/43161245/99150858-bbc66700-26da-11eb-959b-d6d7848767a8.PNG)


![젠킨스 설치3](https://user-images.githubusercontent.com/43161245/99150794-612d0b00-26da-11eb-8515-364e09cb2107.PNG)

```
$ wget https://ftp-nyc.osuosl.org/pub/jenkins/war-stable/2.249.3/jenkins.war
```

#### 2-2 Jenkins 실행 <br /><br />
##### 2-2-1 Jenkins 내장 WAS 수동 실행
```
# 9090 포트로 실행

$ cd /jenkins.war 설치 폴더
$ java -jar jenkins.war --httpPort=9090
```
##### 2-2-2 Wildfly 포트에서 실행
```
# jenkins.war 파일 위치 : wildfly 설치 폴더/standalone/deployments/

$ cd /wildfly 설치 폴더/standalone/deployments
$ wget https://ftp-nyc.osuosl.org/pub/jenkins/war-stable/2.249.3/jenkins.war
$ cd ../../bin
$ ./standalone.sh
```
<p align="center">
<img src="https://user-images.githubusercontent.com/43161245/99181015-dea25b00-276e-11eb-8ce2-a28848a05454.PNG" >
</p>

##### ID, PASSWORD 설정 후 로그인.
- ID : admin
- Password : admin

#### 2-3 Jenkins 설정

![jenkins](https://user-images.githubusercontent.com/43161245/99181136-e0205300-276f-11eb-82d0-0ca5e1d53e34.PNG)

##### JDK 설정

![jdk](https://user-images.githubusercontent.com/43161245/99181328-1b6f5180-2771-11eb-86f9-cb5ab624ed88.PNG)
- install automatically 체크 해제

![jdk-2](https://user-images.githubusercontent.com/43161245/99181297-ef53d080-2770-11eb-89ed-9e6821427c5c.PNG)

##### Git 설정
![git](https://user-images.githubusercontent.com/43161245/99182549-e23adf80-2778-11eb-84f7-01cd9ab82c5e.PNG)

##### Maven 설정
![maven](https://user-images.githubusercontent.com/43161245/99182550-e2d37600-2778-11eb-8e4a-1eea26d08c24.PNG)

#### 2-4 Jenkins Gitlab 연결 [참고 블로그](https://tech.osci.kr/2020/01/16/86039236)
```
1. GitLab Access Token 발급하기

1-1 GitLab 개인 계정의 "Settings" 를 클릭합니다.
```
![1번](https://user-images.githubusercontent.com/43161245/99668302-e33d7b00-2ab0-11eb-8187-58e86c7ec68a.jpg)

```
1-2 Access Tokens 를 클릭합니다.

1-3 아래와 같이 설정합니다.

    NAME : Jenkins-Token
    API, read_user, read_api, read_repository, write_repository 체크

1-4 Create presonal access token 을 클릭합니다.
```
![2번](https://user-images.githubusercontent.com/43161245/99668663-6232b380-2ab1-11eb-9578-730a6a62e62a.PNG)
```
1-5 위의 설정을 완료하면, 하단부에 Active personal access tokens 이 생성됩니다.
```
![에세스](https://user-images.githubusercontent.com/43161245/99739799-e9634400-2b10-11eb-9a02-e5e6439fe35b.PNG)

![3번](https://user-images.githubusercontent.com/43161245/99668307-e46ea800-2ab0-11eb-893e-791f668149df.PNG)
```
2. JENKINS 에서 GitLab 과 연동을 위한 Credential 추가
   => 일종의 키라고 생각하세요!

2-1 GitLab token을 사용하는 Credential을 생성합시다. Add Credential 클릭 !!
```
![4번](https://user-images.githubusercontent.com/43161245/99669602-a5d9ed00-2ab2-11eb-996d-3429244ed469.jpg)
```
2-2 "GitLab API token" 이라는 '키'가 생성된 것이 확인되었습니다.
   => 연동을 위해서는, GitLab Plugin 이 사전에 설치되어 있어야합니다. (사전 작업 필!!)
```
![결과 2](https://user-images.githubusercontent.com/43161245/99670709-4a106380-2ab4-11eb-8771-37828fa52e4a.PNG)

```
3. Jenkins 소스 코드 관리 지정 / Clone 받을 곳을 지정해주는 곳!
   => Repository URL : .git 을 받을 곳
   => Credential : 생성키 (필자: Gitlab-test-login)
```
![gitlab 저장](https://user-images.githubusercontent.com/43161245/99757956-dc0b8100-2b33-11eb-8a11-2907b990a63b.PNG)

```
4. Jenkins 자동 Bulid trigger 설정
   => "Bulid when a change is pushed to GitLab webhooK ~~~~" 을 체크합니다.
   => Webhook URL 해당 정보를 기억해야 합니다! (빨간 박스)
```
![빌드 유발](https://user-images.githubusercontent.com/43161245/99740448-65aa5700-2b12-11eb-8566-ca0a4214897a.PNG)
```
5. GitLab webhook 등록

  1. ID / PASSWORD 방식 ( X )
     => ID / PASSWORD 를 직접 입력해야하기 때문에, 공통 비번이라 패스 아래것을 선택
  2. Secret Token 방식 ( 이것으로 진행 )
     => 위의 단계에서 고급을 누르셨다면, Secret Token 이라는 필드를 확인할 수 있습니다.
     => Generate 를 누르면 생성됩니다.
```

```
5-1 GitLab 에서 Push Event 가 발생되면, Jenkins의 Job을 Bulid 하는 webhooK 생성
     => GitLab의 해당 프로젝트로 이동
     => 프로젝트 Setting-> Integration
```
![webhook](https://user-images.githubusercontent.com/43161245/99760289-bfbd1380-2b36-11eb-99f3-7a465cecab47.jpg)
```
5-2 4의 사진과 동일!, 고급을 눌러줍니다.
```
![빌드 유발](https://user-images.githubusercontent.com/43161245/99740448-65aa5700-2b12-11eb-8566-ca0a4214897a.PNG)
```
5-3 Secret Token을 생성하고 Token 값을 복사합니다.
```
![소스코드 관리2](https://user-images.githubusercontent.com/43161245/99740454-66db8400-2b12-11eb-9537-1c6da918bb56.PNG)

```
5-4 GitLab의 Webhook 설정화면에서 다음과 같이 추가하여 설정합니다.
```

![webhook1](https://user-images.githubusercontent.com/43161245/99741398-680db080-2b14-11eb-8195-79c73f469f4d.jpg)

```
5-5 생성이 된다면 다음과 같이 Project Hook 에서 확인할 수 있습니다.
```

![웹훅 2](https://user-images.githubusercontent.com/43161245/99741399-69d77400-2b14-11eb-9b6a-3b1e28341fe6.PNG)

#### 2-5 Jenkins 최종 빌드 설정 및 확인

```
1. GitLab Clone
   - 소스코드 관리 부분
      => Repository URL : Clone 받을 Git 주소
      => Credential : 인증 키

   - Branch : 마스터로 설정
```
![소스코드 관리](https://user-images.githubusercontent.com/43161245/99740450-6642ed80-2b12-11eb-8d86-c5ec89886110.PNG)



```
2. Build
  => 실제 shell script 코드를 이용하여 작성하였습니다.
```

![결과](https://user-images.githubusercontent.com/43161245/99762150-2fcd9880-2b3b-11eb-8cd9-50beb17ecfa8.PNG)

```
* 해당 쉘 코드

#!/bin/bash

#권한을 줘야 한다.
chmod 775 ./mvnw

# war 파일 생성
./mvnw clean package

# ls 로 체크
ls

# deployments 로 war 파일 이동
mv /home/asmanager/.jenkins/workspace/jenkins-younseok/target/springtest-1.0.0-BUILD-SNAPSHOT.war /home/asmanager/local/wildfly/wildfly-18.0.1.Final/standalone/deployments/testv1.war

# WAS 있는 곳 이동
cd /home/asmanager/local/wildfly/wildfly-18.0.1.Final/bin

# WAS 실행
./standalone.sh
```
##### <span style="color:red">Jenkins Port : 8086 / WAS Port : 8090 </span>
![포트번호 지우기](https://user-images.githubusercontent.com/43161245/99762880-d6feff80-2b3c-11eb-8859-d896ccb2de76.PNG)

![캡처-3](https://user-images.githubusercontent.com/43161245/99763021-22191280-2b3d-11eb-895a-6ed9952cdd79.PNG)

</div>
</details>

## DBA Team

### DBA

<details>
<summary>DBA 간단 설명</summary>
<div markdown="1">

#### What is DBA?
```
* DBA란?
- 이종무 차장님

  한 조직 내에서 데이터 베이스 시스템이 원활하게 기능을 수행할 수 있도록 데이터 베이스를 설치, 구성, 업그레이드, 관리 감시등을 하는 직무
```

</div>
</details>

### DB Task

<details>
<summary>DB 백업 및 복원 - 이관 작업</summary>
<div markdown="1">

#### 1. MariaDB Migration

##### 1-1 데이터 이관 조건
```
* 기본 조건
  1. 기간 : 10월 1일 ~ 7일 / 11월 1일 ~ 7일
  2. 오브젝트 : joinsflow, mblack, pnx-phr (택 1) < pnx-phr 을 선택
  3. 추가 데이터 테이블: OBJ_LIST

* 이관용 접속 정보
  IP : 비공개
  Database : scouter
  User : userid / password
```

##### 1-2 MariaDB 작업

```
# 데이터 베이스 생성
$ CREATE DATABASE scouter default CHARACTER SET UTF8;

# 데이터 베이스 사용
$ USE scouter;

# Table 생성

CREATE TABLE `srb` (
  `NAME` varchar(8) NOT NULL,
  `TIME` varchar(30) NOT NULL,
  `ELAPSED` decimal(10,0) DEFAULT NULL,
  `OBJ` varchar(255) DEFAULT NULL,
  `OBJECTNAME` varchar(255) DEFAULT NULL,
  `SERVICE` varchar(255) DEFAULT NULL,
  `ERROR` decimal(10,0) DEFAULT NULL,
  `USERID` varchar(255) DEFAULT NULL,
  `SqlCnt` decimal(10,0) DEFAULT NULL,
  `SqlTime` decimal(10,0) DEFAULT NULL,
  `CpuTime` decimal(10,0) DEFAULT NULL,
  `kBytes` decimal(10,0) DEFAULT NULL,
  `ApicallCnt` decimal(10,0) DEFAULT NULL,
  `ApicallTime` decimal(10,0) DEFAULT NULL,
  KEY `index1` (`TIME`),
  KEY `index2` (`OBJ`,`SERVICE`),
  KEY `obj_time` (`OBJ`,`TIME`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
##### <span style="color:red">! TIME varchar 확인 후, Between 사용을 위한 구조 형식 필요. 차장님께 형식 요구 </span>

##### 1-3 데이터 이관 소스 코드

```
# mysqldump 사용 -u [userid] -p[password] -h [host_ip] -P [Port] [DB] [TABLE] --where [조건 3개] > scouter.sql
$ mysqldump -P 3306 -h ip_address -u userid -ppassword --routines scouter srb --where="((TIME BETWEEN '2020-10-01 00:00:00' AND  '2020-10-07 23:59:59') OR (TIME BETWEEN '2020-11-01 00:00:00' AND  '2020-11-07 23:59:59')) AND OBJ='pnx-phr'" > srb_seo.sql

# OBJ_LIST 다 가져오기.
$ mysqldump -P 3306 -h ip_address -u userid -ppassword --routines scouter OBJ_LIST> obj_list_seo.sql
```

##### <span style="color:red">! Tip 첨부파일 작성시 유니코드 주의</span>

![반디집](https://user-images.githubusercontent.com/43161245/99231884-9188bc80-2834-11eb-9bcc-92bbc1346609.PNG)


##### 1-4 데이터 복원 소스코드
```
$ mysql -u root -p scouter < dump_sys_sys.sql
$ mysql -u root -p scouter < dump_obj_sys.sql
```

##### 1-5 데이터 저장 구조 파악 (사진)
```
* srb 테이블

  - SELECT * FROM srb LIMIT 20; (왼쪽 사진)
  -
* OBJ_LIST 테이블

  - SELECT * FROM OBJ_LIST (오른쪽 사진)

```
<img src="https://user-images.githubusercontent.com/43161245/99231952-a1080580-2834-11eb-8c1f-fcac13c78cb3.PNG" width="48%" height="500">
<img src="https://user-images.githubusercontent.com/43161245/99231973-a82f1380-2834-11eb-9d5d-51f301661ba2.PNG" width="50%" height="500">

</div>
</details>

<details>
<summary>DB Query 초기 구조 파악 및 개발 </summary>
<div markdown="1">

##### 1-6 개발 쿼리 (단계별 분해)
```
* 필수 개발 요소
  1. 전월 데이터, 비교 수치
  2. 결과 데이터는 최소 20개가 출력 되게끔 조회 조건을 임의 수정 (비교 값등)
  3. 정렬 순서는 개인 판단
  4. 화면 구성 (아래)
```
![만들결과물](https://user-images.githubusercontent.com/43161245/99357547-70d16d00-28ef-11eb-9eb1-697ce2f5851e.PNG)

<br />

##### 1-6-1 초기 코드 분해 - 테스트 코드
<br />

```
1. 테스트 코드 그대로 사용 (왼쪽 사진)

* 빨강 : 사용되는 곳이 한정되어 있어서, 필요없을시 삭제 => 삭제

* 초록 : 수식이 적혀있기 때문에, 나중에 수식으로 활용

* 실행 시간 : 1분 21초

2.  빨강 박스 제거 및 수정후 실행 (오른쪽 사진)

* 실행 시간 : 5초
```
<img src="https://user-images.githubusercontent.com/43161245/99390996-d4be5a80-291c-11eb-8b63-05f36ed3cdb6.PNG" width="48%" height="350px">
<img src="https://user-images.githubusercontent.com/43161245/99391662-d4728f00-291d-11eb-8fe5-c17198e4884d.PNG" width="48%" height="350px">

<br />
<br />

##### 1-6-2 나에게 맞게 코드 수정
<br />

```
* 기존 코드를 수정하였습니다.

왼쪽 코드 : HAVING 을 사용  | 오른쪽 코드 : WHERE 사용
```
<br />

<img src="https://user-images.githubusercontent.com/43161245/99392921-ddfcf680-291f-11eb-9a69-45b1160796e2.png" width="48%" height="500px">
<img src="https://user-images.githubusercontent.com/43161245/99392925-df2e2380-291f-11eb-9bf0-7066988190f2.png" width="48%" height="500px">

##### * 실행 시간 비교 결론 : 상황에 따라 다르지만 WHERE을 쓸 수 있게 효율을 올리자.<br /><br /><br />

|TEST |HAVING|WHERE|
|------|---|---|
|테스트1|33.062 초|32.969 초|
|테스트2|33.141 초|32.610 초|
|테스트3|33.031 초|32.922 초|
|테스트4|33.156 초|32.672 초|
|테스트5|33.000 초|32.969 초|

<br />

##### 1-6-3 쿼리 작성 진행.v1 (RIGHT JOIN 사용) - 필수 조건 출력 완료 <br />
```
 * 10월과 11월의 서비스 개수가 같다면?
   - 교집합을 사용한다.
   - INNER JOIN을 사용한다.

 * 11월에 서비스가 추가 된다면?
   - 겹치는 부분 뿐만아니라, 차집합을 이용하여 추가하여야한다. (A-B = 빨간색 막대기 추가)
   - RIGHT OUTER JOIN을 사용한다.

 왼쪽 위 사진  : 교집합(INNER)  그림
 왼쪽 아래 사진 : 차집합(RIGHT,LEFT) 그림
 오른쪽 사진 : RIGHT OUTER JOIN 작성 코드
```
<img src="https://user-images.githubusercontent.com/43161245/99539389-875fed00-29f1-11eb-957c-3059d9176ca6.PNG" width="40%" height="550">

<img src="https://user-images.githubusercontent.com/43161245/99398608-fbce5980-2927-11eb-8ce9-d1f755a0fc86.PNG" width="55%" height="550">

</div>
</details>

<details>
<summary>DB Query 중간 피드백 (+ 기본 코드 완성) &#128515</summary>
<div markdown="1">

##### 1-6-4 중간 피드백 by 이종무 차장님 &#128515;
```

# 피드백 1. RIGHT OUTER -> LEFT OUTER
-> 대상의 기준이 오른쪽이냐 왼쪽이냐의 차이점입니다.

# 해당 피드백 관련 팁 (블로그)
  OUTER은 안 적어도 상관 없지만 명시적으로 적어주는 것이 좋습니다.

# 결론

  큰 차이는 없으나, 대부분의 블로그에서 LEFT OUTER 을 정석으로 사용하고 있다.
  기준을 상단으로 진행하도록 한다.
```

```
피드백 2. 성능도 중요하지만 기능들을 추가하였으면 좋겠다.

  - 위 코드에서 출력 가능한 컬럼

  - 앞으로 추가할 예정인 컬럼

```
```
중간 질문. UNION 과 UNION ALL 을 사용하거나 알고 있는가? NO..
+ 정답은 아니지만 시간이 된다면 조사해보고 사용해보는 기회가 되었으면 좋겠다.

+ 무조건 사용해보겠다.
```
##### UNION 과 UNION ALL 이 무엇이며, 차이점은?
```
* 2개의 별도의 조회 쿼리 결과 값을 한번에 보고 싶을 때, UNION 을 사용한다.

1. UNION : 중복 값 제외하여 출력 (합집합 - 교집합)
2. UNION ALL : 중복 관계 없이 출력 (합집합)

속도면에서, UNION 은 UNION ALL에 비해서 중복을 제거해주어야하기 때문에 느릴 수 밖에 없습니다. (느리다!)
+ 현 과제 에서는 TIME 으로 중복을 가를 수 있으니, 만약 사용한다면 UNION ALL로 작성을 해보자

* JOIN 도 합치는 건데, 그렇다면 차이점은?
```
##### JOIN 과 UNION 의 차이점은? [참고 자료](https://qastack.kr/programming/905379/what-is-the-difference-between-join-and-union)
```
* JOIN
- 간단하게 말해 데이터를 새로운 열로 결합합니다.
```
![image](https://user-images.githubusercontent.com/43161245/99632587-d1dc7a80-2a80-11eb-84ac-be66e83ce4f5.png)

```
* UNION
- 데이터를 새로운 행에 결합합니다.

! 한 마디로, 가지고 있는 컬럼이 같아야합니다. (필요없는 정보를 가져와야 할 수도 있으니 단점이 될수도 있고, 주의하자)
  => 과제 코드에서는 큰 장점이 될 수 있을 것 같다.
```

![image](https://user-images.githubusercontent.com/43161245/99632604-d6a12e80-2a80-11eb-8bba-ec201cdb0b95.png)

##### ! OUTPUT 은 있어야한다. (결과는 나와야하고, 빠르게 성공하고 쿼리 튜닝하자) - UNION 사용 X

##### 1-6-5 쿼리 작성 완료.v2 (기능마다 서브쿼리 작성 후 JOIN 형식) - 58초

```
* 주황색 네모 박스 : LEFT OUTER 를 사용한 서브 쿼리
* 연두색 네모 박스 : 다른 테이블

방식 : JOIN을 각 GROUP BY 별로 뽑아낸 정보들을 메인 쿼리에서 뽑아내주는 방식
  = 필요한 것들을 각 쿼리 부분에서 뽑아내서 사용해보자라는 생각에서 진행하였습니다.
```
```
# 각 항목들을 출력

* 주황색 박스 1 (서브 쿼리 AS NOW_MOTH_SQL)

  금월오류건수, 금월수행건수, 금월오류율, 금월평균응답시간, 평균CPU사용시간

* 주황색 박스 2 (서브 쿼리 AS LASE_MOTH_SQL)

  전월오류건수, 전월수행건수, 전월오류율, 전월평균응답시간

* 주황색 박스 3 (서브 쿼리 AS ERROR_NOW_CL)

  금월오류최대발생일자, 금월오류최대발생일건수

* 주황색 박스 4 (서브 쿼리 AS ERROR_LAST_CL)

  전월오류최대발생일자, 전월오류최대발생일건수

```
```
문제점 (이슈)

실행 시간: 58초 ~ 1분
1. 서브 쿼리 및 조인의 다수 존재 때문에 속도가 너무너무 느리다.
  = 주황색이 몇개야...!!

2. INDEX 가 가공된 데이터를 사용하므로 타지 못하는 것 같다.
  예) MONTH(SRB_TB.TIME)
```

![sql 캡처 1번](https://user-images.githubusercontent.com/43161245/99640279-0570d200-2a8c-11eb-97b2-448671563237.PNG)
![결과 캡처-1](https://user-images.githubusercontent.com/43161245/99642484-e889ce00-2a8e-11eb-8e92-22e1445e0d60.PNG)



##### 1-6-6 쿼리 작성 완료.v3 (조금 수정 INNER 로 묶기) - 58초

```
* 상황

  - INNER JOIN 을 사용해야 한다는 블로그의 말을 믿고 LEFT OUTER 의 수를 4 -> 2 로 줄였다.
  - 속도 1 ~ 2 초..? 정도가 절약되었다.
  - 결과로 나오는 필드 수가 14개로 늘어났다.

    이유 : 맨 하단부 WHERE 문에서 전에 오브젝트를 검사해주는 경우가 있었는데, 11월에만 있는 서비스에서는
           10월의 오브젝트는 NULL 값이므로, 제거해주어야 한다.
```

```
SELECT NOW_MONTH_SQL.WOKR_CL AS 코드,
		 OBJ_TB.OBJ_TEXT AS 업무,
		 NOW_MONTH_SQL.SERVICE_CL AS 서비스,

		 LAST_MONTH_SQL.ErrCnt_Sum AS 전월오류건수,
		 LAST_MONTH_SQL.Cnt_Sum AS 전월수행건수,

		 NOW_MONTH_SQL.ErrCnt_Sum AS 금월오류건수,
		 NOW_MONTH_SQL.Cnt_Sum AS 금월수행건수,

		 ROUND((LAST_MONTH_SQL.ErrCnt_Sum/LAST_MONTH_SQL.Cnt_Sum)*100,2) AS 전월오류율,
		 ROUND((NOW_MONTH_SQL.ErrCnt_Sum/NOW_MONTH_SQL.Cnt_Sum)*100,2) AS 금월오류율,

		 ROUND(LAST_MONTH_SQL.AVG_Elapsed , 0) AS 전월평균응답시간,
		 ROUND(NOW_MONTH_SQL.AVG_Elapsed , 0) AS 금월평균응답시간,

		 ERROR_NOW_CL.ErrCnt_Sum AS 금월오류최대발생건수,
		 ERROR_LAST_CL.ErrCnt_Sum AS 전월오류최대발생건수,

		 ERROR_NOW_CL.DATE_CL AS 금월오류최대발생일자,
		 ERROR_LAST_CL.DATE_CL AS 전월오류최대발생일자,	 

		 ROUND(LAST_MONTH_SQL.Cpu_CL, 0) AS 전월CPU평균사용시간,
		 ROUND(NOW_MONTH_SQL.Cpu_CL, 0) AS 금월CPU평균사용시간

FROM ((SELECT SRB_TB.obj AS WOKR_CL, SRB_TB.service        AS SERVICE_CL,
               Sum(CASE
                     WHEN SRB_TB.error NOT IN (0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   end)              AS ErrCnt_Sum,
               Count(SRB_TB.elapsed) AS Cnt_Sum,
               Avg(SRB_TB.elapsed)   AS AVG_Elapsed,
               AVG(SRB_TB.CpuTime)   AS Cpu_CL

		         FROM   scouter.srb AS SRB_TB
				   WHERE  SRB_TB.time >= "2020-11-01" AND SRB_TB.time < "2020-11-06"
		         GROUP  BY SERVICE_CL) AS NOW_MONTH_SQL

        -- 11월 오류최대발생건수, 일자
		  INNER JOIN (SELECT SRB_TB.obj AS WOKR_CL, SRB_TB.service AS SERVICE_CL,
		          Sum(CASE
                     	WHEN SRB_TB.error NOT IN (0, 924229217, 529547390 ) THEN 1
                     	ELSE 0
                   	END) AS ErrCnt_Sum,
			       DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS DATE_CL		  
			       FROM   scouter.srb AS SRB_TB
			       WHERE  SRB_TB.time >= "2020-11-01" AND SRB_TB.time < "2020-11-08"
			       GROUP  BY SERVICE_CL, DATE_CL
					 ORDER BY ErrCnt_Sum DESC) AS ERROR_NOW_CL

		  ON NOW_MONTH_SQL.SERVICE_CL= ERROR_NOW_CL.SERVICE_CL AND NOW_MONTH_SQL.WOKR_CL=ERROR_NOW_CL.WOKR_CL)

        -- 10월 수행,오류 건수를 위한 LEFT JOIN  (11월 기준)
		  LEFT OUTER JOIN ((SELECT SRB_TB.obj AS WOKR_CL, SRB_TB.service AS SERVICE_CL,  
               Sum(CASE
                     WHEN SRB_TB.error NOT IN (0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   end)              AS ErrCnt_Sum,
               Count(SRB_TB.elapsed) AS Cnt_Sum,
               Avg(SRB_TB.elapsed)   AS AVG_Elapsed,
               AVG(SRB_TB.CpuTime)   AS Cpu_CL

		         FROM   scouter.srb AS SRB_TB
		         WHERE  SRB_TB.time >= "2020-10-01" AND SRB_TB.time < "2020-10-06"
		         GROUP  BY SERVICE_CL) AS LAST_MONTH_SQL

		  INNER JOIN (SELECT SRB_TB.obj AS WOKR_CL, SRB_TB.service AS SERVICE_CL,
         		Sum(CASE
                     WHEN SRB_TB.error NOT IN (0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   END) AS ErrCnt_Sum,          		  
		         DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS DATE_CL    	   
		         FROM   scouter.srb AS SRB_TB
		         WHERE  SRB_TB.time >= "2020-10-01" AND SRB_TB.time < "2020-10-08"
		         GROUP  BY SERVICE_CL, DATE_CL
				   ORDER BY ErrCnt_Sum DESC) AS ERROR_LAST_CL

		  ON LAST_MONTH_SQL.SERVICE_CL= ERROR_LAST_CL.SERVICE_CL AND LAST_MONTH_SQL.WOKR_CL=ERROR_LAST_CL.WOKR_CL)

		  ON NOW_MONTH_SQL.SERVICE_CL= LAST_MONTH_SQL.SERVICE_CL AND NOW_MONTH_SQL.WOKR_CL=LAST_MONTH_SQL.WOKR_CL

		  INNER JOIN scouter.OBJ_LIST AS OBJ_TB

		  ON OBJ_TB.OBJ_NAME = NOW_MONTH_SQL.WOKR_CL

WHERE (NOW_MONTH_SQL.Cnt_Sum >= '10'  AND NOW_MONTH_SQL.ErrCnt_Sum >= '3' AND NOW_MONTH_SQL.WOKR_CL='pnx-phr')
GROUP BY NOW_MONTH_SQL.SERVICE_CL
ORDER BY 4 DESC LIMIT  20;
```
##### 1-6-7 쿼리 작성 완료.v4 (UNION ALL 써보기) - 1분
```
* 상황

  - 차장님과 피드백 중 나온 UNION ALL 을 사용해보도록 한다.
  - 합쳐지는 부분에서 JOIN 보다 빠르므로, 속도가 줄것이라고 예상
  - 그러나, 조건문 처리 과정에서 시간이 오래 걸림(너무 지저분하게 만들었다)

* 결과

  - 조인을 활용했을 때랑 비슷한 속도가 나왔다.
  - 이론적으로는 UNION ALL이 조금 더 빨랐어야했는데, 마지막 뽑아내는 부분에서 조건문 사용을 계속해서 더 오래걸렸다.
```
![v3,v4 비교](https://user-images.githubusercontent.com/43161245/100095149-6a1d9980-2e9d-11eb-875f-ed1c15d7b131.PNG)

```
SELECT
	ALL_DATA_SQL.Work_CL AS 업무,
	ALL_DATA_SQL.Service_CL AS 서비스,

	MAX(CASE
		WHEN ALL_DATA_SQL.DATE_CL < '2020-10-08' THEN ALL_DATA_SQL.ErrCnt_Sum
 	END) AS 전월오류건수,

	SUM(CASE
		WHEN ALL_DATA_SQL.DATE_CL < '2020-10-08' THEN ALL_DATA_SQL.Cnt_Sum
 	END) AS 전월수행건수,

	SUM(CASE
		WHEN ALL_DATA_SQL.DATE_CL >= '2020-11-01' AND ALL_DATA_SQL.DATE_CL < '2020-11-08'  THEN ALL_DATA_SQL.ErrCnt_Sum
 	END) AS 금월오류건수,
	SUM(CASE
		WHEN ALL_DATA_SQL.DATE_CL >= '2020-11-01' AND ALL_DATA_SQL.DATE_CL < '2020-11-08'   THEN ALL_DATA_SQL.Cnt_Sum
 	END) AS 금월수행건수,

 	SUM(CASE
		WHEN ALL_DATA_SQL.DATE_CL < '2020-10-08' THEN ALL_DATA_SQL.Error_Percent
 	END) AS 전월오류율,

	(CASE
		WHEN ALL_DATA_SQL.DATE_CL >= '2020-11-01' AND ALL_DATA_SQL.DATE_CL < '2020-11-08'  THEN  ALL_DATA_SQL.Error_Percent
 	END) AS 금월오류율,

	SUM(CASE
		WHEN ALL_DATA_SQL.DATE_CL < '2020-10-08'  THEN  ALL_DATA_SQL.AVG_Elapsed
 	END) AS 전월평균응답시간,
 
 	SUM(CASE
		WHEN ALL_DATA_SQL.DATE_CL >= '2020-11-01' AND ALL_DATA_SQL.DATE_CL < '2020-11-08'   THEN ALL_DATA_SQL.AVG_Elapsed
 	END) AS 금월평균응답시간,

 	SUM(CASE
		WHEN ALL_DATA_SQL.DATE_CL < '2020-10-08' THEN ALL_DATA_SQL.MAXSUM_CL
 	END) AS 전월오류최대발생건수,

	(CASE
		WHEN ALL_DATA_SQL.DATE_CL >= '2020-11-01' AND ALL_DATA_SQL.DATE_CL < '2020-11-08'  THEN  ALL_DATA_SQL.MAXSUM_CL
 	END) AS 금월오류최대발생건수,
 

	MAX(CASE
		WHEN ALL_DATA_SQL.DATE_CL < '2020-10-08'  THEN  ALL_DATA_SQL.DATE_CL
 	END) AS 전월오류최대발생일자,
 		  
	(CASE
		WHEN ALL_DATA_SQL.DATE_CL >= '2020-11-01' AND ALL_DATA_SQL.DATE_CL < '2020-11-08'  THEN  ALL_DATA_SQL.DATE_CL
 	END) AS 금월오류최대발생일자
 
	FROM
		(SELECT NOW_MAIN_SQL.Work_CL,
				  NOW_MAIN_SQL.Service_CL ,
				  NOW_MAIN_SQL.ErrCnt_Sum ,
				  NOW_MAIN_SQL.Cnt_Sum,
				  ROUND((NOW_MAIN_SQL.ErrCnt_Sum/NOW_MAIN_SQL.Cnt_Sum)*100,2) AS Error_Percent,
				  ROUND(NOW_MAIN_SQL.AVG_Elapsed , 0) AS AVG_Elapsed,
			     ROUND(NOW_MAIN_SQL.Cpu_CL, 0) AS Cpu_CL,
			    
			     NOW_EXTRA_SQL.DATE_CL,
			     NOW_EXTRA_SQL.MAXSUM_CL


			FROM			
					(SELECT

						SRB_TB.obj			  AS Work_CL,
						SRB_TB.service      AS Service_CL,  
			         Sum(CASE
			                  WHEN SRB_TB.error NOT IN (0, 924229217, 529547390 ) THEN 1
			                  ELSE 0
			             end)              AS ErrCnt_Sum,
			         Count(SRB_TB.elapsed) AS Cnt_Sum,
			         Avg(SRB_TB.elapsed)   AS AVG_Elapsed,
			         AVG(SRB_TB.CpuTime)   AS Cpu_CL
			        		
						FROM   scouter.srb AS SRB_TB
						WHERE  SRB_TB.time >= "2020-11-01" AND SRB_TB.time < "2020-11-06"
						GROUP BY Service_CL) AS NOW_MAIN_SQL


					INNER JOIN

					(SELECT

						SRB_TB.obj			  AS Work_CL,
						SRB_TB.service      AS Service_CL,  
			         Sum(CASE
			                  WHEN SRB_TB.error NOT IN (0, 924229217, 529547390 ) THEN 1
			                  ELSE 0
			             END)              AS MAXSUM_CL,
			         DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS DATE_CL      
			         FROM   scouter.srb AS SRB_TB
						WHERE  SRB_TB.time >= "2020-11-01" AND SRB_TB.time < "2020-11-08"
						GROUP BY Service_CL, DATE_CL
			         ORDER BY MAXSUM_CL DESC) AS NOW_EXTRA_SQL
			        
			        
			         ON NOW_MAIN_SQL.Work_CL = NOW_EXTRA_SQL.Work_CL AND NOW_MAIN_SQL.Service_CL = NOW_EXTRA_SQL.Service_CL
			WHERE NOW_MAIN_SQL.ErrCnt_Sum >= '3'		
			GROUP BY NOW_MAIN_SQL.Service_CL
			        
			        
		UNION ALL

		SELECT  LAST_MAIN_SQL.Work_CL,
				   LAST_MAIN_SQL.Service_CL,
				   LAST_MAIN_SQL.ErrCnt_Sum,
				   LAST_MAIN_SQL.Cnt_Sum,
				   ROUND((LAST_MAIN_SQL.ErrCnt_Sum/LAST_MAIN_SQL.Cnt_Sum)*100,2) AS Error_Percent,
				   ROUND(LAST_MAIN_SQL.AVG_Elapsed , 0) AS AVG_Elapsed,
			      ROUND(LAST_MAIN_SQL.Cpu_CL, 0) AS Cpu_CL,
			      
			      LAST_EXTRA_SQL.DATE_CL,
			      LAST_EXTRA_SQL.MAXSUM_CL
			FROM			
					(SELECT

						SRB_TB.obj			  AS Work_CL,
						SRB_TB.service      AS Service_CL,  
			         Sum(CASE
			                  WHEN SRB_TB.error NOT IN (0, 924229217, 529547390 ) THEN 1
			                  ELSE 0
			             end)              AS ErrCnt_Sum,
			         Count(SRB_TB.elapsed) AS Cnt_Sum,
			         Avg(SRB_TB.elapsed)   AS AVG_Elapsed,
			         AVG(SRB_TB.CpuTime)   AS Cpu_CL
			        		
						FROM   scouter.srb AS SRB_TB
						WHERE  SRB_TB.time >= "2020-10-01" AND SRB_TB.time < "2020-10-06"
						GROUP BY Service_CL) AS LAST_MAIN_SQL

					INNER JOIN

					(SELECT

						SRB_TB.obj			  AS Work_CL,
						SRB_TB.service      AS Service_CL,  
			         Sum(CASE
			                  WHEN SRB_TB.error NOT IN (0, 924229217, 529547390 ) THEN 1
			                  ELSE 0
			             end)              AS MAXSUM_CL,
			         DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS DATE_CL      
			         FROM   scouter.srb AS SRB_TB
						WHERE  SRB_TB.time >= "2020-10-01" AND SRB_TB.time < "2020-10-08"
						GROUP BY Service_CL, DATE_CL
			         ORDER BY MAXSUM_CL DESC) LAST_EXTRA_SQL
			        
			         ON LAST_MAIN_SQL.Work_CL = LAST_EXTRA_SQL.Work_CL AND LAST_MAIN_SQL.Service_CL = LAST_EXTRA_SQL.Service_CL

			GROUP BY LAST_MAIN_SQL.Service_CL		
						) AS ALL_DATA_SQL

	WHERE ALL_DATA_SQL.Work_CL='pnx-phr'					
 	GROUP BY ALL_DATA_SQL.Service_CL
   HAVING 금월수행건수 >= '10' AND 금월오류건수 >= '3'
	ORDER BY 5 DESC;
```

</div>
</details>

<details>
<summary>DB Query 구조 튜닝</summary>
<div markdown="1">


##### 1-6-8 쿼리 작성 완료.v5 (WITH 사용해서 호출 줄이기) - 38초 ~ 40
```
* 상황
  - V1 ~ V4 구조로는 더 이상 빨라지지 않을 것 같아서 구조를 바꾸기로 했습니다.
  - 원래는 각각 뽑아낼 수 있게 AS 를 사용해서 만들었지만, SELECT FROM 안에 SELECT FROM 안에 SELECT 이런 식으로 제작하였다.
```

![v4, v5 비교](https://user-images.githubusercontent.com/43161245/100095152-6ab63000-2e9d-11eb-89d0-27d8bb19c10f.PNG)

```
WITH NOW_CTE_SQL AS (
		SELECT
					SRB_TB.obj        	   AS Work_CL,                 
					SRB_TB.service          AS Service_CL,  
               Sum(CASE
                     WHEN SRB_TB.error NOT IN ( 0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   END)                AS ErrCnt_Sum,
               Count(SRB_TB.elapsed)   AS Cnt_Sum,
               Avg(SRB_TB.elapsed)     AS AVG_Elapsed,                    
					DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS TIME_CL,
					AVG(SRB_TB.CpuTime)   AS Cpu_CL

        FROM   scouter.srb AS SRB_TB
		  USE INDEX (index1)
        WHERE (SRB_TB.time >= '2020-11-01' AND SRB_TB.time < '2020-11-08') AND SRB_TB.OBJ ='pnx-phr'
		  GROUP BY Service_CL, DATE_FORMAT(SRB_TB.time,'%Y-%m-%d')

		  ORDER BY ErrCnt_Sum DESC
		  ),
		LAST_CTE_SQL AS (
		SELECT
					SRB_TB.obj        	   AS Work_CL,                 
					SRB_TB.service          AS Service_CL,

               Sum(CASE
                     WHEN SRB_TB.error NOT IN ( 0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   END)                AS ErrCnt_Sum,
					Count(SRB_TB.elapsed)   AS Cnt_Sum,
               Avg(SRB_TB.elapsed)     AS AVG_Elapsed,
					DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS TIME_CL,
					AVG(SRB_TB.CpuTime)   AS Cpu_CL

        FROM   scouter.srb AS SRB_TB
        USE INDEX (index1)
		  WHERE SRB_TB.time >= '2020-10-01' AND SRB_TB.time < '2020-10-08' AND SRB_TB.OBJ ='pnx-phr'
		  GROUP BY Service_CL, DATE_FORMAT(SRB_TB.time,'%Y-%m-%d')
		  ORDER BY ErrCnt_Sum DESC)  

SELECT

NOW_MONTH_SQL.Work_CL AS 코드,
NOW_MONTH_SQL.Service_CL AS 서비스,

LAST_MONTH_SQL.ErrCnt_Sum AS 전월오류건수,
LAST_MONTH_SQL.Cnt_Sum AS 전월수행건수,

NOW_MONTH_SQL.ErrCnt_Sum AS 금월오류건수,
NOW_MONTH_SQL.Cnt_Sum AS 금월수행건수,

ROUND((LAST_MONTH_SQL.ErrCnt_Sum/LAST_MONTH_SQL.Cnt_Sum)*100,2) AS 전월오류율,
ROUND((NOW_MONTH_SQL.ErrCnt_Sum/NOW_MONTH_SQL.Cnt_Sum)*100,2) AS 금월오류율,

ROUND(LAST_MONTH_SQL.AVG_Elapsed , 0) AS 전월평균응답시간,
ROUND(NOW_MONTH_SQL.AVG_Elapsed , 0) AS 금월평균응답시간,

NOW_MONTH_SQL.MAX_ErrCnt_Sum AS 금월오류최대발생건수,
LAST_MONTH_SQL.MAX_ErrCnt_Sum AS 전월오류최대발생건수,

NOW_MONTH_SQL.TIME_CL AS 금월오류최대발생일자,
LAST_MONTH_SQL.TIME_CL AS 전월오류최대발생일자,	 

ROUND(LAST_MONTH_SQL.Cpu_CL, 0) AS 전월CPU평균사용시간,
ROUND(NOW_MONTH_SQL.Cpu_CL, 0) AS 금월CPU평균사용시간

FROM
		(SELECT NOW_CTE_SQL.Work_CL AS Work_CL,
				 NOW_CTE_SQL.Service_CL AS Service_CL,
				 MAX(NOW_CTE_SQL.ErrCnt_Sum) AS MAX_ErrCnt_Sum,

             Sum(CASE
               	WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.ErrCnt_Sum
               	ELSE 0
            	END)                AS ErrCnt_Sum,

				 SUM(CASE
                  WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.Cnt_Sum
                  ELSE 0
               END)                AS Cnt_Sum,

				 Avg(CASE
                  WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.AVG_Elapsed
                  ELSE 0
               END)                AS AVG_Elapsed,				 


				 NOW_CTE_SQL.TIME_CL AS TIME_CL,
				 AVG(NOW_CTE_SQL.Cpu_CL)   AS Cpu_CL

		FROM NOW_CTE_SQL
		GROUP BY Service_CL) AS NOW_MONTH_SQL

		LEFT OUTER JOIN

		(SELECT LAST_CTE_SQL.Work_CL AS Work_CL,
				 LAST_CTE_SQL.Service_CL AS Service_CL,
				 MAX(LAST_CTE_SQL.ErrCnt_Sum) AS MAX_ErrCnt_Sum,

             Sum(CASE
               	WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.ErrCnt_Sum
               	ELSE 0
            	END)                AS ErrCnt_Sum,

				 SUM(CASE
                  WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.Cnt_Sum
                  ELSE 0
               END)                AS Cnt_Sum,

				 Avg(CASE
                  WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.AVG_Elapsed
                  ELSE 0
               END)                AS AVG_Elapsed,				 

				 LAST_CTE_SQL.TIME_CL AS TIME_CL,
				 AVG(LAST_CTE_SQL.Cpu_CL)   AS Cpu_CL

		FROM LAST_CTE_SQL
		GROUP BY Service_CL) AS LAST_MONTH_SQL

		ON NOW_MONTH_SQL.Service_CL = LAST_MONTH_SQL.Service_CL

WHERE (NOW_MONTH_SQL.Cnt_Sum >= '10'  AND NOW_MONTH_SQL.ErrCnt_Sum >= '3' AND NOW_MONTH_SQL.Work_CL='pnx-phr')
ORDER BY 4 DESC LIMIT  20;
```
##### 1-6-9 쿼리 작성 완료.v6 (내부에서 OBJ 검사)

```
* 구조 간단 설명
  - 제일 안쪽에서 휘닉스 오브젝트만 골라줍니다.
  - GROPU BY 로 휘닉스의 서비스끼리 묶어줍니다.
  - 서비스 하나당 일자별로 묶어줍니다. (예) SERVER - 11월 1일 , 2일 ---  6일
  - ordet by로 최대 오류로 내림차순을 설정해주고 최고 값을 하나만 꺼내어 사용한다(선택적 과제)
  - 두번째 SELECT 문에서는, 안쪽에 일자별로 모은 것들을 다시 서비스로 합쳐준다. (예) SERVICE - 11월
  - 두번째 SELECT 문에서 사용할 데이터들을 제일 바깥쪽 SELECT에서 불러서 사용합니다.
```
![v5 group](https://user-images.githubusercontent.com/43161245/100098957-19f50600-2ea2-11eb-84f0-935a541362b3.PNG)

```
* 제일 안쪽에서 pnx-phr 을 검사하는 문구를 제작하고 싶었다.
  scouter.sbr.OBJ = 'pnx-phr'
* 실행 속도가 3배는 느려지고 나오지 않음.
```
##### 기존 EXPLAIN 출력시
![5](https://user-images.githubusercontent.com/43161245/100168264-45a8d800-2f04-11eb-9cf9-106848ed49b0.PNG)

##### PNX-PHR 조건 추가시
![6](https://user-images.githubusercontent.com/43161245/100168266-46da0500-2f04-11eb-9398-6b620b49142d.PNG)

```
* 차이점
  = 인덱스 키의 변화
    index1 -> index1, index2, obj_time

  1. 인덱스 키가 여러개이면, 자동으로 판단해서 빠른 것으로 진행된다고 들은적이 있다..(정확하지 않음)
  2. 인덱스 키를 확실하게 설정하기로 함.
    * USE INDEX (index1)
```
```
WITH NOW_CTE_SQL AS (
		SELECT
					SRB_TB.obj        	   AS Work_CL,                 
					SRB_TB.service          AS Service_CL,  
               Sum(CASE
                     WHEN SRB_TB.error NOT IN ( 0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   END)                AS ErrCnt_Sum,
               Count(SRB_TB.elapsed)   AS Cnt_Sum,
               Avg(SRB_TB.elapsed)     AS AVG_Elapsed,                    
					DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS TIME_CL,
					AVG(SRB_TB.CpuTime)   AS Cpu_CL

        FROM   scouter.srb AS SRB_TB
        USE INDEX (index1)
		  WHERE SRB_TB.time >= '2020-11-01' AND SRB_TB.time < '2020-11-08' AND SRB_TB.OBJ='pnx-phr'
		  GROUP BY Service_CL, DATE_FORMAT(SRB_TB.time,'%Y-%m-%d')
		  ORDER BY ErrCnt_Sum DESC
		  ),
		LAST_CTE_SQL AS (
		SELECT
					SRB_TB.obj        	   AS Work_CL,                 
					SRB_TB.service          AS Service_CL,

               Sum(CASE
                     WHEN SRB_TB.error NOT IN ( 0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   END)                AS ErrCnt_Sum,
					Count(SRB_TB.elapsed)   AS Cnt_Sum,
               Avg(SRB_TB.elapsed)     AS AVG_Elapsed,
					DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS TIME_CL,
					AVG(SRB_TB.CpuTime)   AS Cpu_CL

        FROM   scouter.srb AS SRB_TB
        USE INDEX (index1)
		  WHERE SRB_TB.time >= '2020-10-01' AND SRB_TB.time < '2020-10-08' AND SRB_TB.OBJ='pnx-phr'
		  GROUP BY Service_CL, DATE_FORMAT(SRB_TB.time,'%Y-%m-%d')
		  ORDER BY ErrCnt_Sum DESC)  

SELECT

NOW_MONTH_SQL.Work_CL AS 코드,
NOW_MONTH_SQL.Service_CL AS 서비스,

LAST_MONTH_SQL.ErrCnt_Sum AS 전월오류건수,
LAST_MONTH_SQL.Cnt_Sum AS 전월수행건수,

NOW_MONTH_SQL.ErrCnt_Sum AS 금월오류건수,
NOW_MONTH_SQL.Cnt_Sum AS 금월수행건수,

ROUND((LAST_MONTH_SQL.ErrCnt_Sum/LAST_MONTH_SQL.Cnt_Sum)*100,2) AS 전월오류율,
ROUND((NOW_MONTH_SQL.ErrCnt_Sum/NOW_MONTH_SQL.Cnt_Sum)*100,2) AS 금월오류율,

ROUND(LAST_MONTH_SQL.AVG_Elapsed , 0) AS 전월평균응답시간,
ROUND(NOW_MONTH_SQL.AVG_Elapsed , 0) AS 금월평균응답시간,

NOW_MONTH_SQL.MAX_ErrCnt_Sum AS 금월오류최대발생건수,
LAST_MONTH_SQL.MAX_ErrCnt_Sum AS 전월오류최대발생건수,

NOW_MONTH_SQL.TIME_CL AS 금월오류최대발생일자,
LAST_MONTH_SQL.TIME_CL AS 전월오류최대발생일자,	 

ROUND(LAST_MONTH_SQL.Cpu_CL, 0) AS 전월CPU평균사용시간,
ROUND(NOW_MONTH_SQL.Cpu_CL, 0) AS 금월CPU평균사용시간

FROM
		(SELECT NOW_CTE_SQL.Work_CL AS Work_CL,
				 NOW_CTE_SQL.Service_CL AS Service_CL,
				 MAX(NOW_CTE_SQL.ErrCnt_Sum) AS MAX_ErrCnt_Sum,

             Sum(CASE
               	WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.ErrCnt_Sum
               	ELSE 0
            	END)                AS ErrCnt_Sum,

				 SUM(CASE
                  WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.Cnt_Sum
                  ELSE 0
               END)                AS Cnt_Sum,

				 Avg(CASE
                  WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.AVG_Elapsed
                  ELSE 0
               END)                AS AVG_Elapsed,				 


				 NOW_CTE_SQL.TIME_CL AS TIME_CL,
				 AVG(NOW_CTE_SQL.Cpu_CL)   AS Cpu_CL

		FROM NOW_CTE_SQL
		GROUP BY Service_CL) AS NOW_MONTH_SQL

		LEFT OUTER JOIN

		(SELECT LAST_CTE_SQL.Work_CL AS Work_CL,
				 LAST_CTE_SQL.Service_CL AS Service_CL,
				 MAX(LAST_CTE_SQL.ErrCnt_Sum) AS MAX_ErrCnt_Sum,

             Sum(CASE
               	WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.ErrCnt_Sum
               	ELSE 0
            	END)                AS ErrCnt_Sum,

				 SUM(CASE
                  WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.Cnt_Sum
                  ELSE 0
               END)                AS Cnt_Sum,

				 Avg(CASE
                  WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.AVG_Elapsed
                  ELSE 0
               END)                AS AVG_Elapsed,				 

				 LAST_CTE_SQL.TIME_CL AS TIME_CL,
				 AVG(LAST_CTE_SQL.Cpu_CL)   AS Cpu_CL

		FROM LAST_CTE_SQL
		GROUP BY Service_CL) AS LAST_MONTH_SQL

		ON NOW_MONTH_SQL.Service_CL = LAST_MONTH_SQL.Service_CL

WHERE (NOW_MONTH_SQL.Cnt_Sum >= '10'  AND NOW_MONTH_SQL.ErrCnt_Sum >= '3')
ORDER BY 4 DESC LIMIT  20;
```

##### 1-6-10 쿼리 작성 완료.v6 (인덱스 생성) - 41초

```
* 인덱스 생성
  - 서브쿼리의 조건 중 TIME 과 OBJ 가 걸려있는 INDEX가 딱히 보이지 않아서 생성했다.

* 코드
  ALTER TABLE srb ADD INDEX seoindex (TIME, OBJ)
```
![인덱스](https://user-images.githubusercontent.com/43161245/100171178-0ed5c080-2f0a-11eb-8caa-a1b61bf945ba.PNG)

```
* 인덱스 태우기
* 위에서 설명한 것처럼, FROM 절 아래에, USE INDEX (seoindex)를 작성해준다.
```
![인덱스2](https://user-images.githubusercontent.com/43161245/100171192-1006ed80-2f0a-11eb-9c32-c5f52357c88b.PNG)

```
* 결과

  - 인덱스를 태워보내서 빨라진 것인지는 확실하지 않지만, 기존 속도는 유지했다.
  - 서브 쿼리들은 인덱스를 안타고 있는 것을 확인했고,
```
```
-- WITH 로 10월 데이터 (서비스 - 일별 로 묶은 것) , 11월 데이터( 서비스 - 일별로 묶은 것) 을 가져옵니다.
WITH NOW_CTE_SQL AS (
		-- 11월 데이터(서비스 - 일별로 묶음)
		SELECT
					SRB_TB.obj        	   AS Work_CL,                 
					SRB_TB.service          AS Service_CL,  
               Sum(CASE
                     WHEN SRB_TB.error NOT IN ( 0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   END)                AS ErrCnt_Sum,
               Count(SRB_TB.elapsed)   AS Cnt_Sum,
               Avg(SRB_TB.elapsed)     AS AVG_Elapsed,                    
					DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS TIME_CL,
					AVG(SRB_TB.CpuTime)   AS Cpu_CL

        FROM   scouter.srb AS SRB_TB
        -- 인덱스는 생성한 것을 한 번 사용해 보도록 합니다.^^
        USE INDEX (seoindex)
		  WHERE SRB_TB.time >= '2020-11-01' AND SRB_TB.time < '2020-11-08' AND SRB_TB.OBJ='pnx-phr'
		  GROUP BY Service_CL, DATE_FORMAT(SRB_TB.time,'%Y-%m-%d')
		  ORDER BY ErrCnt_Sum DESC
		  ),
		LAST_CTE_SQL AS (
		-- 10월 데이터(서비스 - 일별로 묶음)
		SELECT
					SRB_TB.obj        	   AS Work_CL,                 
					SRB_TB.service          AS Service_CL,

               Sum(CASE
                     WHEN SRB_TB.error NOT IN ( 0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   END)                AS ErrCnt_Sum,
					Count(SRB_TB.elapsed)   AS Cnt_Sum,
          Avg(SRB_TB.elapsed)     AS AVG_Elapsed,
					DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS TIME_CL,
					AVG(SRB_TB.CpuTime)   AS Cpu_CL

      FROM   scouter.srb AS SRB_TB
      -- 인덱스는 생성한 것을 한 번 사용해 보도록 합니다.^^
		  USE INDEX (seoindex)
		  WHERE SRB_TB.time >= '2020-10-01' AND SRB_TB.time < '2020-10-08' AND SRB_TB.OBJ='pnx-phr'
		  GROUP BY Service_CL, DATE_FORMAT(SRB_TB.time,'%Y-%m-%d')
		  ORDER BY ErrCnt_Sum DESC)  

-- 마지막 작업
-- 아래 두 항목에서 뽑아낸 것들을 각각 별칭을 작성하여 작업합니다.		  
SELECT

NOW_MONTH_SQL.Work_CL AS 업무,
NOW_MONTH_SQL.Service_CL AS 서비스,

LAST_MONTH_SQL.ErrCnt_Sum AS 전월오류건수,
LAST_MONTH_SQL.Cnt_Sum AS 전월수행건수,

NOW_MONTH_SQL.ErrCnt_Sum AS 금월오류건수,
NOW_MONTH_SQL.Cnt_Sum AS 금월수행건수,

ROUND((LAST_MONTH_SQL.ErrCnt_Sum/LAST_MONTH_SQL.Cnt_Sum)*100,2) AS 전월오류율,
ROUND((NOW_MONTH_SQL.ErrCnt_Sum/NOW_MONTH_SQL.Cnt_Sum)*100,2) AS 금월오류율,

ROUND(LAST_MONTH_SQL.AVG_Elapsed , 0) AS 전월평균응답시간,
ROUND(NOW_MONTH_SQL.AVG_Elapsed , 0) AS 금월평균응답시간,

NOW_MONTH_SQL.MAX_ErrCnt_Sum AS 금월오류최대발생건수,
LAST_MONTH_SQL.MAX_ErrCnt_Sum AS 전월오류최대발생건수,

NOW_MONTH_SQL.TIME_CL AS 금월오류최대발생일자,
LAST_MONTH_SQL.TIME_CL AS 전월오류최대발생일자,	 

ROUND(LAST_MONTH_SQL.Cpu_CL, 0) AS 전월CPU평균사용시간,
ROUND(NOW_MONTH_SQL.Cpu_CL, 0) AS 금월CPU평균사용시간

FROM
		-- WITH 절에서 가져온 11월 데이터(서비스- 일별)들을 뽑아내는 작업입니다.
		(SELECT NOW_CTE_SQL.Work_CL AS Work_CL,
				 NOW_CTE_SQL.Service_CL AS Service_CL,
				 MAX(NOW_CTE_SQL.ErrCnt_Sum) AS MAX_ErrCnt_Sum,

				 -- 아래 항목 3개는 일자가 1일~5일로 제한 합니다.
             Sum(CASE
               	WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.ErrCnt_Sum
               	ELSE 0
            	END)                AS ErrCnt_Sum,

				 SUM(CASE
                  WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.Cnt_Sum
                  ELSE 0
               END)                AS Cnt_Sum,

				 Avg(CASE
                  WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.AVG_Elapsed
                  ELSE 0
               END)                AS AVG_Elapsed,				 


				 NOW_CTE_SQL.TIME_CL AS TIME_CL,
				 AVG(NOW_CTE_SQL.Cpu_CL)   AS Cpu_CL

		FROM NOW_CTE_SQL
		-- 일별로 뽑은 데이터들도 합쳐주기 위해서, Service 로만 묶어줍니다.
		GROUP BY Service_CL) AS NOW_MONTH_SQL

		-- 11월 데이터를 기준으로 하기 때문에, LEFT OUTER JOIN 을 사용합니다.				
		LEFT OUTER JOIN

		-- WITH 절에서 가져온 10월 데이터(서비스- 일별)들을 뽑아내는 작업입니다.
		(SELECT LAST_CTE_SQL.Work_CL AS Work_CL,
				 LAST_CTE_SQL.Service_CL AS Service_CL,
				 MAX(LAST_CTE_SQL.ErrCnt_Sum) AS MAX_ErrCnt_Sum,

				 -- 아래 항목 3개는 일자가 1일~5일로 제한 합니다.
             Sum(CASE
               	WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.ErrCnt_Sum
               	ELSE 0
            	END)                AS ErrCnt_Sum,

				 SUM(CASE
                  WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.Cnt_Sum
                  ELSE 0
               END)                AS Cnt_Sum,

				 Avg(CASE
                  WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.AVG_Elapsed
                  ELSE 0
               END)                AS AVG_Elapsed,				 

				 LAST_CTE_SQL.TIME_CL AS TIME_CL,
				 AVG(LAST_CTE_SQL.Cpu_CL)   AS Cpu_CL

		FROM LAST_CTE_SQL
		-- 일별로 뽑은 데이터들도 합쳐주기 위해서, Service 로만 묶어줍니다.
		GROUP BY Service_CL) AS LAST_MONTH_SQL

		ON NOW_MONTH_SQL.Service_CL = LAST_MONTH_SQL.Service_CL

WHERE (NOW_MONTH_SQL.Cnt_Sum >= '10'  AND NOW_MONTH_SQL.ErrCnt_Sum >= '3')
ORDER BY 4 DESC LIMIT  20;
```

</div>
</details>

<details>
<summary>DB Query 최종 피드백 (+ 최종 코드 완성)&#128515;</summary>
<div markdown="1">

#### 1-7 최종 피드백 by 이종무 차장님 &#128515;
```
1. 나중에 통계 데이터를 만들어 놓은 테이블을 사용하는 방법으로 생각을 해보아라.
 => 테이블 제한이 없었는데도 생각을 전혀 하지 못했다.
 => 확실히 통계 테이블이 있다면, 불필요한 수식과 과한 데이터 양을 줄일 수 있다고 생각했다..

2. 날짜 값에 대한 교육을 진행해주셨다.
 => Between 에 대한 데이터 누적 실수
 => Char 타입의 경우에는 % 사용하고 나머지는 X
 => 2020-11-25 00:00:00.999 ms 값도 있으니 초가 끝이라고 생각하지 마라! (그냥 1일 더해..)

 # 한 마디로 가장 효율적인 날짜 찾기는, >= , < 를 사용하자!

3. function base index
 => 이종무 차장님 말씀하신 것 + 찾아본 것 아래 작성

4. 전월과 당월의 비교하는 컬럼 없음, 정렬이 되어있지 않았다.
 => 정렬 및 추가적인 부분만 추가
```

#### 인덱스 관련 질문

```
* 상황
  => 이종무 차장님께 인덱스 관련 질문이 생겨서 질문을 하게 되었습니다.

  질문. 쿼리에 인덱스를 태우고 싶은데, 만약 TIME 값을 MONTH(TIME) 이렇게 하면 인덱스를 못 타는 것 같은데 대안 방안이 있을까요?

  답변. 가공된 데이터이기 때문에 기본 인덱스(TIME) 타지 못한다
        + Function Based Index 을 사용해야한다.
        + 물론 적당히 사용해야한다고도 부연 설명을 해주셨습니다.
```
##### Function Based Index 이란?  [참고블로그](http://minsql.com/mysql8/B-3-C-functionBasedIndex/)

```
* MySQL 8.0.13 부터 Function Based Index (Function을 적용한 Column에 대한 값을 인덱싱)를 지원하게 되었다.

* Function Based Index의 경우, 매우 필요한 기능이었지만 그동안 구현이 되지 않은 관계로 아래와 같이 진행되었다.
    - 5.6 이하 버젼에서는 컬럼을 추가하고 인덱싱을 건 후, trigger를 이용하여 추가된 컬럼에 데이터를 넣는 방식
    - 5.7 버젼에서는 Virtual Column을 만들고 해당 컬럼에 Index를 거는 방식

* MariaDB Command
    - ALTER TABLE srb ADD INDEX seoindex ((MONTH(TIME)));

* 단순한 쿼리 Function Based Index 를 사용하면 위의 코드를 이용할 수 있을 것 같다.
  인덱스를 사용하는 곳에 따라 고려를 해야할 것 같다.
```
##### 최종 쿼리 코드

```
* 정렬 순서 변경 (전월, 금월(당월))
* 인덱스 튜닝 (WHERE 과 GROUP BY 를 둘 다 태우고 싶었으나, 안 되었다. 구조를 맨 처음 구조로 도전중!)
```

```
-- WITH 로 10월 데이터 (서비스 - 일별 로 묶은 것) , 11월 데이터( 서비스 - 일별로 묶은 것) 을 가져옵니다.
WITH NOW_CTE_SQL AS (
		-- 11월 데이터(서비스 - 일별로 묶음)

		SELECT
					SRB_TB.obj        	   AS Work_CL,                 
					SRB_TB.service          AS Service_CL,  
               Sum(CASE
                     WHEN SRB_TB.error NOT IN ( 0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   END)                AS ErrCnt_Sum,
               Count(SRB_TB.elapsed)   AS Cnt_Sum,
               Avg(SRB_TB.elapsed)     AS AVG_Elapsed,                    
					DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS TIME_CL,
					AVG(SRB_TB.CpuTime)   AS Cpu_CL

        FROM   scouter.srb AS SRB_TB
        -- 인덱스는 생성한 것을 한 번 사용해 보도록 합니다.
        USE INDEX (seoindex)
		  WHERE SRB_TB.time >= '2020-11-01' AND SRB_TB.time < '2020-11-08' AND SRB_TB.OBJ='pnx-phr'
		  GROUP BY Service_CL, DATE_FORMAT(SRB_TB.time,'%Y-%m-%d')
		  ORDER BY ErrCnt_Sum DESC
		  ),
		LAST_CTE_SQL AS (
		-- 10월 데이터(서비스 - 일별로 묶음)
		SELECT
					SRB_TB.obj        	   AS Work_CL,                 
					SRB_TB.service          AS Service_CL,

               Sum(CASE
                     WHEN SRB_TB.error NOT IN ( 0, 924229217, 529547390 ) THEN 1
                     ELSE 0
                   END)                AS ErrCnt_Sum,
					Count(SRB_TB.elapsed)   AS Cnt_Sum,
               Avg(SRB_TB.elapsed)     AS AVG_Elapsed,
					DATE_FORMAT(SRB_TB.time,'%Y-%m-%d') AS TIME_CL,
					AVG(SRB_TB.CpuTime)   AS Cpu_CL

        FROM   scouter.srb AS SRB_TB
        -- 인덱스는 생성한 것을 한 번 사용해 보도록 합니다.^^
		  USE INDEX (seoindex)
		  WHERE SRB_TB.time >= '2020-10-01' AND SRB_TB.time < '2020-10-08' AND SRB_TB.OBJ='pnx-phr'
		  GROUP BY Service_CL, DATE_FORMAT(SRB_TB.time,'%Y-%m-%d')
		  ORDER BY ErrCnt_Sum DESC)  

-- 마지막 작업
-- 아래 두 항목에서 뽑아낸 것들을 각각 별칭을 작성하여 작업합니다.		  
SELECT

NOW_MONTH_SQL.Work_CL AS 코드,
OBJ_TB.OBJ_TEXT AS 업무,
NOW_MONTH_SQL.Service_CL AS 서비스,


LAST_MONTH_SQL.ErrCnt_Sum AS 전월오류건수,
NOW_MONTH_SQL.ErrCnt_Sum AS 금월오류건수,

LAST_MONTH_SQL.Cnt_Sum AS 전월수행건수,		 
NOW_MONTH_SQL.Cnt_Sum AS 금월수행건수,

ROUND((LAST_MONTH_SQL.ErrCnt_Sum/LAST_MONTH_SQL.Cnt_Sum)*100,2) AS 전월오류율,
ROUND((NOW_MONTH_SQL.ErrCnt_Sum/NOW_MONTH_SQL.Cnt_Sum)*100,2) AS 금월오류율,

ROUND(LAST_MONTH_SQL.AVG_Elapsed , 0) AS 전월평균응답시간,
ROUND(NOW_MONTH_SQL.AVG_Elapsed , 0) AS 금월평균응답시간,

LAST_MONTH_SQL.MAX_ErrCnt_Sum AS 전월오류최대발생건수,		 
LAST_MONTH_SQL.TIME_CL AS 전월오류최대발생일자,		 

NOW_MONTH_SQL.MAX_ErrCnt_Sum AS 금월오류최대발생건수,
NOW_MONTH_SQL.TIME_CL AS 금월오류최대발생일자,	 

ROUND(LAST_MONTH_SQL.Cpu_CL, 0) AS 전월CPU평균사용시간,
ROUND(NOW_MONTH_SQL.Cpu_CL, 0) AS 금월CPU평균사용시간

FROM
		-- WITH 절에서 가져온 11월 데이터(서비스- 일별)들을 뽑아내는 작업입니다.
		(SELECT NOW_CTE_SQL.Work_CL AS Work_CL,
				 NOW_CTE_SQL.Service_CL AS Service_CL,
				 MAX(NOW_CTE_SQL.ErrCnt_Sum) AS MAX_ErrCnt_Sum,

				 -- 아래 항목 3개는 일자가 1일~5일로 제한 합니다.
             Sum(CASE
               	WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.ErrCnt_Sum
               	ELSE 0
            	END)                AS ErrCnt_Sum,

				 SUM(CASE
                  WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.Cnt_Sum
                  ELSE 0
               END)                AS Cnt_Sum,

				 Avg(CASE
                  WHEN NOW_CTE_SQL.TIME_CL >= '2020-11-01' AND NOW_CTE_SQL.TIME_CL < '2020-11-06' THEN NOW_CTE_SQL.AVG_Elapsed
                  ELSE 0
               END)                AS AVG_Elapsed,				 


				 NOW_CTE_SQL.TIME_CL AS TIME_CL,
				 AVG(NOW_CTE_SQL.Cpu_CL)   AS Cpu_CL

		FROM NOW_CTE_SQL
		-- 일별로 뽑은 데이터들도 합쳐주기 위해서, Service 로만 묶어줍니다.
		GROUP BY Service_CL) AS NOW_MONTH_SQL

		-- 11월 데이터를 기준으로 하기 때문에, LEFT OUTER JOIN 을 사용합니다.				
		LEFT OUTER JOIN

		-- WITH 절에서 가져온 10월 데이터(서비스- 일별)들을 뽑아내는 작업입니다.
		(SELECT LAST_CTE_SQL.Work_CL AS Work_CL,
				 LAST_CTE_SQL.Service_CL AS Service_CL,
				 MAX(LAST_CTE_SQL.ErrCnt_Sum) AS MAX_ErrCnt_Sum,

				 -- 아래 항목 3개는 일자가 1일~5일로 제한 합니다.
				 -- 8일로 변경하면 된다.
             Sum(CASE
               	WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.ErrCnt_Sum
               	ELSE 0
            	END)                AS ErrCnt_Sum,

				 SUM(CASE
                  WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.Cnt_Sum
                  ELSE 0
               END)                AS Cnt_Sum,

				 Avg(CASE
                  WHEN LAST_CTE_SQL.TIME_CL >= '2020-10-01' AND LAST_CTE_SQL.TIME_CL < '2020-10-06' THEN LAST_CTE_SQL.AVG_Elapsed
                  ELSE 0
               END)                AS AVG_Elapsed,				 

				 LAST_CTE_SQL.TIME_CL AS TIME_CL,
				 AVG(LAST_CTE_SQL.Cpu_CL)   AS Cpu_CL

		FROM LAST_CTE_SQL
		-- 일별로 뽑은 데이터들도 합쳐주기 위해서, Service 로만 묶어줍니다.
		GROUP BY Service_CL) AS LAST_MONTH_SQL

		ON NOW_MONTH_SQL.Service_CL = LAST_MONTH_SQL.Service_CL

		INNER JOIN scouter.OBJ_LIST AS OBJ_TB

		ON OBJ_TB.OBJ_NAME = NOW_MONTH_SQL.Work_CL

WHERE (NOW_MONTH_SQL.Cnt_Sum >= '10'  AND NOW_MONTH_SQL.ErrCnt_Sum >= '3')

ORDER BY 4 DESC LIMIT  20;
```
</div>
</details>

<details>
<summary>DB 작업시 생긴 오류 &#10060</summary>
<div markdown="1">

### PUTTY 실행 후 갑자기 생긴 오류 !! &#128552;
```
* 상황

  1. Local 에서 쿼리를 서버에 날려서 테스트를 진행.
  2. 서버를 키고, TAB 키를 이용해 이동을 하려고 했다.
  3. 다음과 같은 오류가 나왔다

  " cannot create temp file for here-document: no space left on device "

  4. 파일 시스템의 디스크 공간이 없다는 오류, 메모리와 CPU를 확인
```
##### 메모리 사용량 학인 (문제 없음)
```
$ free -m
```
![메모리 문제3](https://user-images.githubusercontent.com/43161245/99905019-93112380-2d11-11eb-9aa3-65bb039b8acf.PNG)

##### 문제 발견 : CPU 및 메모리 정보 확인 (mysql %CPU 100.0 확인) &#128552;

![메모리 문제5](https://user-images.githubusercontent.com/43161245/99905015-82f94400-2d11-11eb-9379-2be35a1316da.PNG)

#### 해결방안 [블로그](https://m.blog.naver.com/PostView.nhn?blogId=debolm74&logNo=67642577&proxyReferer=https:%2F%2Fwww.google.com%2F)

```
* 블로그

  CPU 100%로 문제가 발생할 경우,
  " SQL 쿼리일 가능성이 99% 입니다. "

* MariaDB 접속 후 다음과 같은 명령어 작성으로 실행중인 쿼리 확인.

  $ MariaDB [none] > show processlist;
```
![메모리 문제1](https://user-images.githubusercontent.com/43161245/99905299-5c3c0d00-2d13-11eb-8929-1c2a3138057b.PNG)

```
* 다른 데몬들이 접근할 때, Local 에서 작동하는 쿼리가 계속 도는 것으로 추측!
* HOST 를 확인했을 때, local 에서 서버에 쏜 쿼리가 계속도는 것으로 파악
* 해당 쿼리를 실행 정지.

$ kill (id 번호)
```
![메모리 문제2](https://user-images.githubusercontent.com/43161245/99905507-84783b80-2d14-11eb-8fb7-5a811271536d.PNG)
```
* 이후 정상 실행 = Tab 사용 가능

* mysqld  CPU 100% 없어짐.
$ top
```
![메모리 문제 4](https://user-images.githubusercontent.com/43161245/99905553-d3be6c00-2d14-11eb-9aec-71dc7edddc2f.PNG)

</div>
</details>

### Grafana

<details>
<summary>Grafana 란?</summary>
<div markdown="1">

### Grafana [참고 블로그](https://www.44bits.io/ko/keyword/grafana), [공식 LABS](https://grafana.com/success/)
##### 오픈소스 메트릭 데이터 시각화 도구

![image](https://user-images.githubusercontent.com/43161245/99773701-2a2e7d80-2b50-11eb-9f89-db6d3fc631b2.png)

```
- 데이터를 시각화하여 분석 및 모니터링을 용이하게 해주는 오픈 소스 분석 플랫폼
- 여러 데이터 소스를 연동하여 사용할 수 있고, 시각화 된 데이터들을 대시보드로 만들 수 있다.
```

</div>
</details>

<details>
<summary>Grafana 설치</summary>
<div markdown="1">

#### 1. 그라파나 (Grafana) 설치 [홈페이지](https://grafana.com/grafana/download?platform=linux)

![1](https://user-images.githubusercontent.com/43161245/99878132-98ee0280-2c46-11eb-89d3-7ccc9e54235b.png)

```
# 터미널에서 다음과 같은 명령어를 작성하세요.

$wget https://dl.grafana.com/oss/release/grafana-7.3.3.linux-amd64.tar.gz

$tar -zxvf grafana-7.3.3.linux-amd64.tar.gz
```

</div>
</details>

<details>
<summary>Grafana 실행</summary>
<div markdown="1">


#### 2. 그라파나 실행

##### 2-1 Linux
```
# 실행 파일로 이동
$ cd 실행 파일로 이동

# grafana-server 실행
$ ./grafana-server web
```
##### 2-2 Window

![7](https://user-images.githubusercontent.com/43161245/99879059-30eeea80-2c4d-11eb-9762-d8396455726a.png)

</div>
</details>

<details>
<summary>Grafana + MariaDB(MySQL) 연결 </summary>
<div markdown="1">


#### 3. MariaDB(MySQL) 연결

##### 3-1 홈페이지 로그인
```
# 기본 설정
  - IP PORT  = 서버IP:3000
  - ID       = admin
  - Password = admin
```
![그라파나 1번](https://user-images.githubusercontent.com/43161245/99879387-1bc78b00-2c50-11eb-850d-36d8b9c94b81.PNG)
##### 3-2 ADD DATA SOURCES
```
- 홈페이지 로그인시 기본 페이지 사진
  => 해당 빨강 박스 클릭 (Add your first data source)
```
![그라파나 2번](https://user-images.githubusercontent.com/43161245/99879389-208c3f00-2c50-11eb-9302-672f8fd51684.PNG)
```
- MySQL 을 검색합니다.
  => MariaDB 검색하면 안 나옵니다!
```
![4](https://user-images.githubusercontent.com/43161245/99878123-8d024080-2c46-11eb-9328-e36fd835c401.png)

```
# 기본 설정

- Name = 사용하고자 하는 이름입니다.

# 1번 사진
# MySQL Connetion
- Host     = 서버IP:3306 (MariaDB 기본 포트)
- Database = 설정한 DB (필자는 scouter)
- USER     = root
- Password = root 비밀번호

# 2번 사진
- 생성이 성공한다면 다음과 같은 문구가 나옵니다.
  => "Database Connection OK"
```
![2](https://user-images.githubusercontent.com/43161245/99878116-84aa0580-2c46-11eb-9449-7291f198c882.png)
![3](https://user-images.githubusercontent.com/43161245/99878117-87a4f600-2c46-11eb-9ba5-684a454e0918.png)
```
# Dashboard 생성
  => DB 연결에 성공한다면, 다음과 같은 대시보드를 생성할 수 있습니다.
```
![5](https://user-images.githubusercontent.com/43161245/99878128-9095c780-2c46-11eb-9274-01b03bd4dd7e.png)
```
# Query 설정 후 데이터 정보 가져오기
  => 1. Format as 를 Table 로 변경합니다. (테이블 형식으로 보여주겠다.)
  => 2. 보내고 싶은 쿼리를 작성합니다. (필자는 상단 코드를 사용했습니다.)
```
![6](https://user-images.githubusercontent.com/43161245/99878129-955a7b80-2c46-11eb-9d8a-65659a3f20af.png)
```
# 결과 확인
 => TEST      : HeidiSQL
 => DashBoard : Grafana
 -- 결과 같음 (동일) --
```
![8](https://user-images.githubusercontent.com/43161245/99880254-b5de0200-2c55-11eb-969f-2680850e5757.png)

</div>
</details>

