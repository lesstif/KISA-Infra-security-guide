# WEB-04: 웹 서비스 디렉터리 리스팅 방지 설정

**분류**: Web Service

**중요도**: 상

---

## 개요

### 점검 내용

디렉터리 리스팅 기능 차단 여부 점검

### 점검 목적

웹 서버에 대한 디렉터리 리스팅 기능을 차단하여 디렉터리 내의 모든 파일에 대한 접근 및 정보 노출을 차단하기 위함

### 보안 위협

디렉터리 리스팅 기능이 차단되지 않은 경우, 비인가자가 해당 디렉터리 내의 모든 파일의 리스트 확인 및 접근이 가능하고, 웹 서버의 구조 및 백업 파일이나 소스 파일 등 공개되면 안 되는 중요 파일들이 노출될 위험이 존재함

### 참고

!!! info "디렉터리 리스팅(Directory Listing)"
    웹 서버의 취약한 설정으로 인해 웹 서버의 파일 시스템 목록을 보여주는 것

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, IIS, JEUS, WebtoB

### 판단 기준

**✅ 양호**: 디렉터리 리스팅이 설정되지 않은 경우

**❌ 취약**: 디렉터리 리스팅이 설정된 경우

## 조치 방법

디렉터리 리스팅 기능 차단 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Apache

1.  **`httpd.conf` 파일 내 모든 디렉터리의 `Options` 지시자에서 `Indexes` 옵션 제거**
    ```bash
    vi /<Apache 설치 디렉터리>/httpd.conf
    # 또는 apache.conf
    ```
    설정 예시:
    ```apache
    <Directory />
        Options Indexes  # 삭제 (또는 -Indexes 설정)
    </Directory>
    ```
2.  **Apache 재시작**
    ```bash
    systemctl restart apache2
    ```

!!! info "참고"
    - `httpd.conf` 뿐 아니라 `sites-available` 디렉터리 내 모든 사이트에 적용
    - 파일 위치 및 서비스명은 사용하는 운영체제에 따라 달라질 수 있음

### Tomcat

1.  **`web.xml` 파일 내 `listings` 옵션 비활성화**
    ```bash
    vi /<Tomcat 설치 디렉터리>/web.xml
    ```
    설정 예시:
    ```xml
    <init-param>
        <param-name>listings</param-name>
        <param-value>false</param-value>
    </init-param>
    ```

### Nginx

1.  **`nginx.conf` 파일 내 `autoindex` 지시자 `off` 설정**
    ```bash
    vi /<Nginx 설치 디렉터리>/conf/nginx.conf
    ```
    설정 예시:
    ```nginx
    server {
        autoindex off;
    }
    ```
2.  **Nginx 재시작**
    ```bash
    systemctl restart nginx
    ```

### IIS

1.  **시작 > Windows 관리 도구 > 인터넷 정보 서비스(IIS) 관리자 > 해당 웹 사이트 > IIS > `디렉터리 검색` 선택, "사용"을 "사용 안 함"으로 설정**
    ![디렉터리 검색 기능 비활성화 ](../../images/03_web/web-012.jpg)
    *(Note: Image path inferred from context or placeholder needed if not visible in source list. Using text placeholder if image missing)*
    **[ 디렉터리 검색 기능 비활성화 ]**

### JEUS

1.  **`jeus-web-dd.xml` 파일 내 디렉터리 리스팅 설정 변경**
    ```bash
    vi /<JEUS 설치 디렉터리>/WEB-INF/jeus-web-dd.xml
    ```
    설정 예시:
    ```xml
    <allow-indexing>false</allow-indexing>
    ```

### WebtoB

1.  **`*Node`, `*URL` 절에 `Options` 지시자 설정 삭제 또는 `-Indexes`로 설정**
    ```bash
    nano /<WebtoB 설치 디렉터리>/config/http.m
    ```
    설정 예시:
    ```text
    *NODE
    imuser   WEBTOBDIR="/root/webtob",
             Options = "-Indexes",
    ```
2.  **설정 파일 컴파일 및 재시작**
    ```bash
    wscfl -I http.m
    wsdown
    wsboot
    ```

