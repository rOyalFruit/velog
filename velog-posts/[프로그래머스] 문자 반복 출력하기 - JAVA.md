<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120825">https://school.programmers.co.kr/learn/courses/30/lessons/120825</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.Collectors;

class Solution {
    public String solution(String my_string, int n) {
        return my_string.chars()
            .mapToObj(ch -&gt; String.valueOf((char) ch).repeat(n))
            .collect(Collectors.joining());
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>문자열의 각 문자를 스트림으로 변환</li>
<li>각 문자를 n번 반복하는 문자열로 변환</li>
<li>반복된 문자열들을 하나의 문자열로 결합</li>
</ol>