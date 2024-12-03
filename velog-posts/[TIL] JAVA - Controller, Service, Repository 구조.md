<h2 id="역할">역할</h2>
<h3 id="controller">controller</h3>
<ul>
<li>클라이언트로부터 HTTP 요청을 받아 처리</li>
<li>요청에 따라 적절한 Service 메서드를 호출</li>
<li>처리 결과를 클라이언트에게 응답으로 반환</li>
</ul>
<h3 id="service">service</h3>
<ul>
<li>비즈니스 로직을 구현하는 계층</li>
<li>Controller로부터 받은 요청을 처리</li>
<li>필요한 경우 Repository를 통해 데이터베이스에 접근</li>
<li>트랜잭션 관리를 담당</li>
</ul>
<h3 id="repository">repository</h3>
<ul>
<li>데이터를 저장하거나 조회</li>
<li>결과를 Service에 반환</li>
</ul>
<hr />
<h2 id="장점">장점</h2>
<p><strong>관심사의 분리</strong></p>
<ul>
<li>컨트롤러는 사용자 요청을 처리하고, 서비스는 비즈니스 로직을 수행하며, 리포지토리는 데이터 관리에 집중한다.따라서, 각 계층이 서로 독립적이므로 코드 유지보수가 쉬워진다.</li>
</ul>
<p><strong>유연성</strong></p>
<ul>
<li>각각의 계층을 독립적으로 변경할 수 있다. 예를 들어, 데이터 저장 방식을 데이터베이스로 변경할 때 Repository만 수정하면 된다.</li>
</ul>
<p><strong>테스트 가능성</strong></p>
<ul>
<li>각 계층을 독립적으로 테스트할 수 있다. </li>
<li>Mock 객체를 사용해 다른 계층의 의존성을 대체할 수 있다.</li>
</ul>
<p><strong>재사용성</strong></p>
<ul>
<li>Service 계층의 로직을 여러 Controller에서 재사용할 수 있다.</li>
</ul>
<hr />
<h2 id="예시">예시</h2>
<ul>
<li><p>스프링의 계층적 구조를 그대로 구현하되, 순수 자바만을 사용하여 만들어진 예제다. 의존성 주입(Dependency Injection)도 생성자를 통해 수동으로 구현되어 있다.</p>
<h3 id="entity">Entity</h3>
<pre><code class="language-java">public class User {
  private Long id;
  private String name;

  public User(Long id, String name) {
      this.id = id;
      this.name = name;
  }

  public Long getId() {
      return id;
  }

  public String getName() {
      return name;
  }

  @Override
  public String toString() {
      return &quot;User{id=&quot; + id + &quot;, name='&quot; + name + &quot;'}&quot;;
  }
}</code></pre>
</li>
</ul>
<h3 id="controller-1">Controller</h3>
<ul>
<li><p>Controller: 클라이언트의 요청(addUser, getUserById)을 처리</p>
<pre><code class="language-java">public class UserController {
  private final UserService userService;

  public UserController(UserService userService) {
      this.userService = userService;
  }

  public void getUserById(Long id) {
      User user = userService.getUserById(id);
      if (user != null) {
          System.out.println(&quot;User found: &quot; + user);
      } else {
          System.out.println(&quot;User not found with ID: &quot; + id);
      }
  }

  public void addUser(User user) {
      userService.addUser(user);
      System.out.println(&quot;User added: &quot; + user);
  }
}</code></pre>
</li>
</ul>
<h3 id="service-1">Service</h3>
<ul>
<li><p>Service: 비즈니스 로직(getUserById, addUser)을 담당</p>
<pre><code class="language-java">public class UserService {
  private final UserRepository userRepository;

  public UserService(UserRepository userRepository) {
      this.userRepository = userRepository;
  }

  public User getUserById(Long id) {
      return userRepository.findById(id);
  }

  public void addUser(User user) {
      userRepository.save(user);
  }
}</code></pre>
</li>
</ul>
<h3 id="repository-1">Repository</h3>
<ul>
<li>Repository: 데이터를 저장하고 검색하는 기능(save, findById)을 제공<pre><code class="language-java">import java.util.ArrayList;
import java.util.List;
</code></pre>
</li>
</ul>
<p>public class UserRepository {
    private final List users = new ArrayList&lt;&gt;();</p>
<pre><code>public User findById(Long id) {
    for (User user : users) {
        if (user.getId().equals(id)) {
            return user;
        }
    }
    return null; // User not found
}

public void save(User user) {
    users.add(user);
}</code></pre><p>}</p>
<pre><code>
### Main
```java
public class Main {
    public static void main(String[] args) {
        // 의존성 주입
        UserRepository userRepository = new UserRepository();
        UserService userService = new UserService(userRepository);
        UserController userController = new UserController(userService);

        // 사용자 추가
        userController.addUser(new User(1L, &quot;Alice&quot;));
        userController.addUser(new User(2L, &quot;Bob&quot;));

        // 사용자 조회
        userController.getUserById(1L);
        userController.getUserById(3L); // 존재하지 않는 사용자
    }
}</code></pre><h3 id="실행-결과">실행 결과</h3>
<pre><code class="language-plaintext">User added: User{id=1, name='Alice'}
User added: User{id=2, name='Bob'}
User found: User{id=1, name='Alice'}
User not found with ID: 3</code></pre>