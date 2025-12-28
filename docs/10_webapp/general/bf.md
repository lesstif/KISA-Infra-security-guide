# BF: Web Application(웹)

**분류**: 10_webapp

**중요도**: 상

---

9. 약한 비밀번호 정책

## 개요

### 점검 내용

웹 애플리케이션 내 로그인 폼 등 비밀번호를 설정하는 단계에서 약한 강도의 문자열 사용 여부를 점검하고, 비밀번호 복잡성 요구사항(최소 길이, 대문자 및 소문자, 숫자 및 특수문자 포함 등)을 준수 여부 점검

### 점검 목적

유추 가능한 취약한 문자열(이름, 생년월일 등)이나 낮은 복잡성의 비밀번호 사용을 제한하여 계정 및 비밀번호 추측 공격을 방지하기 위함

### 보안 위협

해당 취약점이 존재할 경우, 유추가 용이한 계정 및 비밀번호 사용으로 인해 사용자 권한 탈취 위험이 있으며, 이를 방지하기 위해 비밀번호의 적절성 및 복잡성을 검증하는 로직을 구현해야 함

### 참고

!!! info "약한 비밀번호 정책 취약점"
    웹 사이트에서 취약한 비밀번호로 회원가입이 가능할 경우, 공격자는

비밀번호를 추측하거나 주변 정보를 수집하여 작성한 사전 파일을 이용하여 사용자 계정의 탈취가 가능한 취약점

!!! info "소스코드 및 취약점 점검 필요"

## 점검 대상 및 판단 기준

### 대상

웹 애플리케이션 소스코드

### 판단 기준

**✅ 양호**: 관리자 및 사용자 계정의 비밀번호가 유추하기 어려운 값으로 설정되어 있거나 높은 복잡성의

비밀번호 정책이 설정되어 있는 경우

**❌ 취약**: 관리자 및 사용자 계정의 비밀번호가 유추하기 쉬운 값으로 설정되어 있거나 낮은 복잡성의

비밀번호 정책이 설정되어 있는 경우

## 조치 방법

높은 복잡성의 비밀번호 설정 기준을 확립하며, 계정 및 비밀번호의 체크 로직 추가 구현

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

**점검 방법**

**Step 1) 웹 애플리케이션 내 추측 가능한 계정이나 비밀번호를 통하여 로그인이 가능한 계정이 존재 여부 확인**

**[ 취약한 계정 정보를 통한 로그인 시도 ]**

10. Web Application(웹) 보안

**Step 2) 회원가입 및 사용자 정보 수정 페이지 내 비밀번호 설정 시 높은 복잡도를 요구하는지 확인**

**[ 높은 복잡도의 비밀번호 정책 설정 유무 확인 ]**

**조치 방법**

1. 테스트 및 관리자 계정 사용 시 취약한 ID 및 비밀번호 사용을 제한

2. 사용자가 취약한 계정이나 비밀번호를 등록하지 못하도록 비밀번호 규정이 반영된 체크 로직을 회원가입,

정보 변경, 비밀번호 변경 등 적용 필요한 페이지에 모두 구현(아래의 예시와 같이 정규 표현식을 통해 비밀번호 복잡도 검증 로직을 구현)

3. KISA 지식플랫폼 → 법령·가이드라인 → 안내서에서 비밀번호 선택 및 이용 안내서를 통해 안전하게

비밀번호를 생성하고 관리하는 방법 참고 (https://www.kisa.or.kr/2060305/form?postSeq=14)

!!! info "취약한 ID/비밀번호 예시"

구분 예시

**❌ 취약한 ID**

admin, administrator, manager, guest, tomcat, root, user, operator, anonymous 등

**❌ 취약한 비밀번호**

abcd, 1234, 1111, test, password, public, blank 비밀번호, ID와 동일한 비밀번호 등

!!! info "규정 예시"

규정 예시

**Step 1) 다음 각 목의 문자 종류 중 2종류 이상을 조합하여 최소 10자리 이상 또는 3종류 이상을 조합하여 최소 8자리**

이상의 길이로 구성 (1) 영문 대문자(26개) (2) 영문 소문자(26개) (3) 숫자(10개) (4) 특수문자(32개)

**Step 2) 연속적인 숫자나 생일, 전화번호 등 추측하기 쉬운 개인정보 및 아이디와 비슷한 비밀번호는 사용하지 않는 것을**

권고

**Step 3) 비밀번호에 유효기간을 설정하여 반기별 1회 이상 변경**

**Step 4) 최근 사용되었던 비밀번호 재사용 금지**

!!! info "Javascript"

비밀번호 복잡성 검증 예시 function isPasswordStrong(password) { const minLength = 8; const hasUpperCase = /[A-Z]/.test(password); const hasLowerCase = /[a-z]/.test(password); const hasNumber = /[0-9]/.test(password); const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>]/.test(password); return password.length >= minLength && hasUpperCase && hasLowerCase && hasNumber && hasSpecialChar; }document.getElementById('password').addEventListener('input', function() { const password = this.value; const strengthMessage = isPasswordStrong(password) ? 'Strong' : 'Weak'; document.getElementById('strengthText').textContent = `Strength: ${strengthMessage}`; });

4. 약한 비밀번호 정책을 사용하고, 비밀번호 입력 실패 횟수 제한 설정 또한 미흡한 경우 사전대입 공격 및

무차별 대입 공격을 통한 계정 탈취가 가능하므로 일정 횟수(3~5회) 이상 초과할 경우 계정이 잠기도록 서버 사이드 스크립트(PHP, ASP, JSP 등)를 통하여 임계 로직 구현 권고

