---
title: lighsail에 node 배포하기
date: 2025-06-05 00:27
categories:
  - aws
  - lighsail
tags:
  - aws
  - lightsail
  - Node
  - MySQL
_sort:
---
안녕하세요 🐸  
AWS의 Lightsail 에서 Node.js 프로젝트를 올려보는 기록입니다.  

## 1. Lightsail 생성하기
Lightsail은 AWS에서 검색하면 바로 확인할 수 있습니다.  

인스턴스가 없는 첫 화면 입니다. 인스턴스 생성을 누릅니다.  
![](/assets/img/screenshot/Pasted%20image%2020250607204152.png)  
<br>
플랫폼은 리눅스를 선택하고 블루프린트는 Node.js 를 선택합니다.  
![](/assets/img/screenshot/Pasted%20image%2020250607204222.png)  
<br>
첫 90일은 무료인 플랜이 있습니다. 이걸로 선택합니다. 90일 끝나기전에 꼭 삭제합시다.  
![](/assets/img/screenshot/Pasted%20image%2020250607204254.png)  
<br>
만들어졌습니다 짠! 눌러서 인스턴스에 접근해봅니다  
![](/assets/img/screenshot/Pasted%20image%2020250607204421.png)  
<br>
퍼블릭 IPv4 주소와 "SSH를 사용하여 연결" 버튼이 있습니다.  
![](/assets/img/screenshot/Pasted%20image%2020250607204533.png)  
<br>
퍼블릭 IPv4 주소를 사용해 웹에서 접근해보면 아래와 같은 페이지가 나옵니다. 어차피 이 글의 주소로 접근해보면 제가 삭제해서 안들어가지니 직접 만든 인스턴스로 테스트 해봅시다.  
![](/assets/img/screenshot/Pasted%20image%2020250607204645.png)   
<br>
이번엔 "SSH를 시용하여 연결" 버튼을 눌러봅니다.  
![](/assets/img/screenshot/Pasted%20image%2020250607204800.png)  
인스턴스에 정상적으로 접근이 되었습니다.  

## 2. mysql 설치하기

명령어 순서

> sudo apt update
> sudo apt install -y gnupg
> sudo wget https://dev.mysql.com/get/mysql-apt-config_0.8.30-1_all.deb
> sudo dpkg -i mysql-apt-config_0.8.30-1_all.deb

![](/assets/img/screenshot/Pasted%20image%2020250610210053.png) 
여기서는 OK 를 선택합니다.  

> sudo apt update
> sudo apt install -y mysql-server

![](/assets/img/screenshot/Pasted%20image%2020250610210154.png)  
사용할 패스워드를 입력합니다. 다음 화면에서도 동일하게 입력해줍니다.  

이후 아래의 명령어를 통해 mysql 을 실행하여 DB에 접근합니다.  

> sudo mysql -uroot -p

 아래의 명령어를 실행시킵니다.
 
```SQL
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '비밀번호';
```

## 3. 깃허브 프로젝트 내려받고 실행하기

깃허브에 올려놓은 레포지토리를 클론해봅니다

> git clone {깃허브주소}

그리고 프로젝트 경로로 이동하여 npm 패키지를 설치해줍니다.

> npm ci

혹은

> npm i

그리고 vim 으로 직접 `.env` 파일을 생성해줘야 합니다.  보안 때문에 깃허브에 올리지 않는 파일이기 떄문이죠.  

> vim .env

위 명령어를 통해 `.env` 파일이 생성되고 입력이 활성화 됩니다. 내용을 입력하고 ESC 를 눌른 다음 `:wq!` 를 입력하여 입력 모드를 종료합니다.  

저는 pm2 를 전역으로 설치하여 사용했기에 아래의 명령어를 사용했습니다.

> sudo npm install -g pm2

그리고 이제 프로젝트의 DB를 만들기 위해 시퀄라이즈 명령어를 통해 생성합니다.  

> sudo npx sequelize db:create --env production

이제 기존에 실행중인 아파치를 종료시킵니다.  
이게 80번 포트를 점유 중이기 때문에 그냥 실행시키면 포트가 충돌합니다.

> cd /opt/bitnami
> sudo ./ctlscript.sh stop apache

그리고 이제 서버를 실행시키고 앞서 확인한 퍼블릭 IPv4 로 접근해보면 서버가 돌고 있습니다

---

## 주의점
블루프린트를 Node.js 로 하면 AWS 에서 설정한 데비안 리눅스로 자동 세팅됩니다.  
여기서 글 작성일과 실제 테스트를 해보는 일시에 따라 버전이 다를 수 있습니다.  
데비안 리눅스의 버전이 다르다면 mysql 설치 버전도 달라져야하기 때문에 현재 버전이 몇버전인지 확인해보고 버전에 맞는 리눅스를 설치하면 됩니다.  


---
참고 : <https://docs.vultr.com/how-to-install-mysql-on-debian-12>