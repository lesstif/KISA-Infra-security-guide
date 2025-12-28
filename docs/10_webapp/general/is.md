# IS: Web Application(웹)

**분류**: 10_webapp

**중요도**: 상

---

10. Web Application(웹) 보안

16. 불충분한 세션 관리

## 개요

### 점검 내용

세션 만료 기간 설정, 예측 가능한 세션 ID 생성, 고정된 세션 ID 발행 등 세션 관리 정책을 점검하고, 인증 토큰(JWT 등) 사용 시에도 안전한 서명 알고리즘 사용 여부, 안전한 비밀 키 사용 여부, 토큰 만료 등의 정책 점검

### 점검 목적

사용자의 세션 ID를 적절히 관리하여 공격자가 불법적으로 접근하거나 비인가적인 세션 탈취를 차단하기 위함

### 보안 위협

사용자에게 발급되는 세션 ID가 만료되지 않거나, 고정 및 예측 가능한 형태일 경우, 공격자는 해당 세션 ID를 탈취하여 타 사용자나 시스템에 무단 접근할 수 있으며, 이로 인해 중요 데이터의 무결성이 훼손될 수 있음

### 참고

!!! info "세션(Session)"
    일정 시간 동안 같은 사용자(브라우저)로 부터 들어오는 일련의 요구를 하나의

상태로 보고 그 상태를 일정하게 유지시키는 기술

!!! info "JSON Web Token (JWT)"
    헤더, 페이로드, 서명으로 구성되는 JSON 객체 형식이며, 사용자

정보가 클라이언트에 저장되고 서버에 요청 시마다 전송하여 인증 상태를 유지시키는 기술

!!! info "소스코드 및 취약점 점검 필요"

## 점검 대상 및 판단 기준

### 대상

웹 애플리케이션 소스코드, 웹 애플리케이션 서버

### 판단 기준

**✅ 양호**: 추측 불가능한 세션 ID가 발급되고, 세션 종료 시간이 설정되어 있는 경우

**❌ 취약**: 세션 ID가 일정한 패턴으로 발급되거나 세션 종료 시간이 설정되지 않아 세션 재사용이 가능한

경우

## 조치 방법

추측 불가능한 세션 ID가 발급되도록 로직을 구현하고, 세션 종료 시간 설정 또는 자동 로그아웃 기능을 구현하여 사이트 특성에 맞게 적정 시간을 설정

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

**점검 방법**

### 세션(Session)

**Step 1) 로그인 과정에서 발급받은 세션 ID를 확인하고 로그아웃 후 재 로그인 시 각각 발급받은 세션 ID에 대해**

일정한 패턴이 존재하는지 검증

!!! info "각각 발급받은 세션 ID를 확인하여 예측 가능한 패턴으로 생성되는지 검증"

!!! info "각각 발급받은 세션 ID를 확인하여 고정된 값으로 생성되는지 검증"

**[ 세션 발급 시 동일 패턴 유무 확인 ]**

**Step 2) 로그인 후 웹 페이지 사용을 중지한 상태로 일정 시간이 경과하였을 때 세션이 유지되는지 확인**

**[ 일정 시간 경과 시 세션 만료 유무 확인 ]**

10. Web Application(웹) 보안

### JWT(JSON Web Token)

**Step 1) 사용자 인증 및 상태 유지를 위해 JWT(JSON Web Token)가 사용되는지 확인**

(JWT는 일반적으로 Authorization 헤더에 포함되고, 경우에 따라 Cookie 헤더에 포함, 또는 POST 데이터로 전송할 수 있음)

**[ JWT 토큰 사용 유무 확인 ]**

**Step 2) JWT(JSON Web Token)의 헤더와 페이로드 부분을 각각 Base64 디코딩하여 내용을 확인할 수 있으며,**

안전한 알고리즘을 사용하는지, 토큰 만료 설정이 적절하게 되었는지, 민감정보가 노출되는지 등을 확인

**[ JWT 토큰 디코딩 시 민감정보 노출 유무 확인 ]**

**Step 3) 페이로드 변조 후 서명을 삭제하거나 None 알고리즘으로 변조하였을 때 토큰이 유효하여 타 계정의**

권한 탈취가 가능한지 확인

**[ JWT 토큰 익스플로잇 가능성 유무 확인 ]**

**조치 방법**

### 세션 고정 및 예측

1. 공격자가 하나의 유효한 세션 ID를 추측하는 것이 불가능에 가깝도록 길고 복잡한 세션 ID를 생성하여야

하며, 새 로그인 시 기존 세션 ID를 폐기하고 새로운 세션 ID로 발급해야 하며, 이용 중인 계정에 대해 접속 IP와 디바이스 정보를 통해 동시 세션 로그인 여부를 검사해야함

!!! info "Java"

