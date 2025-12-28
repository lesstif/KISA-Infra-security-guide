# PR: Web Application(웹)

**분류**: 10_webapp

**중요도**: 상

---

10. Web Application(웹) 보안

12. 취약한 비밀번호 복구 절차

## 개요

### 점검 내용

비밀번호 복구 기능 사용 시, 단순 정보(이름, 사번, 아이디 등)만을 활용하거나 SMS나 이메일 인증 시 발급되는 임시 비밀번호가 동일하거나 유추 가능 여부 점검

### 점검 목적

비밀번호 복구 로직을 유추하기 어렵게 구현하고, 인증된 사용자 메일이나 SMS에서만 복구 비밀번호를 확인할 수 있도록 하여 비인가자가 사용자 비밀번호를 획득 및 변경하지 못하도록 방지하기 위함

### 보안 위협

**❌ 취약한 비밀번호 복구 로직(비밀번호 찾기 등)으로 인하여 공격자가 불법적으로 다른 사용자의**

비밀번호를 획득, 변경할 수 있음

### 참고

!!! info "소스코드 및 취약점 점검 필요"

## 점검 대상 및 판단 기준

### 대상

웹 애플리케이션 소스코드

### 판단 기준

**✅ 양호**: 비밀번호 재설정 시 난수를 이용하여, 인증된 사용자 메일이나 SMS로 임시 비밀번호 또는

비밀번호 재설정을 위한 링크가 전송될 경우

**❌ 취약**: 비밀번호 재설정 시 일정 패턴으로 재설정되고 웹 사이트 화면에 바로 출력될 경우

## 조치 방법

비밀번호 복구 로직을 강화하고, 인증된 사용자 메일이나 SMS에서만 재설정된 비밀번호를 확인할 수 있도록 하여 비인가자가 비밀번호를 획득하지 못하도록 조치

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

**점검 방법**

**Step 1) 비밀번호 복구 기능 유무를 파악하고, 복구 과정에서 보안 질문이 추측 가능하거나 소셜 엔지니어링으로**

쉽게 답을 찾을 수 있는 단순 정보를 요구하는지 확인

**[ 비밀번호 복구 시 보안 질문 단순성 확인 ]**

**Step 2) 비밀번호 복구 시 재설정된 비밀번호에 대하여 추측가능한 일정 패턴으로 발급 유무 확인**

**[ 발급 비밀번호에 대한 복잡성 판단 ]**

**Step 3) 비밀번호 복구 과정에서 해당 계정에 등록된 이메일 및 전화번호가 아닌 공격자의 정보로 변조가 가능한**

포인트의 점검 및 패킷 변조를 통해 공격자 측으로 인증 번호 및 임시 비밀번호 발급 여부 확인

**[ 비밀번호 초기화 시 사용자 정보에 대한 무결성 검증 ]**

**조치 방법**

1. 추측이 어렵고 공개적으로 알 수 없는 정보를 기반으로 한 보안 질문 사용

2. 비밀번호 재설정 과정에서 사용자의 메일이나 SMS로 인증 코드 전송 후 이를 확인하는 2단계 인증 도입

3. 사용자의 개인정보(연락처, 주소, 메일 주소 등)로 비밀번호의 생성을 제한하며, 불규칙적이고 최소 길이(6자

이상 권고) 이상의 복잡도를 만족하는 비밀번호를 발급 및 웹 사이트 화면에 바로 출력하지 않고 인증된 사용자의 메일이나 SMS로 전송되게 구현

4. 비밀번호 재발급 검증 실패에 대한 임계값을 설정하여 일정 횟수 이상 실패한 경우 다른 방식으로 비밀번호

찾기 기능을 제공하고, 검증 후 기존의 비밀번호가 아닌 임시 비밀번호를 발급하도록 설계하며, 사용자가 임시 비밀번호를 재사용하지 못하도록 발급받은 즉시 새로운 비밀번호로 재설정하도록 구현

10. Web Application(웹) 보안

!!! info "Java"

!!! info "SecureRandom"
    난수를 생성하는 데 사용되는 클래스로, 일반 Random 클래스와 달리 시드를 명시적으로

설정할 수 없으므로 키 생성, 보안 토큰 등 보안이 필요한 애플리케이션의 경우 SecureRandom 사용 SecureRandom 클래스를 사용하여 안전한 임시 비밀번호를 생성하는 예시 private static final String CHARACTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"; //대소문자 영문, 숫자 조합으로 12자리의 난수 생성 규칙 정의 private static final int PASSWORD_LENGTH = 12; private String generateTemporaryPassword() { SecureRandom secureRandom = new SecureRandom(); StringBuilder password = new StringBuilder(PASSWORD_LENGTH); for (int i = 0; i < PASSWORD_LENGTH; i++) { int randomIndex = secureRandom.nextInt(CHARACTERS.length()); password.append(CHARACTERS.charAt(randomIndex)); } return password.toString() }  //임시 비밀번호로 사용될 난수 생성

!!! info "ASP.NET"

!!! info "RNGCryptoServiceProvider"
    난수를 생성하는 데 사용되는 클래스로, 사전에 정의된 문자 집합에서 안전한

암호화 알고리즘을 기반으로 문자를 선택하여 복잡한 난수를 생성 RNGCryptoServiceProvider 클래스를 사용하여 안전한 임시 비밀번호를 생성하는 예시 protected void btnCheckRandom_Click(object sender, EventArgs e) { lblRandomNumber.Text = GenerateAlphanumericRandom(12); // 자리수 정의(12) } private string GenerateAlphanumericRandom(int length) { const string chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"; //대소문자 영문, 숫자 조합으로 생성 규칙 정의 StringBuilder result = new StringBuilder(length); byte[] randomBytes = new byte[4 * length]; using (var rng = new RNGCryptoServiceProvider()) {

!!! info "PHP"

!!! info "rand 메소드 보다 random_int 메소드가 암호학적으로 안전한 난수를 생성 (PHP 7.0 버전부터 사용 가능)"

random_int() 메소드를 사용하여 안전한 임시 비밀번호를 생성하는 예시 ... function generateRandomPassword($length = 12) { $characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!@#$%^&*()'; $charactersLength = strlen($characters); $randomPassword = ''; for ($i = 0; $i < $length; $i++) { $randomPassword .= $characters[random_int(0, $charactersLength - 1)]; } return $randomPassword; } .... if ($_SERVER['REQUEST_METHOD'] == 'POST') { //세션 정보와 'userid' 값이 일치하는지 검증 $userid = $_POST[‘userid’]; $email = $_POST[‘email’]; if (isset($_SESSION['userid']) && $_SESSION['userid'] === $userid) { $temporaryPassword = generateRandomPassword(); updateUserPassword($userid, password_hash($temporaryPassword, PASSWORD_DEFAULT)); if (sendPasswordEmail($email, $temporaryPassword)) { echo "귀하의 이메일로 임시 비밀번호가 발송되었습니다."; .... rng.GetBytes(randomBytes); for (int i = 0; i < length; i++) { uint randomInt = BitConverter.ToUInt32(randomBytes, i * 4); result.Append(chars[(int)(randomInt % (uint)chars.Length)]); } } return result.ToString(); } ...

