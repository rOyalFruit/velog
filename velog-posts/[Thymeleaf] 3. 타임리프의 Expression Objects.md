<blockquote>
</blockquote>
<p>타임리프의 기본 객체와 유틸리티 객체는 템플릿에서 데이터 처리와 표현을 간소화하며, 다양한 기능을 제공한다. 
특히 날짜 포맷팅, 문자열 조작, 컬렉션 처리 등은 자주 사용되므로 활용법을 숙지하자.</p>
<h2 id="expression-basic-objects"><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#appendix-a-expression-basic-objects">Expression Basic Objects</a></h2>
<ul>
<li>#ctx: 컨텍스트 객체.</li>
<li>#vars: 컨텍스트 변수.</li>
<li>#locale: 컨텍스트 로케일.<ul>
<li>locale: 사용자 인터페이스에서 사용되는 언어, 지역 설정, 출력 형식 등을 정의하는 문자열</li>
</ul>
</li>
</ul>
<p>아래처럼 사용한다</p>
<pre><code>Established locale country: &lt;span th:text=&quot;${#locale.country}&quot;&gt;US&lt;/span&gt;.</code></pre><h2 id="expression-utility-objects"><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#appendix-b-expression-utility-objects">Expression Utility Objects</a></h2>
<ul>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#execution-info">#execInfo</a>: 처리 중인 템플릿에 대한 정보.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#messages-1">#messages</a>: 변수 표현식 내에서 #{…} 구문을 사용하는 것과 동일한 방식으로 외부화된 메시지를 가져오는 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#urisurls">#uris</a>: URL/URI의 일부를 이스케이프 처리하는 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#conversions">#conversions</a>: 구성된 변환 서비스를 실행하는 메서드(있는 경우).</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#dates">#dates</a>: <code>java.util.Date</code> 객체를 위한 메서드: 포맷팅, 구성 요소 추출 등.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#calendars">#calendars</a>: <code>#dates</code>와 유사하지만, java.util.Calendar 객체를 위한 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#temporals-java.time">#temporals</a>: JDK8+의 <code>java.time</code> API를 사용하여 날짜와 시간을 처리하기 위한 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#numbers">#numbers</a>: 숫자 객체를 포맷팅하기 위한 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#strings">#strings</a>: 문자열 객체를 위한 메서드: <code>contains</code>, <code>startsWith</code>, <code>prepending/appending</code>, etc.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#objects">#objects</a>: 일반 객체를 다루기 위한 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#booleans">#bools</a>: boolean 평가를 위한 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#arrays">#arrays</a>: Array를 다루기 위한 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#lists">#lists</a>: List를 다루기 위한 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#sets">#sets</a>: Set을 다루기 위한 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#maps">#maps</a>: Map을 다루기 위한 메서드</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#aggregates">#aggregates</a>: Array나 Collections에 대한 aggregate를 생성하기 위한 메서드.</li>
<li><a href="https://www.thymeleaf.org/doc/tutorials/3.1/usingthymeleaf.html#ids">#ids</a>: 반복될 수 있는 id 속성을 처리하기 위한 메서드. (ex. 반복의 결과)</li>
</ul>