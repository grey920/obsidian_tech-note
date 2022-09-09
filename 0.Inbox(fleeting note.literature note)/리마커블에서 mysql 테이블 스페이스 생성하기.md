## 날짜 : 2022-08-29 22:53

## 주제 : #server #db #mysql 
----
## 메모
>
```sql
-- ------------------------------------------------------------

-- mysql 접속

-- ------------------------------------------------------------

mysql -u root -p      /* root 사용자로 MySQL 접속 */

-- ------------------------------------------------------------

-- 데이터베이스 생성

-- ------------------------------------------------------------

CREATE DATABASE REMARKABLE CHARACTER SET utf8 COLLATE utf8_bin;

-- ------------------------------------------------------------

-- 사용자 생성

-- ------------------------------------------------------------
 /* 로컬 접속 계정 생성 */
CREATE USER week@localhost identified by 'week';

/* REMARKABLE 이라는 데이터베이스의 모든 테이블, 모든 로컬에서만 허용*/
GRANT ALL PRIVILEGES ON REMARKABLE.* TO 'week'@'localhost' IDENTIFIED BY 'week';

/* REMARKABLE 이라는 데이터베이스의 모든 테이블, 모든 IP 접속 허용*/
GRANT ALL PRIVILEGES ON REMARKABLE.* TO 'week'@'%' IDENTIFIED BY 'week';
FLUSH PRIVILEGES;    /* 권한 적용 */
```
[remarkablesoft](https://github.com/remarkablesoft)/**[remarkable-framework-web](https://github.com/remarkablesoft/remarkable-framework-web)**


위의 명령어를 DB서버에서 실행한다.
> 리마커블의 mysql 서버는 175.126.82.216 를 사용한다.


### database 확인하기
```bash
[root@q561-0564 ~]# mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4528292
Server version: 10.4.12-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| CYBER_TRADE        |
| EPAPYRUS           |
| GW_CHILDREN        |
| HERELAW            |
| HERELAW_DEV        |
| ROLTECH_OTP2       |
| WEEKLY_REPORT      |
| WITH_YOUTH         |
| engine_db          |
| everysearch        |
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
14 rows in set (0.001 sec)

MariaDB [(none)]>
```
root 계정으로 접속 후 `show databases;` 를 입력하면 현재 사용가능한 데이터베이스 리스트를 확인할 수 있다.


### 데이터베이스 선택
```bash
MariaDB [test]> use WEEKLY_REPORT
Database changed
MariaDB [WEEKLY_REPORT]>
```

`use 데이터베이스명;` 을 입력하여 DB를 변경한다

### 테이블 확인
```bash
MariaDB [WEEKLY_REPORT]> show tables;
Empty set (0.001 sec)
```
`show tables;` 명령어로 현재 WEEKLY_REPORT 데이터베이스의 테이블을 확인한다



## 출처(참고문헌)
- 리눅스 mysql 설치 및 권한 설정 :  https://continuous-development.tistory.com/5

## 연결문서
- 
