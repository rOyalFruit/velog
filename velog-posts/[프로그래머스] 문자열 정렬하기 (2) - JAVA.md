<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120911">https://school.programmers.co.kr/learn/courses/30/lessons/120911</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;
import java.util.stream.*;

class Solution {
    public String solution(String my_string) {
        return Arrays.stream(my_string.toLowerCase()
            .split(&quot;&quot;))
            .sorted()
            .collect(Collectors.joining());
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>문자열을 소문자로 변환</li>
<li>각 문자로 분리하여 배열로 변환</li>
<li>배열을 정렬</li>
<li>정렬된 문자를 하나의 문자열로 결합하여 반환</li>
</ol>