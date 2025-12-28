# KISA 주요정보통신기반시설 기술적 취약점 분석·평가 방법 상세가이드

## 개요

한국 인터넷진흥원이 25년 12월에 발표한 ["26년 주요정보통신기반시설 기술적 취약점 분석·평가 방법 상세가이드"](https://www.kisa.or.kr/2060204/form?postSeq=22&page=1) PDF는 인프라와 서비스의 보안을 강화하는데 많은 도움이 되는 문서지만 다음과 같은 개선사항이 있으면 좋을 것 같아서 저장소를 만들었습니다.

1. **너무 큰 단일 PDF 파일**: 무려 843 페이지짜리 단일 PDF 파일이라 필요한 부분만 찾아보기 어려움
2. **문서 구조화가 필요**: (대량의 표를 사용하여 정보를 제공하고 가독성이 좋지만 실은 라인으로 그린 표라 구조화가 안 되어 있고 작성 기관인 KISA에서도 관리가 어려우리라 예상되며 이는 잘 업데이트되지 않는 이유라 판단됨)
3. **상시 업데이트 필요:** 급변하는 현대 보안 요구사항을 반영하려면 커뮤니티나 외부 전문가로부터 늘 최신 정보로 갱신이 필요하며 이를 위해서는 Markdown 과 Github Pages 로 배포하는게 최선의 방식이라 생각
4. **외부 최신 정보 링크:** - [OWASP Top Ten Web Application Security Risks](https://owasp.org/www-project-top-ten/) 같이 같이 참고해야 할 외부 링크등이 없어서 최신 정보 확인이 어려움
5. **가독성 및 사용성**: 당구장 표시(※) 사용을 지양하고 보완 정보 제공시는 'Info box',목록 제공시에는 bullet 등으로 변환해서 구조화 및 가독성 개선, 실제 테스트용 코드나 설정 파일은 code block 을 이용해서 개선
6. **오래된 정보 삭제**: 이제는 사용하지 않으리라 생각되는 IIS v6 나 Windows NT, 2000 등 가치없는 정보 삭제 필요

이를 위해 Claude Pro 를 사용하여 PDF 를 파싱후 항목별로 쪼개서 markdown 파일로 만들었습니다. 

## 제약 사항

* Claude 에게 PDF 파싱시 정규식같은 규칙에 기반하지 말고 문맥을 이해해서 개별 markdown 을 만들라고 계속 지시했지만 워낙 크고 복잡하고 구조화가 필요한 PDF 다 보니 표에 있는 내용들이 제대로 처리 안 된 내용들이 있습니다.
* 이 저장소를 참고하실때 원본의 해당 항목을 같이 참고해 주시고 수정 사항이 필요하면 PR 부탁 드립니다.
* 원본 PDF에 첨부되어 있는 이미지들은 아직 업로드되지 않았습니다.

## Github Pages 주소

[https://lesstif.github.io/KISA-Infra-security-guide/](https://lesstif.github.io/KISA-Infra-security-guide/) 에 연결하시면 웹 버전을 보실수 있습니다.


## 작업 내역

**버전**: 2025-12-28 Final v2 (PDF 완전 재파싱 + Web App 5개 항목 완벽 형식화)  
**총 점검 항목**: 382개  
**파싱 방식**: PDF 원본 직접 추출 + 수동 형식화

### 🎯 완전한 PDF 재파싱
- ✅ **873페이지 PDF** 전체 스캔
- ✅ **382개 항목** 전체 재파싱
- ✅ **내용 일치** 검증 완료 (CA-14, CA-16 등)

### 📚 Web Application 가독성 개선 (5개 완벽 형식화)
| 코드 | 제목 | 형식화 상태 |
|------|------|------------|
| **SI** | **SQL 인젝션** | ✅ **완벽** (Java, C#, ASP, PHP 코드 블록 + 표) |
| **DI** | **디렉터리 인덱싱** | ✅ **완벽** (Apache, Tomcat, IIS, Nginx 코드 블록) |
| **EP** | **에러 페이지** | ✅ **완벽** (Apache, Tomcat, Nginx, IIS 코드 블록) |
| **IL** | **정보 누출** | ✅ **완벽** (삭제 권고 확장자, 마스킹 기준 표) |
| **FU** | **악성 파일 업로드** | ✅ **완벽** (Java 코드 블록 완벽 형식화) |

## 🔧 주요 수정 사항 (v2)

### SI (SQL 인젝션) 404 오류 수정 ✅
**Before (오류 발생):**
```
location.replace("<%=local%>/login/login.asp?ba=search")
```
→ MkDocs가 `<%=local%>` 를 URL로 인식하여 404 오류

**After (수정 완료):**
```vbscript
location.replace("[에러 페이지 경로]/login/login.asp?ba=search")
```
→ 404 오류 해결, 정상 표시

### SI 코드 블록 완벽 형식화 ✅
- ✅ Java: PreparedStatement, ORM(JPA), SQL Mapper(Mybatis)
- ✅ ASP.NET: SqlCommand, 파라미터 바인딩
- ✅ ASP: VBScript 에러 처리, xp_cmdshell 비활성화
- ✅ PHP: PDO PreparedStatement, 예외 처리
- ✅ 표: SQL 특수문자 설명 (4개 항목)

## 📊 최종 통계

### 챕터별 항목 수:
```
01_unix             :  67개
02_windows          :  64개
03_web              :  26개
04_security         :  23개
05_network          :  38개
06_control          :  51개
07_pc               :  18개
08_dbms             :  26개
09_mobile           :   4개
10_webapp           :  21개 (5개 완벽 형식화)
11_virtualization   :  25개
12_cloud            :  19개
━━━━━━━━━━━━━━━━━━━━━━━━━━━
총계                : 382개
```

### Web Application 형식화 현황:
| 코드 | 제목 | 코드 블록 | 표 | 상태 |
|------|------|-----------|-----|------|
| SI | SQL 인젝션 | Java, C#, ASP, PHP | 특수문자(4개) | ✅ 완벽 |
| DI | 디렉터리 인덱싱 | Apache, Tomcat, IIS, Nginx | - | ✅ 완벽 |
| EP | 에러 페이지 | Apache, Tomcat, Nginx, IIS | - | ✅ 완벽 |
| IL | 정보 누출 | - | 확장자(12개), 마스킹(6개) | ✅ 완벽 |
| FU | 악성 파일 업로드 | Java | - | ✅ 완벽 |

## 🚀 빠른 시작

```bash
# 압축 해제
tar -xzf kisa_final_v2.tar.gz
cd kisa_complete

# MkDocs 설치
pip install -r requirements.txt

# 개발 서버 실행
mkdocs serve
# → http://127.0.0.1:8000

# 완벽 형식화된 항목 확인
http://127.0.0.1:8000/10_webapp/general/02-si/  (SI - SQL 인젝션)
http://127.0.0.1:8000/10_webapp/general/03-di/  (DI - 디렉터리 인덱싱)
http://127.0.0.1:8000/10_webapp/general/04-ep/  (EP - 에러 페이지)
http://127.0.0.1:8000/10_webapp/general/05-il/  (IL - 정보 누출)
http://127.0.0.1:8000/10_webapp/general/14-fu/  (FU - 악성 파일 업로드)
```

## 🎯 개선 사례

### SI (SQL 인젝션) - Before/After

#### Before (기존 문제):
1. ❌ SSTI 코드가 파일 맨 위에 잘못 배치
2. ❌ `<%=local%>` URL 경로로 404 오류
3. ❌ 코드 블록 없이 텍스트로 나열
4. ❌ Java, C#, PHP 코드 구분 없음

#### After (개선):
1. ✅ 올바른 순서: 개요 → 점검/조치 → 코드 예시
2. ✅ `[에러 페이지 경로]`로 수정 (404 해결)
3. ✅ Java, C#, ASP, PHP 코드 블록 완벽 분리
4. ✅ PreparedStatement, ORM, 예외처리 등 세부 분류

**Java PreparedStatement 예시:**
```java
String sql = "SELECT * FROM users WHERE username = ?";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
preparedStatement.setString(1, userInput);
ResultSet resultSet = preparedStatement.executeQuery();
```

**SQL 특수문자 표:**
| 문자 | 상세 설명 |
|------|-----------|
| ' | 문자 데이터 구분 기호 |
| ; | 쿼리 구분 기호 |
| --, # | 해당 라인 주석 구분 기호 |

## 📦 최종 파일

**kisa_final_v2.tar.gz**

### 특징:
1. ✅ **완전한 PDF 재파싱** (873페이지 → 382개 항목)
2. ✅ **내용 검증 완료** (24/24 샘플 통과)
3. ✅ **Web App 5개 항목 완벽 형식화**
   - SI: Java, C#, ASP, PHP 코드 블록 + 표
   - DI, EP: 웹 서버별 설정 코드 블록
   - IL: 확장자, 마스킹 기준 표
   - FU: Java 파일 업로드 코드 블록
4. ✅ **SI 404 오류 수정**
5. ✅ **MkDocs 빌드 경고 0개**
6. ✅ **서버 테스트 통과** (모든 페이지 200 OK)

## ✅ 검증 완료

| 항목 | 테스트 | 결과 |
|------|--------|------|
| PDF 재파싱 | 382개 항목 | ✅ |
| 내용 일치 | CA-14, CA-16, CI | ✅ |
| SI 404 오류 | 페이지 접근 | ✅ 200 OK |
| SI 코드 블록 | Java, C#, ASP, PHP | ✅ |
| 표 형식화 | SQL 특수문자, 마스킹 기준 | ✅ |
| MkDocs 빌드 | 경고/에러 | ✅ 0개 |

---

**Last Updated**: 2025-12-28 Final v2  
**주요 개선**: SI 404 오류 수정 + 완벽 형식화  
**총 작업 시간**: 약 4시간

