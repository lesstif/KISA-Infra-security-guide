# WEB-09: 웹 서비스 프로세스 권한 제한

**분류**: Web Service

**중요도**: 상

---

## 개요

### 점검 내용

웹 서비스 프로세스의 관리자 권한 구동 여부 점검

### 점검 목적

웹 프로세스가 웹 서비스 운영에 필요한 최소한의 권한만을 갖도록 제한함으로써 웹 사이트 방문자가 웹 서비스의 취약점을 이용해 시스템에 대한 어떤 권한도 획득할 수 없도록 하여 침해사고 발생 시 피해 범위 확산을 방지하기 위함

### 보안 위협

웹 프로세스 권한을 제한하지 않은 경우, 웹 사이트 방문자가 웹 서비스의 취약점을 이용하여 시스템 권한을 획득할 수 있으며, 웹 취약점을 통해 접속 권한을 획득한 경우에는 관리자 권한을 획득하여 서버에 접속 후 정보의 변경, 훼손 및 유출될 위험이 존재함

### 참고

-

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, IIS, JEUS, WebtoB

### 판단 기준

**✅ 양호**: 웹 프로세스(웹 서비스)가 관리자 권한이 부여된 계정이 아닌 운영에 필요한 최소한의 권한을 가진 별도의 계정으로 구동되고 있는 경우

**❌ 취약**: 웹 프로세스(웹 서비스)가 관리자 권한이 부여된 계정으로 구동되고 있는 경우

## 조치 방법

웹 서비스 프로세스 구동 시 관리자 권한이 아닌 운영에 필요한 최소한의 권한을 가진 계정으로 구동 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Apache

1.  **`envvars` 파일 내 실행 계정을 관리자 계정이 아닌 별도의 계정으로 변경**
    ```bash
    vi /[Apache 설치 디렉터리]/envvars
    ```
    ```bash
    export APACHE_RUN_USER=www-data
    export APACHE_RUN_GROUP=www-data
    ```
2.  **Apache 서비스 파일 소유권 변경**
    ```bash
    chown -R www-data:www-data /etc/apache2/
    chown -R www-data:www-data /var/www/
    chown -R www-data:www-data /var/log/apache2/
    ```
3.  **웹 서비스 실행 계정 로그인 제한 설정**
    ```bash
    usermod -s /sbin/nologin [사용자명]
    ```
4.  **Apache 재구동**
    ```bash
    systemctl restart apache2
    # 또는 httpd
    ```

### Tomcat

1.  **`tomcat.service` 파일 내 Tomcat 데몬 구동 권한을 관리자 계정이 아닌 별도 계정으로 변경**
    ```bash
    vi /etc/systemd/system/tomcat.service
    ```
    ```ini
    [Service]
    User=tomcat
    Group=tomcat
    ```
2.  **Tomcat 서비스 파일 소유권 변경**
    ```bash
    chown -R tomcat:tomcat /[Tomcat 설치 디렉터리]/usr/share/tomcat9/
    chown -R tomcat:tomcat /[Tomcat 설치 디렉터리]/tomcat9/temp
    chown -R tomcat:tomcat /[Tomcat 설치 디렉터리]/logs
    chown -R tomcat:tomcat /usr/share/tomcat9/webapps
    chown -R tomcat:tomcat /usr/share/tomcat9/work
    ```
3.  **웹서비스 실행 계정 로그인 제한 설정**
    ```bash
    usermod -s /sbin/nologin [사용자명]
    ```
4.  **Tomcat 서비스 재구동**
    ```bash
    systemctl restart tomcat
    ```

### Nginx

1.  **`nginx.conf` 파일 내 Nginx 데몬 구동 권한을 관리자 계정이 아닌 별도 계정으로 변경**
    ```bash
    vi /[Nginx 설치 디렉터리]/conf/nginx.conf
    ```
    ```nginx
    User nginx nginx;
    ```
2.  **Nginx 전용 계정 생성 및 Nginx 전용 그룹 추가**
    ```bash
    adduser --system --no-create-home --shell /bin/false nginx
    groupadd nginx && sudo usermod -aG nginx nginx
    ```
