<blockquote>
</blockquote>
<p>※주의※
마땅히 뭐라고 번역해야 할지 모르는 것들은 의역했으니 웬만하면 영어로 볼 것.</p>
<h2 id="📖표준-표현식standard-expression-syntax">📖표준 표현식(Standard Expression Syntax)</h2>
<pre><code>- 기본 표현식(Simple expressions)
- Literals
- Text operations:
- Arithmetic operations:
- Boolean operations:
- Comparisons and equality:
- Conditional operators:
- Special tokens:</code></pre><ul>
<li>타임리프의 표준 표현식은 이렇게 7가지 종류가 있다. 하나씩 짚고 넘어가 보자.</li>
</ul>
<hr />
<h2 id="1-기본-표현식simple-expressions">1. 기본 표현식(Simple expressions)</h2>
<table>
<thead>
<tr>
<th>표현식</th>
<th>설명</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td><code>${...}</code></td>
<td>변수 표현식</td>
<td><code>${user.name}</code></td>
</tr>
<tr>
<td><code>*{...}</code></td>
<td>선택 변수 표현식</td>
<td><code>*{name}</code></td>
</tr>
<tr>
<td><code>#{...}</code></td>
<td>메시지 표현식</td>
<td><code>#{home.welcome}</code></td>
</tr>
<tr>
<td><code>@{...}</code></td>
<td>링크 URL 표현식</td>
<td><code>@{/users/profile}</code></td>
</tr>
<tr>
<td><code>~{...}</code></td>
<td>프래그먼트 표현식</td>
<td><code>~{commons :: main}</code></td>
</tr>
</tbody></table>
<ul>
<li>표현식은 중첩 및 조합이 가능.</li>
<li>스프링과 함께 사용 시 Spring EL(Expression Language)을 사용</li>
</ul>
<hr />
<h3 id="1-1-변수-표현식variable-expressions">1-1. 변수 표현식(Variable Expressions)</h3>
<ul>
<li><code>${...}</code></li>
<li>컨텍스트 변수나 모델 속성에 접근</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;span th:text=&quot;${user.name}&quot;&gt;사용자 이름&lt;/span&gt;</code></pre>
<hr />
<h3 id="1-2-선택-변수-표현식selection-variable-expressions">1-2. 선택 변수 표현식(Selection Variable Expressions)</h3>
<ul>
<li><code>*{...}</code></li>
<li>특정 객체의 속성을 선택적으로 접근</li>
<li><code>th:object</code> 속성과 함께 사용</li>
<li>중첩된 객체의 속성을 간결하게 표현</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;div th:object=&quot;${user}&quot;&gt;
    &lt;span th:text=&quot;*{name}&quot;&gt;이름&lt;/span&gt;
    &lt;span th:text=&quot;*{email}&quot;&gt;이메일&lt;/span&gt;
&lt;/div&gt;</code></pre>
<hr />
<h3 id="1-3-메시지-표현식message-expressions">1-3. 메시지 표현식(Message Expressions)</h3>
<ul>
<li><code>#{...}</code></li>
<li>국제화(i18n) 메시지 처리</li>
<li>다국어 지원을 위한 메시지 리소스 접근</li>
<li>언어별 메시지 파일에서 키 기반 텍스트 로드</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;h1 th:text=&quot;#{welcome.title}&quot;&gt;환영 메시지&lt;/h1&gt;
</code></pre>
<hr />
<h3 id="1-4-링크-url-표현식link-url-expressions">1-4. 링크 URL 표현식(Link URL Expressions)</h3>
<ul>
<li><code>@{...}</code></li>
<li>동적 URL 생성</li>
<li>컨텍스트 경로 자동 처리</li>
<li>쿼리 파라미터 추가 지원</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;a th:href=&quot;@{/users/{id}(id=${user.id})}&quot;&gt;사용자 프로필&lt;/a&gt;</code></pre>
<hr />
<h3 id="1-5-프래그먼트-표현식fragment-expressions">1-5. 프래그먼트 표현식(Fragment Expressions)</h3>
<ul>
<li><code>~{...}</code></li>
<li>템플릿의 특정 부분 참조 및 포함</li>
<li>템플릿 조각 재사용</li>
<li>레이아웃 및 공통 컴포넌트 구성에 유용</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;div th:insert=&quot;~{fragments/header :: menu}&quot;&gt;
    헤더 메뉴 조각
