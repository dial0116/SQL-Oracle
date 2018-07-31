--sql oracle
--Day1 01. Introduction to Oracle
/* 관계형 데이터 베이스: 엑셀 시트 형태의 테이블 모음
 데이터 베이스 + 자바 APP, 네이버 검색창 등 응용 어플리케이션: 데이터 베이스 시스템 
 이를 변경 작업하기 위한 언어가 SQL이다. 
  
  SELECT 문을 사용하여 데이터 검색
  1) SELECT 절 
  select (행 전체:) * or (일부 coluimn:) column name
  from table name
  산술연산자: +, -, *, / <-숫자와 일부 날짜(+,-)에 사용가능 cf.null값 과의 사칙연산-> 결과: null
  결합연산자: || <- 특징: 문자데이터로 자동으로 변환시킴
  작은따옴표 '': 문자데이터 임을 표시
  실제 데이터로서의 작은따옴표 구분법: '를 데이터로서의 '앞에 붙임 or q'[ ]' 안에 데이터 입력
  날짜 데이터: (영문) 일-(영어약자)월-년
  중복된 데이터를 제거하고 보여주는 키워드: Distinct
  쿼리 사용시 대소문자 구분 안함(보통 실무에서 대문자 또는 소문자로 통일)
  cf. 쿼리(질의어): 정보 수집에 대한 요청에 쓰이는 컴퓨터 언어
  끝난 쿼리에 ; 사용
  null: 정해지지 않은 값(공백이나 0 아님)
  세로운 colname 지정할때 : as 나 공백 사용
  열은 대문자로 표시되며 한절이어야 함-> 만약 소문자나 두개 이상 절을 쓰고 싶으면 "" 사용
  
  예제1.
  select las_name ||', '|| as "Employee and Title"
  from employees;
  */
  
--Day2 02. 데이터 제한 및 정렬
 /* 
 1) WHERE 절 (조건절)
 WHERE column 비교연산자 값
 같지 않음 연산자: <>, !=, ^=
 모든 DB에서 내부 데이터는 대소문자가 구분되어 저장됨
 논리연산자: and, or
 column name between 값 and 값 : 두 값 사이(경계값 포함)
 in (값 나열) : 값 리스트 중 일치하는 값 검색
 like '문자데이터%(글자수 지정안함) or 문자데이터_(한글자 의미)' : 특정 단어 포함한 데이터값 구하기 즉, 일치하는 문자 패턴 검색
 is null : null 값 찾음
 escape '특수문자': 와일드카드가 아닌 실제 % , _ 기호를 검색 ->사용법) 특수문자%, 특수문자_
 not 을 논리 연산자 앞에 씀: 부정문 (cf. is not null: 영어문법 생각)
 우선순위: and > or 또는 괄호 안 연산 먼저
 
 2) ORDER BY 절
 order by: 기본) 오름차순, desc: 내림차순 <- 두 열 사용시 각각 뒤에 두번 써줌, 새로운 alias(colname) 사용가능, select 절에 쓰인 열의 숫자 위치 사용하여 정렬 가능
 nulls last 생략되어있음 -> nulls first
 
 3)치환변수
 단일 치환변수 & : &a(&아무변수이름), 다중 치환변수 && : 똑같은 column이 들어가는 경우 cf. (select절 아닌) from절 부터 &사용, 그 이후 &&사용 cf2. 한번 실행시키면 변수명이 입력된 값으로 고정됨
 undefine 변수이름: 이미 할당된 변수를 clear시킴 <-> define 변수이름=값
 cf. 문자데이터 값은 '&변수이름'을 사용
 set verify on: 사용한 변수명이 입력된 코드부터 입력된 변수명 결과까지 보여줌
 
 03. 단일행함수를 사용하여 출력 커스터마이즈(스스로 사용하기 쉬운 환경을 만드는 것)
 열을 사용하는 위치에는 함수 사용 가능
 input값=인수=argument -> SQL 함수 -> (반드시 한개의) 결과값
 단일행 함수 : 행당 하나의 결과 반환 vs 여러행 함수 : 행의 그룹 당 하나의 결과 반환
 cf. 표현식: 열을 변형시킨 식
 1) 문자함수 
 lower,upper, initcap : 대소문자 변환 함수
 문자 조작 함수
 concat: 연결연산자 || 와 같은 기능을 갖지만 두 문자만 결합 가능
 ★substr: (첫번째 들어온 input값을) n2 자리부터 n3 길이만큼 추출 cf. n3 값 없으면 끝까지 추출 
 length: 길이값
 instr: n3 위치부터 검색 시작하여 n2와 일치하는 위치 중 n4번째로 나타난 숫자 위치 반환
 lpad, rpad: 길이가 n2가 되도록 왼쪽/오른쪽부터 문자식으로 채움
 replace: n2를 n3으로 대체한 문자열 반환
 trim: 맨앞과 뒤의 문자 자름 <- '자를문자' from '기존문자'
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
 where job_id like '%T$_%' escape '$';
 select first_name, department_id, salary sal, job_id, commission_pct
 from employees
 order by department_id desc, salary desc;
 select first_name, department_id, salary sal, job_id, commission_pct
 from employees
 where &a;
 select *
 from &a;
 select &a
 from employees;
 
 undefine dno
 set verify on
 select first_name, &&dno, salary
 from employees
 where &dno in (30,50,80)
 order by &&dno;
 
 --03. 수업코드
select last_name, first_name, upper(last_name), lower(last_name),initcap('ABC DEF')
from employees;
select concat(last_name,first_name), 
--lpad(last_name,20,'$'),rpad(last_name,20,'#')
--replace(concat(last_name,first_name), 'ate', '%')
trim('D' from upper(concat(last_name,first_name)))
from employees;

 --예제2
 select last_name, salary
 from employees
 where salary>12000;
 select first_name, salary
 from employees
 where salary not between 5000 and 12000;
 select last_name, job_id, hire_date
 from employees
 where last_name in ('Matos','Taylor')
 order by hire_date;
 select last_name, salary, commission_pct
 from employees
 where commission_pct is not null
 order by 2 desc, 3 desc;
 select last_name
 from employees
 where last_name like '__a%';
 select last_name
 from employees
 where last_name like '%a%' and last_name like '%e%';
 
 --예제 3
 select last_name
 from employees
 where substr(last_name,1,1) in ('J','K','L','M');
 select 
 last_name || ' '|| salary|| ' ' || lpad('',salary/1000,'*') AS EMPLOYEES_AND_THEIR_SALARIES
 from employees
 order by salary desc;
