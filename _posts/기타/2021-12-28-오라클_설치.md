---

title: "오라클 설치 따라해보기"
excerpt: "오라클을 사용하기 위해서 macos에 오라클 설치하는 방법"
description: "오라클을 사용하기 위해서 macos에 오라클 설치하는 방법"
tag : [오라클, 설치]
categories: etc
author_profile : true 
sidebar:
  nav: "sidebar-sample"


---
## 오라클 설치
  목차 
    [MacOS에서 Oracle DB 설치하기](#macos에서-oracle-db-설치하기)
    [1.준비물](#1-준비물)
    [2. 오라클 설치 시작](#2-오라클-설치-시작)
    [3. 오라클 설치 오류](#3-오라클-설치-오류)

#### MacOS에서 Oracle DB 설치하기 
- 집에서도 DB 환경 구축하고 실습해보려고 했다.   
- 마침 맥북도 있어서 금방 설치할줄 알았는데.. 생각보다 오랜시간 투자를 했다.
- 지우고 설치하고.. 신기한 MacOS

#### 1. 준비물 
 - Hoembrew (아래 프로그램을 설치하기 위해서 이 녀석이 가장 필요했다. )
 - 맥북을 활용 많이 못했지만 여기서 윈도우보다 명령어만 입력해서 간편?하지만 설정이 어렵다는 느낌이 많이 들었다. 
  ```
  아래 명령어 입력하면 설치가 된다
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```
 - cask  (설치 시 아이콘으로 나오게 해주는 프로그램같다.)
   ```
    brew install cask
   ```
 <img src="https://user-images.githubusercontent.com/78027688/147571716-25a66a21-4f41-4df0-9698-8ff3054edf4d.png">

 - docker (인터넷이랑 명령어가 달랐다.)
  ```
   brew install --cask docker
  ```
  <img src="https://user-images.githubusercontent.com/78027688/147571940-0386a3f5-6697-4e86-ac04-0c7438c36246.png">

#### 2. 오라클 설치 시작..
 - 위 과정이 끝나게 되면 시간을 투자해야하는 부분이다. 여러번 포기하고 싶었지만...

 1. 도커에서 이미지를 검색하여 다운로드한다. 
```
   docker search oralce
```

 <img  src="https://user-images.githubusercontent.com/78027688/147572519-bc13afb7-bc92-451c-a8d0-74efed9501f8.png">

   명령어 구조는 search [검색어]로 보인다 
   명령어를 입력하면 아래처럼 결과가 보인다
 <img  src="https://user-images.githubusercontent.com/78027688/147572572-5471126d-b297-4257-b279-4ef850bf7638.png">

 2. 이미지 다운 (다운로드 시간이 조금 걸린다.)
```
 docker pull vitorfec/oracle-xe-18c
```
 <img src="https://user-images.githubusercontent.com/78027688/147573003-7d53a760-969e-40e9-abc4-5f74a370a28d.png">

 . 다운로드 완료 후 이미지 확인 
  ```
   docker images
  ```
 <img  src="https://user-images.githubusercontent.com/78027688/147573223-74bc90c3-780f-4e45-a2d8-8a87a174f381.png">

 4. 다운로드 이미지를 컨테이너 생성 후 실행
```
  docker run --name oracle -d -p 5150:22 -p 5151:1521 -e ORACLE_PWD:1234 vitorfec/oracle-xe-18c
 ```
 5. 실행 후 확인 (4번 명령어 입력하고 조금 이후에 서버가 부팅완료됩니다.)
```
docker ps
```
 <img src="https://user-images.githubusercontent.com/78027688/147573824-3cbc188d-443c-4237-beed-04937eee66fd.png">

 6. 확인 완료 후 sqlplus 접속
```
docker exec -it -u oracle oracle sqlplus / as sysdba 
```
위 방법은 다운로드 받은 이미지에 설정된 패스워드를 모를 때 실행하는 방법 같아보인다.

추가 설명 
* docker: 도커가
* exec: 컨테이너 실행한다
* it: 쉘을 연다  (-i: interactive, -t: tty)
* -u: 접속하는 유저 이름 설정(임의로 변경 가능/위에서는 이름이 동일하게 설정) 
* oracle: oracle18c 라는 컨테이너 실행
* sqlplus / as sysdba: sqlplus / as sysdba 명령어 실행!!!

#### 3. 오라클 설치 오류 
 오라클 서버 기동 시 아래 오류 발생 
  mv: cannot stat '/opt/oracle/product/18c/dbhomeXE/dbs/spfileXE.ora': No such file or directory

<img src="https://user-images.githubusercontent.com/78027688/147574218-aeaae818-3e06-463e-b22e-4a8ed88de927.png">

[구글링 찾아보니까..]("https://github.com/oracle/docker-images/issues/1522") 

docker의 cpu를 줄이면 해결된다고한다?
<img  src="https://user-images.githubusercontent.com/78027688/147574793-71ee9cbb-c3a2-4f2d-9aa9-08f94d4d8af3.png">

### 속는셈치고 해봤다.

<img  src="https://user-images.githubusercontent.com/78027688/147574898-6c4fff5b-b0d4-41e8-b53e-189e372d44ed.png">

## 물론 부팅속도는 엄청나게 느려졌지만 오라클 설치 완료
