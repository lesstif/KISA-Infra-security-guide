# WM: Web Application(웹)

**분류**: 10_webapp

**중요도**: 상

---

10. Web Application(웹) 보안

21. 불필요한 Method 악용

## 개요

### 점검 내용

PUT, DELETE, TRACE 등 불필요한 HTTP 메소드의 악용 여부 확인

### 점검 목적

불필요한 HTTP 메소드(PUT, DELETE, TRACE 등) 요청을 제한하여 서버의 무단 접근 및 악의적인 행위(임의 파일 생성, 데이터 삭제 등)를 방지하기 위함

### 보안 위협

PUT, DELETE, TRACE 등의 메소드가 활성화되어 있는 것만으로는 취약 여부를 판단하기 어려우나, 해당 메소드들이 악용될 경우, 조작된 Server Side Script 파일 업로드, 민감 데이터 삭제, 서버 정보 노출 등이 가능함

### 참고

!!! info "Server Side Script"
    웹에서 사용되는 스크립트 언어 중 서버 측에서 실행되는 스크립트

!!! info "XST(Cross-Site Tracing)"
    TRACE 메소드를 악용하여 httpOnly 등으로 보호된 세션 및 쿠키를

탈취할 수 있는 공격 기법

## 점검 대상 및 판단 기준

### 대상

웹 애플리케이션 서버

### 판단 기준

**✅ 양호**: 웹 애플리케이션에 불필요한 메소드(PUT, DELETE, TRACE, CONNECT 등)로 변조한 요청

패킷 전송 시 해당 메소드를 통하여 임의의 행위로부터 악용이 제한되는 경우

**❌ 취약**: 웹 애플리케이션에 불필요한 메소드(PUT, DELETE, TRACE, CONNECT 등)로 변조한 요청

패킷 전송 시 해당 메소드를 통하여 임의의 행위에 대한 악용이 가능한 경우

## 조치 방법

WEB/WAS의 설정파일을 변경하여 불필요한 메소드의 사용을 제한

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

**점검 방법**

**Step 1) PUT 메소드를 사용하여 악성 파일(Server Side Script, 악성 스크립트 등) 생성 시도**

**[ 프록시 도구를 이용한 임의 파일 생성 시도 ]**

**Step 2) 생성된 악성 파일을 이용하여 웹쉘(Webshell) 업로드, 크로스사이트 스크립팅 등의 공격 수행 가능**

**[ 생성 파일 실행 유무 확인 ]**

**Step 3) DELETE 메소드를 사용하여 서버에 저장된 임의의 데이터 삭제 가능**

**[ 프록시 도구를 이용한 임의 파일 삭제 시도 ]**

10. Web Application(웹) 보안

**조치 방법**

1. PUT, DELETE 메소드를 이용해 파일 생성/삭제가 가능한 WebDAV(Web Distributed Authoring and

Versioning) HTTP 확장 프로그램은 비활성화를 권장하며, 부득이하게 사용해야할 경우 망 분리 되어있는 내부망에서 적절한 인증 절차를 거쳐 운용

2. RESTful API 애플리케이션 상의 PUT, DELETE 메소드의 경우 기본적으로 취약하게 설계된 메소드가

아니며, POST 메소드를 통하여 구현하는 것과 동일하게 핵심 비즈니스 로직에 대한 시큐어 코딩을 구현

3. 불필요하게 활성화된 메소드는 비활성화하여 잠재적인 취약점의 최소화를 권장

(웹 서버 설정 파일 구성은 리눅스 OS 내에서도 종류 및 버전에 따라 차이가 있으므로 개발 환경에 맞게 조치)

### Apache

!!! info "명령어를 통한 dav 및 dav_fs 모듈 비활성화(WebDAV)"

WebDAV 기능 비활성화 sudo a2dismod dav sudo a2dismod dav_fs

!!! info "apache2.conf 파일 수정을 통한 WebDAV 서비스 비활성화"

