<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181833">https://school.programmers.co.kr/learn/courses/30/lessons/181833</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.IntStream;

class Solution {
    public int[][] solution(int n) {
        return IntStream.range(0, n)
            .mapToObj(i -&gt; IntStream.range(0, n)
                .map(j -&gt; i == j ? 1 : 0)
                .toArray())
            .toArray(int[][]::new);
    }
}</code></pre>
<h3 id="alternative-solution">Alternative Solution</h3>
<pre><code class="language-java">import java.util.stream.IntStream;

class Solution {
    public int[][] solution(int n) {
        int[][] answer = new int[n][n];
        IntStream.range(0, n).forEach(i -&gt; answer[i][i] = 1);

        return answer;
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>n x n 크기의 2차원 배열(단위 행렬)을 생성</li>
<li>대각선 요소(i==j)는 1로, 나머지는 0으로 설정</li>
</ol>