---
title: "[Linux] DB Dump 배치작업(자동백업 스크립트)"
excerpt: "DB Dump 배치작업(자동백업 스크립트)"

categories:
  - ETC
tags:
  - [ETC]

permalink: /db_dump

toc: true
toc_sticky: true

date: 2023-07-13
last_modified_at: 2023-07-13
---
## Oracle

### 1. Directory 경로 확인

- DBeaver 에서 다음 명령어로 경로 조회

```sql
SELECT * FROM DBA_DIRECTORIES
```

![stack](/assets/images/posts_img/dump/Untitled.png)

DIRECTORY_NAME = ATASSET_DIR

DIRECTORY_PATH = /backup/backup_oracle_dump/atasset/	

### 2. DB서버에서 쉘스크립트 작성 (oracle계정으로 접속! not root계정)

1) 쉘 스크립트 만들 경로로 이동

- DIRECTORY_PATH 하위에 쉘 스크립트 만들 경로로 이동

```bash
cd {DIRECTORY_PATH 하위에 쉘 스크립트 만들 경로}
```

- 예시

```bash
cd /backup/backup_oracle_dump/atasset/backupScript 
```

2) 쉘 스크립트 생성 

```bash
vi {접속서버IP}_{oracle/mariadb}_backup_script_{공용서버일 경우 업체명 명시}.sh
```

- 예시

```bash
vi /backup/mariadb/backupScript/221.139.104.41_oracle_backup_script_PROD_CMS_ATASSET.sh
```

3) 쉘 스크립트 작성

- 양식 및 설명

```bash
#!/bin/bash
source /home/oracle/.bash_profile

FULLDATE=$(date '+%Y-%m-%d %H:%M:%S')
DATE=$(date +%Y%m%d)
BACKUP_DIR={1번에서 확인한 DIRECTORY_PATH 와 같은 경로 설정}

#####ATASSET 
#find 명령어에 -mtime +일수 옵션을 주면 되는데... 생각한 일수보다 1 적게 주어야 함
#예를 들어 3일 초과한 파일을 삭제하려면 -mtime +2
# find 폴더 -name 파일명 -mtime +일수 -exec rm -f {} \;
find $BACKUP_DIR/ -name "{파일명}_*.dmp" -mtime {삭제주기} -exec rm -rf {} \;
find $BACKUP_DIR/ -name "{파일명}_*.tar.gz" -mtime {삭제주기} -exec rm -rf {} \;
# dmp, tar.gz 파일 find 후 삭제

expdp '{DB계정}/"{DB계정비밀번호}"' directory={1번에서 확인한 DIRECTORY_NAME} dumpfile={파일명}_$DATE.dmp
# expdp 덤프 파일 생성

#dmp file tar.gz
cd $BACKUP_DIR/
tar -zcvf {파일명}_$DATE.tar.gz {파일명}_$DATE.dmp
# tar 명령어로 덤프파일 묶고 gz형태로 압축

#delete dmp file
rm -rf $BACKUP_DIR/{파일명}_*.dmp
# 마지막에 압축되었으니 생성된 덤프파일은 삭제

#####
```
[https://zetawiki.com/wiki/리눅스_날짜_기준으로_파일_삭제하기](https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%82%A0%EC%A7%9C_%EA%B8%B0%EC%A4%80%EC%9C%BC%EB%A1%9C_%ED%8C%8C%EC%9D%BC_%EC%82%AD%EC%A0%9C%ED%95%98%EA%B8%B0)(보관주기/삭제 참고)

- 예시

```bash
#!/bin/bash
source /home/oracle/.bash_profile

FULLDATE=$(date '+%Y-%m-%d %H:%M:%S')
DATE=$(date +%Y%m%d)
BACKUP_DIR=/backup/backup_oracle_dump/atasset

#####ATASSET
find $BACKUP_DIR/ -name "ATASSET_*.dmp" -mtime +30 -exec rm -rf {} \;
find $BACKUP_DIR/ -name "ATASSET_*.tar.gz" -mtime +30 -exec rm -rf {} \;

expdp 'ATASSET/"KlH|bx*!2C56"' directory=ATASSET_DIR dumpfile=ATASSET_$DATE.dmp

#dmp file tar.gz
cd $BACKUP_DIR/
tar -zcvf ATASSET_$DATE.tar.gz ATASSET_$DATE.dmp

#delete dmp file
rm -rf $BACKUP_DIR/ATASSET_*.dmp

#####
```

### 3. 쉘 스크립트 실행권한 부여 (oracle계정으로 접속! not root계정)

- 쉘스크립트 경로로 이동 후 다음 명령어로 권한 부여

```bash
chmod +x {파일명}
```

- 예시

```bash
chmod +x 221.139.104.41_oracle_backup_script_PROD_CMS_ATASSET.sh
```

### 4. 크론 배치 설정 (root계정으로 접속! not oracle계정)

크론 설정 방법은 2가지가 있음

첫번째는 cron -I 와 같은 명령어 실행하는 방법

두번째는 /etc/crontab 을 직접 수정하는 방법

첫번째 방법은 명령어가 안먹힐 수 있으니 두번째 방법 이용 권장

1) crontab 경로로 이동 후 crontab 열기

