# 데이터베이스 (DataBase)
<!--Table of Contents-->
- 데이터베이스란?
- 데이터베이스의 특징
- 데이터베이스 관리 시스템(DBMS) 이란?
- 데이터베이스 관리 시스템(DBMS)의 특징
- 데이터베이스 관리 시스템(DBMS)의 종류

## You Can Answer
- 데이터 베이스가 무엇인가?
- DBMS에 대해 설명해보아라.
- DBMS의 특징은 무엇인가?
- DBMS의 종류에 대해 설명해보아라.
-----------------------

## 데이터베이스란?
- 데이터베이스란 **여러 사람들이 공유하고 사용할 목적으로 통합 관리되는 데이터들의 모임**이다. 이는 중복된 데이터를 없애고, 자료를 구조화하여, 효율적인 처리를 할 수 있도록 관리된다. 이러한 데이터베이스는 응용 프로그램과는 다른 별도의 미들웨어에 의해 관리된다. 데이터베이스를 관리하는 이러한 미들웨어를 **데이터베이스 관리 시스템(DBMS: DataBase Management System)**이라고 부른다. 

</br>

## 데이터베이스의 특징
- 사용자의 질의에 대하여 즉각적인 처리와 응답이 이루어진다.
- 생성, 수정, 삭제를 통하여 항상 최신의 데이터를 유지한다.
- 사용자들이 원하는 데이터를 동시에 공유할 수 있다.
- 사용자가 원하는 데이터를 주소가 아닌 내용에 따라 참조할 수 있다.
- 응용 프로그램과 데이터베이스는 독립되어 있으므로, 데이터의 논리적 구조와 응용 프로그램은 별개로 동작된다.

</br>

## 데이터베이스 관리 시스템(DBMS)이란?
- DBMS란 사용자와 데이터베이스 사이에서 사용자의 요구에 따라 정보를 생성해주고 데이터베이스를 관리할 수 있게 해주는 소프트웨어이다. 
DMBS는 기존의 파일 시스템이 갖는 데이터의 종속성과 중복성의 문제를 해결하기 위해 제안된 시스템으로 모든 응용 프로그램들이 데이터베이스를 공용할 수 있도록 관리해준다. 

</br>

## 데이터베이스 관리 시스템(DBMS)의 특징
- DBMS는 파일 시스템의 문제점을 해결하기 위해 만들어졌기 때문에 DBMS의 특징은 곧 파일 시스템의 단점을 의미한다.

1. 데이터의 독립성
   - 물리적 독립성: 데이터베이스 사이즈를 늘리거나 성능 향상을 위해 데이터 파일을 늘리거나 새롭게 추가하더라도 관련된 응용 프로그램을 수정할 필요가 없다. 
   - 논리적 독립성: 데이터베이스는 다양한 응용 프로그램의 논리적 요구를 만족시켜줄 수 있다.

2. 데이터의 무결성
   - 여러 경로를 통해 잘못된 데이터가 발생하는 경우의 수를 방지하는 기능으로 데이터의 유효성 검사를 통해 데이터의 무결성을 구현하게 된다. 예를들면 입력 조건에 맞지 않는 입력값은 저장할 수 없도록 방지하는 기능이 있을 수 있다.

3. 데이터의 보안성
   - 허가된 사용자들만 데이터베이스나 데이터베이스 내의 자원에 접근할 수 있도록 계정 관리 또는 접근 권한을 설정함으로써 모든 데이터에 보안을 구현할 수 있다.

4. 데이터의 일관성
   - 연관된 정보를 논리적인 구조로 관리함으로써 어떤 하나의 데이터만 변경했을 경우 발생할 수 있는 데이터의 불일치성을 배제할 수 있다. 또한 작업 중 일부 데이터만 변경되어 나머지 데이터와 일치하지 않는 경우의 수를 배제할 수 있다. 

5. 데이터의 중복 최소화
   - 데이터베이스는 데이터를 통합해서 관리함으로써 데이터 중복 문제를 해결할 수 있다. 

</br>

## 데이터베이스 관리 시스템(DBMS)의 종류
- 대표적인 DBMS에는 오라클(Oracle), MySQL, MSSQL, MariaDB 등이 있고, 각각의 DBMS는 다음과 같은 특징들을 가지고 있다.

1. Oracle
   - 오라클에서 만들어 판매중인 상업용 데이터베이스
   - 윈도우, 리눅스, 유닉스 등 다양한 운영체제(OS)에서 설치 가능
   - MySQL, MSSQL보다 대량의 데이터 처리 용이
   - 대기업에서 주로 사용하며, 글로벌 DB 시장 점유율 1위
   - 비공개 소스, 폐쇄적인 운영
   - 가장 널리 사용되는 RDBMS

2. MySQL
   - MySQL사에서 개발, 썬마이크로시스템즈를 거쳐 현재 오라클에 인수합병
   - 윈도우, 리눅스, 유닉스 등 다양한 운영체제(OS)에서 설치 가능
   - 오픈소스로 이루어져있는 무료 프로그램(상업적 사용 시 비용 발생)
   - 가격 등의 장점을 앞세워 다수의 중소기업에서 사용중
   - RDBMS

3. MSSQL
   - 마이크로소프트(MS)사에서 개발한 상업용 데이터베이스
   - 다른 운영체제에서도 사용가능하지만 윈도우에 특화됨
   - 비공개 소스로 폐쇄적인 운영(리눅스 버전은 오픈소스)
   - 중소기업에서 주로 사용중
   - RDBMS

4. MariaDB
   - MySQL이 오라클에 인수합병된 후 불확실한 라이선스 문제를 해결하려고 나온 오픈소스 RDBMS
   - 구현언어 : C++
   - MySQL과 동일한 소스 코드 기반
   - MySQL과 비교해 애플리케이션 부분 속도가 약 4~5천배 정도 빠름

</br>

## Reference
- https://noahlogs.tistory.com/36
