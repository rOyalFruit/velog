<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181849">https://school.programmers.co.kr/learn/courses/30/lessons/181849</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public int solution(String num_str) {
        return Arrays.stream(num_str.split(&quot;&quot;))
                .mapToInt(Integer::parseInt)
                .sum();
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>문자열을 한 글자씩 분리하여 배열로 만들고, 스트림으로 변환</li>
<li>각 문자열을 정수로 변환</li>
<li>변환된 정수들의 합을 계산하여 반환</li>
</ol>