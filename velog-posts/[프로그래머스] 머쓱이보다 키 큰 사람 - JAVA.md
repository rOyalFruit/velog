<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120585">https://school.programmers.co.kr/learn/courses/30/lessons/120585</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public int solution(int[] array, int height) {
        return (int)Arrays.stream(array)
                .filter(e -&gt; e &gt; height)
                .count();
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>배열을 스트림으로 변환</li>
<li>height보다 큰 요소만 선택</li>
<li>해당 요소들의 개수를 반환</li>
<li>long 타입의 count를 int로 형변환</li>
</ol>