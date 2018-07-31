--sql oracle
--Day1 01. Introduction to Oracle
/* 관계형 데이터 베이스: 엑셀 시트 형태의 테이블 모음
 데이터 베이스 + 자바 APP, 네이버 검색창 등 응용 어플리케이션: 데이터 베이스 시스템 
 이를 변경 작업하기 위한 언어가 SQL이다. 
  
  SELECT 문을 사용하여 데이터 검색
  1) SELECT 절 
  select (행 전체:) * or (일부 coluimn:) column name
  from table name
  산술연산자: +, -, *, / <-숫자와 일부 날짜(+,-)에 사용가능
  결합연산자: || <- 특징: 문자데이터로 자동으로 변환시킴
  작은따옴표 '': 문자데이터 임을 표시
  실제 데이터로서의 작은따옴표 구분법: '를 데이터로서의 '앞에 붙임 or q'[ ]' 안에 데이터 입력
  날짜 데이터: (영문) 일-(영어약자)월-년
  중복된 데이터를 제거하고 보여주는 키워드: Distinct
  열은 대문자로 표시되며 한절이어야 함-> 만약 소문자나 두개 이상 절을 쓰고 싶으면 "" 사용
  쿼리 사용시 대소문자 구분 안함(보통 실무에서 대문자 또는 소문자로 통일)
  cf. 쿼리(질의어): 정보 수집에 대한 요청에 쓰이는 컴퓨터 언어
  끝난 쿼리에 ; 사용
  null: 정해지지 않은 값(공백이나 0 아님)
  
  예제1.
  select las_name ||', '|| as "Employee and Title"
  from employees;
  */
  
--Day2 02. 데이터 제한 및 정렬
 /* 
 2) WHERE 절 (조건절)
 WHERE column 비교연산자 값
 같지 않음 연산자: <>, !=, ^=
 모든 DB에서 내부 데이터는 대소문자가 구분되어 저장됨
 논리연산자: and, or
 column name between 값 and 값 : 두 값 사이(경계값 포함)
 in (값 나열) : 값 리스트 중 일치하는 값 검색
 like '문자데이터%(글자수 지정안함) or 문자데이터_(한글자 의미)' : 특정 단어 포함한 데이터값 구하기 즉, 일치하는 문자 패턴 검색
 is null : null 값 찾음
 escape '특수문자'
 not 을 논리 연산자 앞에 씀: 부정문 (cf. is not null)
 우선순위: and > or 또는 괄호 안 연산 먼저
 */ 
 
 --수업 코드
 select first_name, department_id, salary, job_id
 from employees
 where   10000<=salary and salary<=12000;
 select first_name, department_id, salary, job_id
 from employees
 where salary between 10000 and 12000;

 select first_name, department_id, salary, job_id
 from employees
 where salary =10000 or salary =11000 or salary =12000;
 select first_name, department_id, salary, job_id
 from employees
 where salary in (10000, 11000, 12000);
 
 select first_name, department_id, salary, job_id
 from employees
 where first_name like '__t%';
 select first_name, department_id, salary, job_id, commission_pct
 from employees
 where commission_pct is null;
 select first_name, department_id, salary, job_id, commission_pct
 from employees
 where  escape '$';
