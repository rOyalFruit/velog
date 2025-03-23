<p>깃허브 액션은 수준 높은 자동화를 제공하지만, 이는 보안 위험을 수반할 수 있다. 저장소 소유자는 적절한 예방 조치와 보안 조치를 취해야 한다. 이 장에서는 워크플로와 액션의 보안 영향과 깃허브가 제공하는 보안 메커니즘을 살펴본다.</p>
<p>액션 사용의 보안은 다층적 접근이 필요하며, 다음과 같이 분류할 수 있다:</p>
<ul>
<li>설정을 통한 보안</li>
<li>설계를 통한 보안</li>
<li>모니터링을 통한 보안</li>
</ul>
<hr />
<h2 id="설정을-통한-보안">설정을 통한 보안</h2>
<p><code>액션 및 워크플로의 실행 허용 여부</code>와 <code>허용되는 경우 실행하는 기준</code>을 설정해 관리한다. 저장소의 Settings 탭에서 Actions 메뉴의 General 옵션을 선택하여 구성할 수 있다.</p>
<h3 id="액션-권한-관리">액션 권한 관리</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/fe073381-d649-4f1e-bf66-0842abacc418/image.png" /></p>
<p>마지막 옵션을 선택하면 다음과 같은 세부 기준을 지정할 수 있다:</p>
<ul>
<li>GitHub에서 생성한 액션 허용</li>
<li>마켓플레이스 인증 제작자의 액션 허용</li>
<li>특정 액션 및 재사용 가능한 워크플로 허용(쉼표로 구분해서 직접 입력)</li>
</ul>
<h3 id="풀-리퀘스트에서-워크플로-실행-관리">풀 리퀘스트에서 워크플로 실행 관리</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/90f36aa8-879b-446a-b273-9ccab6988077/image.png" /></p>
<p>저장소에 대한 풀 리퀘스트에서 워크플로를 실행할 외부 공동 작업자를 관리한다. 저장소를 포크한 사람들이 저장소에 정의한 워크플로를 실행하는 걸 제한할 때 사용한다.</p>
<h3 id="워크플로-권한-허가">워크플로 권한 허가</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/e2bb8a5f-ca15-4501-91d4-9a09e801cd14/image.png" /></p>
<p>워크플로 실행 시 GITHUB_TOKEN에 부여되는 기본 권한을 설정할 수 있다:</p>
<ul>
<li>읽기 및 쓰기 권한</li>
<li>저장소 콘텐츠 및 패키지에 대한 읽기 권한만</li>
</ul>
<h3 id="codeowners-파일">CODEOWNERS 파일</h3>
<p><a href="https://docs.github.com/ko/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners">CODEOWNERS 파일</a>은 저장소의 코드를 책임지는 개인이나 팀을 정의한다. 이 파일을 통해 풀 리퀘스트에 대한 자동 검토자와 승인자를 지정할 수 있다.</p>
<pre><code># 각 줄은 파일 패턴과 소유자로 구성됨
# 더 구체적인 줄이 이전 전역-기본 소유자를 재정의함
* @global-default-owner # 전역 기본 소유자
*.go @github-userid # .go 파일의 소유자
testresults/ @tester@mycompany.com # testresults 트리의 소유자</code></pre><h3 id="보호된-태그">보호된 태그</h3>
<p>저장소에서 태그 생성 또는 삭제를 제한하는 규칙을 구성할 수 있다. 보호된 태그를 생성하거나 삭제하려면 <code>관리자 권한</code>이나 <code>유지보수 권한</code> 혹은 <code>규칙 편집 권한을 가진 사용자 지정 역할</code>이 필요하다.</p>
<h3 id="보호된-브랜치">보호된 브랜치</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/a9719f85-914e-4cce-a1a8-b3722f723eaa/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/a4a2d0fa-b728-424e-9847-4144b5339e46/image.png" /></p>
<p>브랜치 보호 규칙을 통해 특정 브랜치에 대한 작업을 제한할 수 있다. 다음과 같은 규칙을 설정할 수 있다:</p>
<ul>
<li>병합 전 풀 리퀘스트 검토 필요</li>
<li>병합 전 상태 확인 필요</li>
<li>병합 전 대화 해결 필요</li>
<li>서명된 커밋 필요</li>
<li>선형 기록 필요</li>
<li>병합 대기열 필요</li>
<li>병합 전 배포 성공 필요</li>
<li>위 설정 우회 금지</li>
<li>일치하는 브랜치에 푸시할 수 있는 사용자 제한</li>
<li>강제 푸시 허용</li>
<li>삭제 허용</li>
</ul>
<h3 id="저장소-규칙">저장소 규칙</h3>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/893f18aa-7ef3-4546-bc3c-1cc2cecf7365/image.png" /></p>
<p>저장소 규칙 집합(Ruleset)은 지정된 태그와 브랜치에 대한 사용자 상호 작용을 제어하는 규칙 모음이다. 다음을 지정할 수 있다:</p>
<ul>
<li>규칙 세트 이름</li>
<li>규칙 세트가 사용하는 브랜치 또는 태그</li>
<li>규칙 세트를 우회할 수 있는 사용자</li>
<li>규칙 세트가 적용할 보호 규칙</li>
</ul>
<p>구성 설정이 저장소의 코드를 원치 않는 변경이나 실행을 방지하는데 큰 도움이 되기는 하지만 이는 워크플로 및 관련 부분의 보안을 보장하는 일부일 뿐이다. 설계 단계부터 보안을 고려하여 워크플로의 기능을 안전하게 설계해야 한다.</p>
<hr />
<h2 id="설계를-통한-보안">설계를 통한 보안</h2>
<p>워크플로를 실행할 때 애초에 접근 권한을 보호하며 오용을 방지하는 방법은 크게 두 가지가 있다:</p>
<ul>
<li>비밀 변수 및 토큰 사용을 통한 개인 데이터 보호</li>
<li>스크립트 인젝션과 같은 일반적인 공격 방지</li>
</ul>
<h3 id="비밀-변수">비밀 변수</h3>
<p>비밀 변수는 시스템에 안전하게 저장된 권한 있는 자격 증명이다. 비밀 변수는 생성 시점부터 암호화되며, 워크플로에서 사용할 때만 복호화된다.</p>
<p>비밀 변수는 조직, 저장소 또는 배포 환경 수준에서 생성할 수 있다. 비밀 변수 이름은 각 수준에서 고유해야 한다.</p>
<p>워크플로 파일에서 비밀 변수에 접근하는 방법:</p>
<pre><code>steps:
  - name: My custom action with input
    secret: ${{ secrets.MySecret }}
    env:
      environment_variable: ${{ secrets.MySecret }}</code></pre><h3 id="비밀-변수-보호">비밀 변수 보호</h3>