&lt;/div&gt;</code></pre>
<hr />
<h2 id="2-리터럴literals">2. 리터럴(Literals)</h2>
<pre><code>- 텍스트 리터럴: `one text`, `Another one!`, `…`
- 숫자 리터럴: `0`, `34`, `3.0`, `12.3`, `…`
- 불리언 리터럴: `true`, `false`
- Null 리터럴: `null`
- 리터럴 토큰: `one`, `sometext`, `main`, `…`</code></pre><hr />
<h3 id="2-1-텍스트-리터럴text-literals">2-1. 텍스트 리터럴(Text Literals)</h3>
<ul>
<li><strong>작은따옴표(<code>'</code>)로 감싼 문자열</strong></li>
<li>고정된 텍스트 값 표현</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;span th:text=&quot;'안녕하세요'&quot;&gt;인사말&lt;/span&gt;
&lt;p th:text=&quot;'환영합니다: ' + ${name}&quot;&gt;메시지&lt;/p&gt;</code></pre>
<hr />
<h3 id="2-2-숫자-리터럴number-literals">2-2. 숫자 리터럴(Number Literals)</h3>
<ul>
<li>정수, 실수 모두 지원</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;span th:text=&quot;34&quot;&gt;정수&lt;/span&gt;
&lt;span th:text=&quot;3.0&quot;&gt;실수&lt;/span&gt;
&lt;span th:text=&quot;-12.3&quot;&gt;음의 실수&lt;/span&gt;</code></pre>
<hr />
<h3 id="2-3-불리언-리터럴boolean-literals">2-3. 불리언 리터럴(Boolean Literals)</h3>
<ul>
<li><code>true</code>, <code>false</code> 값</li>
<li>조건부 렌더링에 주로 사용</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;div th:if=&quot;${user.isAdmin() == true}&quot;&gt;관리자 영역&lt;/div&gt;
&lt;span th:text=&quot;${isActive ? '활성' : '비활성'}&quot;&gt;상태&lt;/span&gt;</code></pre>
<hr />
<h3 id="2-4-null-리터럴null-literal">2-4. Null 리터럴(Null literal)</h3>
<ul>
<li><code>null</code> 값 직접 표현</li>
<li>객체의 부재를 나타냄</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;div th:if=&quot;${someObject.someProperty} == null&quot;&gt;
    값이 null입니다.
&lt;/div&gt;</code></pre>
<hr />
<h3 id="2-5-리터럴-토큰-literal-tokens">2-5. 리터럴 토큰 (Literal Tokens)</h3>
<ul>
<li>따옴표 없이 사용 가능한 간단한 식별자</li>
<li>공백, 특수문자 없는 경우</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;div th:class=&quot;main&quot;&gt;메인 영역&lt;/div&gt;
&lt;span th:text=&quot;sometext&quot;&gt;텍스트&lt;/span&gt;</code></pre>
<hr />
<h2 id="3-텍스트-연산text-operations">3. 텍스트 연산(Text operations)</h2>
<pre><code>- 문자열 연결: `+`
- 리터럴 대체: `|The name is ${name}|`</code></pre><hr />
<h3 id="3-1-문자열-연결string-concatenation">3-1. 문자열 연결(String concatenation)</h3>
<ul>
<li><code>+</code></li>
<li>문자열과 표현식을 결합</li>
<li>다양한 데이터 타입 연결 가능</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;span th:text=&quot;'안녕, ' + ${name} + '!'&quot;&gt;인사말&lt;/span&gt;</code></pre>
<hr />
<h3 id="3-2-리터럴-대체literal-substitutions">3-2. 리터럴 대체(Literal substitutions)</h3>
<ul>
<li><code>|The name is ${name}|</code></li>
<li>문자열 내부에 표현식을 직접 삽입</li>
<li>문자열 내에서 변수 삽입을 간결하게 해줌.</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;span th:text=&quot;|사용자 이름은 ${name}입니다.|&quot;&gt;사용자 정보&lt;/span&gt;</code></pre>
<hr />
<h2 id="4-산술-연산arithmetic-operations">4. 산술 연산(Arithmetic operations)</h2>
<pre><code>- 이항 연산자: `+`, `-`, `*`, `/`, `%`
- 단항 연산자: `-`</code></pre><hr />
<h3 id="4-1-이항-연산자binary-operators">4-1. 이항 연산자(Binary operators)</h3>
<table>
<thead>
<tr>
<th>연산자</th>
<th>의미</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td><code>+</code></td>
<td>덧셈</td>
<td><code>${count + 1}</code></td>
</tr>
<tr>
<td><code>-</code></td>
<td>뺄셈</td>
<td><code>${price - discount}</code></td>
</tr>
<tr>
<td><code>*</code></td>
<td>곱셈</td>
<td><code>${quantity * price}</code></td>
</tr>
<tr>
<td><code>/</code></td>
<td>나눗셈</td>
<td><code>${total / count}</code></td>
</tr>
<tr>
<td><code>%</code></td>
<td>나머지</td>
<td><code>${total % count}</code></td>
</tr>
</tbody></table>
<p>예시:</p>
<pre><code class="language-html">&lt;span th:text=&quot;${(price * quantity) - discount}&quot;&gt;
    총 가격
&lt;/span&gt;</code></pre>
<hr />
<h3 id="4-2-단항-연산자minus-sign-unary-operator">4-2. 단항 연산자(Minus sign (unary operator))</h3>
<ul>
<li><code>-</code>: 숫자의 부호 변경</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;div th:text=&quot;-${number}&quot;&gt;부호 반전(양수→음수)&lt;/div&gt;</code></pre>
<hr />
<h2 id="5-불리언-연산boolean-operations">5. 불리언 연산(Boolean operations)</h2>
<h3 id="5-1-이항-연산자binary-operators">5-1. 이항 연산자(Binary operators)</h3>
<table>
<thead>
<tr>
<th>연산자</th>
<th>의미</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td><code>and</code></td>
<td>논리곱</td>
<td><code>${isAdmin and isActive}</code></td>
</tr>
<tr>
<td><code>or</code></td>
<td>논리합</td>
<td><code>${isStudent or isEmployee}</code></td>
</tr>
</tbody></table>
<h3 id="5-2-단항-연산자boolean-negation-unary-operator">5-2. 단항 연산자(Boolean negation (unary operator))</h3>
<table>
<thead>
<tr>
<th>연산자</th>
<th>의미</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td><code>!</code></td>
<td>논리 부정</td>
<td><code>!${condition}</code></td>
</tr>
<tr>
<td><code>not</code></td>
<td>논리 부정 (동일)</td>
<td><code>not ${condition}</code></td>
</tr>
</tbody></table>
<hr />
<h2 id="6-비교-동등-연산comparisons-and-equality">6. 비교, 동등 연산(Comparisons and equality)</h2>
<h3 id="6-1-비교-연산자comparators">6-1. 비교 연산자(Comparators)</h3>
<table>
<thead>
<tr>
<th>기호</th>
<th>영문</th>
<th>의미</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td><code>&gt;</code></td>
<td><code>gt</code></td>
<td>초과</td>
<td><code>${age &gt; 19}</code></td>
</tr>
<tr>
<td><code>&lt;</code></td>
<td><code>lt</code></td>
<td>미만</td>
<td><code>${score &lt; 80}</code></td>
</tr>
<tr>
<td><code>&gt;=</code></td>
<td><code>ge</code></td>
<td>이상</td>
<td><code>${count &gt;= 10}</code></td>
</tr>
<tr>
<td><code>&lt;=</code></td>
<td><code>le</code></td>
<td>이하</td>
<td><code>${price &lt;= 1000}</code></td>
</tr>
</tbody></table>
<h3 id="6-2-동등-연산자equality-operators">6-2. 동등 연산자(Equality operators)</h3>
<table>
<thead>
<tr>
<th>기호</th>
<th>영문</th>
<th>의미</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td><code>==</code></td>
<td><code>eq</code></td>
<td>같음</td>
<td><code>${status == 'ACTIVE'}</code></td>
</tr>
<tr>
<td><code>!=</code></td>
<td><code>ne</code></td>
<td>다름</td>
<td><code>${type != 'ADMIN'}</code></td>
</tr>
</tbody></table>
<hr />
<h2 id="7-조건-연산자conditional-operators">7. 조건 연산자(Conditional operators)</h2>
<pre><code>- If-then: (if) ? (then)
- If-then-else: (if) ? (then) : (else)
- Default: (value) ?: (defaultvalue)</code></pre><ul>
<li>If-then과 Default는 <code>?</code> 뒤에 <code>:</code> 유무 주의</li>
</ul>
<hr />
<h3 id="7-1-if-then">7-1. If-Then</h3>
<ul>
<li>조건이 참일 때만 값 표시</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;span th:text=&quot;${condition} ? '참일 때 값'&quot;&gt;
    기본 텍스트
&lt;/span&gt;</code></pre>
<hr />
<h3 id="7-2-if-then-else">7-2. If-Then-Else</h3>
<ul>
<li>조건에 따라 두 값 중 하나 선택</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;div th:text=&quot;${user.isAdmin()} ? '관리자' : '일반 사용자'&quot;&gt;
    사용자 유형
&lt;/div&gt;</code></pre>
<hr />
<h3 id="7-3-기본값-연산자default">7-3. 기본값 연산자(Default)</h3>
<ul>
<li><code>?:</code></li>
<li>Null 또는 빈 값일 경우 기본값 제공</li>
<li>Elvis operator라고도 하며, 일종의 단축 표현임</li>
<li>아래 코드는 <code>&quot;${username} ? ${username} : '손님'&quot;</code>과 동일</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;span th:text=&quot;${username} ?: '손님'&quot;&gt;
    사용자 이름
&lt;/span&gt;</code></pre>
<hr />
<h2 id="8-특수-토큰special-token">8. 특수 토큰(Special token)</h2>
<pre><code>- No-Operation: `_`</code></pre><ul>
<li>특정 표현식의 결과로 아무 작업도 수행하지 않도록 지정</li>
<li>프로토타입 텍스트를 기본값으로 사용할 때 유용</li>
</ul>
<p>예시:</p>
<pre><code class="language-html">&lt;div th:text=&quot;${condition} ? ${value} : _&quot;&gt;기본 텍스트 유지&lt;/div&gt;</code></pre>
<hr />
<h2 id="마무리">마무리</h2>
<ul>
<li>앞서 살펴본 기능들은 자유롭게 결합 및 중첩이 가능하다.<pre><code class="language-html">'User is of type ' + (${user.isAdmin()} ? 'Administrator' : (${user.type} ?: 'Unknown'))</code></pre>
</li>
</ul>