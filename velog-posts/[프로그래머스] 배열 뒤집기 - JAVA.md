<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120821">https://school.programmers.co.kr/learn/courses/30/lessons/120821</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.IntStream;

class Solution {
    public int[] solution(int[] num_list) {
    return IntStream.rangeClosed(1, num_list.length)
                    .map(i -&gt; num_list[num_list.length - i])
                    .toArray();
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>1부터 배열 길이까지의 스트림 생성</li>
<li>각 인덱스를 역순으로 매핑</li>
<li>배열로 변환</li>
</ol>