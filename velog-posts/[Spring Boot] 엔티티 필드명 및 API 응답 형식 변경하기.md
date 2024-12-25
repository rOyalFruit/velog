<h2 id="요구사항">요구사항</h2>
<ul>
<li>Post 생성일과 수정일을 나타내는 createdAt과 modifiedAt을 createDatetime과 modifyDateTime으로 바꿔야 함.</li>
</ul>
<h2 id="기본-세팅">기본 세팅</h2>
<p>BaseEntity</p>
<pre><code class="language-java">@Getter
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
@MappedSuperclass
public class BaseEntity {
    @Id
    @GeneratedValue(strategy = IDENTITY) // AUTO_INCREMENT
    @Setter(AccessLevel.PRIVATE)
    @EqualsAndHashCode.Include
    private Long id;
}</code></pre>
<p>BaseTime</p>
<pre><code class="language-java">@Getter
@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
public class BaseTime extends BaseEntity {
    @CreatedDate
    @Setter(AccessLevel.PRIVATE)
    private LocalDateTime createdAt;

    @LastModifiedDate
    @Setter(AccessLevel.PRIVATE)
    private LocalDateTime modifiedAt;
}</code></pre>
<p>Post Entity</p>
<pre><code class="language-java">@Entity
@Getter
@Setter
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Post extends BaseTime {
    @Column(length = 100)
    private String title;
    @Column(columnDefinition = &quot;TEXT&quot;)
    private String content;
}</code></pre>
<p>PostController - 현재 Post 목록을 보여줌</p>
<pre><code class="language-java">@RestController
@RequestMapping(&quot;/api/v1/posts&quot;)
@RequiredArgsConstructor
public class ApiV1PostController {

    private final PostService postService;

    @GetMapping
    public List&lt;Post&gt; getItems() {
        return postService.findAllByOrderByIdDesc();
    }
}</code></pre>
<p>PostService</p>
<pre><code class="language-java">@Service
@RequiredArgsConstructor
public class PostService {

    private final PostRepository postRepository;

    public List&lt;Post&gt; findAllByOrderByIdDesc() {
        return postRepository.findAllByOrderByIdDesc();
    }
}</code></pre>
<p>PostRepository</p>
<pre><code class="language-java">public interface PostRepository extends JpaRepository&lt;Post, Long&gt; {

    List&lt;Post&gt; findAllByOrderByIdDesc();

}</code></pre>
<p>초기 데이터를 넣고 DB 확인
<img alt="" src="https://velog.velcdn.com/images/ekdeon/post/5da92795-20ed-4740-8bb2-9343c4700b2a/image.png" /></p>
<p>GET - api/v1/posts
<img alt="" src="https://velog.velcdn.com/images/ekdeon/post/d516de0d-1e69-468b-b3cc-2b8d94e138a7/image.png" /></p>
<hr />
<h2 id="해결-방법">해결 방법</h2>
<h3 id="1-엔티티-필드명을-직접-수정">1. 엔티티 필드명을 직접 수정</h3>
<p>BaseTime.java</p>
<pre><code class="language-java">@Getter
@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
public class BaseTime extends BaseEntity {
    @CreatedDate
    @Setter(AccessLevel.PRIVATE)
    private LocalDateTime createDateTime;  // 이부분

