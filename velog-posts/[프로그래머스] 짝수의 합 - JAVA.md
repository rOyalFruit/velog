<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120831">https://school.programmers.co.kr/learn/courses/30/lessons/120831</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.IntStream;

class Solution {
    public int solution(int n) {
        return IntStream.rangeClosed(1, n)
                .filter(i -&gt; i % 2 == 0)
                .sum();
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>1부터 n까지의 정수 스트림 생성</li>
<li>짝수만 필터링</li>
<li>필터링된 짝수들의 합 계산</li>
</ol>