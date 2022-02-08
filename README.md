## docker에서 mysql 세팅 과정

환경 : window10, hyper-v 활성화, wsl 2 설치

```
docker -v
```

![image](https://user-images.githubusercontent.com/58525009/152751418-de986074-10ca-433c-8f8a-949b89c91ebf.png)

```
docker pull mysql
```

![image](https://user-images.githubusercontent.com/58525009/152751471-a46202a0-621f-4a31-b300-b2c403af17f1.png)

```
docker pull mysql:5.7
```

![image](https://user-images.githubusercontent.com/58525009/152761216-83c7eb1a-f474-4b56-b44c-02d9eb81b2ed.png)

```
docker images
```

![image](https://user-images.githubusercontent.com/58525009/152751579-c0956189-b05a-491e-8fde-b972f83da0f8.png)

mysql-container은 원하는 컨테이너 이름으로 설정한다.

<password>도 원하는 비밀번호를 입력하여 설정한다.

container을 utf8로 설정한다.

```
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<password> -d -p 3306:3306 mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

컨테이너 접속

```
docker exec -it mysql-pc-manager-contianer bash
```

mysql 서버로 접속한다.

```
mysql -u root -p
```

비밀번호를 입력하면 다음과 같이 접속할 수 있다.

![image](https://user-images.githubusercontent.com/58525009/152754577-3a5c37c3-99d1-40b5-94aa-a7c4739e254b.png)

명령어를 입력하여 mysql 서버로 접속한다.

```
mysql -u root -p
```

![image](https://user-images.githubusercontent.com/58525009/152761767-35a0c835-bab8-4815-b9d3-62e058fc36fc.png)

### 클라이언트 utf8 설정

container를 utf8로 설정하였지만 container 환경내에 여러 클라이언트 중 mysql도 utf8로 따로 설정해주어야 한다.

![image](https://user-images.githubusercontent.com/58525009/152752333-accbd63b-e4cc-4d76-8d26-6d118967ace3.png)

위에서 bash명령어로 컨테이너를 실행하였는데 docker desktop에서도 가능하다.

원하는 컨테이너의 2번째 아이콘(>\_)을 눌러 cli를 연다.

해당 디렉토리로 이동한다.

```
cd ./etc/mysql/confg.d
```

다음을 입력하여 파일을 생성한다.

```
vi utf8.cnf
```

vi를 사용할 수 없다면 다음 명령어로 설치한다.

```
apt-get update
```

```
apt-get install vim
```

다음을 입력하고 :wq 명령어로 저장한다.

```
# for utf8 characterset

[client]
default-character-set = utf8

[mysqld]
init_connect = SET collation_connection = utf8_general_ci
init_connect = SET NAMES utf8
character-set-server = utf8
collation-server = utf8_general_ci

[mysqldump]
default-character-set = utf8

[mysql]
default-character-set = utf8
```

마지막에 EOF를 작성하라는 글도 있었지만 mysql 서버 접속시 알수없는 --EOF라는 문구가 떠서 제외하고 입력하였다.

명령어를 입력하여 mysql 서버로 접속할 수 있다.

```
mysql -u root -p
```

status를 입력하여 utf8로 변경됨을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/58525009/152752956-ff37c766-9c67-45c1-b727-71b6d1fbae4f.png)

- 참고

[Docker를 사용하여 MySQL 설치하고 접속하기](https://poiemaweb.com/docker-mysql)

[맥 docker에 mysql 설치 & character set 설정 locale 설정](https://velog.io/@kyukim/docker-mysql)

![image](https://user-images.githubusercontent.com/58525009/152855189-eeec4015-8ae8-480a-ad29-394cd4560541.png)

맨 끝에는 password를 입력한다.

![image](https://user-images.githubusercontent.com/58525009/152855243-6d788d47-d6ed-421c-b756-9c70429af654.png)

![image](https://user-images.githubusercontent.com/58525009/152855290-7a05fd5d-aae8-4ff4-a4f8-a4525dab8094.png)

![image](https://user-images.githubusercontent.com/58525009/152855325-bab2f55e-2828-4c1d-8a33-10897efc89ac.png)

![image](https://user-images.githubusercontent.com/58525009/152855356-44beed35-3348-4086-9df0-839dc4c8a10e.png)

```
cd /etc/mysql/mysql.conf.d
```

```
# bind-adress=127.0.0.1
```

mysql.conf.d에 접근하여 mysqld.cnf에서 위와 같이 주소를 주석처리하고 새로운 컨테이너 run할때에 8080:3306으로 하였지만 원격접속이 되지 않아서 local host로 진행하였다.
포트포워딩 하는 법을 더 찾아봐야겠지만 시간이 너무 오래걸려서 추후 과제로 남겼다.

### 추가할 사항

- 포트포워딩
