<p>GitHub Actions의 워크플로를 생성하고 관리하는 방법에 대해 살펴보는 챕터이다. 이 챕터에서는 리포지터리에서 워크플로를 생성하고, 커밋하며, VS Code 확장 기능을 활용하는 방법을 다룬다.</p>
<hr />
<h2 id="리포지터리에서-워크플로-생성하기">리포지터리에서 워크플로 생성하기</h2>
<p>GitHub 리포지터리에서 워크플로를 생성하는 과정은 다음과 같다:</p>
<ol>
<li>리포지터리에서 &quot;Actions&quot; 탭을 클릭하면 스타터 워크플로 템플릿 목록이 표시된다.</li>
<li>필요한 템플릿을 선택하거나 &quot;Simple Workflow&quot; 같은 기본 템플릿을 선택할 수 있다.</li>
<li>템플릿을 선택하면 GitHub 에디터에서 워크플로 파일이 열리고, 이를 수정할 수 있다.</li>
</ol>
<p>혹은 깃허브 외부에서 워크플로 파일을 만들어 리포지터리의 .github/workflows 디렉터리에 추가한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/20e35383-ad79-4f7f-a9e2-8af58fc209aa/image.png" /></p>
<p>스타터 워크플로의 전체 코드를 살펴보자. 다음과 같은 주요 섹션으로 구성된다:</p>
<ul>
<li><code>name</code>: 워크플로의 이름</li>
<li><code>on</code>: 워크플로를 트리거하는 이벤트(예: push, workflow_dispatch)</li>
<li><code>jobs</code>: 실행할 작업과 단계 정의</li>
</ul>
<p>예를 들어, 스타터 워크플로 코드는 다음과 같은 구조를 가진다:</p>
<pre><code class="language-yaml"># 워크플로의 이름 정의
name: CI

# 워크플로를 트리거하는 이벤트 정의
on:
  # main 브랜치에 푸시가 발생할 때 워크플로 실행
  push:
    branches: [ main ]
  # main 브랜치에 대한 풀 리퀘스트가 발생할 때 워크플로 실행
  pull_request:
    branches: [ main ]
  # 수동으로 워크플로를 실행할 수 있는 옵션 활성화
  workflow_dispatch:

# 실행할 작업 정의
jobs:
  # 'build'라는 이름의 작업 정의
  build:
    # 작업이 실행될 환경 지정 (Ubuntu 최신 버전)
    runs-on: ubuntu-latest
    # 작업 내에서 실행할 단계 정의
    steps:
      # GitHub 저장소의 코드를 체크아웃하는 액션 사용
      - uses: actions/checkout@v3
      # 한 줄 스크립트를 실행하는 단계
      - name: Run a one-line script
        run: echo Hello, world!
      # 여러 줄 스크립트를 실행하는 단계
      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
</code></pre>
<hr />
<h2 id="워크플로-커밋하기">워크플로 커밋하기</h2>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/0a60ee07-6b35-4944-86e7-8c2326d71c83/image.png" /></p>
<p>워크플로 파일을 생성한 후에는 이를 리포지터리에 커밋해야 한다. 커밋 과정은 다음과 같다:</p>
<ol>
<li>워크플로 파일 경로를 확인한다(기본적으로 <code>.github/workflows/</code> 디렉토리에 저장됨).</li>
<li>필요한 경우 워크플로 파일 내용을 수정한다.</li>
<li>변경 사항을 커밋하기 전에 미리보기를 통해 확인할 수 있다.</li>
<li>커밋 대화 상자에서 커밋 메시지를 입력하고 커밋한다.</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/a5552da6-63c1-4294-b542-b4fc36506256/image.png" /></p>
<p>워크플로 파일이 커밋되면 자동으로 첫 번째 워크플로 실행이 트리거된다. 워크플로 실행 결과는 &quot;Actions&quot; 탭에서 확인할 수 있으며, 다음과 같은 정보를 제공한다:</p>
<ul>
<li>워크플로 실행 목록과 상태</li>
<li>각 작업의 세부 단계 및 실행 결과</li>
<li>실행 로그 및 오류 메시지</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/d99b0ec1-bec7-43b7-822c-6959f81239ea/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/2af00c2c-bfc0-4953-9f7b-05d73691f94c/image.png" /></p>
<p>또한 워크플로에 대한 <a href="https://docs.github.com/ko/actions/monitoring-and-troubleshooting-workflows/monitoring-workflows/adding-a-workflow-status-badge">상태 배지</a>를 생성하여 README.md 파일에 추가할 수 있다. 이 배지는 워크플로의 현재 상태(성공, 실패 등)를 시각적으로 보여준다.</p>
<hr />
<h2 id="vs-code용-github-actions-확장-기능-사용">VS Code용 GitHub Actions 확장 기능 사용</h2>
<p>VS Code에서 GitHub Actions 워크플로를 더 효율적으로 관리하기 위해 GitHub Actions 확장 기능을 사용할 수 있다. 이 확장 기능의 주요 기능은 다음과 같다:</p>
<ol>
<li><p><strong>워크플로 탐색</strong>: EXPLORER 뷰에서 워크플로 파일, 작업, 실행 등을 쉽게 탐색할 수 있다. </p>
</li>
<li><p><strong>문맥 감지 도움말</strong>: 워크플로 편집 시 키워드에 마우스를 올리면 해당 요소에 대한 설명이 팝업으로 나타난다.</p>
</li>
<li><p><strong>구문 오류 감지</strong>: 워크플로 파일 작성 중 발생하는 구문 오류를 자동으로 감지하고 표시한다. </p>
</li>
<li><p><strong>코드 자동 완성</strong>: 워크플로 파일 편집 시 사용 가능한 옵션들을 자동으로 제안한다. 이는 워크플로 작성 효율성을 높인다.</p>
</li>
<li><p><strong>로그 탐색</strong>: OUTLINE 섹션을 통해 로그 파일의 특정 부분으로 쉽게 이동할 수 있다. 긴 로그 파일 분석 시 유용하다.</p>
</li>
<li><p><strong>액션 소스 코드 접근</strong>: <code>uses</code> 구문에 마우스를 올리면 해당 액션의 소스 코드로 바로 이동할 수 있는 링크가 제공된다.</p>
</li>
</ol>