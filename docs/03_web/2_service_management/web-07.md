# WEB-07: 웹 서비스 경로 내 불필요한 파일 제거

**분류**: Web Service

**중요도**: 중

---

## 개요

### 점검 내용

웹 서비스 설치 시 기본으로 생성되는 불필요한 파일 및 디렉터리 제거 여부 점검

### 점검 목적

웹 서비스 설치 시 기본으로 생성되는 샘플, 매뉴얼 파일 등 서비스에 불필요한 파일을 제거하여 불필요한 공격 대상으로 이용되는 것을 방지하기 위함

### 보안 위협

웹 서비스 설치 시 기본으로 생성되는 파일 및 디렉터리나 백업, 테스트 파일 등을 제거하지 않은 경우, 비인가자에게 시스템 관련 정보 및 웹 서버 정보가 노출되거나 해킹에 악용될 수 있음

### 참고

!!! info "불필요한 파일"
    샘플 파일, 매뉴얼 파일, 임시 파일, 테스트 파일, 백업 파일 등

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, IIS, JEUS, WebtoB

### 판단 기준

**✅ 양호**: 기본으로 생성되는 불필요한 파일 및 디렉터리가 존재하지 않을 경우

**❌ 취약**: 기본으로 생성되는 불필요한 파일 및 디렉터리가 존재하는 경우

## 조치 방법

불필요한 파일 및 디렉터리를 제거하도록 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Apache

1.  **`rm` 명령어로 확인된 불필요한 매뉴얼 디렉터리 및 파일 제거**
    ```bash
    rm -rf /<Apache 설치 디렉터리>/htdocs/manual
    rm -rf /<Apache 설치 디렉터리>/manual
    ```

!!! info "참고"
    2.4 버전 이상은 htdocs 디렉터리가 기본 제공되지 않으므로 `/var/www/html` 사용

### Tomcat

1.  **`rm` 명령어로 확인된 불필요한 매뉴얼 디렉터리 및 파일 제거**
    ```bash
    rm -rf /<Tomcat 설치 디렉터리>/webapps/docs/<불필요 파일>
    ```

!!! info "참고"
    BUILDING.txt, RELEASE-NOTES.txt, jndi-resources-howto.html 등 매뉴얼 파일 포함

### Nginx

1.  **`rm` 명령어로 확인된 불필요한 매뉴얼 디렉터리 및 파일 제거**
    ```bash
    rm -rf /<Nginx 설치 디렉터리>/html/index.html
    ```

### IIS

1.  **샘플 디렉터리 존재여부 확인 및 제거**
    -   샘플 디렉터리 경로 예시:
        -   `c:\inetpub\iissamples`
        -   `c:\winnt\help\iishelp`
        -   `c:\program files\common files\system\msadc\sample`
        -   `%SystemRoot%\System32\Inetsrv\IISADMPWD`

### JEUS

1.  **`rm` 명령어로 확인된 불필요한 매뉴얼 디렉터리 및 파일 제거**
    ```bash
    rm -rf /<JEUS 설치 디렉터리>/docs/manuals/default/web-manager/<불필요 파일>
    rm -rf /<JEUS 홈 디렉터리>/samples/<불필요 파일>
    ```

### WebtoB

1.  **`rm` 명령어로 확인된 불필요한 매뉴얼 디렉터리 및 파일 제거**
    ```bash
    rm -rf /<WebtoB 설치 디렉터리>/docs/manuals/<불필요 파일>
    rm -rf /<WebtoB 홈 디렉터리>/samples/<불필요 파일>
    ```

