<h3 id="문제-링크">문제 링크</h3>
<p><a href="https://school.programmers.co.kr/learn/courses/30/lessons/181853">https://school.programmers.co.kr/learn/courses/30/lessons/181853</a></p>
<hr />
<h3 id="solution">Solution</h3>
<pre><code class="language-java">import java.util.Arrays;

class Solution {
    public int[] solution(int[] num_list) {
        return Arrays.stream(num_list)
                .sorted()
                .limit(5)
                .toArray();
    }
}</code></pre>
<hr />
<h3 id="key-point">Key point</h3>
<ol>
<li>정수 배열을 스트림으로 변환</li>
<li>스트림의 요소를 오름차순으로 정렬</li>
<li>정렬된 스트림에서 처음 5개의 요소만 선택</li>
<li>선택된 요소들을 새로운 정수 배열로 변환하여 반환</li>
</ol>