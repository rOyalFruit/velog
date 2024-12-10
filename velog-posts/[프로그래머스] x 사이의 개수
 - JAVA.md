<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181867">https://school.programmers.co.kr/learn/courses/30/lessons/181867</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public int[] solution(String myString) {
        return Arrays.stream(myString.split(&quot;x&quot;, -1))
                .mapToInt(String::length)
                .toArray();
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>문자열을 'x'로 분리(빈 문자열 포함)하고 스트림으로 변환</li>
<li>문자열 길이를 정수 스트림으로 변환</li>
<li>최종 결과를 정수 배열로 반환</li>
</ol>