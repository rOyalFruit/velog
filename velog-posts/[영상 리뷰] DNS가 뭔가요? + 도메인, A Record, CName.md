<p><a href="https://www.youtube.com/watch?v=6fc9NAQkcv0&amp;ab_channel=%EC%96%84%ED%8C%8D%ED%95%9C%EC%BD%94%EB%94%A9%EC%82%AC%EC%A0%84"><img alt="movie" src="https://img.youtube.com/vi/6fc9NAQkcv0/sddefault.jpg" /></a></p>
<h3 id="dnsdomain-name-system">DNS(Domain Name System)</h3>
<ul>
<li>도메인(<code>www.naver.com</code>같은 웹사이트 주소) 이름을 IP 주소로 변환하는 시스템</li>
</ul>
<h3 id="dns-작동-방식">DNS 작동 방식</h3>
<ul>
<li>Local DNS 서버, Root DNS 서버, TLD DNS 서버 등 여러 단계를 거쳐 IP 주소를 찾음(영상 1:39 ~ 3:00)</li>
<li>DNS 캐싱을 통해 자주 사용하는 도메인의 IP 주소를 빠르게 찾을 수 있음</li>
</ul>
<h3 id="dns-관련-설정-및-보안">DNS 관련 설정 및 보안</h3>
<ul>
<li>로컬 DNS 서버는 통신사마다 지정된 곳이 있음 </li>
<li>사용자는 로컬 DNS 서버를 변경할 수 있음. </li>
<li>로컬 DNS 서버를 변경을 통해 특정 서비스의 속도를 개선하거나 차단된 사이트에 접속할 수 있음.</li>
<li>DNS 스푸핑과 같은 보안 위협이 존재하므로 주의가 필요</li>
</ul>
<h3 id="도메인-관리">도메인 관리</h3>
<ul>
<li>도메인은 여러 사이트에서 구매할 수 있음(후이즈, 가비아, GoDaddy 등)</li>
<li>구매 후 DNS 설정을 통해 원하는 서버와 연결할 수 있음.</li>
<li>도메인을 서버나 다른 도메인과 연결하는 방식으로 <code>A 레코드</code>와 <code>CNAME</code>이 있음.<ul>
<li>A 레코드: 도메인을 서버의 IP로 직접 연결(접속이 빠르다는 장점)</li>
<li>CNAME: 도메인의 별명을 만들어 다른 도메인으로 연결(서버의 IP가 수시로 바뀌기 때문. AWS나  Firebase 등 쓸 때 사용)</li>
</ul>
</li>
</ul>