<p>비밀 변수를 보호하기 위한 주의사항:</p>
<ul>
<li>저장소에 대한 쓰기 권한이 있는 사용자 제한</li>
<li>비밀 변수 출력 시 주의</li>
<li>비밀 변수 내에서 구조화된 데이터 사용 금지(YAML, JSON, XML...)</li>
<li>정기적으로 비밀 변수 검토 및 주기적 교체</li>
</ul>
<p>비밀 변수는 값을 안전하게 숨기거나 저장하지만, 사용 방법이나 액세서 허용 대상 등에 대한 추가적인 설정을 조정하는 방법은 제공하지 않는다. 보안 설정에 더 많은 컨텍스트와 범위 지정이 필요한 경우 토큰을 사용하면 된다.</p>
<h3 id="토큰">토큰</h3>
<p>토큰은 리소스에 액세스하는 데 사용할 수 있는 전자 키다. 깃허브 액션에 사용하는 토큰에는 개인 액세스 토큰(PAT)과 깃허브 토큰 두가지 유형이 있다.</p>
<h4 id="개인-액세스-토큰pat">개인 액세스 토큰(PAT)</h4>
<p>개인 액세스 토큰은 깃허브 저장소에 개인적으로 액세스하기 위한 것이다. 개발자 설정에서 생성할 수 있다.</p>
<p>워크플로에서 PAT에 액세스하려면 먼저 비밀 변수에 저장해야 한다.</p>
<p>다음은 깃허브 API를 호출하는 curl 명령의 코드이며, 비밀 변수를 통해 PAT가 전달되는 예시다:</p>
<pre><code>steps:
  - name: invoke GitHub API
    run: |
      curl -X POST -H &quot;authorization: Bearer ${{ secrets.PIPELINEUSE }}&quot; ...</code></pre><h4 id="github-토큰">GitHub 토큰</h4>
