<p><a href="https://www.youtube.com/watch?v=fB3MB8TXNXM&amp;t=0s&amp;ab_channel=%EC%96%84%ED%8C%8D%ED%95%9C%EC%BD%94%EB%94%A9%EC%82%AC%EC%A0%84"><img alt="movie" src="https://img.youtube.com/vi/fB3MB8TXNXM/sddefault.jpg" /></a></p>
<h3 id="rest-apirestful-api">REST API(=RESTful API)</h3>
<ul>
<li>API의 다양한 형식 중 하나로, 가장 널리 사용되는 방식</li>
<li>HTTP 프로토콜을 기반으로 하며, 리소스는 URL을 통해 고유하게 식별됨</li>
<li>구조화된 데이터 표현을 위해 JSON 형식을 주로 사용</li>
<li>페이징, 필터링 등을 위해 쿼리 파라미터를 사용</li>
<li>URI는 요청 대상 자원을 명확히 표현하며, 작업 내용은 포함하지 않음</li>
<li>작업은 HTTP 메서드로 구분(ex. POST는 생성, GET은 조회 등)</li>
<li>응답에는 다음 가능한 작업에 대한 링크 정보(HATEOAS)를 포함</li>
<li>상태가 없는(Stateless) 통신을 함.</li>
<li>서버나 클라이언트가 응답 데이터를 캐싱하여 동일 요청에 대한 불필요한 작업 방지.</li>
</ul>
<hr />
<h3 id="crud-작업">CRUD 작업</h3>
<ul>
<li>Create: POST 메소드로 새로운 리소스 생성</li>
<li>Read: GET 메소드로 리소스 조회</li>
<li>Update: PUT(전체 수정) 또는 PATCH(부분 수정) 메소드로 리소스 수정</li>
<li>Delete: DELETE 메소드로 리소스 삭제</li>
</ul>
<hr />
<h3 id="응답-상태-코드">응답 상태 코드</h3>
<p>200번대: 성공적인 요청
400번대: 클라이언트 측 오류
500번대: 서버 측 오류</p>