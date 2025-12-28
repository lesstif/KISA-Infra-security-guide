# WEB-13: 웹 서비스 설정 파일 노출 제한

**분류**: 03_web

**중요도**: 상

---

웹 서비스 > 2. 서비스 관리

## 개요

### 점검 내용

DB 연결 파일에 대한 접근 권한, 스크립트 매핑 등 웹 서비스에서의 DB 연결 취약점 존재 여부 점검

### 점검 목적

웹 서비스에서 DB 연결 파일에 대한 접근 권한 제한 및 불필요한 스크립트 매핑을 제거하여, DB 연결 정보(사용자 이름, 비밀번호 등)가 외부에 노출되거나 공격자의 DB 접근 및 관리자 권한 획득 등의 다양한 공격을 방지하기 위함

### 보안 위협

웹 서비스에서 DB 연결 파일에 대한 접근 권한 제한 및 불필요한 스크립트 매핑을 제거하지 않을 경우, DB 연결 파일에 존재하는 데이터베이스 관련 정보(IP주소, DB명, 비밀번호), 서버 내부 IP주소, 웹 서비스 환경설정 정보 등 보안상 민감한 내용이 악의적인 사용자에게 노출될 위험이 존재함

### 참고

-

## 점검 대상 및 판단 기준

### 대상

Tomcat, IIS, JEUS

### 판단 기준

**✅ 양호**: 일반 사용자의 DB 연결 파일에 대한 접근을 제한하고, 불필요한 스크립트 매핑이 제거된 경우

**❌ 취약**: 일반 사용자의 DB 연결 파일에 대한 접근을 제한하지 않거나, 불필요한 스크립트 매핑이 제거되지

않은 경우

## 조치 방법

DB 연결 파일에 대한 접근 권한 제한 또는 불필요한 스크립트 매핑 제거 등을 통한 웹 서비스 내 DB 연결 취약점 제거 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Tomcat

**Step 1) server.xml 파일 내 불필요한 DB 연결 리소스 설정 제거**

<GlobalNamingResources> <Resource name=“jdbc/MyDB” auth=“Container” type=“javax.sql.DataSource” maxTotal=“100” maxIdle=“30” maxWaitMillis=“10000” username=“dbuser”

03. 웹 서비스

password=“dbpassword” driverClassName=“com.mysql.jdbc.Driver” url=“jdbc:mysql://localhost:3306/mydb”/> </GlobalNamingResources>

**Step 2) DB 연결 리소스가 존재하는 설정 파일 접근권한을 600으로 설정**

# chmod 600 /[Tomcat 설치 디렉터리]/conf/server.xml

### IIS

**Step 1) 처리기 매핑에서 불필요한 DB 매핑 설정 제거**

제어판 > 관리 도구 > 인터넷 정보 서비스(IIS) 관리자 > 해당 웹사이트 > IIS > 처리기 매핑 선택, 사용 항목에 *.asa/*.asax 항목 제거

**[ asa/asax 스크립트 매핑 확인 ]**

**Step 2) 요청 필터링에서 DB 매핑 설정제한**

“허용됨” 값이 true인 매핑 제거 및 “파일 이름 확장명 거부”에 등록

**[ asa/asax 파일 필터링 확인 ]**

!!! info "asa/asax 스크립트 매핑 또는 파일 필터링 중 하나라도 설정 시 취약"

### JEUS

**Step 1) 설정 파일 내 불필요 DB 연결 리소스 설정 제거**

#vi /[JEUS 설치 디렉터리]/conf/domain.xml <datasource> <jndi-name>jdbc/UnnecessaryDB</jndi-name> <driver-class>com.example.Driver</driver-class> <url>jdbc:example://localhost:1234/unnecessarydb</url> <username>user</username> <password>password</password> <max-connections>10</max-connections> </datasource>

**Step 2) 설정 파일의 접근권한을 600으로 설정**

# chmod 600 /[JEUS 설치 디렉터리]/conf/domain.xml

!!! info "domain.xml 뿐 아니라 개별 웹 서비스의 설정 파일인 jeus-web-dd.xml에서도 삭제 필요"