PUT, DELETE 메소드 비활성화 (WebDAV 서비스 비활성화) /etc/apache2/apache2.conf (예시 :Apache/2.4.52-ubuntu) ... <Directory "/var/www/html"> Dav On #On으로 활성화 되어있을 경우 Off 또는 지시어 삭제로 비활성 조치 Options Indexes FollowSymLinks AllowOverride None Require all granted <LimitExcept GET POST PUT DELETE OPTIONS> #PUT, DELETE 메소드 사용이 허용되어 있을 경우, #PUT, DELETE 부분을 삭제하여 해당 메소드 사용을 금지 Order Allow,Deny Deny from all </LimitExcept> </Directory> ...

!!! info "Apache의 경우 TRACE 메소드가 기본적으로 활성화 되어있음"

!!! info "데비안 계열은 /etc/apache2/apache2.conf 파일 내 TraceEnable Off 지시어를 추가하는 것으로 비활성화 조치"

(2.4.X 버전부터 /etc/apache2/conf-available/security.conf 파일에서 TRACE 사용을 별도로 관리하며, 기본적으 로 비활성화 되어있음) TRACE 메소드 비활성화 # /etc/apache2/apache2.conf (예시 :Apache/2.2.8-ubuntu) ... TraceEnable Off # 파일 내용 하단에 추가 ...

!!! info "레드햇 계열의 경우 httpd.conf 파일 수정을 통해 TRACE 메소드 비활성화 조치"

TRACE 메소드 비활성화 # /etc/httpd/conf/httpd.conf (예시: Apache/2.4.6-CentOS) ... TraceEnable Off # 파일 내용 하단에 추가 ...

!!! info "mod_rewrite 모듈을 이용한 CONNECT 메소드 비활성화"

CONNECT 메소드 비활성화 # /etc/apache2/sites-available/000-default.conf (예시: Apache/2.4.52-ubuntu) ... <VirtualHost *:80> ... <IfModule mod_rewrite.c> RewriteEngine On RewriteCond %{REQUEST_METHOD} ^CONNECT #CONNECT 비활성화 RewriteRule .* - [F] </IfModule> ... </VirtualHost>

10. Web Application(웹) 보안

### Tomcat

!!! info "WebDAV 서비스 활성화 시 PUT, DELETE 메소드를 이용하여 임의의 파일 생성 및 삭제 가능"

PUT, DELETE 메소드 비활성화 (WebDAV 서비스 비활성화) // 아래 코드를 제거하여 WebDAV 서비스 비활성화 또는 readonly 옵션을 true로 설정하여 쓰기 권한 제거 <servlet> <servlet-name>webdav</servlet-name> <servlet-class>org.apache.catalina.servlets.WebdavServlet</servlet-class> <init-param> <param-name>debug</param-name> <param-value>1</param-value> </init-param> <init-param> <param-name>listings</param-name> <param-value>false</param-value> </init-param> <init-param> <param-name>readonly</param-name> <param-value>true</param-value> // true로 설정하여 쓰기 권한 제거 </init-param> <load-on-startup>1</load-on-startup> </servlet>

!!! info "server.xml 설정 파일 내 allowTrace="true" 설정 값을 제거하여 TRACE 메소드 비활성화"

TRACE 메소드 비활성화 // <Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" maxParameterCount="1000" allowTrace="true" // 설정 값 제거 />

!!! info "Tomcat의 경우 설계 목적이 웹 애플리케이션 서버임에 따라, 프록시 서버에서 주로 사용되는 CONNECT 메"

소드를 기본적으로 지원하지 않음

### Nginx

!!! info "WebDAV 서비스 활성화 시 PUT, DELETE 메소드를 이용하여 임의의 파일 생성 및 삭제 가능"

!!! info "/etc/nginx/sites-available/default 설정 파일 수정(환경"
    nginx/1.18)

