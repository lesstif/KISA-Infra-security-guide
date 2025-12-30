# WEB-03: 비밀번호 파일 권한 관리

**분류**: 03_web

**중요도**: 상

---

웹 서비스 > 1. 계정 관리

## 개요

### 점검 내용

비밀번호 파일에 대해 적절한 접근 권한 설정 여부 점검

### 점검 목적

비밀번호 파일의 접근 권한을 적절하게 설정하여 비인가자가 비밀번호 파일에 무단 접근 및 유출 등을 방지하기 위함

### 보안 위협

비밀번호 파일의 권한을 적절하게 설정하지 않은 경우, 비인가자에게 비밀번호 정보가 노출될 수 있고 웹 서버에 접속하는 등의 침해사고가 발생할 위험이 존재함

### 참고

-

## 점검 대상 및 판단 기준

### 대상

Tomcat, IIS, JEUS

### 판단 기준

**✅ 양호**: 비밀번호 파일에 권한이 600 이하로 설정된 경우

**❌ 취약**: 비밀번호 파일에 권한이 600 초과로 설정된 경우

## 조치 방법

비밀번호 파일 권한 600 이하로 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Tomcat

1. tomcat-users.xml 파일 권한 변경
```
# chmod 600 /<Tomcat 설치 디렉터리>/tomcat-users.xml
```

### IIS

1. “%systemroot%\system32\config\SAM” 파일 속성 > 보안 > 편집 > Administrators, SYSTEM을 제외한 계정 및 그룹 권한 제거

![SAM 파일 권한 설정](https://example.com/image.png)

### JEUS

1. [비밀번호 파일] 또는 [Role 파일] 권한 설정
```
# chmod 600 /<JEUS 설치 디렉터리>/jeus_domain/config/security/SYSTEM_DOMAIN/accounts.xml 
# chmod 600 /<JEUS 설치 디렉터리>/jeus_domain/config/security/SYSTEM_DOMAIN/policies.xml
```
1. JEUS 재시작
```
# ./stopServer –host <도메인명>:<포트 번호> 
# ./startDomainAdminServer –host <도메인명>:<포트 번호>
```
