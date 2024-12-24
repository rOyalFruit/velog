<h2 id="원인">원인</h2>
<blockquote>
</blockquote>
<p>java.lang.IllegalArgumentException: Name for argument of type [long] not specified, and parameter name information not available via reflection. Ensure that the compiler uses the '-parameters' flag.</p>
<p> 
 </p>
<h2 id="문제가-된-코드">문제가 된 코드</h2>
<pre><code class="language-java">@DeleteMapping(&quot;/{id}&quot;)
public String deleteItem(@PathVariable long id){
    Post post = postService.findById(id).get();
    postService.delete(post);

    return &quot;삭제가 완료되었습니다.&quot;;
}</code></pre>
<ul>
<li>메서드 파라미터의 이름을 자동으로 인식하지 못해 발생하는 문제. </li>
<li>@PathVariable이나 @RequestParam과 같은 어노테이션을 사용할 때 파라미터 이름을 명시적으로 지정하지 않아서 발생한다.</li>
<li>이름을 생략하려면 컴파일 시 -parameters 플래그를 활성화해야 한다.</li>
</ul>
<h2 id="해결방법택1">해결방법(택1)</h2>
<h3 id="1-어노테이션에-파라미터-이름-명시">1. 어노테이션에 파라미터 이름 명시</h3>
<pre><code class="language-java">@GetMapping(&quot;/{itemId}&quot;)
public String item(@PathVariable(&quot;itemId&quot;) Long itemId, Model model) {
    // 메서드 내용
}</code></pre>
<hr />
<h3 id="2--parameters-컴파일-옵션-사용">2. -parameters 컴파일 옵션 사용</h3>
<p>IDE 설정에서 Java 컴파일러에 -parameters 옵션을 추가</p>
<ul>
<li>IntelliJ IDEA: File &gt; Settings &gt; Build, Execution, Deployment &gt; Compiler &gt; Java Compiler</li>
<li>Additional command line parameters에 -parameters 추가</li>
</ul>
<hr />