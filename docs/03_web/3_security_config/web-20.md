# WEB-20: SSL/TLS 활성화

**분류**: 03_web

**중요도**: 상

---

03. 웹 서비스

웹 서비스 > 3. 보안 설정

## 개요

### 점검 내용

서버와 클라이언트 간 통신 시 데이터 암호화 전송 여부 점검

### 점검 목적

서버와 클라이언트 간 통신 시 데이터의 평문 전송을 사용하지 않고 데이터가 암호화되는 SSL/TLS 인증 암호화 접속을 통해 스니핑을 통한 정보 유출의 위험을 방지하기 위함

### 보안 위협

Ÿ 웹상의 데이터 통신 시 서버와 클라이언트 간에 데이터를 평문 전송하는 경우, 간단한 도청(스니핑)을 통해 정보가 탈취 및 도용될 위험이 존재함 Ÿ SSL/TLS가 활성화되어 있지 않을 경우, 데이터는 암호화되지 않아 공격자가 중간에서 데이터를 가로채거나 도청할 수 있으며, 더 나아가 평문으로 전송되어 중간에서 변경될 우려가 있어 데이터의 정확성이 훼손될 위험이 존재함

### 참고

!!! info "스니핑(임의 지정)"
    스니퍼(Sniffer)는 "컴퓨터 네트워크상에 흘러 다니는 트래픽을 엿듣는

도청장치"라고 말할 수 있으며 "스니핑"이란 이러한 스니퍼를 이용하여 네트워크상의 데이터를 도청하는 행위를 말함

!!! info "SSL(Secure Socket Layer)"
    인터넷상에서 정보를 암호화하여 송/수신하는 프로토콜. 현재

인터넷에서 널리 쓰이고 있는 www, FTP 등의 데이터를 암호화하여, 프라이버시에 관한 정보나 신용카드 번호, 기업 비밀 등을 안전하게 송/수신할 수 있음

## 점검 대상 및 판단 기준

### 대상

Apache, Nginx, IIS, WebtoB

### 판단 기준

**✅ 양호**: SSL/TLS 설정이 활성화되어 있는 경우

**❌ 취약**: SSL/TLS 설정이 비활성화되어 있는 경우

## 조치 방법

웹 서비스 내 SSL/TLS 활성화 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Apache

**Step 1) SSL 모듈 활성화 확인**

# apache2ctl –M | grep ssl ssl_module (shared)

**Step 2) SSL 가상 호스트 설정에 SSL 인증서 설정 추가**

# vi /[Apache 설치 디렉터리]/sites-available/default-ssl.conf <VirtualHost *:443> ServerAdmin webmaster@yourdomain.com ServerName yourdomain.com DocumentRoot /var/www/html SSLEngine on SSLCertificateFile /path/to/your_domain_name.crt SSLCertificateKeyFile /path/to/your_domain_name.key ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined </VirtualHost>

**Step 3) SSL 가상 호스트 활성화**

# a2ensite default-ssl

**Step 4) Apache 재구동**

# systemctl restart apache2

### Nginx

**Step 1) SSL 인증서 파일 및 개인키 파일 준비**

**Step 2) nginx.conf 파일 내 SSL/TLS 설정**

# vi /[Nginx 설치 디렉터리]/conf/nginx.conf server { listen 80; server_name example.com; location / { return 301 https://$host$request_uri; } } server { listen 443 ssl; server_name example.com; ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem; ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

03. 웹 서비스

ssl_protocols TLSv1.2 TLSv1.3; ssl_prefer_server_ciphers on; ssl_ciphers 'SSL_CIPHERS'; } }

**Step 3) Nginx 재구동**

# systemctl restart nginx

### IIS

**Step 1) SSL 인증서 바인딩 설정**

제어판 > 관리 도구 > IIS(인터넷 정보 서비스) 관리자 > 해당 웹사이트 > [사이트 바인딩] > [편집] 탭 > SSL 인증서 확인

**[ SSL 인증서 확인 ]**

**Step 2) SSL 인증서 가져오기**

발급받은 인증서 > 인증서 설치 > ‘로컬 컴퓨터’선택 > 다음 > ‘인증서 종류를 기준으로 인증서 저장소를 자동으로 선택’ 선택 > 다음 > 마침

**Step 3) SSL 인증서 설치**

C:\Windows\System32\mmc.exe > 파일 > 스냅인 추가/제거 > 인증서 > 추가 > 이 스냅인이 항상 관리할 인증서 대상 ‘컴퓨터 계정’ > 로컬 컴퓨터 > 확인

**[ SSL 인증서 추가 과정 1 ]**

**[ SSL 인증서 추가 과정 2 ]**

**Step 4) 추가된 인증서 확인**

콘솔 루트 > 인증서(로컬 컴퓨터) > 개인용 > 인증서 > 발급된 인증서 확인

**[ 추가된 인증서 확인 ]**

**Step 5) 인증서 등록**

인터넷 정보 서비스(IIS) 관리자 > 서버 선택 및 우클릭 > “바인딩 편집” 선택 > 추가 > 종류 “https”, IP주소, 포트, 호스트 이름, SSL 인증서 선택 > 확인

03. 웹 서비스

**Step 6) IIS 서버 재구동**

### WebtoB

**Step 1) http.m 파일 내 SSLFlag, SSLName 설정 확인**

# vi  /[WebtoB 설치 디렉터리]/config/http.m *VHOST vhost1 ... ERRORDOCUMENT   = "400,401,403,404,405,406,503", SSLFLAG         = Y, SSLNAME         = "ssl_nxcore", LOGGING         = "acc_https",

**Step 2) http.m 파일 내 인증서 설정 파일 확인**

*SSL ssl_nxcore CertificateFile         = "/sw/webtob5/ssl/2023/cert.crt", CertificateKeyFile      = "/sw/webtob5/ssl/2023/privkey.key", CertificateChainFile    = "/sw/webtob5/ssl/2023/chain.crt", CACertificateFile       = "/sw/webtob5/ssl/2023/rootca.crt", PassPhraseDialog        = "file:/sw/webtob5/ssl/passwd", Protocols               = "-SSLv2, -SSLv3, -TLSv1, -TLSv1.1, TLSv1.2, TLSv1.3", RequiredCiphers         = "HIGH:!RSA:!SHA1"

**Step 3) CA 인증서 및 개인키를 생성해 WebtoB 설정 파일에 설정**

