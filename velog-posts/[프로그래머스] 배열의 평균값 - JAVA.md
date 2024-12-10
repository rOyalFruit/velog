<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120817">https://school.programmers.co.kr/learn/courses/30/lessons/120817</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public double solution(int[] numbers) {
        return Arrays.stream(numbers).average().orElse(0);
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>배열을 스트림으로 변환</li>
<li>평균 계산</li>
<li>배열이 비어있을 경우 0을 반환</li>
</ol>