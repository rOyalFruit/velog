<blockquote>
<p><a href="https://velog.io/write?id=eb8028ac-bd6f-417c-9698-2bea006e7f32">부분 문자열</a> 문제와 풀이가 똑같다.</p>
</blockquote>
<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181843">https://school.programmers.co.kr/learn/courses/30/lessons/181843</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.stream.*;

class Solution {
    public int solution(String my_string, String target) {
        return IntStream.rangeClosed(0, my_string.length() - target.length())
            .mapToObj(i -&gt; my_string.substring(i, i + target.length()))
            .anyMatch(target::equals) ? 1 : 0;
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>my_string에서 target 길이의 부분 문자열을 추출할 수 있는 모든 시작 인덱스 생성</li>
<li>각 시작 인덱스에서 target 길이만큼의 부분 문자열 추출</li>
<li>target과 일치하는 부분 문자열이 있으면 1, 없으면 0 반환</li>
</ol>
<hr />
<h3 id="alternative-solution">Alternative Solution</h3>
<pre><code class="language-java">import java.util.stream.*;

class Solution {
    public int solution(String my_string, String target) {
        return my_string.contains(target) ? 1 : 0;
    }
}</code></pre>