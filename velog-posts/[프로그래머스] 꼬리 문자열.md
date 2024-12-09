<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181841">https://school.programmers.co.kr/learn/courses/30/lessons/181841</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;
import java.util.stream.*;

class Solution {
    public String solution(String[] str_list, String ex) {
        return Arrays.stream(str_list)
                .filter(str -&gt; !str.contains(ex))
                .collect(Collectors.joining());
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>문자열 배열을 스트림으로 변환</li>
<li>ex를 포함하지 않는 문자열만 필터링</li>
<li>필터링된 문자열들을 하나로 연결</li>
</ol>