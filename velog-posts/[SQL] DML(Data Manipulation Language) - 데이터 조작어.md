<blockquote>
</blockquote>
<p><a href="https://dev.mysql.com/doc/refman/8.4/en/select.html">MySQL 문서</a>를 보고 정리하려고 했지만.. 영어 울렁증과 압도적인 길이의 구조에 그만 압도당해버렸다🥲
아래 작성해놓은 구조만 알면 대부분의 경우엔 문제가 없으니 확실히 숙지해놓자.</p>
<table>
<thead>
<tr>
<th>종류</th>
<th>역할</th>
</tr>
</thead>
<tbody><tr>
<td>SELECT</td>
<td>데이터를 조회하거나 검색</td>
</tr>
<tr>
<td>INSERT</td>
<td>테이블에 새로운 데이터를 삽입</td>
</tr>
<tr>
<td>UPDATE</td>
<td>기존 데이터를 수정</td>
</tr>
<tr>
<td>DELETE</td>
<td>저장된 데이터 삭제</td>
</tr>
</tbody></table>
<h2 id="select">SELECT</h2>
<h3 id="기본-구조">기본 구조</h3>
<pre><code class="language-sql">SELECT [DISTINCT] 컬럼1, 컬럼2, ... [AS 별칭]
FROM 테이블명 [AS 별칭]
[JOIN 테이블명 ON 조인조건]
[WHERE 조건식]
[GROUP BY 그룹화할_컬럼]
[HAVING 그룹_조건식]
[ORDER BY 정렬할_컬럼 [ASC|DESC]]
[LIMIT 행_수 [OFFSET 시작점]]
</code></pre>
<br />

