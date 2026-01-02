# WEB-06: 웹 서비스 상위 디렉터리 접근 제한 설정

**분류**: Web Service

**중요도**: 상

---

## 개요

### 점검 내용

".." 와 같은 문자사용 등을 통한 상위 디렉터리 접근 제한 여부 점검

### 점검 목적

상위 디렉터리 접근 제한 설정을 통해 비인가자의 특정 디렉터리에 대한 접근 및 열람을 제한하여 중요 파일 및 데이터를 보호하고, Unicode 버그 및 서비스 거부 공격 등을 방지하기 위함

### 보안 위협

상위 디렉터리로 이동하는 것이 가능할 경우 접근하고자 하는 디렉터리의 하위 경로에서 상위로 이동하며 정보 탐색이 가능하여 중요 정보가 노출될 위험이 존재함. 악의적인 목적을 가진 사용자가 중요 파일 및 디렉터리의 접근이 가능하여 데이터가 유출될 위험이 존재함

### 참고

!!! info "참고"
    ".." 는 Unicode 버그, 서비스 거부와 같은 공격에 쉽게 이용되므로 허용하지 않는 것을 권장함

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, IIS, WebtoB

### 판단 기준

**✅ 양호**: 상위 디렉터리 접근 기능을 제거한 경우

**❌ 취약**: 상위 디렉터리 접근 기능을 제거하지 않은 경우

## 조치 방법

상위 디렉터리 접근 기능 제거 설정

### 조치 시 영향

웹 서버 및 웹 서비스의 특성에 따라 영향을 줄 수 있음

## 점검 및 조치 사례

### Apache

1.  **`AllowOverride` 지시자 `AuthConfig` 옵션 설정 확인**
    ```bash
    vi /<Apache 설치 디렉터리>/httpd.conf
    # 또는 apache.conf
    ```
    ```apache
    <Directory "/usr/local/apache2/htdocs">
        AllowOverride None
    </Directory>
    ```
2.  **`AllowOverride` 지시자 `AuthConfig` 옵션 설정**
    ```bash
    vi /<Apache 설치 디렉터리>/httpd.conf
    # 또는 apache.conf
    ```
    ```apache
    <Directory "/usr/local/apache2/htdocs">
        AllowOverride AuthConfig
    </Directory>
    ```
3.  **사용자 인증을 설정할 디렉터리에 `.htaccess` 파일 생성**
    ```apache
    AuthName "디렉터리 사용자 인증"
    AuthType Basic
    AuthUserFile /usr/local/apache/test/.auth
    Require valid-user
    ```
    -   `AuthName`: 인증 영역(웹 브라우저의 인증 창에 표시되는 문구)
    -   `AuthType`: 인증 형태(Basic 또는 Digest)
    -   `AuthUserFile`: 사용자 정보(아이디 및 비밀번호) 저장 파일 위치
    -   `Require`: 접근을 허용할 사용자 또는 그룹 정의
4.  **사용자 인증에 사용할 아이디 및 비밀번호 생성**
    ```bash
    htpasswd /<Apache 설치 디렉터리>/.htpasswd [사용자명]
    # New password: <비밀번호 입력>
    # Re-type new password: <비밀번호 재입력>
    # Adding password for user <사용자명>
    ```
5.  **Apache 재구동**
    ```bash
    systemctl restart apache2
    ```

### Tomcat

1.  **`server.xml` 파일 내 `Context` 요소에서 `allowLinking` 옵션 확인**
    ```bash
    vi /<Tomcat 설치 디렉터리>/conf/server.xml
    ```
    ```xml
    <Context allowLinking="true">
        <WatchedResource>WEB-INF/web.xml</WatchedResource>
        <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
        <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>
    </Context>
    ```
2.  **`server.xml` 파일 내 `Context` 요소에서 `allowLinking` 옵션 제거**
    ```bash
    vi /<Tomcat 설치 디렉터리>/conf/server.xml
    ```
    ```xml
    <Context>
        <WatchedResource>WEB-INF/web.xml</WatchedResource>
        <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
        <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>
    </Context>
    ```

### Nginx

1.  **`nginx.conf` 파일 내 디렉터리 접근을 기본 인증으로 제한 설정**
    ```bash
    cat /<Nginx 설치 디렉터리>/conf/nginx.conf
    ```
    ```nginx
    location /<접근제한 디렉터리>/ {
        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/.htpasswd;
    }
    ```

### IIS 6.0 이하

1.  **부모 경로 사용 설정**
    -   `시작` > `Windows 관리 도구` > `인터넷 정보 서비스(IIS) 관리자` > `웹 사이트` > `IIS` > `ASP` > `부모 경로 사용` 항목 "False" 설정
    **[ 부모 경로 설정 확인 ]**

### IIS 7.0 이상

1.  **`제어판` > `관리 도구` > `인터넷 정보 서비스(IIS) 관리자` > 해당 웹사이트 > `사이트 편집`에서 루트 디렉터리 > web.config 파일에서 `enableParentPaths` 요소를 "False"로 설정**
    ```xml
    <configuration>
        <system.web>
            <httpRuntime enableParentPaths="false" />
        </system.web>
    </configuration>
    ```

!!! info "참고"
    `web.config` 파일이 없으면 사이트 홈 디렉터리에 새로 생성

### WebtoB

1.  **`http.m` 파일 내 활성화되어 있는 `UpperDirRestrict` 옵션 제거 또는 비활성화**
    ```bash
    vi /<WebtoB 설치 디렉터리>/config/http.m
    ```
    ```text
    UpperDirRestrict = N
    ```
2.  **확인 후 설정 파일 컴파일 및 재구동**
    ```bash
    wscfl -I http.m
    wsdown
    wsboot
    ```