<p>GitHub Actions가 저장소에서 활성화되면, GitHub는 저장소에 GitHub App을 설치한다. 이 앱은 저장소에 대한 액세스 토큰을 가지고 있다.</p>
<p>워크플로에서 GitHub 토큰에 액세스하는 두 가지 방법:</p>
<ol>
<li>내장 비밀 변수를 통해 액세스:</li>
</ol>
<pre><code>- name: Push changes
  uses: ad-m/github-push-action@master
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    branch: ${{ github.ref }}</code></pre><ol start="2">
<li>github 컨텍스트를 통해 직접 액세스:</li>
</ol>
<pre><code>- name: Create Release
  id: create_release
  uses: actions/create-release@latest
  env:
    GITHUB_TOKEN: ${{ github.token }}</code></pre><p>GitHub 토큰의 권한을 수정하려면 <a href="https://docs.github.com/ko/actions/writing-workflows/workflow-syntax-for-github-actions#permissions">permissions 키</a>를 사용할 수 있다:</p>
<pre><code>permissions:
  actions: read|write|none
  checks: read|write|none
  contents: read|write|none
  deployments: read|write|none
  id-token: read|write|none
  issues: read|write|none
  discussions: read|write|none
  packages: read|write|none
  pages: read|write|none
  pull-requests: read|write|none
  repository-projects: read|write|none
  security-events: read|write|none
  statuses: read|write|none</code></pre><h3 id="신뢰할-수-없는-입력-처리">신뢰할 수 없는 입력 처리</h3>
<p>워크플로가 트리거될 때, 이벤트와 관련된 정보 세트가 제공된다. 보안 관점에서 이 데이터는 일반적으로 신뢰할 수 있는 데이터와 신뢰할 수 없는 데이터로 분류된다.</p>
<p>신뢰할 수 없는 데이터의 예:</p>
<ul>
<li>이슈 제목 및 본문</li>
<li>풀 리퀘스트 제목 및 본문</li>
<li>본문 및 댓글 검토</li>
<li>커밋 메시지</li>
<li>작성자 이메일 및 이름</li>
<li>풀 리퀘스트 참조 및 레이블</li>
</ul>
<h4 id="스크립트-인젝션">스크립트 인젝션</h4>
<p>스크립트 인젝션은 공격자가 웹사이트의 텍스트 필드와 같은 사용자 입력에 악성 코드를 삽입하는 보안 취약점이다.</p>
<p>스크립트 인젝션 취약점 예시:</p>
<pre><code>name: sidemo
on:
  push:
    branches: [main]
jobs:
  process:
    runs-on: ubuntu-latest
    steps:
      - run: echo &quot;${{ github.event.head_commit.message }}&quot;</code></pre><p>위 코드는 워크플로를 트리거한 푸시의 커밋 메시지를 출력하는 스크립트다.
만약 누군가 다음과 같은 커밋 메시지를 푸시한다면:</p>
<pre><code>`echo my content &gt; demo.txt; ls -la; printenv;`</code></pre><p>이 코드는 러너에서 실행되어 파일을 생성하고, 디렉토리 목록을 가져오고, 런처에 환경을 출력한다.</p>
<p>스크립트 인젝션 취약점 방지:</p>
<ol>
<li>인라인 스크립트 사용을 피하고 가능한 경우 액션을 사용</li>
<li>run 명령에 전달하는 값을 중간 변수를 만들어 임시로 저장</li>
</ol>
<p>이전 예시를 수정한 워크플로 예시:</p>
<pre><code>steps:
  - env:
      DATA_VALUE: ${{ github.event.head_commit.message }}
    run: echo &quot;$DATA_VALUE&quot;</code></pre><h3 id="종속성-보안">종속성 보안</h3>
