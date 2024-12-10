<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181840">https://school.programmers.co.kr/learn/courses/30/lessons/181840</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.IntStream;

class Solution {
    public int solution(int[] num_list, int n) {
        return IntStream.of(num_list)
            .anyMatch(num -&gt; num == n) ? 1 : 0;
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>배열을 IntStream으로 변환</li>
<li>n과 일치하는 요소가 있으면 1, 없으면 0 반환</li>
</ol>