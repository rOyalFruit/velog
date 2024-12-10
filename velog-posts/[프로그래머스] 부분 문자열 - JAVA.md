<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181842">https://school.programmers.co.kr/learn/courses/30/lessons/181842</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.*;

class Solution {
    public int solution(String str1, String str2) {
        return IntStream.rangeClosed(0, str2.length() - str1.length())
            .mapToObj(i -&gt; str2.substring(i, i + str1.length()))
            .anyMatch(str1::equals) ? 1 : 0;
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>str2에서 str1 길이의 부분 문자열을 추출할 수 있는 모든 시작 인덱스 생성</li>
<li>각 시작 인덱스에서 str1 길이만큼의 부분 문자열 추출</li>
<li>str1과 일치하는 부분 문자열이 있으면 1, 없으면 0 반환</li>
</ol>
<hr />
<h3 id="alternative-solution">Alternative Solution</h3>
<pre><code class="language-java">class Solution {
    public int solution(String str1, String str2) {
        return str2.contains(str1) ? 1 : 0;
    }
}</code></pre>