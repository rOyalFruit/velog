<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120839">https://school.programmers.co.kr/learn/courses/30/lessons/120839</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;
import java.util.stream.*;

class Solution {
    public String solution(String rsp) {
        Map&lt;Character, Character&gt; winningMoves  = Map.of(
                '2', '0',
                '0', '5',
                '5', '2'
        );
        return rsp.chars()
                .mapToObj(c -&gt; winningMoves .get((char)c))
                .map(String::valueOf)
                .collect(Collectors.joining());
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>가위바위보에서 각 손동작을 이기는 손동작을 매핑하는 Map 생성</li>
<li>입력된 rsp 문자열을 문자 스트림으로 변환</li>
<li>각 문자(손동작)를 이기는 손동작으로 변환</li>
<li>Character를 String으로 변환</li>
<li>모든 결과를 하나의 문자열로 결합</li>
</ol>