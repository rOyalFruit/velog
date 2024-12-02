<blockquote>
</blockquote>
<p>단계별로 문제를 풀다가 io, nio 패키지에 대해 새롭게 알게 되어 작성한 포스트!
작성한 코드를 보려면 <a href="https://github.com/rOyalFruit/wise-saying-app">여기</a></p>
<h2 id="🔥미리-보는-요약정리">🔥미리 보는 요약정리</h2>
<p><code>java.io</code> vs <code>java.nio</code></p>
<table>
<thead>
<tr>
<th align="left">특징</th>
<th align="left">java.io</th>
<th align="left">java.nio</th>
</tr>
</thead>
<tbody><tr>
<td align="left">작동 방식</td>
<td align="left">스트림 기반</td>
<td align="left">버퍼 기반</td>
</tr>
<tr>
<td align="left">비동기 지원</td>
<td align="left">지원하지 않음</td>
<td align="left">비동기 작업 가능</td>
</tr>
<tr>
<td align="left">성능</td>
<td align="left">간단한 작업에 적합</td>
<td align="left">대량 데이터 및 고성능 작업에 적합</td>
</tr>
<tr>
<td align="left">사용성</td>
<td align="left">상대적으로 더 직관적</td>
<td align="left">코드가 약간 복잡할 수 있음</td>
</tr>
<tr>
<td align="left">예시 사용</td>
<td align="left">간단한 파일 읽기/쓰기</td>
<td align="left">디렉토리 생성, 파일 처리, 대규모 데이터 작업</td>
</tr>
<tr>
<td align="left">코드 유지보수</td>
<td align="left">레거시 코드에서 널리 사용</td>
<td align="left">최신 Java 애플리케이션에서 선호</td>
</tr>
</tbody></table>
<ul>
<li>Files 클래스는 파일 작업을 간단하고 직관적으로 처리할 수 있게 해준다. </li>
<li>java.io는 BufferedReader와 BufferedWriter처럼 명시적으로 스트림을 열고 닫아야 하는 번거로움이 있다. </li>
</ul>
<hr />
<h2 id="📖문제-요구사항">📖문제 요구사항</h2>
<ul>
<li><p>각 명언은 개별파일로 <code>db/wiseSaying</code> 폴더에 저장.</p>
</li>
<li><p>각 명언 파일명: <code>{명언번호}.json</code></p>
</li>
<li><p>명언을 등록할 때 마다, 수정할 때 마다 해당 파일이 갱신되어야 함.</p>
</li>
<li><p>가장 마지막에 생성된 명언 번호는 <code>db/wiseSaying/lastId.txt</code> 파일에 숫자로 저장.</p>
</li>
<li><p>직렬화, 역직렬화 할 때 Jackson, Gson 등 외부 라이브러리 사용 금지.</p>
<table>
<thead>
<tr>
<th><img alt="" src="https://i.postimg.cc/vZZ3TSkC/2024-11-21-222645.png" /></th>
<th><img alt="" src="https://i.postimg.cc/PJK1kdXJ/2024-11-21-222710.png" /></th>
</tr>
</thead>
</table>
</li>
<li><p>위와 같이 명언 등록/수정/삭제 작업을 하고 프로그램을 종료한 뒤, 재시작 해도 데이터가 남아있어야 함.</p>
<br />

</li>
</ul>
<h4 id="파일-구조">파일 구조</h4>
<ul>
<li><p>db/wiseSaying/1.json</p>
<pre><code class="language-json">{
  &quot;id&quot;: 1,
  &quot;content&quot;: &quot;명언 1&quot;,
  &quot;author&quot;: &quot;작가 1&quot;
}</code></pre>
</li>
<li><p>db/wiseSaying/2.json</p>
<pre><code class="language-json">{
  &quot;id&quot;: 2,
  &quot;content&quot;: &quot;명언 2&quot;,
  &quot;author&quot;: &quot;작가 2&quot;
}</code></pre>
</li>
<li><p>db/wiseSaying/lastId.txt</p>
<p>  2</p>
<ul>
<li>db/wiseSaying/lastId.txt 파일 내용(가장 마지막에 생성된 명언 번호가 저장됨)</li>
</ul>
</li>
</ul>
<hr />
<h2 id="1-javanio-new-io">1. java.nio (New I/O)</h2>
<ul>
<li>비동기적이고 버퍼 기반의 파일 및 데이터 처리를 지원함. </li>
<li>주로 고성능이나 대량의 데이터를 처리하는 작업에 유리함.<br />