    @LastModifiedDate
    @Setter(AccessLevel.PRIVATE)
    private LocalDateTime modifyDateTime; // 이부분
}</code></pre>
<ul>
<li>BaseTime의 필드명을 직접 수정
<img alt="" src="https://velog.velcdn.com/images/ekdeon/post/111890b1-affd-458b-87f0-07baa802b05c/image.png" /></li>
<li>기존 DB를 삭제 후 다시 생성해야 원하는 결과가 나옴.
<img alt="" src="https://velog.velcdn.com/images/ekdeon/post/90b06fff-9415-489a-8775-b98de552bc56/image.png" /></li>
<li>데이터 마이그레이션 없이 DB 구조만 변경하면 위 사진처럼 기존 데이터가 손실되거나 불일치 문제가 발생할 수 있음.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/358397e7-7b29-445c-93a3-a1f4b92b98b0/image.png" /></p>
<ul>
<li>GET - api/v1/posts 결과</li>
<li>필드명이 createAt과 modifiedAt에서 createDateTime과 modifyDateTime으로 변경되었지만, 기존에 있던 데이터가 정상적으로 출력되지 않음.</li>
</ul>
<h4 id="특징">특징</h4>
<ul>
<li>프론트엔드와 협업 시 API 응답 형식이 변경되므로, 예상치 못한 문제를 초래할 수 있음.</li>
<li>기존 데이터를 유지하려면 데이터 마이그레이션 작업이 필요</li>
</ul>
<hr />
<h3 id="2-dto를-만들고-dto를-리턴시키기">2. DTO를 만들고 DTO를 리턴시키기</h3>
<p>PostDto.java.java</p>
<pre><code class="language-java">@Getter
public class PostDto {
    private long id;
    private LocalDateTime createDatetime;
    private LocalDateTime modifiedDatetime ;
    private String title;
    private String content;

    public PostDto(Post post) {
        this.id = post.getId();
        this.createDatetime = post.getCreatedAt();     // 여기
        this.modifiedDatetime  = post.getModifiedAt(); // 여기
        this.title = post.getTitle();
        this.content = post.getContent();
    }
}</code></pre>
<p>PostController.java</p>
<pre><code class="language-java">@GetMapping
public List&lt;PostDto&gt; getItems() {
return postService.findAllByOrderByIdDesc()
        .stream()
        .map(PostDto::new)
        .toList();
}</code></pre>
<p>결과
<img alt="" src="https://velog.velcdn.com/images/ekdeon/post/e5c91be2-ce52-4389-ad66-e07ea7358781/image.png" /></p>
<ul>
<li>DB를 확인해보면 필드명이 그대로인 것을 볼 수 있음.</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/fa90b2fa-7c1b-4efe-a8b6-ce3f7f97f181/image.png" /></p>
<ul>
<li>GET - api/v1/posts 결과 Post 생성과 수정일을 나타내는 필드명이 createdAt, modifiedAt에서 createDatetime과 modifiedDatetime으로 변경됨.</li>
</ul>
<h4 id="특징-1">특징:</h4>
<ul>
<li>DB 구조를 변경하지 않고도 원하는 필드명을 제공할 수 있음</li>
<li>API 응답 형식의 일관성을 유지하고 엔티티 노출을 방지할 수 있음.</li>
<li>DTO는 엔티티와 독립적으로 동작하므로, API에 필요한 데이터만 포함하거나 가공할 수 있음</li>
<li>엔티티와 DTO 간의 매핑 작업이 필요</li>
</ul>
<hr />
<h3 id="3-jsonignore와-getter-이용">3. @JsonIgnore와 getter 이용</h3>
<p>PostDto.java</p>
<pre><code class="language-java">@Getter
public class PostDto {
    private long id;
    @JsonIgnore
    private LocalDateTime createAt;
    @JsonIgnore
    private LocalDateTime modifyAt;
    private String title;
    private String content;

    public PostDto(Post post) {
        this.id = post.getId();
        this.createAt = post.getCreatedAt();
        this.modifyAt  = post.getModifiedAt();
        this.title = post.getTitle();
        this.content = post.getContent();
    }

    public LocalDateTime getCreatedDatetime() {
        return createAt;
    }

