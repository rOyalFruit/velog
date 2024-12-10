<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181834">https://school.programmers.co.kr/learn/courses/30/lessons/181834</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code>import java.util.stream.Collectors;

class Solution {
    public String solution(String myString) {
        return myString.chars()
                .mapToObj(i -&gt; Character.toString(Integer.max(i, 'l')))
                .collect(Collectors.joining());
    }
}</code></pre><hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>문자열을 IntStream으로 변환</li>
<li>&quot;l&quot;보다 앞서는 모든 문자를 &quot;l&quot;로 변경</li>
<li>처리된 문자들을 하나의 문자열로 결합</li>
</ol>
<hr />
<h3 id="alternative-solution">Alternative Solution</h3>
<pre><code class="language-java">class Solution {
    public String solution(String myString) {
        return myString.replaceAll(&quot;[a-k]&quot;, &quot;l&quot;);
    }
}</code></pre>