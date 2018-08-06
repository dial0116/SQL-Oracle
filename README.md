/*
<서브쿼리를 사용하여 문제해결>
Query 명령문: SELECT
select(메인 쿼리 명령문) col, (select)(서브 쿼리 명령문)
★②from (select) : 가공된 테이블 만듦 -> 결과 반환 후 사라짐
★①where col 비교연산자 (select) : 
group by
having 그룹함수 비교연산자 select
order by select ;

서브쿼리는 
메인쿼리 전에 실행되어 메인쿼리에 사용됨
반드시 괄호로 묶음
단일행/여러행 연산자에 따라 단일행/여러행 서브쿼리 사용 cf. in 제외하고는 단일행 연산자(=,>..)
단일행 연산자: 한 행이 한 칼럼으로 이루어짐
단일행 서브쿼리: 하나의 결과만 반환함

<여러행 서브쿼리>
any (or), all (and) : 단일행 연산자와 함께 쓰임
=any, >any : in
=all, <all 

비상관 서브쿼리: 서브쿼리가 먼저 실행
상관 서브쿼리: 서브쿼리가 독자적으로 실행이 안되고 바깥쿼리가 먼저 실행되어 바깥테이블리 서브쿼리에 영향 주는 경우
-첫번째 행 읽고 서브쿼리에 존재하면 자신이 지정한 데이터 반환 실행->

data file: data 저장하는 책
block: 책의 한 페이지
Rowid: 색인, 행의 위치 주소값
데이터 검색할 때 가장 빠른 방법: where rowid='A..A'
dml 작업 시 중복되는 데이터 삭제 작업시 rowid 활용
Rownum: 책에서 데이터(행)를 읽는 순서값

EXISTS 연산자 : 항상 상관임, 서브쿼리 안 데이터 존재 체크 
exists = in, not exists = not in 
예) select *
from 학생테이블
where exists (select 1|'a'|colname from 도서대출테이블 where 도서대출.학번=학생.학번)

cf. 어려운 쿼리의 경우 두개의 sql 문장으로 분리시켰다가 결합시키기

<집합연산자>
union(합집합) <-> union all(교집합 중복된 채로 그대로 반환), intersect, minus
-★select 에서 선택하는 칼럼의 개수와 앞뒤 select에서 사용한 데이터 유형은 같아야 함
-위에서 아래로 순서대로 연산함, 우선순위 변경은 괄호로 함
-order by 명령어는 맨 마지막 select 절에만 올 수 있음
-최종 반환되는 칼럼의 머리글과 order by 에 쓰이는 칼럼은 첫번째 select절의 칼럼만 
-대부분 
union all: 순차적으로 display <-> 이를 제외한 연산자들은 select 절에 쓰여진 모든 칼럼에 대해 정렬작업 후 중복제거. 따라서 자동적으로 정렬됨

*/
select first_name, salary, (select avg(salary) from employees) --한 개의 데이터 값만 반환 가능 
avg_sal
from employees;
select first_name, salary, (select avg(salary) from employees avg_sal)
from employees;
select rowid, rownum, first_name, salary --실제 테이블에 있지 않은 허수의 칼럼(수도코드)
from employees
where rownum = 5; --데이터를 읽으려면 첫번째부터 순차적으로 읽어야함. 중간에 건너뛰어 n번째만 읽을 수 없음.
--where rownum = 1 만 가능

select count(*)
from employees
where to_char(hire_date,'yyyy')='2018'; --단순히 조건에 맞는 데이터의 유무만 체크할 시 : and rownum = 1  <-모든 데이터 값을 읽을 필요 없어 성능을 높여줌

select *
from (select first_name, salary from employees order by salary desc)  --원하는 결과 읽어오기 위해 select절을 이용하여 가공된 테이블 만듦
where rownum<=5;

select first_name, salary
from employees
where salary>(select avg(salary) from employees); 

select first_name, salary, job_id
from employees
where job_id = (select job_id from employees where first_name='Ellen'); --Talyor 는 두명 이상이라 여러행 서브쿼리가 됨

select avg(salary)
from employees
group by department_id; --그룹지어 유니크한 칼럼값을 한번만 출력

select salary
from employees
where last_name='Taylor';

select last_name, first_name, salary
from employees
where salary in (select salary --서브 쿼리에서 리턴되는 값이 몇개인지 모를 때 'in' 효과적
                from employees
                where last_name='Taylor';)
--in = '=any'


v_cnt number := length('abc');
             := count(10)


--사원에게 1명이라도 할당된 부서의 명칭
select *
from departments
where not exists (select 1 from employees where employees.department_id=departments.department_id);
select *
from departments
where department_id not in (select distinct department_id from employees where department_id is not null); --in( 10,20,...null) :null도 포함됨 
---> null 값이 없어야 함 ->so 'not in' 쓸 경우 'is not null' 써줌
select * from employees where commission_pct=null; --에러가 아닌 'no row found'

select department_id, first_name, last_name, salary
from employees
where department_id=110
union 
select employee_id, first_name, last_name, commission_pct
from employees
where salary>=15000
order by salary; --department_id 칼럼에 department_id와 employee_id 모두 표현함

select employee_id, job_id
from job_history
minus
select employee_id, job_id 
from employees;

select location_id, department_name, null --'0'과 같은 명시적 값 삽입
from departments
union
select location_id, null, city
from locations;

--연습문제 7
select employee_id, first_name, salary
from employees
--where department_id = (select department_id from employees where salary>avg(salary) and last_name like '%u%'); x -> 그룹함수는 항상 select절과 함께 써야함
where department_id in (select department_id from employees where salary>(select avg(salary) from employees) and last_name like '%u%');
--여러행 서브쿼리->여러행 연산자 'in' 써야함

select last_name, salary, manager_id
from employees
where manager_id in (select employee_id from employees where last_name='King');

select first_name, department_id, job_id
from employees
where department_id in (select department_id from departments where department_name='Sales'); 

select e.department_id, department_name, count(*) --그룹함수 안 그룹함수
from employees e, departments d
where e.department_id=d.department_id 
group by e.department_id, department_name
having count(*)=(select max(count(*)) from employees group by department_id)
order by e.department_id, department_name;
select *
from (select e.department_id, department_name, count(*) 
from employees e, departments d
where e.department_id=d.department_id 
group by e.department_id, department_name
order by 3 desc)
where rownum=1;

select first_name, hire_date --치환변수 활용
from employees
where department_id in (select department_id from employees where first_name='&b') and first_name<>'&&b'; 
undefine first_name

select distinct job_id, department_id --결합 칼럼에 대해 중복된 값 제거
from employees
where department_id = 10
union all
select distinct job_id, department_id
from employees
where department_id = 50
union all 
select distinct job_id, department_id
from employees
where department_id = 30;
