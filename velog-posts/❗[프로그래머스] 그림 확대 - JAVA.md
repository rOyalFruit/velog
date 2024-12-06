<blockquote>
</blockquote>
<p>이게 Lv.0 ?????😠💢</p>
<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181836">https://school.programmers.co.kr/learn/courses/30/lessons/181836</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;
import java.util.stream.*;

class Solution {
    public String[] solution(String[] picture, int k) {
        return Stream.of(picture)
                .flatMap(row -&gt; Stream.generate(() -&gt; row)
                        .limit(k)
                        .map(s -&gt; s.chars()
                                .mapToObj(ch -&gt; String.valueOf((char)ch).repeat(k))
                                .collect(Collectors.joining())))
                .toArray(String[]::new);
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>입력 배열을 스트림으로 변환</li>
<li>각 행을 k번 반복</li>
<li>각 문자를 k번 반복</li>
<li>결과를 String 배열로 변환</li>
</ol>
<hr />
<h3 id="상세-풀이">상세 풀이</h3>
<h4 id="step-1---배열-값-확인">STEP 1 - 배열 값 확인</h4>
<ul>
<li>입출력 예시 중 첫 번째를 확인해 보자.</li>
<li>[&quot;.xx...xx.&quot;, &quot;x..x.x..x&quot;, &quot;x...x...x&quot;, &quot;.x.....x.&quot;, &quot;..x...x..&quot;, &quot;...x.x...&quot;, &quot;....x....&quot;]의 요소들을 한 줄씩 출력해 보면 아래와 같은 모양이 나온다.<pre><code>.xx...xx.
x..x.x..x
x...x...x
.x.....x.
..x...x..
...x.x...
....x....</code></pre><br />

</li>
</ul>
<h4 id="step-2---모양-세로로-k배-늘리기">STEP 2 - 모양 세로로 k배 늘리기</h4>
<pre><code class="language-java">Stream.of(picture)
        .flatMap(row -&gt; Stream.generate(() -&gt; row).limit(k)) // 세로로 k 배 늘리기
        .forEach(System.out::println);                         // 출력용 코드</code></pre>
<ul>
<li>입력으로 받은 배열의 모든 요소들을 k개로 만들어주는 부분이다.</li>
<li>출력을 위해 forEach를 수행했고, 출력 결과는 아래와 같다.<pre><code>.xx...xx.
.xx...xx.
x..x.x..x
x..x.x..x
x...x...x
x...x...x
.x.....x.
.x.....x.
..x...x..
..x...x..
...x.x...
...x.x...
....x....
....x....</code></pre><br />

</li>
</ul>
<h4 id="step-3---모양-가로로-k배-늘리기">STEP 3 - 모양 가로로 k배 늘리기</h4>
<pre><code class="language-java">Stream.of(picture)
                .flatMap(row -&gt; Stream.generate(() -&gt;  row).limit(k))         // 세로로 k배 늘리기
                .map(s -&gt; s.chars()
                        .mapToObj(ch -&gt; String.valueOf((char)ch).repeat(k)) // 가로로 k배 늘리기
                        .collect(Collectors.joining()))                     // mapToObj의 결과가 Stream&lt;String&gt; 이므로 String으로 변환
                .forEach(System.out::println);                                 // 출력용 코드</code></pre>
<ul>
<li>배열의 모든 요소들의 값을 k번씩 반복해서 가로로 k배 증가하게 만드는 부분이다.</li>
<li>출력해보면 아래와 같은 모양이 나온다.<pre><code>..xxxx......xxxx..
..xxxx......xxxx..
xx....xx..xx....xx
xx....xx..xx....xx
xx......xx......xx
xx......xx......xx
..xx..........xx..
..xx..........xx..
....xx......xx....
....xx......xx....
......xx..xx......
......xx..xx......
........xx........
........xx........</code></pre><br />

</li>
</ul>
<h4 id="step-4---string로-리턴하기">STEP 4 - String[]로 리턴하기</h4>
<ul>
<li>STEP 3에서 모양이 잘 나오는 것을 확인했으니 스트림을 문자열 배열로 반환한다.<pre><code class="language-java">return Stream.of(picture)
              .flatMap(row -&gt; Stream.generate(() -&gt; row)
                      .limit(k)
                      .map(s -&gt; s.chars()
                              .mapToObj(ch -&gt; String.valueOf((char)ch).repeat(k))
                              .collect(Collectors.joining())))
              .toArray(String[]::new);</code></pre>
</li>
</ul>