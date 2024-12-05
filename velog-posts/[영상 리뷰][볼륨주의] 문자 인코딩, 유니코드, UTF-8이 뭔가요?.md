<p><a href="https://www.youtube.com/watch?v=1jo6q4dihoU&amp;ab_channel=%EC%96%84%ED%8C%8D%ED%95%9C%EC%BD%94%EB%94%A9%EC%82%AC%EC%A0%84"><img alt="movie" src="https://img.youtube.com/vi/1jo6q4dihoU/sddefault.jpg" /></a></p>
<h2 id="문자-인코딩의-개념">문자 인코딩의 개념</h2>
<ul>
<li>컴퓨터는 기본적으로 0과 1의 이진수로 정보를 저장함.</li>
<li>문자를 컴퓨터가 이해할 수 있는 숫자로 변환하는 과정을 인코딩이라고 함.</li>
<li>예: 'A'는 65, 'B'는 66 등으로 매핑됨.</li>
</ul>
<h2 id="인코딩-문제와-유니코드의-등장">인코딩 문제와 유니코드의 등장</h2>
<ul>
<li>초기에는 ASCII 코드를 사용했지만, 영어 외 다른 언어를 표현하는 데 한계가 존재.</li>
<li>각 나라마다 다른 인코딩 방식을 사용하면서 호환성 문제가 발생</li>
<li>이를 해결하기 위해 모든 문자를 하나의 체계로 통일한 유니코드가 만들어짐.</li>
</ul>
<h2 id="utf-8">UTF-8</h2>
<ul>
<li>유니코드를 실제로 인코딩하는 방식 중 하나</li>
<li>가변 길이 인코딩 방식으로, 문자에 따라 1~4바이트를 사용함.</li>
<li>ASCII 코드와 호환되며, 전 세계적으로 가장 널리 사용되는 인코딩 방식임.</li>
</ul>
<h2 id="사용-예시">사용 예시</h2>
<ul>
<li><p>웹 주소에 한글이나 특수문자를 포함할 때 UTF-8로 인코딩하여 전송함.</p>
<p><code>before</code>
<a href="https://www.youtube.com/watch?v=1jo6q4dihoU&amp;ab_channel=%EC%96%84%ED%8C%8D%ED%95%9C%EC%BD%94%EB%94%A9%EC%82%AC%EC%A0%84">https://www.youtube.com/watch?v=1jo6q4dihoU&amp;ab_channel=얄팍한코딩사전</a>
<code>after</code>
<a href="https://www.youtube.com/watch?v=1jo6q4dihoU&amp;ab_channel=%EC%96%84%ED%8C%8D%ED%95%9C%EC%BD%94%EB%94%A9%EC%82%AC%EC%A0%84">https://www.youtube.com/watch?v=1jo6q4dihoU&amp;ab_channel=%EC%96%84%ED%8C%8D%ED%95%9C%EC%BD%94%EB%94%A9%EC%82%AC%EC%A0%84</a></p>
</li>
</ul>