세션 생성 로직 예시 @RequestMapping("/login") public String login(HttpServletRequest request) { request.changeSessionId(); // 신규 로그인 시 세션 ID 변경 return "redirect:/home"; } //Spring Security 설정에서 세션 고정 방지 및 세션 ID 생성 @Override protected void configure(HttpSecurity http) throws Exception { http.sessionManagement() .sessionFixation().migrateSession() .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED) .maximumSessions(1).maxSessionsPreventsLogin(false) .expiredUrl("/login?expired"); }

!!! info "ASP.NET"

web.config 파일 설정을 통한 불규칙 세션 ID 발급 설정 예시 <system.web> ... <!--세션 고정 방지 --> <sessionState regenerateExpiredSessionId="true"> ... <!--세션 예측 방지 --> <machineKey validationKey="AutoGenerate,IsolateApps" decryptionKey="AutoGenerate,IsolateApps" validation="HMACSHA512"

10. Web Application(웹) 보안

!!! info "PHP"

세션 생성 로직 예시 // 기존 세션을 삭제하고 새로운 세션 ID를 생성 session_start(); session_regenerate_id(true); // 예측 불가한 안전한 세션 ID 생성 ini_set('session.entropy_length', '256'); ini_set('session.entropy_file', '/dev/urandom');

### 세션 만료

1. 세션 타임아웃이 미흡하게 설정되어있는 경우 사용자가 로그아웃하지 않고 시스템을 떠날 때 타 사용자가 재

사용할 수 있는 여지가 존재하므로, 세션 타임아웃 기능을 구현하고 서비스에 따라 10~60분으로 설정할 것을 권고(아래 예시들은 60분으로 설정)

!!! info "Java"

web.xml 파일 설정을 통한 세션 만료 시간 설정 예시 <session-config> <session-timeout>60</session-timeout> <!-- 60분 --> </session-config>

!!! info "Spring Boot의 경우 application.properties 및 소스코드를 통하여 세션 만료 제어 가능"

application.properties 파일 설정을 통한 세션 만료 시간 설정 예시 server.servlet.session.timeout=60m 소스 코드를 통한 세션 만료 시간 설정 예시 public String login(HttpSession session) { session.setMaxInactiveInterval(3600); // 60분 (3600초) return "Session timeout set to 60 minutes"; ... decryption="AES" />

!!! info "ASP.NET"

web.config 파일 설정을 통한 세션 만료 시간 설정 예시 <configuration> <system.web> <sessionState timeout="60" /> <!-- 60분 --> </system.web> </configuration>

!!! info "Global.asax 파일에서 세션을 확인하고 접근을 제어하는 로직 추가 예시"

(ASP.NET Core의 경우 Startup.cs 파일에서 ConfigureServices, Configure 메소드 수정) Global.asax 파일을 통한 세션 만료 시간 설정 예시 ... public class Global : HttpApplication { void Session_Start(object sender, EventArgs e) { Session.Timeout = 60; // 60분 } } ...

!!! info "PHP"

php.ini 파일 설정을 통한 세션 만료 시간 설정 예시 session.gc_maxlifetime = 3600 ; 60분 (3600초) session.cookie_lifetime = 3600 ; 60분 (3600초) .htaccess 파일 설정을 통한 세션 만료 시간 설정 예시 php_value session.gc_maxlifetime 3600 php_value session.cookie_lifetime 3600 소스 코드를 통한 세션 만료 시간 설정 예시 ... ini_set('session.gc_maxlifetime', 3600); // 60분 (3600초) ini_set('session.cookie_lifetime', 3600); // 60분 (3600초) session_start(); ...

10. Web Application(웹) 보안

### JWT(JSON Web Token)

1. 서명이 없는 JWT(JSON Web Token)는 쉽게 조작할 수 있으므로, 서명이 포함된 JWT를 사용하고 해당

서명에 대한 검증 로직 구현

2. 취약한 서명 알고리즘을 사용하는 경우 공격자가 서명을 위조할 수 있으므로, 강력한 서명 알고리즘 사용

권고

3. 공격자가 JWT의 알고리즘을 none으로 설정하여 서명을 우회할 수 있으므로 토큰의 알고리즘을 명시적으로

설정

!!! info "안전한 서명 알고리즘을 사용하더라도 약한 키(짧거나 예측 가능한 키)는 추측될 위험이 있으므로, 강력하고"

랜덤한 비밀 키를 사용하고, 유출되지 않도록 키를 안전하게 관리

!!! info "토큰이 재사용되지 않도록 짧은 만료 시간을 설정하고, 리프레시 토큰을 사용하여 주기적으로 토큰을 갱신"

안전한 JWT 서명 알고리즘 HS256~512 비밀 키를 사용하여 메시지에 해시를 적용 (숫자가 높을수록 긴 출력 길이를 가지며, 높은 수준의 보안을 제공) RS256~512 비대칭 키 쌍을 사용하여 서명 생성. 서명은 개인 키로 생성되고 공개 키로 검증 ES256~512 타원 곡선 암호화를 사용하여 서명을 생성

**❌ 취약한 JWT 서명 알고리즘**

HS1

**❌ 취약한 암호화 기술인 SHA-1을 기반으로 함**

RS1

**❌ 취약한 암호화 기술인 SHA-1을 기반으로 함**

none 서명을 생략하여 무결성 검증에 취약함 plaintext 서명을 평문으로 전달하여, 무결성 검증에 취약함

