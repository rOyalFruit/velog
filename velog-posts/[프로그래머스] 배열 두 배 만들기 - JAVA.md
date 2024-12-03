<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120809">https://school.programmers.co.kr/learn/courses/30/lessons/120809</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public int[] solution(int[] numbers) {
        return Arrays.stream(numbers)
                .map(n -&gt; n*2)
                .toArray();
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>배열을 스트림으로 변환</li>
<li>각 요소를 2배로 곱함</li>
<li>스트림을 배열로 변환</li>
</ol>