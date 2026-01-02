# WEB-17: 웹 서비스 가상 디렉로리 삭제

**분류**: Web Service

**중요도**: 중

---

## 개요

### 점검 내용

불필요한 가상 디렉터리 삭제 여부 점검

### 점검 목적

불필요한 가상 디렉터리를 삭제하여 공격이 가능한 영역을 최소화하고 정보 노출 방지 및 권한 상승 공격 등의 위험을 제거하기 위함

### 보안 위협

불필요한 가상 디렉터리를 삭제하지 않은 경우, 취약한 가상 디렉터리를 통해 시스템 권한 탈취 및 시스템 구조 등의 중요 정보가 노출될 위험이 존재함

### 참고

!!! info "가상 디렉터리"
    물리적으로 홈 디렉터리와 다른 위치에 있거나 홈 디렉터리에 종속된 디렉터리가 아니어도 웹 브라우저를 통해 사용자가 접속할 때 웹사이트의 하위 디렉터리로 인식되도록 하는 기법

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, WebtoB

### 판단 기준

**✅ 양호**: 불필요한 가상 디렉터리가 존재하지 않는 경우

**❌ 취약**: 불필요한 가상 디렉터리가 존재하는 경우

## 조치 방법

불필요한 가상 디렉터리 존재 여부 점검 및 삭제하도록 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Apache

1.  **Alias 지시자 확인**
    ```bash
    vi /[Apache 설치 디렉터리]/conf/httpd.conf # (또는 apache2.conf)
    ```
    ```apache
    Alias /virtual /var/www/virtual
    <Directory /var/www/virtual>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
    ```
2.  **불필요한 가상 디렉터리 삭제**

### Tomcat

1.  **‘Context’ 블록 요소의 ‘path’ 속성값 확인**
    ```bash
    vi /[Tomcat 설치 디렉터리]/server.xml
    ```
    ```xml
    <Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
        <Context path="/virtual" docBase="/path/to/your/virtual/directory" reloadable="true"/>
    </Host>
    ```
2.  **Context 블록 요소 가상 디렉터리 제거**

### Nginx

1.  **Alias 지시자 확인**
    ```bash
    vi /[Nginx  Dir]/nginx –v
    ```
    ```nginx
    location /virtual {
        alias /var/www/virtual;
        index index.html index.htm;
    }
    ```
2.  **설정된 모든 디렉터리의 불필요한 Alias 지시자 제거**
3.  **Nginx 재구동**
    ```bash
    systemctl restart nginx
    ```

### WebtoB

1.  **NODE절의 Alias 설정 확인**
    ```bash
    vi /[WebtoB 설치 디렉터리]/config/http.m
    ```
    ```text
    *ALIAS
    alias1          URI = “/cgi-bin/”, RealPath = “/home/tmax/webtob/cgi-bin/”
    ```
2.  **NODE절의 불필요한 Alias 설정 삭제**

