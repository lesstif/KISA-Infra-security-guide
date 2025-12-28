# WEB-10: 불필요한 프록시 설정 제한

**분류**: 03_web

**중요도**: 상

---

웹 서비스 > 2. 서비스 관리

## 개요

### 점검 내용

웹 서비스 불필요한 Proxy 설정제한 여부 점검

### 점검 목적

불필요한 Proxy 설정을 제한하여 자원 낭비 예방 및 관리의 복잡성을 감소시키며, 중간자 공격 등의 해킹 공격으로부터 시스템 관련 정보가 노출되거나 악용되는 것을 방지하기 위함

### 보안 위협

불필요한 Proxy 설정을 제한하지 않는 경우 공격자가 Proxy 서버를 이용하여 원래 의도되지 않은 방식으로 시스템에 접근하거나 시스템 관련 정보가 유출될 위험이 존재함

### 참고

-

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, IIS, JEUS, WebtoB

### 판단 기준

**✅ 양호**: 불필요한 Proxy 설정을 제한한 경우

**❌ 취약**: 불필요한 Proxy 설정을 제한하지 않은 경우

## 조치 방법

불필요한 Proxy 설정 존재 여부 점검 및 제한 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Apache

**Step 1) apache2.conf (또는 /conf/httpd.conf) 파일 내 불필요한 Proxy 제거**

<VirtualHost *:80> ServerName www.example.com ProxyPreserveHost On ProxyRequests Off ProxyPass / http://backend-server.example.com/ ProxyPassReverse / http://backend-server.example.com/ </VirtualHost>

### Tomcat

**Step 1) server.xml 파일 내 Connector 요소에서 불필요한 Proxy 설정 제거**

<Connector port="8080" protocol="HTTP/1.1"

03. 웹 서비스

redirectPort="8443" proxyName="proxy.example.com" proxyPort="80" />

### Nginx

**Step 1) nginx.conf 파일 내 웹 사이트에서 불필요한 Proxy 설정 제거**

# cat /[Nginx 설치 디렉터리/nginx.conf location / { proxy_pass http://backendserver:8080; proxy_set_header Host $host; proxy_set_header X-Real-IP $remote_addr; proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; }

### IIS

**Step 1) 제어판 > 관리 도구 > 인터넷 정보 서비스(IIS) 관리자 > 해당 웹 사이트 > 루트 디렉터리에서 불필요한**

Proxy 설정 제거

**[ Proxy 설정 확인 및 제거 ]**

### JEUS

**Step 1) web.xml 파일 내 불필요한 Proxy 제거**

# vi /[WebtoB 설치 디렉터리]/ReverseProxy/WEB-INF/web.xml

### WebtoB

**Step 1) http.m 파일 내 불필요 Proxy 설정 제거**

# vi /[WebtoB 디렉터리]/conf/http.m REVERSE_PROXY(0): Name = rproxy1, PathPrefix = "/proxypath/", ServerAddress = "127.0.0.1:8088",

