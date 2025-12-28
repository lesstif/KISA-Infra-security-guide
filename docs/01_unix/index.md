# Unix Server

**운영체제 보안 설정 및 관리**

**총 67개 항목**

---

## UNIX > 1. 계정 관리

- [U-01: root 계정 원격 접속 제한](1_account_management/u-01.md)
- [U-02: 비밀번호 관리정책 설정](1_account_management/u-02.md)
- [U-03: 계정 잠금 임계값 설정](1_account_management/u-03.md)
- [U-04: 비밀번호 파일 보호](1_account_management/u-04.md)
- [U-05: root 이외의 UID가 ‘0’ 금지](1_account_management/u-05.md)
- [U-06: 사용자 계정 su 기능 제한](1_account_management/u-06.md)
- [U-07: 불필요한 계정 제거](1_account_management/u-07.md)
- [U-08: 관리자 그룹에 최소한의 계정 포함](1_account_management/u-08.md)
- [U-09: 계정이 존재하지 않는 GID 금지](1_account_management/u-09.md)
- [U-10: 동일한 UID 금지](1_account_management/u-10.md)
- [U-11: 사용자 shell 점검](1_account_management/u-11.md)
- [U-12: 세션 종료 시간 설정](1_account_management/u-12.md)
- [U-13: 안전한 비밀번호 암호화 알고리즘 사용](1_account_management/u-13.md)

## UNIX > 2. 파일 및 디렉토리 관리

- [U-14: root 홈, 패스 디렉터리 권한 및 패스 설정](2_file_directory_management/u-14.md)
- [U-15: 파일 및 디렉터리 소유자 설정](2_file_directory_management/u-15.md)
- [U-16: /etc/passwd 파일 소유자 및 권한 설정](2_file_directory_management/u-16.md)
- [U-17: 시스템 시작 스크립트 권한 설정](2_file_directory_management/u-17.md)
- [U-18: /etc/shadow 파일 소유자 및 권한 설정](2_file_directory_management/u-18.md)
- [U-19: /etc/hosts 파일 소유자 및 권한 설정](2_file_directory_management/u-19.md)
- [U-20: /etc/(x)inetd.conf 파일 소유자 및 권한 설정](2_file_directory_management/u-20.md)
- [U-21: /etc/(r)syslog.conf 파일 소유자 및 권한 설정](2_file_directory_management/u-21.md)
- [U-22: /etc/services 파일 소유자 및 권한 설정](2_file_directory_management/u-22.md)
- [U-23: SUID, SGID, Sticky bit 설정 파일 점검](2_file_directory_management/u-23.md)
- [U-24: 사용자, 시스템 환경변수 파일 소유자 및 권한 설정](2_file_directory_management/u-24.md)
- [U-25: world writable 파일 점검](2_file_directory_management/u-25.md)
- [U-26: /dev에 존재하지 않는 device 파일 점검](2_file_directory_management/u-26.md)
- [U-27: $HOME/.rhosts, hosts.equiv 사용 금지](2_file_directory_management/u-27.md)
- [U-28: 접속 IP 및 포트 제한](2_file_directory_management/u-28.md)
- [U-29: hosts.lpd 파일 소유자 및 권한 설정](2_file_directory_management/u-29.md)
- [U-30: UMASK 설정 관리](2_file_directory_management/u-30.md)
- [U-31: 홈디렉토리 소유자 및 권한 설정](2_file_directory_management/u-31.md)
- [U-32: 홈 디렉토리로 지정한 디렉토리의 존재 관리](2_file_directory_management/u-32.md)
- [U-33: 숨겨진 파일 및 디렉토리 검색 및 제거](2_file_directory_management/u-33.md)

## UNIX > 3. 서비스 관리

- [U-34: Finger 서비스 비활성화](3_service_management/u-34.md)
- [U-35: 공유 서비스에 대한 익명 접근 제한 설정](3_service_management/u-35.md)
- [U-36: r 계열 서비스 비활성화](3_service_management/u-36.md)
- [U-37: crontab 설정파일 권한 설정 미흡](3_service_management/u-37.md)
- [U-38: DoS 공격에 취약한 서비스 비활성화](3_service_management/u-38.md)
- [U-39: 불필요한 NFS 서비스 비활성화](3_service_management/u-39.md)
- [U-40: NFS 접근 통제](3_service_management/u-40.md)
- [U-41: 불필요한 automountd 제거](3_service_management/u-41.md)
- [U-42: 불필요한 RPC 서비스 비활성화](3_service_management/u-42.md)
- [U-43: NIS, NIS+ 점검](3_service_management/u-43.md)
- [U-44: tftp, talk 서비스 비활성화](3_service_management/u-44.md)
- [U-45: 메일 서비스 버전 점검](3_service_management/u-45.md)
- [U-46: 일반 사용자의 메일 서비스 실행 방지](3_service_management/u-46.md)
- [U-47: 스팸 메일 릴레이 제한](3_service_management/u-47.md)
- [U-48: expn, vrfy 명령어 제한](3_service_management/u-48.md)
- [U-49: DNS 보안 버전 패치](3_service_management/u-49.md)
- [U-50: DNS ZoneTransfer 설정](3_service_management/u-50.md)
- [U-51: DNS 서비스의 취약한 동적 업데이트 설정 금지](3_service_management/u-51.md)
- [U-52: Telnet 서비스 비활성화](3_service_management/u-52.md)
- [U-53: FTP 서비스 정보 노출 제한](3_service_management/u-53.md)
- [U-54: 암호화되지 않는 FTP 서비스 비활성화](3_service_management/u-54.md)
- [U-55: FTP 계정 shell 제한](3_service_management/u-55.md)
- [U-56: FTP 서비스 접근 제어 설정](3_service_management/u-56.md)
- [U-57: Ftpusers 파일 설정](3_service_management/u-57.md)
- [U-58: 불필요한 SNMP 서비스 구동 점검](3_service_management/u-58.md)
- [U-59: 안전한 SNMP 버전 사용](3_service_management/u-59.md)
- [U-60: SNMP Community String 복잡성 설정](3_service_management/u-60.md)
- [U-61: SNMP Access Control 설정](3_service_management/u-61.md)
- [U-62: 로그인 시 경고 메시지 설정](3_service_management/u-62.md)
- [U-63: sudo 명령어 접근 관리](3_service_management/u-63.md)

## UNIX > 4. 패치 관리

- [U-64: 주기적 보안 패치 및 벤더 권고사항 적용](4_patch_management/u-64.md)

## UNIX > 5. 로그 관리

- [U-65: NTP 및 시각 동기화 설정](5_log_management/u-65.md)
- [U-66: 정책에 따른 시스템 로깅 설정](5_log_management/u-66.md)
- [U-67: 로그 디렉터리 소유자 및 권한 설정](5_log_management/u-67.md)

