# FD: Web Application(웹)

**분류**: 10_webapp

**중요도**: 상

---

10. Web Application(웹) 보안

15. 파일 다운로드

## 개요

### 점검 내용

웹 사이트에서 허용된 경로 외 다른 경로의 파일 접근 및 다운로드 가능 여부 점검

### 점검 목적

허용된 경로 외 다른 경로의 비인가된 접근을 방지하여, 공격자가 임의의 경로에 존재하는 파일을 열람하거나 다운로드하는 것을 차단하기 위함

### 보안 위협

Ÿ 해당 취약점이 존재할 경우, 공격자는 파일 다운로드 시 애플리케이션의 파라미터 값을 조작하여 웹 사이트의 중요한 파일(DB 커넥션 파일, 애플리케이션 파일 등) 또는 웹 애플리케이션 서버 루트에 있는 중요한 설정 파일(/etc/passwd, /etc/shadow 등)을 다운로드할 수 있음 Ÿ CGI, JSP, PHP 등 파일 다운로드 기능을 제공하는 애플리케이션에서 입력 경로를 검증하지 않는 경우, 공격자는 임의의 문자(../.. 등)나 주요 파일명을 입력하여 웹 애플리케이션 서버의 홈 디렉터리를 벗어나 임의의 위치에 있는 파일을 열람하거나 다운로드할 수 있음

### 참고

!!! info "경로 추적"
    웹 서버와 웹 애플리케이션의 파일 또는 디렉터리에 대한 접근이 적절히 통제되지 않아,

중요한 파일과 데이터에 비인가된 접근을 허용하는 취약점

!!! info "소스코드 및 취약점 점검 필요"

## 점검 대상 및 판단 기준

### 대상

웹 애플리케이션 소스코드, 웹 애플리케이션 서버, 웹 방화벽

### 판단 기준

**✅ 양호**: 입력값이 검증되어 허용된 경로와 파일만 접근 가능하고, 경로 조작 및 임의 시스템 파일

다운로드가 불가능하며, 다운로드 디렉터리 외의 접근과 상위 디렉터리 접근이 차단된 경우

**❌ 취약**: 입력값이 검증없이 처리되어 임의의 경로로 접근이 가능하거나 비인가된 파일을 다운로드할 수

있는 경우

## 조치 방법

다운로드 시 허용된 경로 이외의 디렉터리와 파일에 접근할 수 없도록 구현하고, 서버 사이드에서 ../..와 같은 경로 이동 관련 문자열에 대해 입력값 검증을 수행하여 비인가된 접근을 차단함

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

**점검 방법**

**Step 1) 웹 사이트 내 파일 다운로드 기능 식별 후 파일 다운로드 요청 및 응답 패킷 내 URL 파라미터, POST**

데이터, 쿠키 등을 통해 파일 이름 및 경로 노출 여부 확인(또는 웹 사이트 내 이미지 렌더링 시 이미지 파일의 경로를 참조하는 경우)

**[ 프록시 도구를 이용한 파일 다운로드 파라미터 확인 ]**

**Step 2) 파일 다운로드 시 요청 패킷 내 파일 경로를 상대 경로(../../ 또는 ..\..\)로 변조하여 요청할 경우**

정상적으로 해당 경로 내 파일 다운로드 여부 확인

**[ 프록시 도구를 이용한 경로 조작 및 임의 시스템 파일 다운로드 시도 ]**

10. Web Application(웹) 보안

**Step 3) "Step 2"에서 파일 다운로드가 불가능한 경우 변조한 파일 경로를 아래의 인코딩(또는 치환, 종단 문자**

추가)을 적용하여 우회 가능 여부 확인

**[ 입력값 인코딩을 통한 우회 공격 시도 ]**

!!! info "참고"
    운영체제별 중요 시스템 파일 예시

구분 파일 경로 설명 Linux /etc/passwd 시스템유저 계정 리스트 /etc/group 시스템그룹 정보 리스트 /etc/hosts 호스트명과 IP주소 매핑 정보 /var/log/message 시스템 메시지 로그 ~.bash_history 사용자 명령어 기록 /etc/service 활성화된 서비스 정보 Windows C:\Windows\System32\config\SAM 사용자 계정 및 암호의 해시 정보 C:\Windows\System32\config\SYSTEM 시스템 설정과 관련된 레지스트리 파일 C:\Windows\System32\config\AppEvent.evt 응용프로그램 이벤트 로그 C:\Windows\System32\config\SecEvent.evt 보안 이벤트 로그 C:\Windows\System32\drivers\etc\hosts 호스트명과 IP주소 매핑 정보

!!! info "참고"
    우회 방안 예시

!!! info "인코딩 적용"
    필터링 시스템이 특정 문자열 패턴을 차단하는 경우 문자열을 인코딩하여 우회 시도

인코딩 방식 사용 예시 16bit 유니코드 인코딩 .(%u002e), /(%u2215), \(%u2216) URL 인코딩 .(%2e), /(%2f), \(%5c) 더블 URL 인코딩 .(%252e), /(%252f), \(%255c) Base64인코딩 ../../etc/passwd (Li4vLi4vZXRjL3Bhc3N3ZA==) HTML 엔티티 인코딩 ..&#x2F;..&#x2F;etc&#x2F;passwd

