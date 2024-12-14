<blockquote>
</blockquote>
<p>특별한 상황이 아니라면 <code>지연 로딩</code>을 사용하자.</p>
<h2 id="즉시-로딩">즉시 로딩</h2>
<ul>
<li>@ManyToOne(fetch = FetchType.EAGER)</li>
<li>@ManyToOne만 작성 시 기본으로 FetchType은 EAGER로 설정됨.</li>
<li>엔티티 조회 시 즉시 연관된 엔티티도 함께 로딩</li>
<li>추가 쿼리 없이 연관 엔티티 데이터 즉시 접근 가능</li>
<li>트랜잭션 외부에서도 연관 엔티티 접근 가능</li>
</ul>
<hr />
<h2 id="지연-로딩">지연 로딩</h2>
<ul>
<li>@ManyToOne(fetch = FetchType.LAZY)</li>
<li>엔티티 조회 시 연관된 엔티티는 실제 접근 시점에 로딩</li>
<li>트랜잭션 범위 내에서만 안전하게 연관 엔티티 로딩</li>
<li>추가 쿼리 발생, 성능 최적화에 유리</li>
</ul>
<hr />
<h2 id="eager-vs-lazy">EAGER vs. LAZY</h2>
<table>
<thead>
<tr>
<th>구분</th>
<th>FetchType.EAGER</th>
<th>FetchType.LAZY</th>
</tr>
</thead>
<tbody><tr>
<td>로딩 시점</td>
<td>즉시 로딩</td>
<td>실제 접근 시점</td>
</tr>
<tr>
<td>트랜잭션 필요성</td>
<td>낮음</td>
<td>높음</td>
</tr>
<tr>
<td>성능</td>
<td>초기 로딩 비용 높음</td>
<td>필요한 시점에 로딩</td>
</tr>
<tr>
<td>N+1 문제</td>
<td>발생 가능성 높음</td>
<td>발생 가능성 낮음</td>
</tr>
</tbody></table>
<hr />
<h2 id="주의사항">주의사항</h2>
<ul>
<li>Eager 전략은 불필요한 조인 발생 가능</li>
<li>Lazy 전략은 반드시 트랜잭션 범위 내에서 처리</li>
<li>성능 최적화를 위해 fetch join 또는 @EntityGraph 고려</li>
</ul>
<hr />
<h2 id="전략별-실행-결과-살펴보기">전략별 실행 결과 살펴보기</h2>
<blockquote>
</blockquote>
<p>전체 코드를 보려면 <a href="https://github.com/rOyalFruit/jpa-loading-strategies/tree/main/src/main/java/com/example/jpaloadingstrategies">여기</a>를 클릭</p>
<h3 id="1-즉시-로딩-방식">1. 즉시 로딩 방식</h3>
<pre><code class="language-java">@Bean
@Order(2)
public ApplicationRunner eagerLoadingApplicationRunner() {
    return args -&gt; {
        PostComment postComment3 = postCommentService.findById(3).get();

        Post postOfComment3 = postComment3.getPost();

        System.out.println(&quot;즉시 로딩: &quot; + postOfComment3.toString());
    };
}</code></pre>
<pre><code class="language-sql">select
    pc1_0.id,
    pc1_0.content,
    p1_0.id,
    p1_0.blind,
    p1_0.content,
    p1_0.created_at,
    p1_0.modified_at,
    p1_0.title
from
    post_comment pc1_0
left join
    post p1_0
on p1_0.id=pc1_0.post_id
where
    pc1_0.id=?</code></pre>
