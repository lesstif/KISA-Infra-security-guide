# WEB-25: 주기적 보안 패치 및 벤더 권고사항 적용

**분류**: Web Service

**중요도**: 상

---

## 개요

### 점검 내용

최신 보안 패치 적용 여부 점검

### 점검 목적

주기적인 최신 보안 패치를 통해 보안성 및 시스템 안정성을 확보하기 위함

### 보안 위협

주기적으로 최신 보안 패치를 적용하지 않을 경우, 알려진 취약점을 이용한 공격 또는 새로운 공격에 대한 침해사고 발생 위험이 존재함

### 참고

!!! info "참고"
    최신 버전을 사용하도록 권고하고 있으나 시스템 운영상 적용이 어려운 경우 최신이 아닌 취약점이 존재하지 않는 버전도 허용하고 있음

## 점검 대상 및 판단 기준

### 대상

Apache, Tomcat, Nginx, IIS, JEUS, WebtoB

### 판단 기준

**✅ 양호**: 최신 보안 패치가 적용되어 있으며, 패치 적용 정책을 수립하여 주기적인 패치 관리를 하는 경우

**❌ 취약**: 최신 보안 패치가 적용되어 있지 않거나 패치 적용 정책을 수립 및 주기적인 패치 관리를 하지 않는 경우

## 조치 방법

패치 적용에 따른 서비스 영향 정도를 정확히 파악하여 주기적인 패치 적용 정책 수립 및 적용하도록 설정

### 조치 시 영향

시스템 영향도를 파악하여 충분한 테스트 후 적용 권고

## 점검 및 조치 사례

### Apache

1.  **웹 서버 버전과 최신 패치 버전을 비교하여 확인**
    ```bash
    /[Apache 설치 디렉터리]/httpd –v
    ```
    **[ Apache 웹 서버 버전 확인 ]**
2.  **Apache 사이트를 통해 주기적으로 버전 점검을 하며, 최신 버전 적용 시 충분한 테스트 후 적용 권고**
    !!! info "참고 사이트"
        http://httpd.apache.org/download.cgi

### Tomcat

1.  **웹 서버 버전과 최신 패치 버전을 비교하여 확인**
    ```bash
    cd /[Tomcat 설치 디렉터리]/lib
    java -cp catalina.jar org.apache.catalina.util.ServerInfo
    ```
    **[ Tomcat 웹 서버 버전 확인 ]**
2.  **Tomcat 사이트를 통해 주기적으로 버전 점검을 하며, 최신 버전 적용 시 충분한 테스트 후 적용 권고**
    !!! info "참고 사이트"
        https://tomcat.apache.org/

### Nginx

1.  **웹 서버 버전과 최신 패치 버전을 비교하여 확인**
    ```bash
    /[Nginx  Dir]/nginx –v
    ```
    **[ Nginx 웹 서버 버전 확인 ]**
2.  **Nginx 사이트를 통해 주기적으로 버전 점검을 하며, 최신 버전 적용 시 충분한 테스트 후 적용 권고**
    !!! info "참고 사이트"
        https://nginx.org/en/download.html

### IIS

1.  **웹 서버 버전과 최신 패치 버전을 비교하여 확인**
    ```cmd
    reg query “HKLM\SOFTWARE\Microsoft\InetStp” /v VersionString
    ```
    **[ Windows Server 버전 확인 ]**
2.  **IIS 사이트를 통해 주기적으로 버전 점검을 하며, 최신 버전 적용 시 충분한 테스트 후 적용 권고**
    !!! info "참고 사이트"
        https://www.iis.net/downloads/microsoft

### JEUS

1.  **웹 서버 버전과 최신 패치 버전을 비교하여 확인**
    ```bash
    jeusadmin -version # 또는 jeusadmin –fullversion
    ```
2.  **JEUS 사이트를 통해 주기적으로 버전 점검을 하며, 최신 버전 적용 시 충분한 테스트 후 적용 권고**
    !!! info "참고 사이트"
        https://technet.tmaxsoft.com/ko/front/download/findDownloadList.do

### WebtoB

1.  **웹 서버 버전과 최신 패치 버전을 비교하여 확인**
    ```bash
    wscfl –version
    ```
2.  **WebtoB 사이트를 통해 주기적으로 버전 점검을 하며, 최신 버전 적용 시 충분한 테스트 후 적용 권고**
    !!! info "참고 사이트"
        https://technet.tmaxsoft.com/ko/front/download/findDownloadList.do?cmProductCode=0102

