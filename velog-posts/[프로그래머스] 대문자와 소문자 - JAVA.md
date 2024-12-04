<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120893">https://school.programmers.co.kr/learn/courses/30/lessons/120893</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.Collectors;

class Solution {
    public String solution(String my_string) {
        return my_string.chars()
                .mapToObj(c -&gt; Character.isUpperCase(c) ?
                        String.valueOf((char)Character.toLowerCase(c)) :
                        String.valueOf((char)Character.toUpperCase(c)))
                .collect(Collectors.joining());
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>문자열을 스트림으로 변환</li>
<li>현재 문자가 대문자면 소문자로, 소문자면 대문자로 변환</li>
<li>변환된 문자들을 하나의 문자열로 결합</li>
</ol>