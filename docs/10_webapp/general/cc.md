# CC: Web Application(웹)

**분류**: 10_webapp

**중요도**: 상

---

10. Web Application(웹) 보안

18. 쿠키 변조

## 개요

### 점검 내용

쿠키 변조를 통한 임의의 타 사용자 권한 탈취 여부 점검

### 점검 목적

쿠키를 사용하는 경우, 안전한 알고리즘으로 암호화하여 공격자가 쿠키 값 변조를 통해 다른 사용자로 위장하거나 권한을 변경하는 것을 방지하기 위함

### 보안 위협

클라이언트에 전달되는 쿠키에 사용자 식별 값이 평문으로 노출될 경우 쿠키 변조를 통해 타 사용자의 유효한 세션을 취득할 수 있으며, 기타 중요 정보의 유출 및 변조 가능함

### 참고

!!! info "쿠키(Cookie)"
    서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각. 브라우저는 그 데이터

조각들을 저장 후, 동일한 서버에 대하여 재요청 시 저장된 데이터를 함께 전송

!!! info "소스코드 및 취약점 점검 필요"

## 점검 대상 및 판단 기준

### 대상

웹 애플리케이션 소스코드

### 판단 기준

**✅ 양호**: 쿠키를 사용하지 않고 서버 사이드 세션을 사용하고 있거나, 쿠키를 사용하는 경우 안전한

알고리즘(SEED, 3DES, AES)이 적용되어 있는 경우

**❌ 취약**: 안전한 알고리즘이 적용되지 않은 쿠키를 사용하거나, 쿠키로만 인증 및 권한 부여를 적용하는

경우

## 조치 방법

쿠키 대신 서버 사이드 세션 방식을 사용하거나, 쿠키를 통해 인증 등 중요한 기능을 구현해야 하는 경우 안전한 알고리즘(SEED, 3DES, AES 등)을 적용

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

**점검 방법**

**Step 1) 일반 사용자 계정으로 로그인 한 뒤 쿠키 내용 및 발행되는 쿠키에 중요 정보(인증을 위한 ID, 권한을**

위한 구분자 등)의 노출 여부 확인 후 변조 시도

**[ 쿠키 내 사용자 및 주요 권한 검증값 존재 유무 확인 ]**

**Step 2) 쿠키 내 노출되는 중요 정보를 변조하여 다른 사용자 및 권한으로 정상 이용이 가능한지 확인**

**[ 쿠키 값 변조를 통한 타 사용자 권한 탈취 여부 확인 ]**

**조치 방법**

1. 쿠키 대신 보안성이 강한 서버 사이드 세션 방식 사용. 클라이언트 사이드 방식인 쿠키는 구조상 다양한

**❌ 취약점에 노출될 가능성이 존재**

2. 쿠키를 사용해서 중요 정보나 인증을 구현해야 할 경우엔 안전한 알고리즘(SEED, 3DES, AES 등) 적용

3. 쿠키 서명(HMAC)을 통해 변조 여부 검증 및 HttpOnly, Secure, SameSite 속성을 적용하여 보안 강화

!!! info "Java"

