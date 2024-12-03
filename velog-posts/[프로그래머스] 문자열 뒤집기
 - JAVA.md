<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120822">https://school.programmers.co.kr/learn/courses/30/lessons/120822</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">class Solution {
    public String solution(String my_string) {
    return my_string.chars()
            .mapToObj(ch -&gt; String.valueOf((char)ch))
            .reduce(&quot;&quot;, (a, b) -&gt; b + a);
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>문자열의 각 문자를 스트림으로 변환</li>
<li>각 문자를 문자열로 변환 (Character -&gt; String)</li>
<li>문자열을 역순으로 누적하여 역순으로 만듦 (reduce 연산)</li>
</ol>
<hr />
<h3 id="alternative-solution">Alternative Solution</h3>
<pre><code class="language-java">class Solution {
    public String solution(String my_string) {
        return new StringBuffer(my_string) // StringBuilder를 사용하여 입력 문자열을 생성
                .reverse()                    // 문자열을 역순으로 뒤집음
                .toString();                // 최종 뒤집힌 문자열을 반환
    }
}</code></pre>