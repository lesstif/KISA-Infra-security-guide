# CI: Web Application(웹)

**분류**: 10_webapp

**중요도**: 상

---

10. Web Application(웹) 보안

1. 코드 인젝션 (Code Injection)

## 개요

### 점검 내용

웹 애플리케이션 내 다양한 인젝션 공격(LDAP, 운영체제 명령 실행, SSI, XPATH, XML, SSTI 인젝션 등)에 대해 외부 입력값이 쿼리나 명령어로 삽입되어 비인가된 접근이나 코드 실행의 가능 유무 점검

### 점검 목적

허용되지 않은 코드 및 쿼리 실행을 방지하여 비인가된 접근, 데이터 유출, 시스템 변조, 악성 코드 실행 등의 위협을 차단하여, 데이터 보호와 시스템 안정성을 확보하기 위함

### 보안 위협

해당 취약점이 존재하는 경우 비인가된 데이터 접근으로 민감 정보가 탈취될 수 있으며, 시스템 명령어나 스크립트가 실행되어 서버 제어나 악성 코드 실행이 가능하고, 데이터 무결성이 훼손되어 정보의 신뢰성이 떨어지며, 서비스 거부 공격(DoS)으로 시스템 가용성이 저하될 수 있음. 따라서 "|", ";", "`", "<" 등의 특수 문자에 대한 필터링 구현과 함께 입력값 검증, 화이트리스트 적용 등의 추가적인 보안 조치가 필요함

### 참고

!!! info "LDAP 인젝션"
    입력값이 LDAP(Lightweight Directory Access Protocol) 쿼리에 삽입되어

디렉터리 서비스 데이터에 대한 비인가된 조회, 수정, 삭제를 유발하는 공격

!!! info "운영체제 명령 실행"
    입력값이 시스템 명령어로 실행되어 서버의 운영체제 명령을 비인가로

수행하거나 민감한 시스템 정보를 노출하는 공격

!!! info "SSI 인젝션"
    입력값이 서버 사이드 인클루드(Server Side Include) 명령어로 실행되어 웹

애플리케이션 서버에서 비인가된 스크립트 실행이나 파일 접근을 유발하는 공격

!!! info "XPath 인젝션"
    입력값이 XPath 쿼리에 삽입되어 XML 데이터의 비인가된 조회, 추가, 삭제를

유발하는 공격

!!! info "XXE 인젝션"
    입력값이 XML 문서나 쿼리에 삽입되어 XML 데이터에 대한 비인가된 접근, 외부

엔티티 참조(XML External Entities)를 통한 시스템 정보 유출을 유발하는 공격

!!! info "SSTI 인젝션"
    입력값이 서버 사이드 템플릿 엔진에 삽입되어 템플릿 렌더링 과정에서 비인가된 코드

실행이나 서버 내부 데이터 노출을 유발하는 공격

!!! info "소스코드 및 취약점 점검 필요"

## 점검 대상 및 판단 기준

### 대상

웹 애플리케이션 소스코드, 웹 방화벽

### 판단 기준

**✅ 양호**: 임의의 입력값에 대하여 철저한 검증이 이루어져, 허용되지 않은 값이 필터링되고 허용된 값만

처리되는 경우

**❌ 취약**: 임의의 입력값에 대하여 검증 없이 명령이 실행되는 경우

## 조치 방법

화이트리스트 방식으로 쿼리를 허용된 값만 처리하고, 특수 문자에 대해 입력값 검증

### 조치 시 영향

일반적인 경우 영향 없음

## 점검 및 조치 사례

### LDAP(Lightweght Directory Access Protocol) 인젝션

**점검 방법**

**Step 1) 사용자 입력값에 대하여 변조된 LDAP 쿼리 삽입 후 실행 가능 여부 확인**

**[ LDAP 쿼리 삽입 ]**

**조치 방법**

1. 사용자 입력값을 화이트리스트로 지정하여 영문(a-z, A-Z)과 숫자(0-9)만을 허용

2. 특수문자를 사용해야 하는 경우 입력값(DN에 사용되는 특수문자는 '\'를 붙여 이스케이프 처리, 필터에 사용

되는 특수문자는 '= + < > # \ , ; 앞뒤 공백' 등)에 대해서는 실행 명령이 아닌 일반문자로 인식되도록 처리

3. DN과 필터에 사용되는 사용자 입력값에는 특수문자 제거

4. 웹 방화벽에 LDAP 관련 특수문자를 필터링하도록 룰셋 적용

!!! info "특수문자 필터링 예시"

구분 필터링 예시 변경 전 \ = + * ( ) \0 변경 후 \5c \3d \2b \2a \28 \29 \00

!!! info "필터링 대상 예시"

