# WEB-18: 웹 서비스 WebDAV 비활성화

**분류**: Web Service

**중요도**: 상

---

## 개요

### 점검 내용

웹 서비스 WebDAV 비활성화 여부 점검

### 점검 목적

WebDAV 서비스를 비활성화하여, WebDAV에서 발견되는 다수의 인증 우회 취약점을 제거하고자 함

### 보안 위협

WebDAV가 활성화되어 있는 경우 웹 서비스에 악의적으로 작성된 요청을 이용하여 인증을 우회함으로써 비밀번호로 보호된 WebDAV의 자원에 접근 (디렉터리 열람, 파일 다운로드 등)이 가능하며, WebDAV에 의해 호출된 일부 구성요소에 매개 변수를 정확하게 점검하지 않는 결함이 존재하여, 이로 인해 버퍼 오버런이 발생할 위험이 존재함

### 참고

!!! info "WebDAV(Web Distributed Authoring and Versioning)"
    사용자가 원격 World Wide Web 서버를 이용하여 파일을 수정하거나 처리할 수 있도록 하는 HTTP의 확장 서비스. 웹상의 공동개발을 지원하기 위한 IETF 표준안(RFC 2518)으로써, 원격지 사용자 간에 인터넷상에서 파일을 공동 편집하고 관리할 수 있도록 함

## 점검 대상 및 판단 기준

### 대상

Apache, Nginx, IIS, WebtoB

### 판단 기준

**✅ 양호**: WebDAV 서비스를 비활성화하고 있는 경우

**❌ 취약**: WebDAV 서비스를 활성화하고 있는 경우

## 조치 방법

WebDAV 서비스 비활성화 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Apache

1.  **WebDAV 설정 확인**
    -   `httpd.conf` 파일 내 모든 디렉터리에서 `WebDAV` 설정 확인
    ```bash
    cat /[Apache_Dir]/conf/httpd.conf # (또는 apache2.conf)
    ```
    ```apache
    Dav On
    ```
2.  **모든 디렉터리에서 WebDAV 설정 비활성화 또는 주석 처리**
    ```bash
    vi /[Apache_Dir]/conf/httpd.conf # (또는 apache2.conf)
    ```
    ```apache
    <Directory "/path/to/directory">
        Dav Off
    </Directory>
    ```
3.  **Apache 재구동**
    ```bash
    systemctl restart apache2
    ```

### Nginx

1.  **WebDAV 설정 확인**
    -   `nginx.conf` 파일 내 모든 디렉터리에서 `WebDAV` 설정 확인
    ```bash
    cat /[Nginx 설치 디렉터리]/conf/nginx.conf
    ```
    ```nginx
    location /webdav {
        root /path/to/webdav;
        dav_methods PUT DELETE MKCOL COPY MOVE;
        dav_access user:rw group:rw all:r;
        create_full_put_path on;
    }
    ```
2.  **WebDAV 설정 주석 처리 또는 제거**
    -   `nginx.conf` 파일 내 모든 디렉터리에서 `WebDAV` 설정 주석 처리 또는 제거
3.  **Nginx 재구동**
    ```bash
    systemctl restart nginx
    ```

### IIS

1.  **WebDAV 금지 설정 확인**
    -   `제어판` > `관리 도구` > `인터넷 정보 서비스(IIS) 관리자` > 서버 선택 > `IIS` > ‘ISAPI 및 CGI 제한’ 선택, `WebDAV` 항목 선택 > ‘확장 경로 실행 허용(A)’ 체크 확인
    **[ WebDAV 제한 설정 ]**
2.  **WebDAV 금지 설정 확인**
    -   `제어판` > `관리 도구` > `인터넷 정보 서비스(IIS) 관리자` > 서버 선택 > `IIS` > ‘ISAPI 및 CGI 제한’ 선택, `WebDAV` 항목 선택 > [작업]에서 제거하거나, 편집 > ‘확장 경로 실행 허용(A)’ 체크 해제
    **[ WebDAV 제한 설정 ]**

### WebtoB

1.  **Server 설정 파일 내 NODE절 vhost 메소드 설정 확인**
    ```bash
    vi  /[WebtoB 설치 디렉터리]/config/http.m
    ```
    ```text
    *VHOST
    vhost1   .
             Method = “GET, POST, HEAD, OPTIONS, PROPFIND, PUT, DELETE, MKCOL, COPY, MOVE”
    ```
2.  **NODE절 WebDAV 설정 삭제**

