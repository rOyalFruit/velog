<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181863">https://school.programmers.co.kr/learn/courses/30/lessons/181863</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;
import java.util.stream.*;

class Solution {
    public String solution(String rny_string) {
        return Arrays.stream(rny_string.split(&quot;&quot;))
                .map(s -&gt; s.equals(&quot;m&quot;) ? &quot;rn&quot; : s)
                .collect(Collectors.joining());
    }
}</code></pre>
<h3 id="key-point">Key point</h3>
<ol>
<li>문자열을 개별 문자열 배열로 분리한 뒤 스트림으로 변환</li>
<li>각 문자열이 m이면 rn으로, 아니면 원래 문자열 유지</li>
<li>변환된 문자열을 하나의 문자열로 결합</li>
</ol>
<hr />
<h3 id="alternative-solution">Alternative Solution</h3>
<pre><code class="language-java">class Solution {
    public String solution(String rny_string) {
        return rny_string.replaceAll(&quot;m&quot;, &quot;rn&quot;);
    }
}</code></pre>