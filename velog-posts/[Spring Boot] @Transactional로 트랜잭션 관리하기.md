<h2 id="트랜잭션이란">트랜잭션이란?</h2>
<ul>
<li>데이터베이스의 상태를 변화시키는 작업의 최소 단위로, 데이터 무결성과 일관성을 보장하는 핵심 메커니즘.</li>
<li><code>ACID</code>(각 특징별 앞 글자를 딴 약어)라고 불리는 4가지 특징을 가짐.</li>
</ul>
<h3 id="1-원자성-atomicity">1. 원자성 (Atomicity)</h3>
<ul>
<li><strong>모든 작업이 완전히 수행되거나 전혀 수행되지 않아야 함</strong></li>
<li>예: 은행 송금 시 출금과 입금이 동시에 성공해야 함</li>
<li>실패 시 <strong>롤백(Rollback)</strong> 메커니즘 작동</li>
</ul>
<h3 id="2-일관성-consistency">2. 일관성 (Consistency)</h3>
<ul>
<li>트랜잭션 수행 전후 데이터베이스의 <strong>일관된 상태 유지</strong></li>
<li>미리 정의된 규칙/제약조건을 항상 만족해야 함</li>
<li>예: 나이는 음수가 될 수 없는 규칙</li>
</ul>
<h3 id="3-격리성-isolation">3. 격리성 (Isolation)</h3>
<ul>
<li>동시 실행되는 트랜잭션들이 서로 간섭하지 않도록 보장</li>
<li><strong>격리 수준</strong>에 따라 동시성 제어<ul>
<li>READ UNCOMMITTED</li>
<li>READ COMMITTED</li>
<li>REPEATABLE READ</li>
<li>SERIALIZABLE</li>
</ul>
</li>
</ul>
<h3 id="4-지속성-durability">4. 지속성 (Durability)</h3>
<ul>
<li>성공적으로 수행된 트랜잭션은 <strong>영구적으로 저장</strong></li>
<li>시스템 장애 발생해도 데이터 보존</li>
<li>로그와 복구 메커니즘을 통해 구현</li>
</ul>
<hr />
<h2 id="transactional-이란">@Transactional 이란?</h2>
<ul>
<li><p>스프링에서 제공하는 선언적 트랜잭션 관리 어노테이션으로, 메서드나 클래스에 트랜잭션 동작을 쉽게 적용할 수 있게 해줌.</p>
</li>
<li><p>다음과 같은 경우에 사용</p>
<ul>
<li>여러 데이터베이스 작업을 하나의 단위로 묶을 때</li>
<li>데이터의 일관성과 무결성을 보장해야 할 때</li>
<li>복잡한 비즈니스 로직에서 데이터 변경을 관리할 때</li>
</ul>
</li>
<li><p>트랜잭션 흐름</p>
<pre><code>메서드 호출 → 트랜잭션 시작 → 메서드 실행 → 정상 종료(커밋) 또는 예외 발생(롤백)</code></pre></li>
</ul>
<hr />
<h2 id="transactional-우선순위">@Transactional 우선순위</h2>
<ul>
<li>클래스 메서드에 선언된 트랜잭션의 우선순위가 가장 높고, 인터페이스에 선언된 트랜잭션의 우선순위가 가장 낮음.<pre><code>클래스 메서드 &gt; 클래스 &gt; 인터페이스 메서드 &gt; 인터페이스</code></pre><ul>
<li>클래스에 적용하면 메서드는 자동 적용됨.</li>
</ul>
</li>
</ul>
<hr />
<h2 id="transactional의-주요-속성">@Transactional의 주요 속성</h2>
<h3 id="1-propagation-전파-속성">1. Propagation (전파 속성)</h3>
<ul>
<li>트랜잭션의 경계와 동작 방식을 결정함.</li>
</ul>
<table>
<thead>
<tr>
<th>전파 옵션</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><strong>REQUIRED</strong></td>
<td>기존 트랜잭션이 있으면 참여하고, 없으면 새로운 트랜잭션 생성</td>
</tr>
<tr>
<td><strong>REQUIRES_NEW</strong></td>
<td>항상 새로운 트랜잭션 생성</td>
</tr>
<tr>
<td><strong>NESTED</strong></td>
<td>기존 트랜잭션 내에 중첩 트랜잭션 생성</td>
</tr>
<tr>
<td><strong>SUPPORTS</strong></td>
<td>기존 트랜잭션이 있으면 참여하고, 없으면 트랜잭션 없이 진행</td>
</tr>
</tbody></table>
<h3 id="2-isolation-격리-수준">2. Isolation (격리 수준)</h3>
<ul>
<li>동시에 실행되는 트랜잭션 간의 격리 정도를 결정함.</li>
</ul>
<table>
<thead>
<tr>
<th>격리 수준</th>
<th>설명</th>
<th>동시성 문제</th>
</tr>
</thead>
<tbody><tr>
<td><strong>DEFAULT</strong></td>
<td>데이터베이스 기본 격리 수준</td>
<td>-</td>
</tr>
<tr>
<td><strong>READ_UNCOMMITTED</strong></td>
<td>가장 낮은 격리 수준</td>
<td>더티 리드 가능</td>
</tr>
<tr>
<td><strong>READ_COMMITTED</strong></td>
<td>커밋된 데이터만 읽기</td>
<td>일부 동시성 문제 해결</td>
</tr>
<tr>
<td><strong>REPEATABLE_READ</strong></td>
<td>반복 읽기 보장</td>
<td>팬텀 리드 가능성</td>
</tr>
<tr>
<td><strong>SERIALIZABLE</strong></td>
<td>완전 격리</td>
<td>동시성 최소</td>
</tr>
</tbody></table>
<h3 id="3-timeout">3. Timeout</h3>
<ul>
<li>트랜잭션의 최대 수행 시간을 제한함.<ul>
<li>기본값: -1 (무제한)</li>
<li>초 단위로 지정 가능</li>
</ul>
</li>
</ul>
<h3 id="4-readonly">4. ReadOnly</h3>
<ul>
<li>읽기 전용 트랜잭션을 설정함.<ul>
<li>성능 최적화 목적</li>
<li>쓰기 작업 의도적 방지</li>
<li>기본값: false</li>
</ul>
</li>
</ul>
<h3 id="5-rollbackfor">5. RollbackFor</h3>
<ul>
<li>특정 예외 발생 시 롤백을 설정함.</li>
</ul>
<hr />
<h2 id="transactional-사용-예시">@Transactional 사용 예시</h2>
<h3 id="1-기본-사용">1. 기본 사용</h3>
<pre><code class="language-java">@Service
public class UserService {
    @Transactional
    public void registerUser(User user) {
        userRepository.save(user);
    }
}</code></pre>
<h3 id="2-상세-설정">2. 상세 설정</h3>
<pre><code class="language-java">@Transactional(
    propagation = Propagation.REQUIRED,
    isolation = Isolation.READ_COMMITTED,
    timeout = 10,
    readOnly = true,
    rollbackFor = {Exception.class}
)
public void complexBusinessLogic() {
    // 트랜잭션 로직
}</code></pre>
<hr />
<h2 id="transactional-주의사항">@Transactional 주의사항</h2>
<ul>
<li>private 메서드에는 적용되지 않음</li>
<li>self-invocation(같은 클래스 내 메서드 호출) 시 트랜잭션 동작하지 않음</li>
<li>런타임 예외만 자동 롤백</li>
<li>체크 예외는 기본적으로 롤백 되지 않음<ul>
<li>체크 예외: RuntimeException 클래스를 상속받지 않은 예외 클래스. 예외 처리를 해야 함.</li>
</ul>
</li>
<li>테스트 환경에서 @Transactional은 메서드 종료 후 자동 롤백 됨.</li>
<li>JPA 사용 시 대부분의 메서드에 이미 @Transactional이 선언되어 있음</li>
<li>인터페이스에 @Transactional을 사용하는 것은 권장되지 않음.</li>
<li>트랜잭션 범위는 서비스 계층에서 관리하는 것이 좋음</li>
<li>과도한 트랜잭션 사용은 성능 저하 유발 가능</li>
</ul>