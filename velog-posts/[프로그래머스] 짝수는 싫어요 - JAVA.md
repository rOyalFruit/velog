<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120813">https://school.programmers.co.kr/learn/courses/30/lessons/120813</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.IntStream;

class Solution {
    public int[] solution(int n) {
        return IntStream.rangeClosed(1, n)
                .filter(num -&gt; num % 2 == 1)
                .toArray();
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>1부터 n까지의 연속된 정수 스트림 생성</li>
<li>홀수만 선택 (2로 나누었을 때 나머지가 1인 숫자)</li>
<li>필터링된 홀수들을 정수 배열로 변환</li>
</ol>