PUT, DELETE 메소드 비활성화 (WebDAV 서비스 비활성화) ... location /dav/ { ... dav_methods PUT DELETE MKCOL COPY MOVE; #dav_methods 지시어 부분 삭제 ... }

!!! info "Nginx는 보안상의 이유로 0.5.17버전 이후로 TRACE 메소드를 항상 405 응답 코드로 거부하도록 패치되었으"

며, 기본적으로 포워드 프록시 기능을 지원하지 않으므로 CONNECT 메소드를 허용하지 않음

### IIS 5.0 이하

!!! info "IIS 5.0의 경우 기본적으로 WebDAV 기능이 활성화 되어있어 PUT/DELETE 등의 메소드를 통하여 임의적인"

파일 생성 삭제가 가능하므로 WebDAV 기능을 비활성화하거나 서비스 중인 애플리케이션의 디렉터리 내 '쓰 기' 권한을 제거하여 PUT/DELETE를 통한 파일 생성 삭제 제한 가능

!!! info "레지스트리 편집기(Regedt32.exe) → HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3S"

VC\Parameters → 편집 → 값 추가 → 값 이름: DisableWebDAV, 데이터형식: REG_DWORD, 데이터: 1 → II S 재시작

**[ 생성 파일 실행 유무 확인 ]**

10. Web Application(웹) 보안

!!! info "웹 애플리케이션 홈 디렉터리 또는 가상 디렉터리 내 쓰기 권한 제거"

**[ 디렉터리 쓰기 권한 제거 ]**

### IIS 6.0 이상

!!! info "IIS 6.0 이상의 버전의 경우 IIS 웹 서버 설치 시 WebDAV 기능이 비활성화 되어있으며, OPTIONS/TRACE/G"

ET/HEAD/POST 메소드가 기본적으로 활성화되어있음

!!! info "TRACE 메소드 비활성화를 위하여 요청 필터링 → HTTP 동사 → 동사 거부 메뉴에 TRACE 메소드 추가"

**[ IIS 관리자 요청 필터링 메뉴 접근 ]**

**[ 동사 거부 설정 내 TRACE 메소드 추가 ]**

가상화 장비 Chapter 11

1. 계정 관리················································································································790

2. 시스템 서비스 관리··································································································812

3. 가상 머신 관리·········································································································837

4. 가상 네트워크 관리··································································································845

가상화 장비 취약점 분석 · 평가 항목

1. 계정 관리

점검항목 항목 중요도 항목코드 계정 로그오프/세션 관리 상 HV-01 가상화 장비 외부접속 차단 상 HV-02 가상화 장비 루트계정 관리 상 HV-03 가상화 장비 계정 권한 관리 상 HV-04 가상화 장비 사용자 인증 강화 상 HV-05 비밀번호 관리정책 설정 상 HV-06 계정 잠금 임계값 설정 상 HV-07

2. 시스템 서비스 관리

점검항목 항목 중요도 항목코드 시스템 사용 주의사항 출력 설정 중 HV-08 NTP 및 시각 동기화 설정 중 HV-09 SNMP Community String 복잡성 적용 중 HV-10 MOB(Managed Object Browser) 서비스 비활성화 상 HV-11 ESXi Shell 비활성화 상 HV-12 ESXi Shell 세션 종료 시간 설정 상 HV-13 원격 로그 서버 이용 중 HV-14 시스템 주요 이벤트 로그 설정 상 HV-15 비휘발성 경로 내 로그 파일 저장 상 HV-16

11. 가상화 장비

3. 가상 머신 관리

점검항목 항목 중요도 항목코드 코어덤프 수집 기능 활성화 상 HV-17 가상 머신의 장치 변경 제한 설정 상 HV-18 가상 머신의 불필요한 장치 제거 상 HV-19 가상 머신 콘솔 클립보드 복사&붙여넣기 기능 비활성화 상 HV-20 가상 머신 콘솔 드래그 앤 드롭 기능 비활성화 상 HV-21

4. 가상 네트워크 관리

점검항목 항목 중요도 항목코드 가상 스위치 MAC 주소 변경 정책 비활성화 상 HV-22 가상 스위치 무차별(Promiscuous) 모드 정책 비활성화 상 HV-23 가상 스위치 위조전송(Forged Transmits) 모드 정책 비활성화 상 HV-24 주기적 보안 패치 및 벤더 권고사항 적용 상 HV-25