!!! info "특수문자 중첩 사용"
    필터링 시스템이 단일 패턴(../)을 감지하여 소거할 경우, 중복된 경로 문자를 사용하여

우회( ....// → ../ )

!!! info "종단 문자 추가"
    필터링 시스템이 특정 확장자만 허용할 때 Nullbyte, 개행 문자 등을 사용하여 우회([파일

명]%00.jpg , [파일명]%0a.jpg 등)

!!! info "문자열 패턴 검증 우회"
    시스템 파일 관련 단어(etc, passwd 등)들을 필터링할 때 대소문자 변환(EtC, PaSswD

등), URL인코딩과 조합(e%74c%2Fpa%73%73wd) 등의 방법을 사용하여 우회

**조치 방법**

1. 파일 다운로드의 취약점은 주로 파일 이름 조작으로 인해 발생되므로 파일의 이름을 데이터베이스에

저장하고 다운로드 수행 시 요청 파일 이름과 비교하여 적절한지 확인하여 사용자가 조작할 수 있는 공격 포인트를 제거

2. 파일 다운로드가 가능한 디렉터리를 별도의 파티션에서 관리하거나 특정 디렉터리로 한정하여 다른

디렉터리에서는 파일을 다운받을 수 없도록 설정

3. URL을 통해 파일 경로를 직접 노출시키지 않고, 파일 ID나 토큰을 사용하여 파일 경로를 은닉

4. 공격자가 입력값 변조를 통한 경로 추적이 불가하도록 파일 확장자 등의 일부 패턴을 화이트리스트로

적용하여 검증하고, 경로 추적 관련 특수문자(. / \ % 등) 필터링 로직 구현

10. Web Application(웹) 보안

!!! info "Java"

특수문자 필터링을 통한 입력값 검증 로직 예시 public class FileDownloadUtil { //파일 이름에 영문,숫자,일부 특수문자를 제외한 \,/ 문자 등이 탐지되었을 때 다운로드 차단 public static boolean isValidFileName(String fileName) { return fileName != null && fileName.matches("^[a-zA-Z0-9._-]+$"); } private static final Set<String> ALLOWED_EXTENSIONS; //허용 확장자 리스트 static { Set<String> tempSet = new HashSet<>(); tempSet.add("jpg"); tempSet.add("png"); ALLOWED_EXTENSIONS = Collections.unmodifiableSet(tempSet); } public static boolean isAllowedExtension(String filePath) { String extension = filePath.substring(filePath.lastIndexOf(".") + 1).toLowerCase(); // 파일 확장자 검증 return ALLOWED_EXTENSIONS.contains(extension); } } ... @GetMapping("/downloadFile") public ResponseEntity<Resource> downloadFile(@RequestParam String filename, HttpServletResponse response) throws IOException { if(!FileDownloadUtil.isValidFileName(filename) || !FileDownloadUtil.isAllowedExtension(filename)) { // 다운로드 컨트롤러에 검증 로직 적용 return ResponseEntity.badRequest().build(); } ...

!!! info "ASP.NET"

!!! info "경로 정규화"
    다양한 형태의 파일 경로를 표준화된 형식으로 변환시키는 과정으로 ../ 와 같은 상대 경로

참조를 해석하고 제거하여 허가되지 않은 디렉터리 접근을 차단함 경로 정규화를 통한 경로 검증 로직 예시 filename = Path.GetFileName(filename); var uploadsFolder = Path.Combine(_environment.WebRootPath, "uploads"); //지정된 업로드 폴더와 정규화된 파일 이름을 결합 var filePath = Path.GetFullPath(Path.Combine(uploadsFolder, filename)); ... if (!filePath.StartsWith(uploadsFolder, StringComparison.OrdinalIgnoreCase)) { //해당 파일이 업로드 폴더 내 존재하는지 검증 return BadRequest("Invalid file path."); } ...

!!! info "PHP"

!!! info "php.ini 에서 magic_quotes_gpc를 On으로 설정하여 .\./ 와 같은 역 슬러시 문자 입력 시 치환되도록 설정이"

가능하나, PHP 5.4.0 이후로는 해당 기능이 제거됨

!!! info "filter_input() 함수를 이용해 사용자 입력 데이터를 안전하게 필터링할 수 있으며, realpath() 함수를 이용해"

경로를 정규화하고 실제 경로를 반환하므로, 이를 통해 상위 경로 탐색을 방지함 경로 정규화를 통한 경로 검증 로직 예시 // 파일 다운로드 요청 처리 if (isset($_GET['file'])) { // file 파라미터 검증 및 처리 $file = filter_input(INPUT_GET, 'file', FILTER_SANITIZE_STRING); $filePath = realpath('../uploads/' . $file); //지정된 업로드 디렉터리에 실제 존재하는지 확인 // 파일 경로가 지정된 디렉토리 내에 있는지 확인 if ($filePath && strpos($filePath, realpath('../uploads/')) === 0) { downloadFile($filePath); } else { echo "잘못된 파일 경로입니다."; exit; ...

