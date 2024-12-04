<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120826">https://school.programmers.co.kr/learn/courses/30/lessons/120826</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.Collectors;

class Solution {
    public String solution(String my_string, String letter) {
        return my_string.chars()
                .filter(c -&gt; c != letter.charAt(0))
                .mapToObj(c -&gt; String.valueOf((char)c))
                .collect(Collectors.joining());
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>문자열을 문자 스트림으로 변환</li>
<li>letter의 첫 문자와 다른 문자만 필터링</li>
<li>스트림을 문자열로 변환</li>
<li>하나의 문자열로 결합</li>
</ol>