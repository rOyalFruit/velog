<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181870">https://school.programmers.co.kr/learn/courses/30/lessons/181870</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public String[] solution(String[] strArr) {
        return Arrays.stream(strArr)
                .filter(s -&gt; s.indexOf(&quot;ad&quot;) == -1)
                .toArray(String[]::new);
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>입력 배열을 스트림으로 변환</li>
<li>&quot;ad&quot; 미포함 문자열만 선택</li>
<li>문자열 배열로 변환</li>
</ol>