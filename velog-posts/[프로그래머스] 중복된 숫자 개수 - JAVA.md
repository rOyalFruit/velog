<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120583">https://school.programmers.co.kr/learn/courses/30/lessons/120583</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public int solution(int[] array, int n) {
        return (int)Arrays.stream(array) 
                .filter(num -&gt; num == n) 
                .count();                  
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>배열을 스트림으로 변환</li>
<li>특정 값과 일치하는 요소만 필터링</li>
<li>필터링된 요소의 총 개수를 반환</li>
<li>long 타입의 count를 int로 형변환</li>
</ol>