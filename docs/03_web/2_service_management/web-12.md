# WEB-12: 웹 서비스 링크 사용 금지

**분류**: 03_web

**중요도**: 중

---

03. 웹 서비스

웹 서비스 > 2. 서비스 관리

## 개요

### 점검 내용

웹 서비스 링크(심볼릭 링크, aliases 등) 사용 제한 여부 점검

### 점검 목적

무분별한 심볼릭 링크, 별칭(aliases) 등을 제거하여 허용하지 않은 경로에서의 접근을 차단해 경로 검증을 우회한 시스템 파일 접근을 방지하기 위함

### 보안 위협

Ÿ 보안상 민감한 내용이 포함되어 있는 파일이 악의적인 사용자에게 노출될 경우 침해사고로 이어질 위험이 존재함 Ÿ 접근을 허용한 웹 디렉터리 내에 서버의 다른 디렉터리나 파일들에 접근할 수 있는 심볼릭 링크, aliases, 바로가기 등이 존재하는 경우 해당 링크를 통해 허용하지 않은 다른 디렉터리에 액세스할 수 있는 위험이 존재함

### 참고

!!! info "심볼릭 링크(Symbolic link, 소프트 링크)"
    사용자가 심볼릭 링크 파일을 요청하면, 시스템이 해당

링크에 저장된 대상 경로를 따라가서 실제 원본 데이터를 가져와 전달

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, IIS, JEUS, WebtoB

### 판단 기준

**✅ 양호**: 심볼릭 링크, aliases, 바로가기 등의 링크 사용을 허용하지 않는 경우

**❌ 취약**: 심볼릭 링크, aliases, 바로가기 등의 링크 사용을 허용하는 경우

## 조치 방법

웹 서비스 링크 사용 제한 설정

### 조치 시 영향

심볼릭 링크를 이용하여 웹페이지가 구성된 경우 해당 서비스가 실행되지 않을 수 있음

## 점검 및 조치 사례

### Apache

**Step 1) apache.conf(또는 /conf/httpd.conf) 파일 내 Options 지시자 FollowSymLinks 옵션 제거**

<Directory /> Options –FollowSymLinks #Options Indexes FollowSymLinks </Directory>

### Tomcat

**Step 1) server.xml 파일 내 Context 요소 allowLinking 옵션 설정**

<Context allowLiking=“true“> <WatchedResource>WEB-INF/web.xml</WatchedResource> <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource> <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource> </Context>

### Nginx

**Step 1) nginx.conf 파일 내 설정된 모든 디렉터리의 disable_symlinks on 설정(기본값 : 설정값 없음)**

location / { root   html; index  index.html index.htm; disable_symlinks on; }

### IIS

**Step 1) 홈 디렉터리 바로가기 파일 확인**

제어판 > 관리 도구 > 인터넷 정보 서비스(IIS) 관리자 > 해당 웹사이트 > 기본 설정 > ‘실제 경로’로 설정된 경로로 이동 > 바로가기 파일 확인 후 제거

**[ 홈 디렉터리 위치 확인 ]**

03. 웹 서비스

### JEUS

**Step 1) jeus-web-dd.xml 파일 내 alias 요소 설정 제거**

<aliasing> <alias> <alias-name>/images/</alias-name> <real-path>/home/web/images/</real-path> </alias> </aliasing>

### WebtoB

**Step 1) http.m 파일 내 ALIAS 절 요소 설정 제거**

# cat /[WebtoB 설치 디렉터리]/config/http.m | grep -C 2 ALIAS *ALIAS alias1 URI = "/cgi-bin/", RealPath = "/home/tmax/webtob/cgi-bin/"

