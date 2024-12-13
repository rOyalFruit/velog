<h2 id="valid의-주요-기능">@Valid의 주요 기능</h2>
<blockquote>
</blockquote>
<p>바인딩, 유효성 검사를 수행하는 어노테이션</p>
<ol>
<li><strong>바인딩</strong></li>
</ol>
<ul>
<li>HTML 폼이나 JSON 요청의 데이터를 자동으로 객체에 매핑</li>
<li>필드명이 일치하는 데이터를 해당 객체의 속성에 자동으로 설정</li>
</ul>
<ol start="2">
<li><strong>유효성 검증</strong></li>
</ol>
<ul>
<li>객체의 필드에 선언된 검증 어노테이션(@NotNull, @Size 등)을 기반으로 데이터 유효성 확인</li>
<li>검증 실패 시 BindingResult나 예외를 통해 오류 정보 제공</li>
</ul>
<hr />
<h2 id="사용-예시">사용 예시</h2>
<h3 id="html-폼">HTML 폼</h3>
<pre><code class="language-html">&lt;form th:action=&quot;@{/question/create}&quot; method=&quot;post&quot;&gt;
    &lt;input type=&quot;text&quot; name=&quot;subject&quot;&gt;
    &lt;input type=&quot;text&quot; name=&quot;content&quot;&gt;
&lt;/form&gt;</code></pre>
<h3 id="questionform-클래스">QuestionForm 클래스</h3>
<pre><code class="language-java">@Getter
@Setter
public class QuestionForm {
    @NotEmpty(message = &quot;제목은 필수항목입니다.&quot;)
    @Size(max = 200)
    private String subject;

