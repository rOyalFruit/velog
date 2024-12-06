<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120886">https://school.programmers.co.kr/learn/courses/30/lessons/120886</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Map;
import java.util.stream.Collectors;

class Solution {
    public int solution(String before, String after) {
        Map&lt;Integer, Long&gt; beforeFrequency = before.chars()
                .boxed()
                .collect(Collectors.groupingBy(c -&gt; c, Collectors.counting()));

        Map&lt;Integer, Long&gt; afterFrequency = after.chars()
                .boxed()
                .collect(Collectors.groupingBy(c -&gt; c, Collectors.counting()));

        return beforeFrequency.equals(afterFrequency) ? 1 : 0;
    }
}</code></pre>
<h3 id="key-points">Key Points</h3>
<ol>
<li>before 문자열의 각 문자 빈도수 계산</li>
<li>after 문자열의 각 문자 빈도수 계산</li>
<li>각 문자별 빈도수가 같으면 1, 다르면 0 반환</li>
</ol>
<hr />
<h3 id="추가-자료">추가 자료</h3>
<h4 id="collect-최종연산에서-map-만들기1">collect 최종연산에서 Map 만들기<a href="https://api.velog.io/rss/@ekdeon#section">[1]</a></h4>
<p>1.
<code>toMap()</code>: 단순히 스트림을 Map으로 변환하고 싶을 때 사용
2.
<code>groupingBy()</code>: 스트림 요소 들을 특정 조건으로 그룹핑하여 맵으로 반환하고 싶을 때 사용</p>
<h4 id="mapequals">Map.equals()</h4>
<ul>
<li>두 Map m1과 m2가 같다는 것은 m1.entrySet().equals(m2.entrySet())이 true라는 것을 의미.</li>
<li>공식 문서를 보려면 <a href="https://docs.oracle.com/en/java/javase/23/docs/api/java.base/java/util/Map.html#equals(java.lang.Object)">여기</a>를 클릭</li>
</ul>
<hr />
<p><a id="section"></a>
참고: 
[1] <a href="https://sihyung92.oopy.io/df74fa36-9d86-4ff0-b802-d088b9bd5074">https://sihyung92.oopy.io/df74fa36-9d86-4ff0-b802-d088b9bd5074</a></p>