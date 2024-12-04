<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/120833">https://school.programmers.co.kr/learn/courses/30/lessons/120833</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.*;

class Solution {
    public int[] solution(int[] numbers, int num1, int num2) {
        return Arrays.stream(numbers)
            .skip(num1)
            .limit(num2 - num1 + 1)
            .toArray();
    }
}</code></pre>
<hr />
<h3 id="key-points">Key Points</h3>
<ol>
<li>배열을 IntStream으로 변환</li>
<li>스트림에서 num1개의 요소를 건너뜀(num1 인덱스부터 시작하기 위함)</li>
<li>num1부터 num2까지의 요소 개수만큼만 스트림을 제한(num2 - num1 + 1은 num1부터 num2까지의 요소 개수)</li>
<li>처리된 스트림을 다시 정수 배열로 변환</li>
</ol>