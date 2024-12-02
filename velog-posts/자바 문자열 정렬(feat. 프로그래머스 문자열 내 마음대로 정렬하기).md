<h3 id="📖문제">📖문제</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/12915"><img alt="문자열 내 마음대로 정렬하기 문제" src="https://i.postimg.cc/cJC2JVcb/Screenshot-2024-11-27-at-12-03-54.png" title="문자열 내 마음대로 정렬하기 문제" /></a></p>
<h3 id="🔥익명-객체-사용">🔥익명 객체 사용</h3>
<h4 id="코드">코드</h4>
<pre><code class="language-java">import java.util.*;

class Solution {
    public String[] solution(String[] strings, int n) {
        Arrays.sort(strings, new Comparator&lt;String&gt;() {
            @Override
            public int compare(String s1, String s2) {
                if (s1.charAt(n) == s2.charAt(n)) return s1.compareTo(s2);
                return s1.charAt(n) - s2.charAt(n);
            }
        });
        return strings;
    }
}</code></pre>
<h4 id="풀이">풀이</h4>
<p>Arrays.sort()를 사용하여 strings 배열을 정렬한다. 이때 두 번째 인자로 커스텀 Comparator를 제공하여 정렬 기준을 정의한다.</p>
<pre><code class="language-java">Arrays.sort(strings, new Comparator&lt;String&gt;(){
    @Override
    public int compare(String s1, String s2){
        // 비교 로직
    }
});</code></pre>
<p>Comparator 로직</p>
<pre><code class="language-java">//n번째 문자가 같으면, 전체 문자열을 사전순으로 비교.
if (s1.charAt(n) == s2.charAt(n)) return s1.compareTo(s2);</code></pre>
<p><br /><br /></p>
<h3 id="🔥람다식-사용">🔥람다식 사용</h3>
<p>위에서 익명 객체를 사용해 작성한 코드는 람다식으로 변경할 수 있다.</p>
<pre><code class="language-java">import java.util.*;

class Solution {
    public String[] solution(String[] strings, int n) {
        Arrays.sort(strings, (s1, s2) -&gt; {
            if(s1.charAt(n) == s2.charAt(n)) return s1.compareTo(s2);
            return s1.charAt(n) - s2.charAt(n);
        });
        return strings;
    }
}</code></pre>
<p><br /><br /></p>
<h3 id="🔥스트림-사용">🔥스트림 사용</h3>
<h4 id="코드-1">코드</h4>
<pre><code class="language-java">import java.util.*;

class Solution {
    public String[] solution(String[] strings, int n) {
        return Arrays.stream(strings)
                .sorted(Comparator.comparing(((String s) -&gt; s.charAt(n)))
                        .thenComparing(Comparator.naturalOrder()))
                .toArray(String[]::new);
    }
}</code></pre>
<h4 id="풀이-1">풀이</h4>
<ol>
<li>Arrays.stream(strings)로 문자열 배열을 스트림으로 변환.</li>
<li>sorted() 내에서 Comparator를 사용하여 정렬 기준을 정의:<pre><code class="language-java">//각 문자열의 n번째 문자를 기준으로 정렬.
Comparator.comparing((String s) -&gt; s.charAt(n))</code></pre>
<pre><code class="language-java">//n번째 문자가 같을 경우 전체 문자열을 사전순으로 정렬.
.thenComparing(Comparator.naturalOrder())</code></pre>
</li>
<li>toArray(String[]::new)를 사용하여 정렬된 스트림을 다시 String 배열로 변환.</li>
</ol>