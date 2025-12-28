# SF: Web Application(웹)

**분류**: 10_webapp

**중요도**: 상

---

10. Web Application(웹) 보안

8. 서버사이드 요청 위조(SSRF)

## 개요

### 점검 내용

입력값을 통해 외부에서 직접적인 접근이 제한된 내부 서버 자원에 접근하여 악의적인 요청을 처리하거나 중요 정보의 유출 여부 점검

### 점검 목적

입력값 검증을 통해 내부 서버 자원에 대한 비인가 접근을 차단하여 중요 정보(개인정보, 금융 정보 등) 탈취, 데이터 변조, 임의 명령 실행 등 악의적인 행위를 방지하기 위함

### 보안 위협

서버 간 통신 시 입력값에 대한 검증이 미흡할 경우, 외부에서 접근이 제한된 내부 서버 자원에 대한 정보 수집, 중요 정보(개인정보, 금융 정보, 인사 정보 등) 탈취, 임의 명령 실행, 클라우드 환경 내 메타데이터 수집을 통한 네트워크 인프라 장악이 가능함

### 참고

!!! info "소스코드 및 취약점 점검 필요"

## 점검 대상 및 판단 기준

### 대상

웹 애플리케이션 소스코드, 웹 애플리케이션 서버, 웹 방화벽, API 서버

### 판단 기준

**✅ 양호**: 외부 입력값이 화이트리스트 방식으로 검증되어, 허용된 URL 또는 IP 범위 내에서만 처리될 경우

**❌ 취약**: 외부 입력값이 검증이 이루어지지 않고 처리되어 허용되지 않는 자원에 임의적인 접근 및 요청이

가능한 경우

## 조치 방법

입력값 검증 및 화이트리스트를 적용하여 허용된 URL과 IP주소만 접근 가능하도록 설정하며, 네트워크를 분리하여 내부 자원에 대한 비인가 접근을 차단

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

**점검 방법**

**Step 1) 사용자 입력을 통해 서버 간 통신이 이루어지는 지점에서 허용되지 않은 주소값을 입력하여 응답, 지연**

시간 등을 분석해 취약점 가능성 확인

**[ 서버 간 통신 유무 확인 ]**

**[ 입력값에 대한 내부 서버 응답값 노출 여부 확인 ]**

**Step 2) 습득한 정보를 바탕으로 우회 기법, 포트 스캔, 내부 정보 탈취 등 익스플로잇 시도 및 영향 평가**

10. Web Application(웹) 보안

**[ 심화 공격 수행 ]**

**조치 방법**

1. 외부 요청에 대해 허용된 URL이나 IP주소를 화이트리스트로 정의하여 허용된 대상에만 접근이 가능하도록

설정

2. 내부 네트워크 대역 및 관리용 포트에 대한 요청을 감지하고 차단

3. URL 접근에 실패할 경우 사용자에게 에러 정보나 응답값을 노출하지 않고, 일반적인 에러메시지 출력

