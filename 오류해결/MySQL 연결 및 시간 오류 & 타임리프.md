### Ubuntu MySQL 설치

배포를 하던 도중 다음과 같은 에러를 만났다

```
com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure
The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.
```

mysql 연결 문제라 생각해서 이렇게 저렇게 구글링을 해 봤지만 우분투에 mysql을 설치해주니 해결되었다

1. ubuntu 패키지 정보 업데이트
   - sudo apt update
2. mysql 설치
   - sudo apt install mysql-server
3. mysql 설치 확인
   - dpkg -l | grep mysql-server
4. mysql 실행여부 확인
   - sudo netstat -tap | grep mysql
   - netstat 에러 발생시 net-tools 설치
     - sudo apt isntall net-tools
5. mysql 외부 접속 설정
   - sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
   - 파일 안 bind-address를 주석 처리

### mysql Timezone 설정

로컬에서는 시간이 잘 나오던게 운영서버에서는 9시간 이르게 나오는 문제가 발생했다, 타임존 설정을 해주니 해결되었다

- datasource.url 에 쿼리파라미터 serverTimezone=Asia/Seoul 추가
- main 메서드 안쪽에 TimeZone.setDefault(TimeZone.getTimeZone("Asia/Seoul")); 추가

### Thymeleaf 

운영 서버는 url에 민감하여 경로를 생략하거나 잘못 쓰게되면 Error resolving template 에러가 발생한다

- 경로가 templates/boards/list 인 경우
- 개발 서버에서는 boards/list 로 썼던걸 ./boards/list.html 이런식으로 경로와 확장자를 정확히 명시해줘야 한다

