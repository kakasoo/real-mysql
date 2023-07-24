# 설치와 설정

학습을 쉽게 하기 위해 docker compose를 사용한다.  
23.07 기준으로 `docker-compose` 대신 `docker compose`라고 입력해야 한다.

```bash
# 아래 스크립트로 일단 docker를 설치한다.
sudo curl -L "https://github.com/docker/compose/releases/download/2.18.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

```bash
# 미리 작성한 docker-compose.yml 파일로 mysql 이미지를 실행시킨다.
# -d 옵션은 백그라운드에서 실행하게 하는 명령어다.
docker compose up -d

# mysql container와 해당 container의 id 확인
docker ps -a

# exmaple
# docker restart 29b84d89620e
docker restart mysql-container

# docker container 접속
docker exec -it mysql-container bash

# mysql 접속
mysql -u root -p
>Enter password : password # 'password' 라고 입력할 것
```

# clean shutdown

```bash
mysql> SET GLOBAL innodb_fast_shutdown=0;
mysql> SHUTDOWN;
```

MySQL의 엔진은 innoDB이다.  
그리고 innoDB는 log 파일을 먼저 기록한 후 실제 데이터 파일을 변경하는데, 이 때 서버가 불안정할 경우 log에만 기록한 다음 데이터 파일을 미처 기록하지 못한 상태로 DB가 종료될 수도 있다.  
따라서 MySQ을 종료시킬 때에는 `SET GLOBAL innodb_fast_shutdown=0` 설정을 해주는 것이 좋다.