AES + HMAC CookieUtil 예시 public class CookieUtil { private static final int IV_LEN = 16, HMAC_LEN = 32; private static final SecureRandom RNG = new SecureRandom(); private final byte[] encKey; private final byte[] hmacKey; public CookieUtil(byte[] encKey, byte[] hmacKey) { if (encKey.length != 32) throw new IllegalArgumentException("AES key must be 32 bytes"); this.encKey = encKey; this.hmacKey = hmacKey; } // 쿠키 생성 (HttpOnly, Secure, SameSite 설정 포함) public void addSecureCookie(HttpServletResponse resp, String name, String plaintext, int maxAgeSec) throws Exception { byte[] iv = new byte[IV_LEN]; RNG.nextBytes(iv);

10. Web Application(웹) 보안

// AES-256-CBC 암호화 Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding"); cipher.init(Cipher.ENCRYPT_MODE, new SecretKeySpec(encKey, "AES"), new IvParameterSpec(iv)); byte[] ciphertext = cipher.doFinal(plaintext.getBytes(StandardCharsets.UTF_8)); Mac mac = Mac.getInstance("HmacSHA256"); mac.init(new SecretKeySpec(hmacKey, "HmacSHA256")); mac.update(iv); mac.update(ciphertext); byte[] hmac = mac.doFinal(); byte[] payload = new byte[iv.length + hmac.length + ciphertext.length]; System.arraycopy(iv, 0, payload, 0, iv.length); System.arraycopy(hmac, 0, payload, iv.length, hmac.length); System.arraycopy(ciphertext, 0, payload, iv.length + hmac.length, ciphertext.length); String encoded = Base64.getUrlEncoder().withoutPadding().encodeToString(payload); // HttpOnly, Secure 속성 설정 Cookie cookie = new Cookie(name, encoded); cookie.setHttpOnly(true); cookie.setSecure(true); cookie.setPath("/"); cookie.setMaxAge(maxAgeSec); // SameSite 속성은 Cookie API로 직접 지정 불가 → 헤더로 세팅 String header = String.format( "%s=%s; Max-Age=%d; Path=/; HttpOnly; Secure; SameSite=Strict", name, encoded, maxAgeSec ); resp.addHeader("Set-Cookie", header); } // 쿠키 읽기 (HMAC 검증 + 복호화) public String readSecureCookie(String value) throws Exception { if (value == null) return null; byte[] data; try { data = Base64.getUrlDecoder().decode(value); } catch (IllegalArgumentException e) { return null; } if (data.length < IV_LEN + HMAC_LEN + 1) return null;

!!! info "ASP.NET"

byte[] iv = Arrays.copyOfRange(data, 0, IV_LEN); byte[] hmac = Arrays.copyOfRange(data, IV_LEN, IV_LEN + HMAC_LEN); byte[] ciphertext = Arrays.copyOfRange(data, IV_LEN + HMAC_LEN, data.length); // 복호화 전 HMAC 검증 Mac mac = Mac.getInstance("HmacSHA256"); mac.init(new SecretKeySpec(hmacKey, "HmacSHA256")); mac.update(iv); mac.update(ciphertext); byte[] calc = mac.doFinal(); if (!MessageDigest.isEqual(hmac, calc)) return null; // 변조 탐지 후 복호화 Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding"); cipher.init(Cipher.DECRYPT_MODE, new SecretKeySpec(encKey, "AES"), new IvParameterSpec(iv)); return new String(cipher.doFinal(ciphertext), StandardCharsets.UTF_8); } } MachineKey 적용 예시 <!-- web.config --> <machineKey validationKey="AutoGenerate,IsolateApps" decryptionKey="AutoGenerate,IsolateApps" validation="HMACSHA256" decryption="AES" /> <!-- cookie.aspx.cs --> // 쿠키 발급 (FormsAuthenticationTicket 활용) var ticket = new FormsAuthenticationTicket( 1, "username", DateTime.Now, DateTime.Now.AddMinutes(30), true, "role=admin"); string encrypted = FormsAuthentication.Encrypt(ticket); // 내부적으로 AES + HMAC 적용 var cookie = new HttpCookie("AuthCookie", encrypted) { // 쿠키 보안 적용 HttpOnly = true, Secure   = true,

10. Web Application(웹) 보안

!!! info "PHP"

SameSite = SameSiteMode.Lax }; Response.Cookies.Add(cookie); OpenSSL + HMAC 적용 예시 <?php function setSecureCookie($name, $value, $encKey, $hmacKey) { $iv = random_bytes(16); // AES-256-CBC IV $ciphertext = openssl_encrypt($value, "AES-256-CBC", $encKey, OPENSSL_RAW_DATA, $iv); // HMAC 생성 (무결성 검증용) $hmac = hash_hmac('sha256', $ciphertext, $hmacKey, true); // payload = IV + HMAC + 암호문 $payload = base64_encode($iv . $hmac . $ciphertext); setcookie($name, $payload, [ 'expires' => time() + 3600, 'httponly' => true, 'secure'   => true, 'samesite' => 'Lax' ]); } function getSecureCookie($name, $encKey, $hmacKey) { if (!isset($_COOKIE[$name])) return null; $data = base64_decode($_COOKIE[$name]); $iv = substr($data, 0, 16); $hmac = substr($data, 16, 32); $ciphertext = substr($data, 48); $calcHmac = hash_hmac('sha256', $ciphertext, $hmacKey, true); if (!hash_equals($hmac, $calcHmac)) { return null; // 무결성 검증 실패 } return openssl_decrypt($ciphertext, "AES-256-CBC", $encKey, OPENSSL_RAW_DATA, $iv); } ?>