<p><code>uses</code>로 액션을 참조해서 타사 코드를 실행하는 순간 컴퓨팅 시간, 컴퓨팅 리소스(자체 호스팅 러너 사용 시), 같은 잡에서 사용되는 비밀 변수와 깃허브 저장소 토큰에 대한 액세스를 제공한다.</p>
<p>워크플로에서 사용하는 추가 액션이나 워크플로를 보호하기 위한 방법:</p>
<ul>
<li>최소 권한 원칙: 코드는 특정 작업을 수행하는 데 필요한 최소한의 권한으로 실행한다.</li>
<li>사용하는 액션 검증: 모든 액션은 보안 활동 및 정보에 접근하므로 액션을 사용하기 전에 검토를 거친다.</li>
<li>코드 검토: 지금까지 나열한 그 어떤 항목도 액션이 무해하다고 보장하지 못한다.</li>
<li>정확한 깃 참조 사용: 워크플로 코드 버전에 대한 깃 참조를 정확히 지정.(브랜치 이름, 태그/릴리스, changeset, 해시 전체 지정, 액션의 특정 버전을 포크해 나만의 사본을 만들어 참조)</li>
</ul>
<hr />
<h2 id="모니터링을-통한-보안">모니터링을 통한 보안</h2>
<h3 id="스캔">스캔</h3>
<p>GitHub Actions는 스타터 워크플로를 통해 저장소의 코드 스캔을 쉽게 설정할 수 있다. Security 카테고리에서 여러 옵션을 선택할 수 있다.</p>
<p>Dependabot은 종속성 업데이트 및 보안 문제를 확인하는 자동화된 스캔 기능을 제공한다.</p>
<p><a href="https://github.com/marketplace/actions/ossf-scorecard-action">OSSF 스코어카드</a>는 보안 스캔을 위한 추가적인 도구다. 소프트웨어 저장소의 보안과 관련된 요소를 검토한 후에 각 영역에 대해 1점부터 10점까지 점수를 매겨 표시한다.</p>
<h3 id="풀-리퀘스트의-안전한-처리">풀 리퀘스트의 안전한 처리</h3>
<p>풀 리퀘스트 이벤트에서 트리거되도록 설정된 워크플로는 병합 전에 요청된 변경 사항의 유효성을 테스트할 수 있다. 그러나 이는 기존 코드 베이스, 러너 시스템 등에 대한 공격 표면을 제공할 수 있다.</p>
<p>GitHub는 다음과 같은 방식으로 위험을 완화한다:</p>
<ul>
<li>대상 저장소에 대한 쓰기 권한 차단</li>
<li>외부 포크에서 저장소의 비밀 변수에 대한 액세스 방지</li>
<li>동일한 저장소의 브랜치에서 시작된 풀 리퀘스트에 대한 액세스 허용</li>
</ul>
<h3 id="풀-리퀘스트-내-워크플로의-취약점">풀 리퀘스트 내 워크플로의 취약점</h3>
<p><code>pull_request_target</code> 트리거를 사용할 때 주의해야 한다. 이 트리거는 풀 리퀘스트의 대상 저장소 컨텍스트에서 실행되며, 비밀 변수 및 쓰기 권한에 대한 액세스를 허용한다.</p>
<p>취약점 예시:</p>
<pre><code>name: some action
on: [push, pull_request, pull_request_target]
jobs:
  pr-validate:
    name: Validate PR
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository PR
        if: ${{ github.event_name == 'pull_request_target' }}
        uses: actions/checkout@v3
        with:
          ref: ${{ github.event.pull_request.head.sha }}</code></pre><p>이 코드는 <code>pull_request_target</code>에 기반한 실행 경로를 가지고 있어, 워크플로에 비밀 변수 및 전체 읽기/쓰기 GitHub 토큰에 대한 액세스 권한이 부여된다.</p>