    public LocalDateTime getModifiedDatetime() {
        return modifyAt;
    }
}</code></pre>
<ul>
<li>Jackson이 Json화 할 때 <code>get</code>으로 시작하고 인자가 없는 메서드는 실행해서 결과로 넣어줌</li>
<li>@JsonIgnore는 JSON 직렬화(Serialization) 또는 역직렬화(Deserialization) 과정에서 특정 필드를 무시하도록 설정하는 데 사용됨.</li>
</ul>
<h4 id="결과">결과</h4>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/31c333f6-06f8-4790-86e2-e9bae0e4403f/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/7287de10-b045-4f62-8c0d-80df3bba01a9/image.png" /></p>
<h4 id="특징-2">특징</h4>
<ul>
<li>기존 데이터를 유지하면서 API 응답 형식을 수정 가능.</li>
<li>@JsonIgnore와 커스텀 Getter를 함께 사용하면 직관성이 떨어질 수 있음</li>
<li>JSON 직렬화 로직이 엔티티 내부에 포함되므로, 엔티티와 API 응답 간의 책임 분리가 어려워질 수 있음.</li>
</ul>
<hr />
<h3 id="4-jsonproperty-사용">4. @JsonProperty 사용</h3>
<p>PostDto.java</p>
<pre><code class="language-java">@Getter
public class PostDto {
    private long id;
    @JsonProperty(&quot;createDatetime&quot;)  // 여기
    private LocalDateTime createAt;
    @JsonProperty(&quot;modifiedDatetime&quot;)  // 여기
    private LocalDateTime modifyAt;
    private String title;
    private String content;

    public PostDto(Post post) {
        this.id = post.getId();
        this.createAt = post.getCreatedAt();
        this.modifyAt  = post.getModifiedAt();
        this.title = post.getTitle();
        this.content = post.getContent();
    }
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/a3884d50-505e-4d3d-aed9-084ddd47bc06/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/f5f72d43-7f49-4e97-9656-a69d64e70273/image.png" /></p>
<h4 id="특징-3">특징</h4>
<ul>
<li>엔티티나 DTO에서 JSON 직렬화 시 필드명을 쉽게 변경 가능</li>
<li>@JsonProperty는 직렬화/역직렬화에만 영향을 미치며, DB나 내부 코드에는 영향을 주지 않음</li>
</ul>
<hr />
<h3 id="-db-컬럼명만-변경하고-응답-형식은-유지해야-하는-경우">+@ DB 컬럼명만 변경하고 응답 형식은 유지해야 하는 경우</h3>
<p>BaseTime.java</p>
<pre><code class="language-java">@Getter
@EntityListeners(AuditingEntityListener.class)
@MappedSuperclass
public class BaseTime extends BaseEntity {
    @CreatedDate
    @Setter(AccessLevel.PRIVATE)
    @Column(name = &quot;createdDatetime&quot;)  // 여기
    private LocalDateTime createdAt;

    @LastModifiedDate
    @Setter(AccessLevel.PRIVATE)
    @Column(name = &quot;modifiedDatetime&quot;)  // 여기
    private LocalDateTime modifiedAt;
}</code></pre>
<ul>
<li>엔티티 필드에 @Column을 사용하면 됨.<h4 id="결과-1">결과</h4>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/9e0d39f8-3309-4284-80d8-c0f882525bb5/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/efb0b867-c442-434f-8400-c6a2e91aae47/image.png" /></p>
<hr />
<h2 id="요약">요약</h2>
<ol>
<li>DB 구조를 변경해도 괜찮은 경우:<ul>
<li>엔티티 필드명을 직접 수정하거나 DB 컬럼 이름을 변경하는 방식이 적합</li>
</ul>
</li>
<li>DB 구조를 유지하면서 API 응답만 수정하고 싶은 경우:<ul>
<li>DTO를 사용하는 방식을 권장. </li>
</ul>
</li>
<li>간단히 JSON 응답 필드명만 바꾸고 싶을 경우:<ul>
<li>@JsonProperty를 사용하는 방식이 적합. </li>
</ul>
</li>
<li>특정 필드를 숨기거나 커스텀 Getter가 필요한 경우:<ul>
<li>@JsonIgnore와 Getter를 조합하는 방법을 사용.</li>
</ul>
</li>
</ol>