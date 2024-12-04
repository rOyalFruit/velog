<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120899">https://school.programmers.co.kr/learn/courses/30/lessons/120899</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;
import java.util.stream.*;

class Solution {
    public int[] solution(int[] array) {
        int max = Arrays.stream(array).max().getAsInt();
        int idx = IntStream.rangeClosed(0, array.length)
                .filter(i -&gt; array[i] == max)
                .findFirst()
                .orElse(-1);

        return new int[]{max, idx};
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>배열에서 최댓값을 찾음</li>
<li>최댓값과 같은 요소의 인덱스 찾기</li>
<li>최댓값과 그 인덱스를 배열로 반환</li>
</ol>