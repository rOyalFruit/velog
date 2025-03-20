<p>GitHub Actions는 자동화된 워크플로우를 실행하기 위해 여러 구성 요소로 이루어진 구조를 제공한다. 이 챕터에서는 GitHub Actions의 작동 방식과 주요 구성 요소를 설명한다.</p>
<hr />
<h2 id="github-actions의-작동-개요">GitHub Actions의 작동 개요</h2>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/9c273bc3-ecc5-4a5d-b456-c0f144de726c/image.png" /></p>
<p>GitHub Actions는 다음과 같은 흐름으로 작동한다:</p>
<ol>
<li><strong>트리거 이벤트 발생</strong>: GitHub 저장소에서 특정 이벤트(예: 코드 푸시, 풀 리퀘스트)가 발생하면 워크플로우가 시작된다.</li>
<li><strong>워크플로우 파일 검색</strong>: <code>.github/workflows</code> 디렉토리에서 이벤트와 일치하는 워크플로우 파일을 검색한다.</li>
<li><strong>워크플로우 실행</strong>: 트리거된 워크플로우는 정의된 작업(jobs)을 실행한다. 작업은 러너(runners)에서 단계(steps)의 형태로 실행된다.</li>
</ol>
<p>워크플로우 파일은 YAML 형식으로 작성되며, 각 워크플로우는 여러 작업으로 구성될 수 있다. 기본적으로 작업은 병렬적으로 실행된다.</p>
<hr />
<h2 id="트리거-이벤트">트리거 이벤트</h2>
<p>GitHub Actions는 다양한 이벤트를 기반으로 워크플로우를 시작할 수 있다:</p>
<ul>
<li><strong>GitHub 내부 이벤트</strong>: 예를 들어, 푸시(push) 또는 풀 리퀘스트(pull request)가 발생할 때.</li>
<li><strong>외부 트리거</strong>: 외부 시스템에서 발생한 이벤트.</li>
<li><strong>스케줄링</strong>: <code>cron</code> 구문을 사용하여 특정 시간에 워크플로우 실행.</li>
<li><strong>수동 실행</strong>: 사용자가 직접 워크플로우를 시작.</li>
</ul>
<p>워크플로우 파일에서 <code>on</code> 키워드를 사용하여 트리거 조건을 정의할 수 있다. 예시는 아래와 같다:</p>
<pre><code class="language-yaml">on:
  push:
    branches:
      - main
  schedule:
    - cron: '0 0 * * *'</code></pre>
<hr />
<h2 id="주요-구성-요소컴포넌트">주요 구성 요소(컴포넌트)</h2>
<p>GitHub Actions는 다음과 같은 주요 구성 요소들로 이루어져 있다:</p>
<h3 id="스텝steps"><strong>스텝(Steps)</strong></h3>
<p>스텝은 작업의 기본 단위이며, 두 가지 형태로 실행된다:</p>
<ul>
<li><strong>사전 정의된 액션 호출</strong> (<code>uses</code> 키워드 사용)</li>
<li><strong>셸 명령 실행</strong> (<code>run</code> 키워드 사용)</li>
</ul>
<p>예제 코드:</p>
<pre><code class="language-yaml">steps:
  - uses: actions/checkout@v3 # 코드 체크아웃 액션 호출
  - name: Setup Node.js
    uses: actions/setup-node@v2
    with:
      node-version: '14'
  - run: npm install # 셸 명령 실행</code></pre>
<h3 id="러너runners"><strong>러너(Runners)</strong></h3>
<p>러너는 워크플로우의 스텝을 실행하는 물리적 또는 가상 시스템이다. GitHub에서 호스팅하거나 사용자가 자체적으로 설정할 수 있다. 러너는 <code>runs-on</code> 키워드를 통해 지정된다.</p>
<pre><code class="language-yaml">jobs:
  build:
    runs-on: ubuntu-latest</code></pre>
<h3 id="잡jobs"><strong>잡(Jobs)</strong></h3>
<p>잡은 여러 스텝을 포함하며, 특정 러너에서 실행된다. 잡은 워크플로우의 논리적 단위이며, 성공 또는 실패 여부가 잡 단위에서 보고된다.</p>
<p>예제 코드:</p>
<pre><code class="language-yaml">jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: echo &quot;Building project...&quot;</code></pre>
<h3 id="워크플로우workflow"><strong>워크플로우(Workflow)</strong></h3>
<p>워크플로우는 이벤트와 조건에 따라 잡을 실행하는 전체 프로세스를 정의한다. 각 워크플로우는 <code>.github/workflows</code> 디렉토리에 저장되며, YAML 형식으로 작성된다.</p>
<pre><code class="language-yaml">name: CI Workflow
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: echo &quot;Hello, World!&quot;</code></pre>