```bash
cd /etc
vi crontab
```

2) crontab 수정

- 수정 양식

```bash
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# run-parts
01 * * * * root run-parts /etc/cron.hourly
02 4 * * * root run-parts /etc/cron.daily
22 4 * * 0 root run-parts /etc/cron.weekly
42 4 1 * * root run-parts /etc/cron.monthly

##Oracle(수수료 시스템)의 경우 보통 수수료 작업이 일어나지 않는 1-15일에는 세번 진행 1,7,15일
#보통 수수료 작업이 시작되는16-말일까지는 하루에 한번 진행
0 2 1,7,15 * * {생성한 계정} {쉘스크립트 경로}/{쉘스크립트명}.sh
0 2 16-31 * * {생성한 계정명} {쉘스크립트 경로}/{쉘스크립트명}.sh
```

- 예시

```bash
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# run-parts
01 * * * * root run-parts /etc/cron.hourly
02 4 * * * root run-parts /etc/cron.daily
22 4 * * 0 root run-parts /etc/cron.weekly
42 4 1 * * root run-parts /etc/cron.monthly

##Oracle(수수료 시스템)의 경우 보통 수수료 작업이 일어나지 않는 1-15일에는 세번 진행 1,7,15일
#보통 수수료 작업이 시작되는16-말일까지는 하루에 한번 진행
0 2 1,7,15 * * oracle /backup/backup_oracle_dump/atasset/backupScript/221.139.104.41_oracle_backup_script_PROD_CMS_ATASSET.sh
0 2 16-31 * * oracle /backup/backup_oracle_dump/atasset/backupScript/221.139.104.41_oracle_backup_script_PROD_CMS_ATASSET.sh
```

### 5. 로그 확인 (root계정으로 접속! not oracle계정)

보통 크론 수정 후 바로 1분 뒤에 크론 RELOAD 됨

```bash
cd /var/log   
tail -f cron
```

![stack](/assets/images/posts_img/dump/Untitled (1).png)


- - -

## MariaDB / Mysql DB

- 오라클과 다르게 root 계정으로 진행
- 과정은 모두 오라클과 같으나 쉘스크립트 구문만 다르다

### 쉘 스크립트 작성 구문

```bash
#!/bin/bash

FULLDATE=$(date '+%Y-%m-%d %H:%M:%S')
DATE=$(date +%Y%m%d)
BACKUP_DIR=/backup/backup_Dump/INSCLAIM

#####tesoro
find $BACKUP_DIR/ -name "insclaim_*.sql" -mtime +6 -exec rm -rf {} \;
find $BACKUP_DIR/ -name "insclaim_*.tar.gz" -mtime +6 -exec rm -rf {} \;

##데이터
mysqldump -uins_claim_app -pinsClaimApp@2021! ins_claim > backup/backup_Dump/INSCLAIM/insclaim_data_$(date +%Y%m$d).sql

##프로시저/트리거/펑션
mysqldump -uins_claim_app -pinsClaimApp@2021! -n -d -t --routines --triggers ins_claim > /backup/backup_Dump/INSCLAIM/insclaim_$(date +%Y%m%d).sql

#file tar.gz
cd $BACKUP_DIR/
tar -zcvf insclaim_$DATE.tar.gz insclaim_data_$DATE.sql insclaim_$DATE.sql

#delete sql file
rm -rf $BACKUP_DIR/insclaim_*.sql
#####./
```

### dump 명령어 양식

- mariaDB에서도 mysqldump 명령어 사용가능

```bash
$> mysqldump -u[사용자아이디] -p[패스워드] 데이터베이스명 > 저장될 파일명

```

- 예시

```bash
mysqldump -ukamkami -p[패스워드] mydatabase > kamkami.pe.kr.sql
```

### dump 명령어에서 계정, 패스워드 작성 시 주의점

```bash
-u계정명 -p패스워드  (O)
-u 계정명 -p 패스워드 (X)
```

### 만약 LOCK 권한 문제로 실패한다면?

**dump 명령어에  -- single -transaction 추가해야 함**

```bash
mysqldump --single-transaction -uins_claim_app -pinsClaimApp@2021! ins_claim > backup/backup_Dump/TEST.sql
```