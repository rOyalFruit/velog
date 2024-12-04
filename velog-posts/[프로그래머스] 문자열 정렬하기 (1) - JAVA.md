<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120850">https://school.programmers.co.kr/learn/courses/30/lessons/120850</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">class Solution {
    public int[] solution(String my_string) {
        return my_string
                .chars()
                .filter(c -&gt; Character.isDigit((char)c))
                .map(Character::getNumericValue)
                .sorted()
                .toArray();
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>문자열을 IntStream으로 변환</li>
<li>숫자 형태의 문자만 필터링</li>
<li>문자를 해당하는 정수 값으로 변환</li>
<li>오름차순으로 정렬</li>
<li>배열로 변환</li>
</ol>
<hr />
<h3 id="alternative-solution">Alternative Solution</h3>
<pre><code class="language-java">import java.util.*;

class Solution {
    public int[] solution(String my_string) {
        return Arrays.stream(my_string.replaceAll(&quot;[^0-9]&quot;, &quot;&quot;).split(&quot;&quot;))
                .sorted()
                .mapToInt(Integer::parseInt)
                .toArray();
    }
}</code></pre>