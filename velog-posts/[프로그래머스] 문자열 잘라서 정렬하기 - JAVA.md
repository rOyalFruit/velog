<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181866">https://school.programmers.co.kr/learn/courses/30/lessons/181866</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public String[] solution(String myString) {
        return Arrays.stream(myString.split(&quot;x&quot;))
                .filter(s -&gt; !s.isEmpty())
                .sorted()
                .toArray(String[]::new);
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>문자열을 'x'로 분리하고 스트림으로 변환</li>
<li>빈 문자열 제외하고 사전순으로 정렬</li>
<li>정렬된 스트림을 문자열 배열로 변환</li>
</ol>