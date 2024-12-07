<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120912">https://school.programmers.co.kr/learn/courses/30/lessons/120912</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;

class Solution {
    public int solution(int[] array) {
        return (int)Arrays.stream(array)
                .flatMap(num -&gt; String.valueOf(num).chars())
                .filter(c -&gt; c == '7')
                .count();
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>배열을 스트림으로 변환</li>
<li>각 숫자를 문자열로 변환하고 문자 스트림으로 평면화</li>
<li>'7'과 같은 문자만 필터링</li>
<li>필터링된 '7'의 개수를 세어 반환</li>
</ol>