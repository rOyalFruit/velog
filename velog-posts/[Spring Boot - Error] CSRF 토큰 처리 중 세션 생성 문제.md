<p>점프 투 스프링부트 추가 기능 구현하기를 진행하며 구현한 기능들을 테스트 하던 도중 심각한 문제가 있는 것을 발견했다.</p>
<h2 id="문제-상황">문제 상황</h2>
<ul>
<li>[로그인→로그아웃→질문 상세 페이지] 순서로 실행하면 글 상세 페이지 로딩이 안됨.</li>
<li>로그인 후 글 상세 페이지 조회, 로그인을 안한 상태에서 질문 상세 페이지 조회, 게시판 카테고리, [질문/답변] [작성/수정/삭제] 등 다른 기능에는 문제가 없었다.</li>
</ul>
<hr />
<h2 id="에러-원인">에러 원인</h2>
<pre><code>[THYMELEAF][http-nio-8080-exec-8] Exception processing template &quot;question_detail&quot;: An error happened during template parsing (template: &quot;class path resource [templates/question_detail.html]&quot;)

org.thymeleaf.exceptions.TemplateInputException: An error happened during template parsing (template: &quot;class path resource [templates/question_detail.html]&quot;)

Caused by: org.attoparser.ParseException: Error during execution of processor 'org.thymeleaf.spring6.processor.SpringActionTagProcessor' (template: &quot;question_detail&quot; - line 132, col 11)

Caused by: org.thymeleaf.exceptions.TemplateProcessingException: Error during execution of processor 'org.thymeleaf.spring6.processor.SpringActionTagProcessor' (template: &quot;question_detail&quot; - line 132, col 11)

Caused by: java.lang.IllegalStateException: Cannot create a session after the response has been committed

Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed: org.thymeleaf.exceptions.TemplateInputException: An error happened during template parsing (template: &quot;class path resource [templates/question_detail.html]&quot;)] with root cause

java.lang.IllegalStateException: Cannot create a session after the response has been committed</code></pre><h3 id="csrf-토큰-처리-중-세션-생성-문제">CSRF 토큰 처리 중 세션 생성 문제</h3>
<ul>
<li><p><code>java.lang.IllegalStateException: Cannot create a session after the response has been committed</code></p>
</li>
<li><p>CSRF 토큰을 생성할 때(HttpSessionCsrfTokenRepository.saveToken)세션이 필요하지만, 이미 HTTP 응답이 커밋된 이후라 세션을 생성할 수 없어서 발생한 문제.</p>
</li>
<li><p>Spring Security에서 CSRF 보호를 위해 사용되는 HttpSessionCsrfTokenRepository가 세션을 필요로 하지만, 요청이 이미 완료된 상태에서 세션을 생성하려고 시도한 것이 원인.</p>
</li>
</ul>
<h3 id="thymeleaf-템플릿-파싱-오류">Thymeleaf 템플릿 파싱 오류</h3>
<ul>
<li><code>org.thymeleaf.exceptions.TemplateProcessingException: Error during execution of processor 'org.thymeleaf.spring6.processor.SpringActionTagProcessor'</code></li>
<li>Thymeleaf의 th:action 또는 관련 속성에서 잘못된 값이 사용되었거나, 필요한 데이터가 모델에 포함되지 않았기 때문일 수 있다.</li>
</ul>
<p>→ - <code>question_detail.html</code> 의 답변 작성 폼 태그 위치를 <code>sec:authorize=&quot;isAuthenticated()&quot;</code> 블록 안으로 이동시키면 문제를 해결할 수 있다!</p>
<hr />
<h2 id="문제가-발생한-코드">문제가 발생한 코드</h2>
<pre><code class="language-html">&lt;form th:action=&quot;@{|/answer/create/${question.id}|}&quot; th:object=&quot;${answerForm}&quot; method=&quot;post&quot; class=&quot;my-3&quot;&gt;
    &lt;div th:replace=&quot;~{form_errors :: formErrorsFragment}&quot;&gt;&lt;/div&gt;
    &lt;div sec:authorize=&quot;isAnonymous()&quot;&gt;
        &lt;textarea disabled class=&quot;form-control&quot; rows=&quot;10&quot; placeholder=&quot;로그인 후 이용해 주세요.&quot;&gt;&lt;/textarea&gt;
    &lt;/div&gt;
    &lt;div sec:authorize=&quot;isAuthenticated()&quot;&gt;
        &lt;textarea th:field=&quot;*{content}&quot; class=&quot;form-control&quot; rows=&quot;10&quot;&gt;&lt;/textarea&gt;
    &lt;/div&gt;
    &lt;input type=&quot;submit&quot; value=&quot;답변등록&quot; class=&quot;btn btn-primary my-2&quot;&gt;
&lt;/form&gt;</code></pre>
<hr />
<h2 id="수정한-코드">수정한 코드</h2>
<pre><code class="language-html">&lt;div sec:authorize=&quot;isAnonymous()&quot;&gt;
    &lt;textarea disabled class=&quot;form-control&quot; rows=&quot;10&quot; placeholder=&quot;답변 작성은 로그인 후 가능합니다.&quot;&gt;&lt;/textarea&gt;
&lt;/div&gt;
&lt;div sec:authorize=&quot;isAuthenticated()&quot;&gt;
    &lt;form th:action=&quot;@{|/answer/create/${question.id}|}&quot; th:object=&quot;${answerForm}&quot; method=&quot;post&quot; class=&quot;my-3&quot;&gt;
        &lt;div th:replace=&quot;~{form_errors :: formErrorsFragment}&quot;&gt;&lt;/div&gt;
        &lt;textarea th:field=&quot;*{content}&quot; class=&quot;form-control&quot; rows=&quot;10&quot;&gt;&lt;/textarea&gt;
        &lt;input type=&quot;submit&quot; value=&quot;답변등록&quot; class=&quot;btn btn-primary my-2&quot;&gt;
    &lt;/form&gt;
&lt;/div&gt;</code></pre>
<ul>
<li>수정 전 코드는 하나의 <code>&lt;form&gt;</code> 태그 안에서 인증 상태를 분기 처리했기 때문에 불필요한 중복 코드가 존재했다.</li>
<li>수정 후 코드는 인증 상태별로 UI를 명확히 분리하여 가독성을 높이고, 불필요한 HTML 요소를 제거해 유지보수성과 UX를 개선했다.</li>
</ul>
<hr />
<h2 id="결론">결론</h2>
<p>에러가 발생했을 때 로그를 확인했지만 question_detail.html를 파싱하는 도중 문제가 발생했다는 내용밖에 못 봤었다. 로그에 어디가 문제인지 위치까지 전부 알려주고 있었는데, 결국 반나절 정도 삽질이 끝난 후에 발견했다.. </p>
<blockquote>
</blockquote>
<p>에러 발생 시 로그는 꼼꼼히 보자.</p>