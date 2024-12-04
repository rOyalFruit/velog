<p><a href="https://www.youtube.com/watch?v=GK3h936Co-k&amp;ab_channel=%EC%96%84%ED%8C%8D%ED%95%9C%EC%BD%94%EB%94%A9%EC%82%AC%EC%A0%84"><img alt="movie" src="https://img.youtube.com/vi/GK3h936Co-k/sddefault.jpg" /></a></p>
<h3 id="ip-주소">IP 주소</h3>
<ul>
<li>컴퓨터 자체의 식별 번호가 아닌, 컴퓨터가 연결된 네트워크 끝단의 주소</li>
<li>같은 컴퓨터라도 연결된 네트워크에 따라 IP 주소가 달라질 수 있음.</li>
</ul>
<h3 id="ipv4와-ipv6">IPv4와 IPv6</h3>
<ul>
<li>IPv4는 0~255 사이의 숫자 4개로 구성되며, 약 43억 개의 주소를 제공.</li>
<li>IPv4 주소 부족 문제로 IPv6가 도입됨.</li>
<li>IPv6는 16진수 8자리가 이어진 형태로, 약 3.4×10^38개의 주소를 제공함.</li>
</ul>
<h3 id="공인-ip와-사설-ip">공인 IP와 사설 IP</h3>
<ul>
<li><p>공인 IP: 인터넷 상에서 고유한 주소로, 외부에서 접근 가능.</p>
</li>
<li><p>사설 IP: 내부 네트워크에서만 사용되는 주소로, 외부에서 직접 접근할 수 없음.</p>
<pre><code>┏━━━━━━━━━━━━━공인IP━━━━━━━━━━━┓ ┏━━사설IP━━┓
서울시 개발구 코딩로12 얄코아파트 101동 202호</code></pre></li>
</ul>
<h3 id="포트-포워딩과-dmz">포트 포워딩과 DMZ</h3>
<ul>
<li>포트 포워딩: 공인 IP의 특정 포트를 내부 사설 IP로 연결하는 방식</li>
<li>DMZ: 공인 IP의 모든 포트를 특정 내부 IP로 연결하는 방식(보안상 위험이 있으므로 권장되지 않음)</li>
</ul>
<h3 id="고정-ip와-유동-ip">고정 IP와 유동 IP</h3>
<ul>
<li>고정 IP: 변하지 않는 IP 주소. 주로 서버에서 사용됨.</li>
<li>유동 IP: 주기적으로 변경되는 IP 주소. 일반 가정에서 주로 사용됨.</li>
</ul>
<h3 id="ddns-dynamic-dns">DDNS (Dynamic DNS)</h3>
<ul>
<li>유동 IP를 사용하는 환경에서 고정된 도메인 이름을 IP 주소에 연결해주는 서비스</li>
</ul>
<hr />
<p>사설IP &amp; 유동IP 조합인 가정집에서 NAS 쓰려면 포트포워딩 &amp; DDNS 설정을 해야 한다!</p>
<ul>
<li>포트 포워딩을 통해 외부 접근을 허용</li>
<li>DDNS를 통해 변동되는 IP 주소를 관리</li>
</ul>