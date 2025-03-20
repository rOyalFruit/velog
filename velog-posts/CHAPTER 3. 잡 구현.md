<p>GitHub Actions의 핵심 구성 요소인 '액션'에 대해 자세히 살펴보는 챕터이다. 액션은 워크플로우 내에서 재사용 가능한 코드 단위로, GitHub Actions 시스템의 기본 빌딩 블록이다.</p>
<hr />
<h2 id="액션의-구조">액션의 구조</h2>
<p>액션은 다음과 같은 구조로 이루어져 있다:</p>
<ol>
<li><p><strong>액션 메타데이터 파일</strong>: <code>action.yml</code> 또는 <code>action.yaml</code> 파일로, 액션의 입력, 출력, 실행 방법 등을 정의한다. 이 파일은 액션의 루트 디렉토리에 위치한다.</p>
</li>
<li><p><strong>소스 코드</strong>: 액션의 실제 기능을 구현하는 코드로, 액션 유형에 따라 다양한 형태를 가질 수 있다.</p>
</li>
</ol>
<p>액션 메타데이터 파일의 형식은 다음과 같은 네 가지 영역으로 나뉜다:</p>
<ul>
<li><code>기본 정보</code>: 이름, 작성자, 설명</li>
<li><code>inputs</code>: 액션이 받을 수 있는 입력 파라미터 정의</li>
<li><code>outputs</code>: 액션이 생성하는 출력 정의</li>
<li><code>runs</code>: 액션의 실행 방법 정의</li>
</ul>
<p>예를 들어, checkout 액션의 메타데이터 파일은 다음과 같은 구조를 가진다:</p>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/946e6ec1-8572-4897-b249-bbcde0565fc0/image.png" /></p>
<hr />
<h2 id="액션과의-상호작용">액션과의 상호작용</h2>
<p>액션은 입력과 출력을 통해 워크플로우와 상호작용한다. 입력은 <code>with</code> 키워드를 사용하여 전달하고, 출력은 액션이 실행된 후 워크플로우에서 참조할 수 있다.</p>
<p>액션의 기본 동작은 <code>defaults</code> 속성을 통해 정의할 수 있으며, 이는 액션이 실행될 때의 기본 환경을 설정한다. 또한 액션은 <code>branding</code> 속성을 통해 GitHub Marketplace에서의 시각적 표현을 정의할 수 있다.</p>
<hr />
<h2 id="액션-사용법">액션 사용법</h2>
<p>워크플로우에서 액션을 사용하는 방법은 다음과 같다:</p>
<pre><code class="language-yaml">steps:
  - uses: actions/checkout@v3
    with:
      repository: 'octocat/hello-world'
      ref: 'master'</code></pre>
<p>여기서 <code>uses</code> 키워드는 사용할 액션을 지정하고, <code>with</code> 키워드는 액션에 전달할 입력 파라미터를 정의한다. 액션 참조 형식은 <code>{소유자}/{저장소}@{참조}</code> 형태를 따른다.</p>
<p>액션 참조에서 <code>@</code> 뒤의 참조는 다음과 같은 형태를 가질 수 있다:</p>
<ul>
<li>브랜치 이름: <code>@main</code></li>
<li>태그: <code>@v1.0.0</code></li>
<li>SHA 해시: <code>@a1b2c3d4e5f6</code></li>
<li>주요 버전: <code>@v1</code> (최신 마이너/패치 버전 사용)</li>
</ul>
<hr />
<h2 id="공개-액션과-마켓플레이스">공개 액션과 마켓플레이스</h2>
<p>액션 마켓플레이스는 개발자들이 액션을 공유하는 저장소이다. 마켓플레이스는 직접 URL, 워크플로우 편집 화면, 또는 GitHub 내 검색을 통해 접근할 수 있다.</p>
<p>GitHub에서 제공하는 공식 액션은 github.com/actions에서 확인할 수 있다. 액션에 접근할 때는 <a href="https://github.com/actions/checkout">저장소 URL</a>과 <a href="https://github.com/marketplace/actions/checkout">마켓플레이스 링크</a> 두 가지 방식이 있으며, 마켓플레이스 링크는 더 사용자 친화적인 정보를 제공한다.</p>