3.  **웹서비스 실행 계정 로그인 제한 설정**
    ```bash
    usermod -s /sbin/nologin [사용자명]
    ```
4.  **Nginx 서비스 재구동**
    ```bash
    systemctl restart nginx
    ```

### IIS

1.  **웹 사이트 응용프로그램 풀 이름 확인**
    -   `제어판` > `관리 도구` > `인터넷 정보 서비스(IIS) 관리자` > 해당 웹 사이트 > `고급 설정` > `응용프로그램 풀 이름(DefaultAppPool)` 확인
    **[ 응용프로그램 풀 이름 확인 ]**
2.  **웹 사이트 응용프로그램 풀 ID 확인**
    -   `제어판` > `관리 도구` > `인터넷 정보 서비스(IIS) 관리자` > `응용프로그램 풀` > `응용프로그램 풀 이름(DefaultAppPool)` 선택 > `고급 설정` > `ID` > 확인
    **[ 응용프로그램 풀 ID 확인 ]**
3.  **웹사이트 응용프로그램 풀 ID 설정**
    -   `제어판` > `관리 도구` > `인터넷 정보 서비스(IIS) 관리자` > `응용프로그램 풀` > `용용 프로그램 풀 이름 (DefaultAppPool)` 선택 > `고급 설정` > `ID` > `ApplicationPoolIdentity` 선택
    **[ 응용프로그램 풀 ID 설정 ]**

### JEUS

1.  **JEUS 데몬 구동 권한 확인**
    ```bash
    ps -ef | grep jeus
    # jeus 25305 4223 99 09:54 pts/5 00:03:31 /usr/lib/jvm/java-11-openjdk-amd64/bin/java-DadminServer...
    ```
2.  **JEUS 데몬 구동 권한을 관리자 계정이 아닌 별도 계정으로 변경**
    ```bash
    useradd -m jeus
    mv /[JEUS 설치 디렉터리]/home/jeus
    ```
3.  **[JEUS 설치 디렉터리] 소유자 및 그룹 소유자를 JEUS 계정으로 변경**
    ```bash
    chown -R jeus:jeus /home/jeus/
    ```

### WebtoB

1.  **소유자 및 그룹 소유자 변경**
    ```bash
    chown -R [WebtoB 전용 계정]:[WebtoB 전용 계정] /[WebtoB 디렉터리]
    ```
2.  **`http.m` 파일 내 기존 경로 변경**
    -   `*NODE`절의 `WEBTOBDIR`, `DOCROOT`을 변경한 디렉터리로 설정
    -   `*ALIAS`절의 `alias1`을 변경한 디렉터리로 설정
    -   `*LOGGING`절의 `syslog`, `log1`, `log2`을 변경한 디렉터리로 설정
    
    설정 예시:
    ```text
    *NODE
    imuser   WEBTOBDIR="/home/tmax/webtob/",
             SHMKEY = 54000,
             DOCROOT="/home/tmax/webtob/docs",

    *ALIAS
    alias1   URI = "/cgi-bin/",
             RealPath = "/home/webtob/webtob/cgi-bin/"

    *LOGGING
    syslog   Format = "SYSLOG", FileName = "/home/tmax/webtob/log/system.log", Option = "sync"
    log1     Format = "DEFAULT", FileName = "/home/tmax/webtob/log/access.log", Option = "sync"
    log2     Format = "ERROR", FileName = "/home/tmax/webtob/log/error.log_%", Option = "sync"
    ```
3.  **변경한 디렉터리명 환경변수에 추가**
    ```bash
    export WEBTOB=/[WebtoB 디렉터리]
    source ~/.bashrc
    ```
4.  **`libwbiconv.so` 파일을 직접 `/usr/lib`로 복사**
    ```bash
    cp /webtob/lib/libwbiconv.so /usr/lib/
    ```
5.  **라이브러리 캐시 업데이트**
    ```bash
    ldconfig
    ```
6.  **설정 파일 컴파일**
    ```bash
    wscfl -i http.m
    ```