    @NotEmpty(message = &quot;내용은 필수항목입니다.&quot;)
    private String content;
}</code></pre>
<h3 id="questioncontroller-클래스">QuestionController 클래스</h3>
<pre><code class="language-java">@PostMapping(&quot;/create&quot;)
public String questionCreate(@Valid QuestionForm questionForm, BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
        return &quot;question_form&quot;; // 유효성 검증 실패 시 처리
    }

    // 유효성 검증 성공 로직 
    this.questionService.create(questionForm.getSubject(), questionForm.getContent());
    return &quot;redirect:/question/list&quot;;
}</code></pre>
<h3 id="바인딩-메커니즘">바인딩 메커니즘</h3>
<ol>
<li>HTML 폼의 name 속성과 자바 클래스의 필드명이 정확히 일치(클래스에 정의된 필드만 바인딩 되고, 바인딩 되지 않은 값은 버려짐)</li>
<li>스프링이 자동으로 subject와 content 값을 매핑</li>
<li>컨트롤러에 전달될 때 필드값들이 자동으로 설정됨</li>
</ol>
<p>-&gt; @Valid가 검증하는 역할, BindingResult는 @Valid로 검증이 수행된 결과를 의미하는 객체</p>
<h3 id="주의사항">주의사항</h3>
<blockquote>
</blockquote>
<ul>
<li>BindingResult는 항상 @Valid 바로 뒤에 위치해야 함. </li>
<li>두 매개변수의 위치가 정확하지 않다면 @Valid만 적용되어 입력값 검증 실패 시 400 오류가 발생.</li>
</ul>
<hr />
<h2 id="validation-의존성-추가">Validation 의존성 추가</h2>
<pre><code class="language-java">implementation 'org.springframework.boot:spring-boot-starter-validation'</code></pre>
<hr />
<h2 id="validation-어노테이션-종류">Validation 어노테이션 종류</h2>
<h3 id="null-관련"><strong>Null 관련</strong></h3>
<table>
<thead>
<tr>
<th>어노테이션</th>
<th>설명</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td>@Null</td>
<td>null만 허용</td>
<td><code>@Null private String value;</code></td>
</tr>
<tr>
<td>@NotNull</td>
<td>null을 허용하지 않음. 빈 문자열과 공백은 허용</td>
<td><code>@NotNull private String name;</code></td>
</tr>
<tr>
<td>@NotEmpty</td>
<td>null, 빈 문자열 허용하지 않음</td>
<td><code>@NotEmpty private List&lt;String&gt; items;</code></td>
</tr>
<tr>
<td>@NotBlank</td>
<td>null, 빈 문자열, 공백 허용하지 않음</td>
<td><code>@NotBlank private String text;</code></td>
</tr>
</tbody></table>
<h3 id="문자열-검증"><strong>문자열 검증</strong></h3>
<table>
<thead>
<tr>
<th>어노테이션</th>
<th>설명</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td>@Size</td>
<td>문자열/컬렉션 길이 제한</td>
<td><code>@Size(min=2, max=50) private String username;</code></td>
</tr>
<tr>
<td>@Length</td>
<td>문자열 길이 제한</td>
<td><code>@Length(min=2, max=50) private String nickname;</code></td>
</tr>
<tr>
<td>@Pattern</td>
<td>정규식 패턴 검증</td>
<td><code>@Pattern(regexp=&quot;^[A-Za-z0-9]+$&quot;) private String code;</code></td>
</tr>
<tr>
<td>@Email</td>
<td>이메일 형식 검증</td>
<td><code>@Email private String email;</code></td>
</tr>
<tr>
<td>@CreditCardNumber</td>
<td>신용카드 번호 형식 검증</td>
<td><code>@CreditCardNumber private String cardNumber;</code></td>
</tr>
</tbody></table>
<h3 id="숫자-범위"><strong>숫자 범위</strong></h3>
<table>
<thead>
<tr>
<th>어노테이션</th>
<th>설명</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td>@Min</td>
<td>최소값 제한</td>
<td><code>@Min(18) private int age;</code></td>
</tr>
<tr>
<td>@Max</td>
<td>최대값 제한</td>
<td><code>@Max(100) private int score;</code></td>
</tr>
<tr>
<td>@Range</td>
<td>숫자 범위 제한</td>
<td><code>@Range(min=0, max=100) private int percentage;</code></td>
</tr>
<tr>
<td>@DecimalMin</td>
<td>소수 최소값 제한</td>
<td><code>@DecimalMin(&quot;0.1&quot;) private BigDecimal value;</code></td>
</tr>
<tr>
<td>@DecimalMax</td>
<td>소수 최대값 제한</td>
<td><code>@DecimalMax(&quot;10.5&quot;) private BigDecimal price;</code></td>
</tr>
<tr>
<td>@Positive</td>
<td>양수만 허용</td>
<td><code>@Positive private int quantity;</code></td>
</tr>
<tr>
<td>@Negative</td>
<td>음수만 허용</td>
<td><code>@Negative private int debt;</code></td>
</tr>
<tr>
<td>@PositiveOrZero</td>
<td>양수와 0 허용</td>
<td><code>@PositiveOrZero private int count;</code></td>
</tr>
<tr>
<td>@NegativeOrZero</td>
<td>음수와 0 허용</td>
<td><code>@NegativeOrZero private int balance;</code></td>
</tr>
</tbody></table>
<h3 id="날짜시간-검증"><strong>날짜/시간 검증</strong></h3>
<table>
<thead>
<tr>
<th>어노테이션</th>
<th>설명</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td>@Future</td>
<td>미래 날짜만 허용</td>
<td><code>@Future private LocalDate eventDate;</code></td>
</tr>
<tr>
<td>@Past</td>
<td>과거 날짜만 허용</td>
<td><code>@Past private LocalDate birthDate;</code></td>
</tr>
<tr>
<td>@FutureOrPresent</td>
<td>미래 또는 현재 날짜</td>
<td><code>@FutureOrPresent private LocalDate reservationDate;</code></td>
</tr>
<tr>
<td>@PastOrPresent</td>
<td>과거 또는 현재 날짜</td>
<td><code>@PastOrPresent private LocalDate completedDate;</code></td>
</tr>
</tbody></table>
<h3 id="기타"><strong>기타</strong></h3>
<table>
<thead>
<tr>
<th>어노테이션</th>
<th>설명</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td>@AssertTrue</td>
<td>boolean 값이 true여야 함</td>
<td><code>@AssertTrue private boolean isAgreed;</code></td>
</tr>
<tr>
<td>@AssertFalse</td>
<td>boolean 값이 false여야 함</td>
<td><code>@AssertFalse private boolean isRejected;</code></td>
</tr>
</tbody></table>