</li>
</ul>
<h4 id="사용한-주요-메서드와-클래스">사용한 주요 메서드와 클래스</h4>
<ul>
<li><p>Paths.get():</p>
<ul>
<li>파일 경로나 디렉토리 경로를 표현하는 Path 객체를 생성.</li>
<li>코드에서 파일 경로를 처리할 때 사용함.<pre><code class="language-java">Path path = Paths.get(&quot;db/wiseSaying/lastId.txt&quot;);</code></pre>
</li>
</ul>
</li>
<li><p>Files.exists():</p>
<ul>
<li>특정 경로나 파일이 존재하는지 확인.<pre><code class="language-java">if (Files.exists(path)) {
  // 파일이 존재할 경우
}</code></pre>
</li>
</ul>
</li>
<li><p>Files.readString():</p>
<ul>
<li>파일 내용을 문자열로 읽어옴.<pre><code class="language-java">String content = Files.readString(path);</code></pre>
</li>
</ul>
</li>
<li><p>Files.writeString():</p>
<ul>
<li>문자열 데이터를 파일에 작성.<pre><code class="language-java">Files.writeString(path, &quot;example content&quot;);</code></pre>
</li>
</ul>
</li>
<li><p>Files.createDirectories():</p>
<ul>
<li>지정된 디렉토리 경로가 존재하지 않을 경우 필요한 모든 디렉토리를 생성.<pre><code class="language-java">Files.createDirectories(Paths.get(&quot;db/wiseSaying&quot;));</code></pre>
</li>
</ul>
</li>
<li><p>Files.deleteIfExists():</p>
<ul>
<li>지정된 파일이 존재하면 삭제.<pre><code class="language-java">Files.deleteIfExists(Paths.get(&quot;db/wiseSaying/1.json&quot;));</code></pre>
</li>
</ul>
</li>
</ul>
<hr />
<p><code>java.nio</code> 예제</p>
<pre><code class="language-java">import java.io.IOException;
import java.nio.file.*;

public class NioExample {
    public static void main(String[] args) {
        String folderPath = &quot;exampleDir&quot;;
        String filePath = folderPath + &quot;/example.txt&quot;;

        try {
            // 1. 디렉토리 생성
            Files.createDirectories(Paths.get(folderPath));
            System.out.println(&quot;디렉토리가 생성되었습니다.&quot;);

            // 2. 파일에 데이터 쓰기
            Files.writeString(Paths.get(filePath), &quot;Hello, NIO!&quot;);
            System.out.println(&quot;파일이 생성되고 데이터가 저장되었습니다.&quot;);

            // 3. 파일 읽기
            String content = Files.readString(Paths.get(filePath));
            System.out.println(&quot;파일 내용: &quot; + content);

            // 4. 파일 삭제
            Files.deleteIfExists(Paths.get(filePath));
            System.out.println(&quot;파일이 삭제되었습니다.&quot;);
        } catch (IOException e) {
            System.out.println(&quot;오류 발생: &quot; + e.getMessage());
        }
    }
}</code></pre>
<hr />
<h2 id="2-javaio-classic-io">2. java.io (Classic I/O)</h2>
<ul>
<li>java.io는 Java의 전통적인 입출력 API로, 스트림 기반으로 데이터를 처리함. </li>
<li>상대적으로 단순한 작업에 적합하며, 낮은 레벨의 입출력 제어를 제공함.<br />

</li>
</ul>
<h4 id="사용한-주요-메서드와-클래스-1">사용한 주요 메서드와 클래스</h4>
<ul>
<li><p>BufferedReader:</p>
<ul>
<li>파일에서 데이터를 줄 단위로 읽어옴.<pre><code class="language-java">BufferedReader reader = new BufferedReader(new FileReader(&quot;example.txt&quot;));
String line = reader.readLine();</code></pre>
</li>
</ul>
</li>
<li><p>BufferedWriter:</p>
<ul>
<li>파일에 데이터를 효율적으로 작성.<pre><code class="language-java">BufferedWriter writer = new BufferedWriter(new FileWriter(&quot;example.txt&quot;));
writer.write(&quot;Hello, IO!&quot;);</code></pre>
</li>
</ul>
</li>
<li><p>File:</p>
<ul>
<li>파일 및 디렉토리를 관리하기 위한 클래스.<pre><code class="language-java">File file = new File(&quot;exampleDir/example.txt&quot;);
if (file.exists()) {
  file.delete();
}</code></pre>
</li>
</ul>
</li>
</ul>
<hr />
<p><code>java.io</code> 예제</p>
<pre><code class="language-java">import java.io.*;

public class IoExample {
    public static void main(String[] args) {
        String folderPath = &quot;exampleDir&quot;;
        String filePath = folderPath + &quot;/example.txt&quot;;

        try {
            // 1. 디렉토리 생성
            File folder = new File(folderPath);
            if (!folder.exists()) {
                folder.mkdirs();
                System.out.println(&quot;디렉토리가 생성되었습니다.&quot;);
            }

            // 2. 파일에 데이터 쓰기
            BufferedWriter writer = new BufferedWriter(new FileWriter(filePath));
            writer.write(&quot;Hello, IO!&quot;);
            writer.close();
            System.out.println(&quot;파일이 생성되고 데이터가 저장되었습니다.&quot;);

            // 3. 파일 읽기
            BufferedReader reader = new BufferedReader(new FileReader(filePath));
            String content = reader.readLine();
            reader.close();
            System.out.println(&quot;파일 내용: &quot; + content);

            // 4. 파일 삭제
            File file = new File(filePath);
            if (file.exists()) {
                file.delete();
                System.out.println(&quot;파일이 삭제되었습니다.&quot;);
            }
        } catch (IOException e) {
            System.out.println(&quot;오류 발생: &quot; + e.getMessage());
        }
    }
}</code></pre>
<hr />