# WEB-19: 웹 서비스 SSI(Server Side Includes) 사용 제한

**분류**: 03_web

**중요도**: 중

---

03. 웹 서비스

웹 서비스 > 3. 보안 설정

## 개요

### 점검 내용

웹 서비스 SSI 사용 제한 설정 여부 점검

### 점검 목적

웹 서비스 내 SSI 사용을 제한하여 불법적인 데이터 접근을 차단하여 웹 서버의 보안을 강화하기 위함

### 보안 위협

Ÿ 웹 서비스 내 SSI 사용을 제한하지 않을 경우, 공격자가 SSI 기능을 이용하여 시스템 명령 실행 및 중요 파일 탈취 등 공격이 가능하며, 이를 통해 서버 시스템 침해, 데이터 유출 등이 발생할 위험이 존재함 Ÿ SSI 공격 시 HTML 페이지에 스크립트를 삽입하거나 원격으로 코드를 실행하여 웹 서비스를 악용할 위험이 존재함

### 참고

!!! info "SSI(Server-Side Includes)"
    CGI 프로그램을 작성하거나 혹은 서버 사이드 스크립트를 사용하는

언어로, 웹 서버가 사용자에게 페이지를 제공하기 전에 구문을 해석하도록 지시하는 역할을 함

!!! info "SSI(Server-Side Includes) Injection"
    HTML 문서 내 입력받은 변수값을 서버 측에서 처리할 때

부적절한 명령문이 포함 및 실행되어 서버의 데이터가 유출되는 취약점

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, IIS, WebtoB

### 판단 기준

**✅ 양호**: 웹 서비스 SSI 사용 설정이 비활성화되어 있는 경우

**❌ 취약**: 웹 서비스 SSI 사용 설정이 활성화되어 있는 경우

## 조치 방법

웹 서비스 내 불필요한 SSI 사용 제한 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Apache

**Step 1) Options 지시자 Includes 옵션 확인**

# vi /[Apache 설치 디렉터리]/conf/httpd.conf(또는 /conf/apache.conf) <Directory /> Options Includes </Directory>

**Step 2) Options 지시자 Includes 옵션 제거**

# vi /[Apache 설치 디렉터리]/conf/httpd.conf(또는 /conf/apache.conf) <Directory /> Options </Directory>

### Tomcat

**Step 1) web.xml 파일 내 SSI 서블릿 또는 필터 사용 설정 확인**

#cat /[Tomcat 설치 디렉터리]/tomcat-users.xml | grep ‘SSIServlet\|SSIFilter’ <servlet-mapping> <servlet-name>SSIServlet</servlet-name> <url-pattern>*.shtml</url-pattern> </servlet-mapping> 또는 <filter-mapping> <filter-name>SSIFilter</filter-name> <url-pattern>*.shtml</url-pattern> </filter-mapping>

**Step 2) web.xml 파일 내 SSI 서블릿 및 필터 설정 삭제 또는 주석 처리**

**Step 3) web.xml 파일 내에서 SSI와 관련한 불필요 mapping 제거 또는 주석 처리**

**Step 4) Tomcat 서비스 재구동**

# systemctl restart tomcat

### Nginx

**Step 1) nginx.conf 파일 내 SSI 옵션 사용 여부 확인**

# cat /[Nginx 설치 디렉터리]/conf/nginx.conf location / { ssi on; }

**Step 2) nginx.conf 파일 내 모든 디렉터리의 SSI 옵션 설정**

# vi /[Nginx 설치 디렉터리]/conf/nginx.conf location / { ssi off; }

### IIS

**Step 1) 매핑 확장자 확인**

03. 웹 서비스

인터넷 정보 서비스(IIS) 관리자 > 서버 선택 > IIS > “처리기 매핑” 선택, .shtml, .shtm, .stm 확장자 매핑 확인(존재할 경우 취약)

**[ 처리기 매핑 확장자 확인 ]**

**Step 2) .shtml, .shtm, .stm과 매핑되는 항목 제거**

**[ 처리기 확장자 삭제 ]**

### WebtoB

**Step 1) http.m 파일 내 SSI 서버 연결 설정 확인**

# vi /[WebtoB 설치 디렉터리]/conf/http.m *SVRGROUP ssig         NodeName = mynode, SvrType = SSI *SERVER ssi          SvgName = ssig, MinProc = 10, MaxProc = 10

**Step 2) SSI 서버 설정 삭제**

