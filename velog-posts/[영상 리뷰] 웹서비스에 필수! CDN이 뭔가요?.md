<p><a href="https://www.youtube.com/watch?v=_kcoeK0ITkQ&amp;ab_channel=%EC%96%84%ED%8C%8D%ED%95%9C%EC%BD%94%EB%94%A9%EC%82%AC%EC%A0%84"><img alt="movie" src="https://img.youtube.com/vi/_kcoeK0ITkQ/sddefault.jpg" /></a></p>
<h2 id="cdncontent-delivery-network">CDN(Content Delivery Network)</h2>
<ul>
<li>웹사이트의 콘텐츠를 사용자에게 효율적으로 전달하는 시스템</li>
</ul>
<h2 id="주요-목적">주요 목적</h2>
<ol>
<li>물리적 거리 문제 해결</li>
<li>서버 부하 분산</li>
<li>전 세계 사용자에게 빠른 서비스 제공</li>
</ol>
<h2 id="작동-방식">작동 방식</h2>
<ul>
<li>전 세계에 분산된 서버(Edge)를 통해 콘텐츠 제공</li>
<li>DNS를 통해 사용자 요청을 가장 가까운 CDN 서버로 연결</li>
<li>정적 캐싱과 동적 캐싱을 통해 콘텐츠 전달 최적화</li>
</ul>
<h2 id="cdn의-장점">CDN의 장점</h2>
<ol>
<li>서버 부하 감소</li>
<li>대역폭 비용 절감</li>
<li>웹사이트 로딩 속도 향상</li>
<li>가용성과 안정성 증가</li>
<li>DDoS 공격 등에 대한 보안 강화</li>
</ol>
<hr />
<h2 id="정적-캐싱-동적-캐싱">정적 캐싱, 동적 캐싱</h2>
<h4 id="정적-캐싱">정적 캐싱</h4>
<ul>
<li>캐싱할 것들을 미리 CDN 서버에 저장.</li>
</ul>
<h4 id="동적-캐싱">동적 캐싱</h4>
<ul>
<li>사용자 요청이 있을 때마다 CDN 서버에 캐시가 있는지 확인</li>
<li>캐시가 있으면(cache hit) 바로 제공하고, 없으면(cache miss) 원본 서버에 요청하여 받아옴.</li>
</ul>
<p>-&gt; 체인점들이 식재료를 미리 받와와서 있는 게 정적 캐싱, 주문 들어왔는데 냉장고에 없으면 본사에 요청하는 게 동적 캐싱</p>
<hr />
<h2 id="정적컨텐츠-동적컨텐츠">정적컨텐츠, 동적컨텐츠</h2>
<h4 id="정적컨텐츠">정적컨텐츠</h4>
<ul>
<li>HTML,CSS,JavaScript파일이나 문서, 이미지 , 동영상 처럼 그 내용이 고정된 것.</li>
</ul>
<h4 id="동적컨텐츠">동적컨텐츠</h4>
<ul>
<li>사용자가 입력한 내용이나, 어떤 게시판의 최신글 불러오기 같은 API 요청, 그때그때 데이터베이스 등의 변수에 따라서 내용이 변할 수 있는 것.</li>
</ul>
<hr />
<h3 id="ps">P.S.</h3>
<ul>
<li>전송속도: 물리적 거리를 얼마나 빠르게 이동을 하는지</li>
<li>대역폭: 동시에 얼마나 많은 데이터가 오갈 수 있는지</li>
</ul>