# WEB-01: Default 관리자 계정명 변경

**분류**: 03_web

**중요도**: 상

---

웹 서비스 > 1. 계정 관리

## 개요

### 점검 내용

웹 서비스 설치 시 기본적으로 설정된 관리자 계정의 변경 후 사용 여부 점검

### 점검 목적

기본 관리자 계정명과 같은 알려진 계정명을 유추하기 어려운 계정명으로 변경 후 사용하여 공격자에 의한 추측 공격 및 무단 접근 등을 방지하고 보안을 강화하기 위함

### 보안 위협

기본 관리자 계정명을 변경하지 않고 사용할 경우, 공격자에 의한 계정 및 비밀번호 추측 공격이 가능하고, 이를 통해 불법적인 접근, 데이터 유출, 시스템 장애 등의 보안 사고가 발생할 수 있는 위험이 존재함

### 참고

!!! info "기본 계정"
    웹 서비스 설치 시 기본적으로 생성되는 관리자 콘솔 계정

!!! info "서비스별 기본 계정"
    IIS(Administrator), Tomcat(tomcat, admin), JEUS(administrator)

## 점검 대상 및 판단 기준

### 대상

Tomcat, JEUS

### 판단 기준

**✅ 양호**: 관리자 페이지를 사용하지 않거나, 계정명이 기본 계정명으로 설정되어 있지 않은 경우

**❌ 취약**: 계정명이 기본 계정명으로 설정되어 있거나, 추측하기 쉬운 문자 조합으로 이루어진 계정명을 사용하는 경우

## 조치 방법

기본 관리자 계정명을 추측하기 어려운 계정명으로 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### Tomcat

1. 기본 계정명 변경 또는 관리자 페이지 비활성화(기본값: 비활성화)
```
# vi <Tomcat 설치 디렉터리>/conf/server.xml
```
예시
```
<user username="admin" password="XNDJxndn264!@" roles="manager-gui"/>
```
1. Tomcat 재구동
```bash
# systemctl restart tomcat
```

!!! info "“roles = manager-gui, manager-script, manager-jmx, manager-status” 설정 시 관리자 계정 및 페이지 활성화 상태"

03. 웹 서비스

### JEUS

1. Security > Security Domains 페이지 해당 도메인 > Account & Policies Management > Users > 기본 관리자 계정의 Name 확인

1. Lock & EDIT > Security > Security Domains 페이지 해당 도메인 > Account & Policies Management > Users > ADD > 기본 관리자 계정의 Name을 유추하기 어려운 계정 이름 입력 
1. Administrators 그룹 체크 후 확인 > Accounts & Policies Management > policies > Role Permissions > AdministratorsRole > “Activate Changes”을 눌러 설정 저장(웹 서비스명과 연관된 단어 “administrator” 계정명 사용금지)
1. “Activate Changes”을 눌러 설정 저장

1. JEUS 재구동
```
# ./stopServer –host [도메인명]:[포트 번호]
# ./startDomainAdminServer –host [도메인명]:[포트 번호]
```

!!! info "기본 계정명 변경이 불가능할 경우 초기 비밀번호 변경으로 보완 필요"