<h4 id="select-절">SELECT 절</h4>
<ul>
<li>컬럼 선택: 조회할 컬럼을 지정. *를 사용하면 모든 컬럼을 선택함</li>
<li>DISTINCT: 중복된 행을 제거.</li>
<li>AS: 컬럼에 별칭을 부여.<pre><code class="language-sql">SELECT DISTINCT name AS 이름, age AS 나이 FROM users;
-- 중복된 이름과 나이를 제거하고, 컬럼에 한글 별칭을 부여함.</code></pre>
</li>
</ul>
<h4 id="from-절">FROM 절</h4>
<ul>
<li>데이터를 조회할 테이블을 지정</li>
<li>여러 테이블 간의 조인을 수행할 수 있음</li>
<li><code>JOIN</code>: 여러 테이블을 연결하여 데이터를 조회하는 방법.(자세한 내용은 추후 포스팅 예정)<pre><code class="language-sql">SELECT * FROM users JOIN orders ON users.id = orders.user_id;
-- users 테이블과 orders 테이블을 user_id를 기준으로 조인하여 모든 컬럼을 선택.</code></pre>
</li>
</ul>
<h4 id="where-절">WHERE 절</h4>
<ul>
<li>특정 조건에 맞는 데이터를 필터링</li>
<li>다양한 연산자와 조건식을 사용할 수 있음<pre><code class="language-sql">SELECT * FROM users WHERE age &gt;= 20 AND city IN ('서울', '부산');
-- 나이가 20 이상이고 도시가 서울 또는 부산인 사용자만 선택.</code></pre>
</li>
</ul>
<h4 id="group-by-절">GROUP BY 절</h4>
<ul>
<li><p>데이터를 그룹화하여 집계 작업을 수행</p>
<table>
<thead>
<tr>
<th>집계 함수</th>
<th>역할</th>
</tr>
</thead>
<tbody><tr>
<td>COUNT()</td>
<td>행의 개수를 반환</td>
</tr>
<tr>
<td>SUM()</td>
<td>숫자 열의 합계를 계산</td>
</tr>
<tr>
<td>AVG()</td>
<td>숫자 열의 평균을 계산</td>
</tr>
<tr>
<td>MAX()</td>
<td>열의 최대값을 반환</td>
</tr>
<tr>
<td>MIN()</td>
<td>열의 최소값을 반환</td>
</tr>
</tbody></table>
<pre><code class="language-sql">SELECT city, COUNT(*) AS 사용자수 FROM users GROUP BY city;
-- 각 도시별로 사용자 수를 계산.</code></pre>
</li>
</ul>
<h4 id="having-절">HAVING 절</h4>
<ul>
<li>그룹화된 결과에 조건을 적용<pre><code class="language-sql">SELECT city, COUNT(*) AS 사용자수 
FROM users 
GROUP BY city 
HAVING COUNT(*) &gt; 100;
-- 사용자 수가 100명 이상인 도시만 선택.</code></pre>
</li>
</ul>
<h4 id="order-by-절">ORDER BY 절</h4>
<ul>
<li>결과를 특정 컬럼 기준으로 정렬.<pre><code class="language-sql">SELECT * FROM users ORDER BY age DESC, name ASC;
-- 나이를 기준으로 내림차순 정렬하고, 같은 나이일 경우 이름으로 오름차순 정렬.</code></pre>
</li>
</ul>
<h4 id="limit-절">LIMIT 절</h4>
<ul>
<li>반환할 행의 수를 제한하며, OFFSET과 함께 사용하여 특정 위치부터 데이터를 가져옴.
```sql
SELECT * FROM users LIMIT 10 OFFSET 20;</li>
<li><ul>
<li>21번째 행부터 10개의 행을 가져옴.<pre><code>
</code></pre></li>
</ul>
</li>
</ul>
<hr />
<h2 id="insert">INSERT</h2>
<h3 id="기본-구조-1">기본 구조</h3>
<pre><code class="language-sql">INSERT INTO 테이블명 (열1, 열2, 열3, ...)
VALUES (값1, 값2, 값3, ...);</code></pre>
<ul>
<li>열 지정: 데이터를 삽입할 열을 명시적으로 지정할 수 있다.</li>
<li>값 순서: VALUES 절에서 지정한 값의 순서는 열 목록의 순서와 일치해야 한다.</li>
<li>열 생략: 모든 열에 값을 삽입하는 경우 열 목록을 생략할 수 있다.</li>
<li>기본값 사용: 열을 생략하면 해당 열의 기본값이 사용된다.</li>
<li>여러 행 삽입: 한 번의 INSERT 문으로 여러 행을 삽입할 수 있다.</li>
</ul>
<h3 id="주의사항">주의사항</h3>
<ul>
<li>데이터 타입 일치: 삽입하는 값의 데이터 타입이 열의 데이터 타입과 일치해야 한다.</li>
<li>NOT NULL 제약조건: NOT NULL로 설정된 열은 반드시 값을 제공해야 한다.</li>
<li>고유 키 제약조건: UNIQUE나 PRIMARY KEY로 설정된 열에는 중복된 값을 삽입할 수 없다.</li>
<li>외래 키 제약조건: 외래 키로 설정된 열에는 참조하는 테이블에 존재하는 값만 삽입할 수 있다.<br />

</li>
</ul>
<h3 id="insert-문-사용-예시">INSERT 문 사용 예시</h3>
<h4 id="1-기본적인-insert-문">1. 기본적인 INSERT 문</h4>
<pre><code class="language-sql">INSERT INTO students (name, email, age)
VALUES ('장발장', 'jang@google.com', 43);
-- students 테이블에 이름, 이메일, 나이를 가진 새로운 학생 정보를 추가.</code></pre>
<h4 id="2-열-이름-생략">2. 열 이름 생략</h4>
<pre><code class="language-sql">INSERT INTO students
VALUES ('장발장', 'jang@google.com', 43);

-- 테이블의 모든 열에 대한 값을 제공할 때 사용할 수 있음.</code></pre>
<h4 id="3-여러-행-동시에-삽입">3. 여러 행 동시에 삽입</h4>
<pre><code class="language-sql">INSERT INTO students (name, email, age)
VALUES ('장발장', 'jang@google.com', 43),
       ('김월래', 'kimw@google.com', 27),
       ('정도전', 'jdoj@google.com', 32);

-- 한 번의 INSERT 문으로 여러 학생의 정보를 동시에 추가.</code></pre>
<h4 id="4-기본값-사용">4. 기본값 사용</h4>
<pre><code class="language-sql">INSERT INTO tasks (title, priority)
VALUES ('Understanding DEFAULT keyword', DEFAULT);

-- priority 열에 기본값을 사용.</code></pre>
<h4 id="5-현재-날짜-삽입">5. 현재 날짜 삽입</h4>
<pre><code class="language-sql">INSERT INTO tasks (title, start_date, due_date)
VALUES ('Use current date for the task', CURDATE(), CURDATE());

-- 현재 날짜를 start_date와 due_date 열에 삽입.</code></pre>
<hr />
<h2 id="update">UPDATE</h2>
<h3 id="기본-구조-2">기본 구조</h3>
<pre><code class="language-sql">UPDATE 테이블명
SET 열1 = 값1, 열2 = 값2, ...
WHERE 조건;</code></pre>
<ul>
<li>테이블 지정: 수정할 데이터가 있는 테이블을 지정.</li>
<li>SET 절: 수정할 열과 새 값을 지정.</li>
<li>WHERE 절: 수정할 행을 선택하는 조건을 지정.</li>
</ul>
<h3 id="주의사항-1">주의사항</h3>
<ul>
<li>WHERE 절 생략: WHERE 절을 생략하면 테이블의 모든 행이 수정됨.</li>
<li>데이터 타입 일치: 수정하는 값의 데이터 타입이 열의 데이터 타입과 일치해야 함.</li>
<li>제약조건 준수: UNIQUE, PRIMARY KEY, FOREIGN KEY 등의 제약조건을 위반하지 않도록 주의해야 함.<br />

</li>
</ul>
<h3 id="update문-사용-예시">UPDATE문 사용 예시</h3>
<h4 id="1-기본적인-update-문">1. 기본적인 UPDATE 문</h4>
<pre><code class="language-sql">UPDATE employees
SET salary = 55000
WHERE employee_id = 1001;

-- employee_id가 1001인 직원의 급여를 55000으로 수정</code></pre>
<h4 id="2-여러-열-동시-수정">2. 여러 열 동시 수정</h4>
<pre><code class="language-sql">UPDATE products
SET price = price * 1.1, stock = stock - 5
WHERE category = '전자제품';

-- '전자제품' 카테고리의 모든 제품의 가격을 10% 인상하고 재고를 5개 감소시킴</code></pre>
<h4 id="3-계산된-값으로-수정">3. 계산된 값으로 수정</h4>
<pre><code class="language-sql">UPDATE orders
SET total_amount = quantity * unit_price
WHERE order_date = CURDATE();

-- 오늘 날짜의 주문에 대해 총 금액을 수량과 단가의 곱으로 계산하여 수정</code></pre>
<h4 id="4-서브쿼리를-사용한-update">4. 서브쿼리를 사용한 UPDATE</h4>
<pre><code class="language-sql">UPDATE employees
SET department_id = (SELECT id FROM departments WHERE name = '마케팅부')
WHERE job_title = '마케팅 전문가';

-- 서브쿼리를 사용하여 '마케팅 전문가' 직책을 가진 직원들의 부서를 '마케팅부'로 변경</code></pre>
<h4 id="5-join을-사용한-update">5. JOIN을 사용한 UPDATE</h4>
<pre><code class="language-sql">UPDATE employees e
JOIN departments d ON e.department_id = d.id
SET e.salary = e.salary * 1.05
WHERE d.name = '영업부';

-- JOIN을 사용하여 '영업부' 소속 직원들의 급여를 5% 인상</code></pre>
<hr />
<h2 id="delete">DELETE</h2>
<h3 id="기본-구조-3">기본 구조</h3>
<pre><code class="language-sql">DELETE FROM 테이블명
WHERE 조건;</code></pre>
<ul>
<li>테이블 지정: 삭제할 데이터가 있는 테이블을 지정.</li>
<li>WHERE 절: 삭제할 행을 선택하는 조건을 지정.</li>
</ul>
<h3 id="주의사항-2">주의사항</h3>
<ul>
<li>WHERE 절 생략: WHERE 절을 생략하면 테이블의 모든 데이터가 삭제됨.</li>
<li>외래 키 제약조건: 외래 키로 참조되는 데이터를 삭제할 때 주의해야 함(<code>참조 무결성을 위반할 수 있음</code>)</li>
</ul>
<h3 id="delete문-사용-예시">DELETE문 사용 예시</h3>
<h4 id="1-기본적인-delete-문">1. 기본적인 DELETE 문</h4>
<pre><code class="language-sql">DELETE FROM customers
WHERE customer_id = 1001;

-- customer_id가 1001인 고객 정보를 삭제</code></pre>
<h4 id="2-여러-조건을-사용한-delete">2. 여러 조건을 사용한 DELETE</h4>
<pre><code class="language-sql">DELETE FROM orders
WHERE order_date &lt; '2023-01-01' AND status = 'completed';

-- 2023년 1월 1일 이전의 완료된 주문을 모두 삭제</code></pre>
<h4 id="3-서브쿼리를-사용한-delete">3. 서브쿼리를 사용한 DELETE</h4>
<pre><code class="language-sql">DELETE FROM products
WHERE category_id IN (SELECT id FROM categories WHERE name = '단종 상품');

-- '단종 상품' 카테고리에 속한 모든 제품을 삭제</code></pre>
<h4 id="4-join을-사용한-delete">4. JOIN을 사용한 DELETE</h4>
<pre><code class="language-sql">DELETE customers
FROM customers
JOIN inactive_accounts ON customers.id = inactive_accounts.customer_id
WHERE inactive_accounts.last_login &lt; DATE_SUB(CURDATE(), INTERVAL 1 YEAR);

-- 1년 이상 로그인하지 않은 비활성 계정의 고객 정보를 삭제</code></pre>
<h4 id="5-limit을-사용한-delete">5. LIMIT을 사용한 DELETE</h4>
<pre><code class="language-sql">DELETE FROM log_entries
ORDER BY created_at ASC
LIMIT 1000;

-- 가장 오래된 로그 항목 1000개를 삭제</code></pre>