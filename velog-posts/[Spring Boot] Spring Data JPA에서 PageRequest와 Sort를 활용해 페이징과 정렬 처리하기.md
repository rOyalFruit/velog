<blockquote>
</blockquote>
<p>해당 포스팅은 <a href="https://wikidocs.net/162028#_5">점프 투 스프링부트 - 최신순으로 데이터 조회하기</a> 내용을 다루고 있습니다.</p>
<h2 id="페이징과-정렬-기본-코드">페이징과 정렬 기본 코드</h2>
<pre><code class="language-java">public Page&lt;Question&gt; getList(int page) {

    List&lt;Sort.Order&gt; sorts = new ArrayList&lt;&gt;();
    sorts.add(Sort.Order.desc(&quot;createDate&quot;));
    Pageable pageable = PageRequest.of(page, 10, Sort.by(sorts));

    return this.questionRepository.findAll(pageable);
}</code></pre>
<ul>
<li>아래와 같이 Sort.by에 정렬 조건을 바로 지정할 수도 있음.<pre><code class="language-java">public Page&lt;Question&gt; getList(int page){
  Pageable pageable = PageRequest.of(page, 10, Sort.by(Sort.Order.desc(&quot;createDate&quot;)));
  return questionRepository.findAll(pageable);
}</code></pre>
<h3 id="동작-과정">동작 과정</h3>
</li>
</ul>
<ol>
<li><p>정렬 조건 설정</p>
<ul>
<li>데이터를 어떤 기준으로 정렬할지 결정.</li>
<li>여기서는 <code>createDate</code> 필드를 기준으로 내림차순 정렬하도록 설정함.</li>
<li>필요하다면 다중 필드 정렬도 가능.</li>
</ul>
</li>
<li><p>페이징 정보 생성 </p>
<ul>
<li>특정 페이지 번호와 페이지 크기(한 페이지당 데이터 개수)를 지정.</li>
<li>앞서 설정한 정렬 조건을 포함해 페이징 정보를 생성.</li>
</ul>
</li>
<li><p>데이터 조회 </p>
<ul>
<li>생성된 <code>Pageable</code> 객체는 Spring Data JPA의 리포지토리 메서드(<code>findAll(Pageable pageable)</code>)에 전달되고 DB에서 페이징 및 정렬된 데이터를 가져옴.</li>
</ul>
</li>
</ol>
<hr />
<h2 id="정렬-조건-정의---sortorder">정렬 조건 정의 - Sort.Order</h2>
<ul>
<li>Spring Data JPA에서 제공하는 클래스.</li>
<li>데이터 정렬의 방향(오름차순/내림차순)과 정렬 필드를 정의하기 위한 객체를 생성하는 데 사용됨.</li>
<li>아래 코드는 DB 쿼리 결과를 <code>createDate</code> 필드를 기준으로 내림차순(최신순)으로 정렬하도록 설정함<pre><code class="language-java">Sort.Order.desc(&quot;createDate&quot;)</code></pre>
</li>
</ul>
<ul>
<li>여러 필드에 대해 정렬하려면, 추가적인 <code>Sort.Order</code> 객체를 리스트에 추가하면 됨.<pre><code class="language-java">List&lt;Sort.Order&gt; sorts = new ArrayList&lt;&gt;();
sorts.add(Sort.Order.desc(&quot;createDate&quot;)); // 최신순
sorts.add(Sort.Order.asc(&quot;subject&quot;));     // 제목 오름차순</code></pre>
</li>
</ul>
<p><strong>주의점</strong><br />   <code>Sort.Order.desc(&quot;필드명&quot;)</code>에서 사용되는 필드명은 반드시 엔티티 클래스에 정의된 속성 이름이어야 함. 잘못된 이름을 사용하면 예외 발생.</p>
<hr />
<h2 id="페이징-정보-생성---pagerequestof">페이징 정보 생성 - PageRequest.of()</h2>
<ul>
<li>Spring Data JPA에서 페이징 처리를 위해 사용하는 메서드.</li>
<li>다음과 같은 정보를 포함하는 객체를 생성함.<ul>
<li><strong>페이지 번호 (<code>page</code>)</strong>: 조회할 페이지 번호 (0부터 시작).</li>
<li><strong>페이지 크기 (<code>size</code>)</strong>: 한 페이지에 포함될 데이터 수.</li>
<li><strong>정렬 조건 (<code>sort</code>)</strong>: 데이터를 어떻게 정렬할지 지정.</li>
</ul>
</li>
</ul>
<p>예제:</p>
<pre><code class="language-java">Pageable pageable = PageRequest.of(0, 10, Sort.by(sorts));</code></pre>
<ul>
<li>첫 번째 인자: <code>0</code> → 첫 번째 페이지 (0-based index).</li>
<li>두 번째 인자: <code>10</code> → 한 페이지당 최대 10개의 데이터.</li>
<li>세 번째 인자: <code>Sort.by(sorts)</code> → 앞서 정의한 정렬 조건(<code>createDate DESC</code>) 적용.</li>
</ul>
<hr />
<h2 id="요약">요약</h2>
<ol>
<li><strong>정렬 조건은 <code>Sort.Order</code>로 정의하고 리스트에 담는다.</strong></li>
<li><strong>페이징 정보는 <code>PageRequest.of()</code>를 사용해 생성한다.</strong></li>
<li><strong>리포지토리 메서드에 전달하여 페이징 및 정렬된 데이터를 조회한다.</strong></li>
</ol>