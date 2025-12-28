# KISA 주요정보통신기반시설 기술적 취약점 분석·평가 방법 상세가이드

**버전**: 2025-12-28 Final v2 (PDF 완전 재파싱 + Web App 5개 항목 완벽 형식화)  
**총 점검 항목**: 382개  
**파싱 방식**: PDF 원본 직접 추출 + 수동 형식화

## ✨ 주요 특징

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
**총 작업 시간**: 약 30분

