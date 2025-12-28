# IA: Web Application(웹)

**분류**: 10_webapp

**중요도**: 상

---

10. Web Application(웹) 보안

10. 불충분한 인증 절차

## 개요

### 점검 내용

중요 페이지 접근 시 추가 인증 절차 존재 여부 및 인증 로직 우회 여부 점검

### 점검 목적

중요 페이지 접근 시 추가 인증 절차를 도입하고, 서버 사이드에서 인증 여부를 검증하여 불필요한 정보 노출 및 변조를 차단하기 위함

### 보안 위협

중요 페이지 및 인증 로직(개인정보 수정, 본인인증, OTP 인증 등)에 대한 인증 절차가 미흡할 경우 무단 접근으로 인해 중요 정보가 유출되거나 변조될 가능성이 있으므로, 해당 구간에는 추가적인 인증 절차를 서버 사이드 방식으로 구현 및 검증

### 참고

!!! info "소스코드 및 취약점 점검 필요"

## 점검 대상 및 판단 기준

### 대상

웹 애플리케이션 소스코드

### 판단 기준

**✅ 양호**: 중요 정보 페이지 접근 시 추가 인증 절차가 존재하며, 인증 로직이 서버 사이드에서 구현되어

우회가 불가능한 경우

**❌ 취약**: 중요 정보 페이지 접근 시 추가 인증 절차가 존재하지 않거나, 인증 로직 우회가 가능하여

비인가자가 접근 가능할 경우

## 조치 방법

중요 페이지에 대한 추가 인증 절차를 도입하고, 서버 사이드에서 인증 여부를 검증하는 로직 구현

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

**점검 방법**

**Step 1)  중요 정보(개인정보 변경 등) 페이지 접근 시 재인증 절차 존재 여부 확인**

**[ 2차 인증 유무 확인 ]**

**Step 2)  로직 변조, 파라미터 변조 등의 행위를 통하여 인증 로직(OTP 인증, 휴대폰 본인인증 등)의 우회 여부**

확인

**[ 인증 로직 우회 여부 확인 ]**

**조치 방법**

1. 중요 정보를 다루는 페이지에 접근 시 본인인증을 재확인하는 로직을 구현하고, 사용자 승인 여부를

페이지마다 검증

2. 인증 과정을 처리하는 로직 구현 시 클라이언트 사이드 방식으로 구현할 경우 사용자가 임의로 인증 과정에

대한 우회가 가능하므로 서버 사이드 방식을 이용하여 구현

3. 접근 통제 코드는 구조화되고 모듈화되어야 하며, 모든 페이지에 로그인 및 권한 체크 기능을 구현하고 공통

모듈 사용 권장

10. Web Application(웹) 보안

!!! info "재인증 로직 구현"
    중요 정보(개인정보 변경 등)를 표시하거나 수정하는 페이지에 접근 시 사용자가 최근에

본인인증을 수행했는지 확인하는 로직을 구현하며, 사용자가 페이지에 접근할 때마다 세션을 통해 인증된 사용자임을 검증 재인증 로직 구현 예시 ... public String editProfile(HttpSession session, Model model) { User user = (User) session.getAttribute("user"); Boolean isVerified = (Boolean) session.getAttribute("isVerified");    // 세션을 통해 인증 유무 검증 if (user == null || isVerified == null || !isVerified) { return "redirect:/user/authenticate"; } model.addAttribute("user", user); return "edit_profile"; } ... @PostMapping("/verify_code") public String verifyCode(@RequestParam String code, HttpSession session) { if (input.equals(code)) { session.setAttribute("isVerified", true); return "redirect:/user/edit_profile"; } else { return "redirect:/user/authenticate?error=true"; } } ...

!!! info "접근 제어 로직을 별도의 클래스로 분리 및  관리하여 접근 통제 로직을 공통 모듈로 구현하여 코드의 일관성"

유지 접근 통제 공통 모듈 예시 ... public class AccessControl { public static boolean isAuthenticated(HttpSession session) { return session.getAttribute("user") != null; }

!!! info "서버 사이드 스크립트 사용"
    인증과정을 처리할 때 클라이언트 사이드 스크립트(Javascript 등)를 사용하지

않고, 서버 사이드 스크립트(PHP, Java, ASP.NET 등)를 사용하여 인증 및 필터링 과정을 수행하며, 모든 인증 및 권한 검증 로직은 서버 사이드에서 수행함으로써 클라이언트 측에서는 단순히 UI 제공 및 요청 전송 역할만 담당하도록 유도 서버 사이드 인증 모듈 예시 @PostMapping("/login") public String login(@RequestParam String username, @RequestParam String password, HttpSession session, Model model) { User user = userService.findByUsername(username); if (user != null && user.getPassword().equals(password)) { session.setAttribute("user", user); session.setAttribute("isVerified", false);     // 인증 세션 초기값 설정 return "redirect:/user/dashboard"; } else { model.addAttribute("error", "Invalid username or password"); return "login"; } } ... public static boolean isVerified(HttpSession session) { return Boolean.TRUE.equals(session.getAttribute("isVerified")); } } ...