<h3 id="풀-리퀘스트-내-소스-코드의-취약점p245">풀 리퀘스트 내 소스 코드의 취약점(p.245)</h3>
<p>풀 리퀘스트 시나리오에서 보안 취약점이 발생할 수 있는 예시:</p>
<pre><code>VALUE1=`git --work-tree=/home/runner/work/pr-demo/pr-demo config --get http.token value location | base64`
echo &quot;VALUE1=$VALUE1&quot;

GITREPO=`git --work-tree=/home/runner/work/pr-demo/pr-demo config --get remote.origin.url`
echo &quot;GITREPO=$GITREPO&quot;
GITUSER=`echo $GITREPO | cut -d &quot;/&quot; -f4`

if [ &quot;$GITUSER&quot; != &quot;gwstudent&quot; ]; then
  echo &quot;We have access to the file system!&quot;
  for i in `ls -R /home/runner/work`; do
    echo &quot;Deleting $i !&quot;
  done
fi</code></pre><p>이 코드는 GitHub 토큰 값을 가져와 base64로 인코딩하고, 파일 시스템에 접근하여 디렉토리를 삭제하려고 시도한다.</p>
<h3 id="풀-리퀘스트-유효성-검사-스크립트-추가">풀 리퀘스트 유효성 검사 스크립트 추가</h3>
<p>풀 리퀘스트 콘텐츠를 검증하기 위한 전용 워크플로 생성:</p>
<pre><code>name: Evaluate PR
on: pull_request_target
permissions:
  contents: read
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 11
        uses: actions/setup-java@v3
        with:
          java-version: '11'
          distribution: 'temurin'
      - name: Build with Gradle
        uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb
        with:
          arguments: build</code></pre><h3 id="안전한-풀-리퀘스트-처리">안전한 풀 리퀘스트 처리</h3>
<p>안전한 풀 리퀘스트 처리를 위한 전략은 다음과 같다:</p>
<ol>
<li><p>대상 저장소의 비밀 변수에 액세스하거나 쓰기 권한이 필요하지 않은 경우 <code>pull_request</code> 트리거를 사용한다.</p>
</li>
<li><p>대상 저장소의 비밀 변수 또는 쓰기 권한이 필요한 경우 워크플로를 여러 부분으로 분할한다:</p>
<ul>
<li>첫 번째 워크플로: 신뢰할 수 없는 코드 처리</li>
<li>두 번째 워크플로: 읽기/쓰기 액세스 및 비밀 변수가 필요한 처리 수행</li>
</ul>
</li>
</ol>
<pre><code class="language-yaml">name: Workflow 1 Handle untrusted code
on: pull_request
jobs:
  process:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: do processing of pull request securely
        ...
      - name: persist results from processing
        uses: actions/upload-artifact@v3
        with:
          name: results
          path: results # 처리 결과 저장

name: Workflow 2 Do processing that needs rw access and/or secrets
on: workflow_run
  workflows: [Workflow 1 Handle untrusted code]
  types: [completed]
jobs:
  process:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.event == 'pull_request' &amp;&amp; github.event.workflow_run.conclusion == 'success' }}
    steps:
      - name: get artifacts
        uses: actions/download-artifact@v3
        with:
          name: results
      - name: do processing with secrets and write access
        ...</code></pre>
<hr />
<h2 id="결론">결론</h2>
<p>깃허브 액션의 보안은 구성, 설계, 모니터링의 세 가지 주요 영역으로 나눌 수 있다. 이러한 접근 방식을 통해 워크플로와 액션을 안전하게 실행할 수 있다. </p>
<p>구성을 통한 보안은 저장소 설정을 통해 액션과 워크플로의 실행 조건을 제어한다. 설계를 통한 보안은 비밀 변수, 토큰, 신뢰할 수 없는 입력 처리 등 워크플로 자체의 보안 설계에 중점을 둔다. 모니터링을 통한 보안은 코드 스캔, 풀 리퀘스트 처리 등을 통해 지속적인 보안 감시를 제공한다.</p>
<p>특히 풀 리퀘스트 처리에서는 <code>pull_request</code>와 <code>pull_request_target</code> 트리거의 차이점을 이해하고, 필요에 따라 워크플로를 분리하여 보안 위험을 최소화하는 것이 중요하다.</p>