<pre><code>[           main] org.hibernate.orm.jdbc.extract           : extracted value (3:BIGINT) -&gt; [2]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (4:BOOLEAN) -&gt; [false]
[           main] org.hibernate.orm.jdbc.bind              : binding parameter (1:BIGINT) &lt;- [3]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (5:VARCHAR) -&gt; [content2]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (6:TIMESTAMP) -&gt; [2024-12-13T22:16:48.094223]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (7:TIMESTAMP) -&gt; [2024-12-13T22:16:48.094223]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (8:VARCHAR) -&gt; [title2]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (2:VARCHAR) -&gt; [comment3]
[           main] o.s.t.i.TransactionInterceptor           : Completing transaction for [org.springframework.data.jpa.repository.support.SimpleJpaRepository.findById]
즉시 로딩: Post(id=2, createdAt=2024-12-13T22:16:48.094223, modifiedAt=2024-12-13T22:16:48.094223, title=title2, content=content2, blind=false)
[ionShutdownHook] j.LocalContainerEntityManagerFactoryBean : Closing JPA EntityManagerFactory for persistence unit 'default'
[ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
[ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.</code></pre><ul>
<li>특정 엔티티를 조회할 때 연관된 모든 엔티티를 같이 로딩하는 것을 볼 수 있음.</li>
</ul>
<h3 id="2-지연-로딩-방식트랜잭션-o">2. 지연 로딩 방식(트랜잭션 O)</h3>
<pre><code class="language-java">@Bean
@Order(3)
public ApplicationRunner LazyLoadingWithTransactionRunner() {
    return new ApplicationRunner() {
        @Transactional
        @Override
        public void run(ApplicationArguments args) throws Exception {
            PostComment postComment3 = postCommentService.findById(3).get();

            Post postOfComment3 = postComment3.getPost();

            System.out.println(&quot;postOfComment3.getId() = &quot; + postOfComment3.getId());
            System.out.println(&quot;postOfComment3.getTitle() = &quot; + postOfComment3.getTitle());
        }
    };
}</code></pre>
<ul>
<li>@Transactional을 사용함으로써 메서드 수행이 완료될 때까지 DB 커넥션을 유지함.</li>
</ul>
<pre><code class="language-sql">select
    pc1_0.id,
    pc1_0.content,
    pc1_0.post_id
from
    post_comment pc1_0
where
    pc1_0.id=?</code></pre>
<pre><code>[           main] org.hibernate.orm.jdbc.bind              : binding parameter (1:BIGINT) &lt;- [3]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (3:BIGINT) -&gt; [2]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (2:VARCHAR) -&gt; [comment3]
[           main] o.s.t.i.TransactionInterceptor           : Completing transaction for [org.springframework.data.jpa.repository.support.SimpleJpaRepository.findById]
postOfComment3.getId() = 2
[           main] org.hibernate.SQL  </code></pre><ul>
<li>postOfComment3.getId()의 값은 처음 SQL을 수행할 때 이미 가져온 상태.</li>
</ul>
<pre><code class="language-sql">select
    p1_0.id,
    p1_0.blind,
    p1_0.content,
    p1_0.created_at,
    p1_0.modified_at,
    p1_0.title
from
    post p1_0
where
    p1_0.id=?</code></pre>
<pre><code>[           main] org.hibernate.orm.jdbc.bind              : binding parameter (1:BIGINT) &lt;- [2]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (2:BOOLEAN) -&gt; [false]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (3:VARCHAR) -&gt; [content2]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (4:TIMESTAMP) -&gt; [2024-12-13T22:42:59.479141]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (5:TIMESTAMP) -&gt; [2024-12-13T22:42:59.479141]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (6:VARCHAR) -&gt; [title2]
postOfComment3.getTitle() = title2
[           main] o.s.t.i.TransactionInterceptor           : Completing transaction for [com.example.jpaloadingstrategies.global.BaseInitData$1.run]
[ionShutdownHook] j.LocalContainerEntityManagerFactoryBean : Closing JPA EntityManagerFactory for persistence unit 'default'
[ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
[ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.
</code></pre><ul>
<li>postOfComment3.getTitle()의 값은 없으므로 두 번째 SQL 수행</li>
</ul>
<h3 id="3-지연-로딩-방식트랜잭션-x">3. 지연 로딩 방식(트랜잭션 X)</h3>
<pre><code class="language-java">@Bean
@Order(4)
public ApplicationRunner LazyLoadingWithoutTransactionRunner() {
    return args -&gt; {
        PostComment postComment3 = postCommentService.findById(3).get(); // 값 가져오고 DB 커넥션 닫힘

        Post postOfComment3 = postComment3.getPost(); // 값 가져오고 DB 커넥션 닫힘
        System.out.println(&quot;postOfComment3.getId() = &quot; + postOfComment3.getId()); // postOfComment3에 id값은 들어 있으므로 정상적으로 동작.
        try{
            System.out.println(&quot;postOfComment3.getTitle() = &quot; + postOfComment3.getTitle()); // 값이 없어서 가져와야 하지만 DB 커넥션이 닫힌 상태이므로 값을 가져오지 못하고 에러남.
        }catch (LazyInitializationException e){
            System.out.println(&quot;LazyInitializationException 발생&quot;);
        }
    };
}</code></pre>
<pre><code class="language-sql">select
    pc1_0.id,
    pc1_0.content,
    pc1_0.post_id
from
    post_comment pc1_0
where
    pc1_0.id=?</code></pre>
<pre><code>[           main] org.hibernate.orm.jdbc.bind              : binding parameter (1:BIGINT) &lt;- [3]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (3:BIGINT) -&gt; [2]
[           main] org.hibernate.orm.jdbc.extract           : extracted value (2:VARCHAR) -&gt; [comment3]
[           main] o.s.t.i.TransactionInterceptor           : Completing transaction for [org.springframework.data.jpa.repository.support.SimpleJpaRepository.findById]
postOfComment3.getId() = 2
LazyInitializationException 발생
[ionShutdownHook] j.LocalContainerEntityManagerFactoryBean : Closing JPA EntityManagerFactory for persistence unit 'default'
[ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
[ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.
</code></pre><ul>
<li>Post의 번호는 가져왔지만 제목은 가져오지 않은 상태.</li>
<li>getTitle()을 수행하려고 할 때는 DB 커넥션이 닫혀있는 상태이므로 <code>LazyInitializationException</code> 발생.</li>
</ul>