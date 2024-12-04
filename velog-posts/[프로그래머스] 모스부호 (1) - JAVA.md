<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120838">https://school.programmers.co.kr/learn/courses/30/lessons/120838</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;
import java.util.stream.Collectors;

class Solution {
    public String solution(String letter) {
        Map&lt;String, String&gt; morse = Map.ofEntries(
                Map.entry(&quot;.-&quot;, &quot;a&quot;), Map.entry(&quot;-...&quot;, &quot;b&quot;), Map.entry(&quot;-.-.&quot;, &quot;c&quot;),
                Map.entry(&quot;-..&quot;, &quot;d&quot;), Map.entry(&quot;.&quot;, &quot;e&quot;), Map.entry(&quot;..-.&quot;, &quot;f&quot;),
                Map.entry(&quot;--.&quot;, &quot;g&quot;), Map.entry(&quot;....&quot;, &quot;h&quot;), Map.entry(&quot;..&quot;, &quot;i&quot;),
                Map.entry(&quot;.---&quot;, &quot;j&quot;), Map.entry(&quot;-.-&quot;, &quot;k&quot;), Map.entry(&quot;.-..&quot;, &quot;l&quot;),
                Map.entry(&quot;--&quot;, &quot;m&quot;), Map.entry(&quot;-.&quot;, &quot;n&quot;), Map.entry(&quot;---&quot;, &quot;o&quot;),
                Map.entry(&quot;.--.&quot;, &quot;p&quot;), Map.entry(&quot;--.-&quot;, &quot;q&quot;), Map.entry(&quot;.-.&quot;, &quot;r&quot;),
                Map.entry(&quot;...&quot;, &quot;s&quot;), Map.entry(&quot;-&quot;, &quot;t&quot;), Map.entry(&quot;..-&quot;, &quot;u&quot;),
                Map.entry(&quot;...-&quot;, &quot;v&quot;), Map.entry(&quot;.--&quot;, &quot;w&quot;), Map.entry(&quot;-..-&quot;, &quot;x&quot;),
                Map.entry(&quot;-.--&quot;, &quot;y&quot;), Map.entry(&quot;--..&quot;, &quot;z&quot;)
        );

        return Arrays.stream(letter.split(&quot; &quot;))
                .map(s -&gt; morse.get(s))
                .collect(Collectors.joining());
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>모스 부호와 해당하는 알파벳을 매핑하는 Map 생성</li>
<li>입력된 모스 부호 문자열을 공백을 기준으로 분리하여 스트림으로 변환</li>
<li>각 모스 부호를 해당하는 알파벳으로 변환</li>
<li>변환된 알파벳들을 하나의 문자열로 결합</li>
</ol>
<hr />
<h3 id="mapofentries-vs-new-hashmap">Map.ofEntries() Vs. new HashMap&lt;&gt;()</h3>
<h4 id="mapofentries">Map.ofEntries()</h4>
<ul>
<li>생성된 Map은 불변(immutable)임.</li>
<li>생성 후에 요소를 추가, 삭제, 수정할 수 없음.</li>
</ul>
<h4 id="new-hashmap">new HashMap&lt;&gt;()</h4>
<ul>
<li>생성된 Map은 가변(mutable)임. </li>
<li>요소를 자유롭게 추가, 삭제, 수정할 수 있음.</li>
</ul>