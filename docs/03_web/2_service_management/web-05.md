# WEB-05: 지정하지 않은 CGI/ISAPI 실행 제한

**분류**: 03_web

**중요도**: 상

---

03. 웹 서비스

웹 서비스 > 2. 서비스 관리

## 개요

### 점검 내용

웹 서비스 CGI 실행 제한 설정 여부 점검

### 점검 목적

CGI 스크립트를 정해진 디렉터리에서만 실행되도록 하여 악의적인 파일의 업로드 및 실행을 방지하기 위함

### 보안 위협

게시판이나 자료실과 같이 업로드되는 파일이 저장되는 디렉터리에 CGI 스크립트가 실행 가능한 경우 악의적인 파일을 업로드하고 이를 실행하여 시스템의 중요 정보가 노출될 수 있으며 침해사고의 경로로 이용될 위험이 존재함

### 참고

!!! info "CGI(Common Gateway Interface)"
    사용자가 서버로 보낸 데이터를 서버에서 작동 중인 데이터

처리프로그램에 전달하고, 여기에서 처리된 데이터를 다시 서버로 되돌려 보내는 등의 일을 하는 표준 인터페이스 규격

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, IIS, WebtoB

### 판단 기준

**✅ 양호**: CGI 스크립트를 사용하지 않거나 CGI 스크립트가 실행 가능한 디렉터리를 제한한 경우

**❌ 취약**: CGI 스크립트를 사용하고 CGI 스크립트가 실행 가능한 디렉터리를 제한하지 않은 경우

## 조치 방법

CGI 스크립트를 정해진 디렉터리 내에서만 실행할 수 있도록 설정

### 조치 시 영향

해당 디렉터리 확인 후 추가적인 파일이 없다면 영향 없음

## 점검 및 조치 사례

### Apache

**Step 1) apache 설정 파일 내 CGI 모듈 비활성화 또는 주석 처리**

#  vi /<Apache 설치 디렉터리>/httpd.conf(또는 apache.conf) #LoadModule cgi_module modules/mod_cgi.so #LoadModule cgid_module modules/mod_cgid.so

**Step 2) apache 설정 파일 내 설정된 모든 디렉터리의 Options 지시자에서 ExecCGI 옵션 제거**

# vi /<Apache 설치 디렉터리>/apache.conf(또는 httpd.conf) <Directory "/var/www/cgi-bin"> Options -ExecCGI </Directory>

**Step 3) Apache 재시작**

### Tomcat

**Step 1) web.xml 파일 내 CGI 매핑 비활성화**

<!-- <servlet-mapping> <servlet-name>cgi</servlet-name> <url-pattern>/cgi-bin/*</url-pattern> </servlet-mapping> -->

**Step 2) Tomcat 재시작**

### Nginx

**Step 1) nginx.conf 파일 내 Fastcgi 사용 여부 확인**

# cat /<Nginx 설치 디렉터리>/conf/nginx.conf location ~ \.cgi$ { #fastcgi_pass <FastCGI 서버 주소>:<FastCGI 서버 통신 포트>; #include fastcgi_params; }

**Step 2) Nginx 재시작**

### IIS

**Step 1) CGI 디렉터리 설정 해제**

IIS 관리자 > 서버 선택 > ISAPI 및 CGI 제한 > 기능 열기 > 작업 > 기능 설정 편집 > 사용하지 않는 CGI/ISAPI 모듈 설정 해제

**[ CGI/ISAPI 모듈 설정 확인 ]**

03. 웹 서비스

### WebtoB

**Step 1) http.m 파일 내 활성화되어 있는 *SVRGROUP, *SERVER, *URI 절에서 CGI 옵션 제거 또는 비활성화**

# vi /<WebtoB 설치 디렉터리>/config/http.m“ *SVRGROUP htmlg SVRTYPE = HTML #cgig SVRTYPE = CGI ssig SVRTYPE = SSI jsvg SVRTYPE = JSV *SERVER #cgi SVGNAME = cgig, MinProc = 2, MaxProc = 10, ASQCount = 1 ssi SVGNAME = ssig, MinProc = 2, MaxProc = 10, ASQCount = 1 MyGroup SVGNAME = jsvg, MinProc = 20, MaxProc = 20 *URI #uri1 Uri = "/cgi-bin/",   Svrtype = CGI

**Step 2) 설정 파일 컴파일 및 재구동**

# wscfl -I http.m # wsdown # wsboot

!!! info "필요한 경우 해당 디렉터리만 제한적으로 CGI 스크립트 실행 설정"

