<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120904">https://school.programmers.co.kr/learn/courses/30/lessons/120904</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">class Solution {
    public int solution(int num, int k) {
        return (&quot; &quot; + num).indexOf(String.valueOf(k));
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>숫자 앞에 공백을 추가하여 인덱스를 1부터 시작하게 함</li>
<li>num과 k를 문자열로 변환</li>
<li>num에서 k의 인덱스 찾기</li>
</ol>
<hr />
<h3 id="alternative-solution">Alternative Solution</h3>
<pre><code class="language-java">import java.util.stream.Collectors;

class Solution {
    public int solution(int num, int k) {
        return (&quot; &quot; + num).chars()
                .mapToObj(c -&gt; String.valueOf((char)c))
                .collect(Collectors.toList())
                .indexOf(String.valueOf(k));
    }
}</code></pre>
<ol>
<li>숫자 앞에 공백을 추가하여 인덱스를 1부터 시작하게 함</li>
<li>문자열을 IntStream으로 변환</li>
<li>각 문자를 문자열로 변환</li>
<li>문자열 리스트로 변환</li>
<li>k의 인덱스를 찾음</li>
</ol>
<p>-&gt; 스트림 사용에 빨리 익숙해지자!</p>