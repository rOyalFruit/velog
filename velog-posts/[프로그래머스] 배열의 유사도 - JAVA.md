<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120903">https://school.programmers.co.kr/learn/courses/30/lessons/120903</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;

class Solution {
    public int solution(String[] s1, String[] s2) {
        Set&lt;String&gt; set2 = new HashSet&lt;&gt;(List.of(s2));
        return (int)Arrays.stream(s1)
                .filter(set2::contains)
                .count();
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>s2 배열의 요소들을 HashSet으로 변환</li>
<li>s1 배열을 스트림으로 변환</li>
<li>s1의 각 요소가 set2에 포함되어 있는지 확인</li>
<li>조건을 만족하는 요소의 개수를 계산</li>
</ol>
<hr />
<h3 id="alternative-solution">Alternative Solution</h3>
<pre><code class="language-java">import java.util.*;

class Solution {
    public int solution(String[] s1, String[] s2) {
        Set&lt;String&gt; set2 = new HashSet&lt;&gt;(List.of(s2));
        set2.retainAll(List.of(s1));

        return set2.size();
    }
}</code></pre>
<ul>
<li>Set의 retainAll()을 사용하면 교집합을 구할 수 있다</li>
</ul>