4. http, https 외의 다른 프로토콜 (FTP, SMB, SMTP 등)과 URL 스키마(file://, gopher://, data://, dict:// 등)에

대한 접근을 차단해야 하며, 내부 호스트명이 외부에 노출되지 않도록 DNS 설정을 조정

5. 애플리케이션 서버와 중요 내부 시스템간 네트워크 분리를 통하여 불필요한 통신을 제한하여 권한 없는 접근

과 외부로부터의 직접적인 접근을 방지

!!! info "Java"

화이트 리스트 방식을 이용한 URL 및 IP주소 접근 제한 로직 예시 ... private final List<String> allowedDomains = Arrays.asList("example.com", ...); private final Map<String, List<Integer>> allowedIPsAndPorts = new HashMap<>(); public UrlValidator() { allowedIPsAndPorts.put("192.168.1.100", Arrays.asList(80, 443, 8080)); allowedIPsAndPorts.put("10.0.0.1", Arrays.asList(80, 443)); } ... public boolean isUrlAllowed(String urlString) { try {

!!! info "ASP.NET"

URL url = new URL(urlString); String protocol = url.getProtocol(); // HTTP와 HTTPS 스키마만 허용 if (!("http".equalsIgnoreCase(protocol) || "https".equalsIgnoreCase(protocol))) { return false; } String host = url.getHost(); int port = url.getPort() == -1 ? url.getDefaultPort() : url.getPort(); if (allowedDomains.contains(host)) { return true; } if (allowedIPsAndPorts.containsKey(host)) { return allowedIPsAndPorts.get(host).contains(port); } return false; } catch (Exception e) { return false; } } ... 화이트 리스트 방식을 이용한 URL 및 IP주소 접근 제한 로직 예시 ... private readonly List<string> _allowedDomains = new List<string> { "example.com", ... }; private readonly Dictionary<string, List<int>> _allowedIPsAndPorts = new Dictionary<string, List<int>> { { "127.0.0.1", new List<int> { 80, 443, 8000 } }, { "10.0.0.1", new List<int> { 80, 443 } } }; public bool IsUrlAllowed(string urlString) {

10. Web Application(웹) 보안

!!! info "PHP"

if (!Uri.TryCreate(urlString, UriKind.Absolute, out Uri uri)) { return false; } // HTTP와 HTTPS 스키마만 허용 if (uri.Scheme != Uri.UriSchemeHttp && uri.Scheme != Uri.UriSchemeHttps) { return false; } string host = uri.Host; int port = uri.Port; // 도메인 확인 if (_allowedDomains.Contains(host)) { return true; } // IP주소와 포트 확인 if (_allowedIPsAndPorts.TryGetValue(host, out List<int> allowedPorts)) { return allowedPorts.Contains(port); } return false; } ... 화이트 리스트 방식을 이용한 URL 및 IP주소 접근 제한 로직 예시 ... function isUrlAllowed($url) { $allowedDomains = ['example.com', 'api.example.com']; $allowedIPsAndPorts = [ '192.168.10.10' => [80, 443, 8000], '10.0.0.1' => [80, 443] ]; $parsedUrl = parse_url($url); if (!$parsedUrl || !isset($parsedUrl['host'])) {

!!! info "php.ini 파일 내 allow_url_fopen 속성 활성화 시 file_get_contents() 함수를 이용하여 원격 URL에 대하여 접근"

및 검색이 가능. 해당 속성을 비활성화하여 원격 URL을 참조할 수 없도록 제한하며 대안으로 cURL 라이브러 리 사용 return false; } $host = $parsedUrl['host']; $port = isset($parsedUrl['port']) ? $parsedUrl['port'] : ($parsedUrl['scheme'] === 'https' ? 443 : 80); // 도메인 확인 if (in_array($host, $allowedDomains, true)) { return true; } // IP주소와 포트 확인 if (filter_var($host, FILTER_VALIDATE_IP)) { if (array_key_exists($host, $allowedIPsAndPorts)) { return in_array($port, $allowedIPsAndPorts[$host], true); } } return false; } ... cURL 라이브러리 사용 예시 ... function fetchUrl($url) { // cURL 세션 초기화 $ch = curl_init(); // cURL 옵션 설정 curl_setopt_array($ch, [ CURLOPT_URL => $url, CURLOPT_RETURNTRANSFER => false,     // 결과를 문자열로 반환 CURLOPT_FOLLOWLOCATION => false,     // 리다이렉트 제한(기본값: false) CURLOPT_MAXREDIRS => 3,             // 최대 리다이렉트 횟수 지정 CURLOPT_TIMEOUT => 30,              // 세션의 최대 허용 시간 지정 (초) // 접근 가능한 프로토콜을 http, https로 제한 CURLOPT_PROTOCOLS => CURLPROTO_HTTP | CURLPROTO_HTTPS,

10. Web Application(웹) 보안

!!! info "allow_url_include 속성 활성화 시 원격 URL을 PHP의 include(), require() 함수를 통해서 사용 가능. 공격자가"

악의적인 코드가 포함된 원격 파일을 실행할 수 있는 위험이 존재하므로 php.ini 파일 내 해당 속성 비활성화 allow_url_include 및 allow_url_fopen 속성 비활성화 ... ;;;;;;;;;;;;;;;;;; ; Fopen wrappers ; ;;;;;;;;;;;;;;;;;; ; Whether to allow the treatment of URLs (like http:// or ftp://) as files. ; https://php.net/allow-url-fopen allow_url_fopen=Off ; Whether to allow include/require to open URLs (like https:// or ftp://) as files. ; https://php.net/allow-url-include allow_url_include=Off ... CURLOPT_SSL_VERIFYPEER => true,     // SSL 인증서 검증 ]); // cURL 실행 및 결과 저장 $response = curl_exec($ch); $error = curl_error($ch); $info = curl_getinfo($ch); // cURL 세션 종료 curl_close($ch); // 결과 반환 return [ 'success' => ($error === ''), 'content' => $response, 'error' => $error, 'info' => $info ]; } ...