필터링 대상 예시 ' " - # ( ) < > = /* */ + * ; & | \ \0 : ` % user_tables table_name column_name Syscolumns union select insert drop update and or If join substring from where declare substr openrowset xp_ sysobject

10. Web Application(웹) 보안

### 운영체제 명령실행

**점검 방법**

**Step 1) 웹 애플리케이션 기능 내 전달되는 파라미터 값에 대하여 운영체제 명령어 삽입 후 실행 여부 확인**

**[ 임의 운영체제 명령어 삽입 ]**

**조치 방법**

1. 웹 애플리케이션 설계 시 운영체제로부터 명령어를 직접적으로 호출하지 않도록 구현하고, 언어/프레임워크

에서 제공하는 안전한 API 사용

2. 명령어를 직접 호출하는 기능이 필요한 경우에는, 데이터가 OS의 명령어 해석기에 전달되기 전에 화이트리스

트 기반으로 입력값을 검증/확인하도록 구현

3. 입력값에 대한 파라미터 데이터의 필터링 처리

- Unix/Linux :　&, &&, |, ||, ;, `, $(), <, > 등 - Windows : &, |, ^, % 등

4. 웹 서버 및 웹 애플리케이션 서버는 공개적으로 알려진 취약점이 제거된 상위 버전으로 업데이트

KISA 보호나라&KrCERT/CC 알림마당 > 보안공지(https://www.boho.or.kr/)

5. 웹 방화벽에 모든 사용자 입력값을 대상으로 악용될 수 있는 특수문자 및 키워드 등에 대한 룰셋 적용

!!! info "특수문자 필터링 처리 문자 설명"

구분 상세 설명 & 첫 번째 명령어는 백그라운드에서 실행되며, 두 번째 명령어는 즉시 실행 && 첫 번째 명령어가 성공했을 때만 두 번째 명령어 실행 | 두 명령어를 연결하여, 첫 번째 명령어의 출력을 두 번째 명령어의 입력으로 전달 ; 첫 번째 명령어 실행 후 성공여부와 무관하게 두 번째 명령어 실행 ` ` 또는 $() 백틱 또는 괄호 안에 있는 명령어를 실행하고 그 출력을 반환 > 또는 >> 명령 실행 결과를 파일로 생성(덮어쓰기 또는 추가) < 파일 내용을 명령어 입력으로 전달 ^ (Windows) 명령어 이스케이프/제어 문자로 사용 % (Windows) 환경 변수 치환 \ 이스케이프 문자 또는 뒤에 오는 특수문자를 무효화하거나 명령 연결 시 사용

10. Web Application(웹) 보안

### SSI(Server Side Includes) 인젝션

**점검 방법**

**Step 1) 사용자가 입력 가능한 파라미터 값에 <!--#echo var="DOCUMENT_ROOT" -->를 삽입하여 전송 후 반**

환되는 페이지에 사이트의 홈 디렉터리가 표시되는지 확인

**[ Server Side Includes 지시어 삽입 및 취약점 유무 판단 ]**

**Step 2) 사용자가 입력 가능한 파라미터 값에 <!-- #exec cmd="ls -al" -->를 삽입하여 전송 후 반환되는 페이지에**

디렉터리의 파일 리스트가 표시되는지 확인

**[ 심화 공격 수행 ]**

**Step 3) HTTP 요청(Request) 헤더에 명령어를 삽입하여 실행되는지 확인**

GET / HTTP/1.0 Referer: <!--#exec cmd="/bin/ps ax"--> User-Agent: <!--#include virtual="/proc/version"-->

**조치 방법**

1. 화이트리스트 방식으로 사용자 입력에 대한 사용 가능한 문자들을 정의하여 정해진 문자를 제외한 나머지 모

든 문자들을 필터링 처리

2. 필터링 해야 하는 대상은 GET 질의 문자열, POST 데이터, 쿠키, URL, 그리고 일반적으로 브라우저와 웹 서버

가 주고받는 모든 데이터를 포함하며, 아래는 특수문자에 대한 엔티티 형태를 표시한 것임

3. 웹 서버의 SSI 기능을 사용하지 않거나, 웹 방화벽에 특수문자를 필터링하도록 룰셋 적용

구분 필터링 예시 변경 전 < > " ( ) # & 변경 후 &lt; &gt; &quot; &#40; &#41; &#35; &amp;

### XPath 인젝션

**점검 방법**

**Step 1) 취약점 존재 유무 판단을 위한 Xpath 쿼리(' or '1'='1, ' and 'a' = 'b 등) 삽입**

(※ 예시로 제시한 것으로, 웹 사이트 환경에 맞춰 점검해야 함)

**[ XPath 구문 삽입 시도 및 취약점 유무 판단 ]**

**Step 2) 추가적인 쿼리 질의를 통하여 데이터 추출 등의 타당성 검토**

' or count(parent::*[position()=1])=0 or 'a'='b 1' or string-length(username)=[COUNT] and '1'='1... 등

**[ 심화 공격 수행 ]**

**조치 방법**

1. XPath 쿼리에 사용자가 값을 입력할 수 있는 경우, 입력값 검증을 통해 필요 문자만을 받아들이게 함. ( ) = ' [ ]

: , * / 등의 오동작를 유발하는 특수문자는 제한하며, 특정 특수문자만을 필터링하는 것이 아닌 허용된 문자 이외의 모든 입력을 화이트리스트 방식으로 필터링 처리

2. Xpath 및 XQuery의 쿼리 삽입 시 사용되는 특수문자를 필터링하도록 웹 방화벽 룰셋 적용

10. Web Application(웹) 보안

### XXE(XML External Entities) 인젝션

**점검 방법**

**Step 1) 파일 업로드 페이지, API 엔드포인트, HTML Form 등 XML을 파싱하는 페이지 포인트 내 XML 객체 삽**

입 시도 (※ 예로 제시한 것으로, 웹 사이트 환경에 맞춰 점검해야 함) <?xml version="1.0" encoding="ISO-8859-1"?> <!DOCTYPE foo [ <!ELEMENT foo ANY > <!ENTITY xxe SYSTEM "file:///C:/Windows/System32/drivers/etc/hosts" > ]> <foo>&xxe;</foo>

**[ 외부 Entity 객체 삽입 및 XXE 공격 수행 ]**

**조치 방법**

1. 허용된 태그와 속성만 사용하도록 화이트리스트 방식을 이용한 입력값 검증 로직 구현

2. 최근 언어별 XML 파서의 경우, 기본적으로 외부 엔티티 처리가 비활성화되어 있으나 소스코드 상에서

명시적으로 비활성화 처리하는 것이 안전한 방법

3. 의도하지 않은 오동작이 발생할 가능성이 존재하는 외부 엔티티 참조 명령어 및 주요 스키마 등에 대하여 웹

방화벽에 룰셋 적용

!!! info "Java"

Java DTD(외부 엔티티) 비활성화 예시 ... // 공통 보안 설정 dbf.setXIncludeAware(false); ... if (mode == SecurityMode.FULL_SECURE) { dbf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true); dbf.setFeature("http://xml.org/sax/features/external-general-entities", false); dbf.setFeature("http://xml.org/sax/features/external-parameter-entities", false); dbf.setFeature("http://apache.org/xml/features/nonvalidating/load-external-dtd", false); } else if (mode == SecurityMode.LIMITED_SECURE) { // DTD를 완전히 비활성화할 수 없는 경우 dbf.setFeature("http://xml.org/sax/features/external-general-entities", false); dbf.setFeature("http://xml.org/sax/features/external-parameter-entities", false); dbf.setFeature("http://apache.org/xml/features/nonvalidating/load-external-dtd", false); } ...

!!! info "ASP.NET"

ASP.NET DTD(외부 엔티티) 비활성화 예시 ... // XmlDocument 객체 생성 XmlDocument doc = new XmlDocument(); // XmlResolver를 null로 설정하여 외부 엔티티의 해석을 비활성화 doc.XmlResolver = null ...

!!! info "PHP 8.0 이전"

PHP 8.0 이전 DTD(외부 엔티티) 비활성화 예시 ... libxml_disable_entity_loader(true); ...

10. Web Application(웹) 보안

!!! info "PHP 8.0 이후"

!!! info "libxml_disable_entity_loader() 함수가 더 이상 사용되지 않으며, 외부 엔티티 로드는 기본적으로 비활성화되"

어있지만, LIBXML_NOENT 플래그로 인하여 XML 파서가 외부 엔티티를 확장하도록 설정되어있는 경우 X XE 취약점 발생 가능 PHP 8.0 이후 DTD(외부 엔티티) 비활성화 예시 ... // DOMDocument 인스턴스 생성 $dom = new DOMDocument(); // 외부 엔티티 비활성화 $dom->loadXML($xmlfile, LIBXML_NONET); ...

### SSTI(Server Side Template Injection)

**점검 방법**

**Step 1) 사용자 입력값이 서버 템플릿 엔진 내에서 처리되는지 확인하기 위해 {{7*7}} 등의 수식 삽입 시도**

**[ 템플릿 엔진 수식 삽입을 통한 취약점 유무 판단 ]**

**Step 2) 상위 컨텍스트 및 객체 접근을 시도하여 원격 코드 실행의 가능 여부를 판별하기 위하여 페이로드 입력**

및 실행 유무 확인

**[ 심화 공격 수행 ]**

**조치 방법**

1. 템플릿 언어에서 예약된 의미를 가지는 문자({ } < > % # @ 등)에 대하여 이스케이프 처리

2. 템플릿 엔진의 안전 모드를 사용함으로써 템플릿 내에서 실행할 수 있는 명령을 제한하여 임의 코드 실행 방

지

3. 안전 모드 및 내장 함수를 활용하여 보안 조치를 하는 경우 엔진의 버전과 그 버전에서 지원하는 기능을 고려

하여 적절한 보안 패치 진행

4. 에러 메시지를 통해 사용된 템플릿 언어 및 관련 취약점에 대한 유의미한 정보를 제공할 가능성이 존재하므

로, 노출되는 에러 메시지를 제한

5. 템플릿 엔진 내 사용되는 특수문자를 필터링하도록 웹 방화벽 룰셋 적용

!!! info "Java"

사용자 입력값 특수문자 인코딩 처리 예시 ... name = org.apache.commons.text.StringEscapeUtils.escapeHtml4(name); // 사용자 입력값 인코딩 // 사용자 입력값 이스케이프 처리 public static String escapeSpecialCharacters(String input) {

10. Web Application(웹) 보안

!!! info "Velocity 템플릿 엔진에서 제공하는 '$esc.html'를 사용하여 사용자 입력을  HTML 인코딩 처리"

Java(Velocity) 안전한 템플릿 사용 예시 ... #set($userInput = $esc.html($params.get("userInput"))) <p>사용자 입력: $userInput</p> ...

!!! info "FreeMarker 템플릿 엔진의 내장 함수인 '?html'를 사용하여 사용자 입력을 HTML 인코딩 처리"

Java(FreeMarker) 안전한 템플릿 사용 예시 ... <p>사용자 입력: ${userInput?html}!</p> ...

!!! info "ASP.NET"

if (input == null) return null; return input.replaceAll("([*{}\\[\\]<>%#@])", "\\\\$1"); } 사용자 입력값 특수문자 인코딩 처리 예시 // 사용자 입력값 인코딩 ... private static readonly Dictionary<char, string> HtmlEntities = new Dictionary<char, string> { { '*', "&#42;" }, { '{', "&#123;" }, { '}', "&#125;" }, { '[', "&#91;" }, { ']', "&#93;" }, { '<', "&lt;" }, { '>', "&gt;" }, { '%', "&#37;" }, { '#', "&#35;" }, { '@', "&#64;" } }; public static string EscapeHtmlEntities(string input) {

!!! info "Python"

if (input == null) return null; StringBuilder escapedString = new StringBuilder(); foreach (char ch in input) { if (HtmlEntities.ContainsKey(ch)) { escapedString.Append(HtmlEntities[ch]); } else { escapedString.Append(ch); } } return escapedString.ToString(); } ... // 사용자 입력값 이스케이프 처리 public static string EscapeSpecialCharacters(string input) { if (input == null) return null; return Regex.Replace(input, @"([*{}\[\]<>%#@])", @"\$1"); } ... 사용자 입력값 특수문자 인코딩 처리 예시 # 사용자 입력값 인코딩 def escape_html_entities(input_string): if input_string is None: return None ... html_entities = { '*': '&#42;', '{': '&#123;', '}': '&#125;', '[': '&#91;', ']': '&#93;', '<': '&lt;', '>': '&gt;', '%': '&#37;', '#': '&#35;',

10. Web Application(웹) 보안

!!! info "사용자 입력을 직접 템플릿에 삽입하는 방식은 SSTI 취약점에 노출될 수 있으므로 사용자 입력을 템플릿 변수"

로 전달하는 방식을 사용하여 안전하게 처리 Python(jinja2) 안전한 템플릿 사용 예시 ... template = "userinput : {{ userinput }}" return render_template_string(template, userinput=param) ...

!!! info "PHP"

'@': '&#64;', ... } return ''.join(html_entities.get(char, char) for char in input_string) ... # 사용자 입력값 이스케이프 처리 def escape_special_characters(input_string): if input_string is None: return None return re.sub(r'([*{}\[\]<>%#@])', r'\\\1', input_string) ... PHP 사용자 입력값 특수문자 인코딩 처리 예시 ... function escape_html_entities($input_string) { if ($input_string === null) { return null; } $html_entities = [ '*' => '&#42;', '{' => '&#123;', '}' => '&#125;', '[' => '&#91;', '<' => '&lt;', '>' => '&gt;', '%' => '&#37;', ... ]; return strtr($input_string, $html_entities); } // 특수 문자 앞에 백슬래시를 추가하여 이스케이프 처리 function escape_special_characters($input_string) { if ($input_string === null) { return null; } return preg_replace('/([*{}\[\]<>%#@])/', '\\\\$1', $input_string); } ... // htmlspecialchars 함수를 이용하여 사용자 입력값을 HTML 인코딩 $userInput = htmlspecialchars($userInput ENT_QUOTES, 'UTF-8'); ...

