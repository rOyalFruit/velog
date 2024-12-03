<blockquote>
</blockquote>
<h3 id="이것만-보면-완벽-이해">이것만 보면 완벽 이해</h3>
<ul>
<li>중복된 스트림을 1차원으로 평면화 시키는 메서드</li>
<li>[[1, 2, 3], [4, 5, 6], [7, 8, 9]] -&gt; [1, 2, 3, 4, 5, 6, 7, 8, 9]</li>
<li>[&quot;Hello&quot;, &quot;World&quot;] -&gt; [&quot;H&quot;, &quot;e&quot;, &quot;l&quot;, &quot;l&quot;, &quot;o&quot;, &quot;W&quot;, &quot;o&quot;, &quot;r&quot;, &quot;l&quot;, &quot;d&quot;]</li>
</ul>
<hr />
<h2 id="사용-예시">사용 예시</h2>
<ol>
<li>중첩 리스트 평탄화<pre><code class="language-java">List&lt;List&lt;Integer&gt;&gt; nestedList = Arrays.asList(
 Arrays.asList(1, 2, 3),
 Arrays.asList(4, 5, 6),
 Arrays.asList(7, 8, 9)
);
</code></pre>
</li>
</ol>
<p>List flatList = nestedList.stream()
    .flatMap(Collection::stream)
    .collect(Collectors.toList());
// 결과: [1, 2, 3, 4, 5, 6, 7, 8, 9]</p>
<pre><code>
2. 문자열을 개별 문자로 분리
```java
List&lt;String&gt; words = Arrays.asList(&quot;Hello&quot;, &quot;World&quot;);
List&lt;Character&gt; chars = words.stream()
    .flatMap(word -&gt; word.chars().mapToObj(ch -&gt; (char) ch))
    .collect(Collectors.toList());
// 결과: [H, e, l, l, o, W, o, r, l, d]</code></pre><ol start="3">
<li>복잡한 객체 구조 평탄화<pre><code class="language-java">class Department {
 private String name;
 private List&lt;Employee&gt; employees;
 // 생성자, getter, setter 생략
}
</code></pre>
</li>
</ol>
<p>class Employee {
    private String name;
    // 생성자, getter, setter 생략
}</p>
<p>List departments = Arrays.asList(
    new Department(&quot;IT&quot;, Arrays.asList(new Employee(&quot;Alice&quot;), new Employee(&quot;Bob&quot;))),
    new Department(&quot;HR&quot;, Arrays.asList(new Employee(&quot;Charlie&quot;), new Employee(&quot;David&quot;)))
);</p>
<p>List allEmployeeNames = departments.stream()
    .flatMap(dept -&gt; dept.getEmployees().stream())
    .map(Employee::getName)
    .collect(Collectors.toList());
// 결과: [Alice, Bob, Charlie, David]</p>
<pre><code>
4. Optional 값 처리
```java
List&lt;Optional&lt;String&gt;&gt; optionals = Arrays.asList(
    Optional.of(&quot;Hello&quot;),
    Optional.empty(),
    Optional.of(&quot;World&quot;)
);

List&lt;String&gt; result = optionals.stream()
    .flatMap(Optional::stream)
    .collect(Collectors.toList());
// 결과: [Hello, World]</code></pre>