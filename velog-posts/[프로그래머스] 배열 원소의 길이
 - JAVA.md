<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120854">https://school.programmers.co.kr/learn/courses/30/lessons/120854</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public int[] solution(String[] strlist) {
        return Arrays.stream(strlist)
                .mapToInt(String::length)
                .toArray();
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>문자열 배열을 스트림으로 변환</li>
<li>각 문자열의 길이를 정수로 매핑</li>
<li>결과를 int 배열로 변환</li>
</ol>