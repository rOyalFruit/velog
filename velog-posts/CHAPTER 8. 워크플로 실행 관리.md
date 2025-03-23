<p>GitHub Actions 워크플로는 본질적으로 명령형보다는 선언형이다. 이는 작업 수행 방법을 정의하는 프로그래밍 로직을 작성하는 대신, 트리거, 작업, 단계 및 러너를 선언하여 워크플로를 주로 생성한다는 의미이다. 그러나 YAML 파일에서 요소를 선언하여 워크플로를 작성한다고 해서 실행 흐름을 더 정밀하게 제어할 수 없다는 의미는 아니다. GitHub Actions는 워크플로가 시작되는 방식과 시작된 후 진행되는 방식을 정밀하게 관리하기 위한 여러 구성 요소와 접근 방식을 제공한다.</p>
<hr />
<h2 id="고급-변경-사항-트리거">고급 변경 사항 트리거</h2>
<p>GitHub Actions에서는 기본적인 트리거링 외에도 트리거링 프로세스에 대한 더 고급 제어가 필요한 상황이 있을 수 있다. 트리거는 일반적인 이벤트뿐만 아니라 변경된 내용의 패턴이나 이벤트 발생 시 진행 중이던 활동 유형 등 더 구체적인 기준을 포함할 수 있다.</p>
<h3 id="활동-유형에-따른-트리거">활동 유형에 따른 트리거</h3>
<p><a href="https://docs.github.com/ko/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows">활동 유형</a> 값을 사용하면 객체에 대한 어떤 종류의 작업이 워크플로를 실행하게 할지 지정할 수 있다. 예를 들어, 새 이슈를 열 때만 간단한 워크플로를 실행하려면 다음과 같이 작성할 수 있다:</p>
<pre><code class="language-yaml">on:
  issues:
    types:
      - opened

jobs:
  notify-for-issue:
    runs-on: ubuntu-latest
    steps:
      - run: echo &quot;An issue was opened ...&quot;</code></pre>
<p>YAML 구문을 사용하여 워크플로가 여러 활동 유형에서 트리거되도록 쉽게 설정할 수도 있다:</p>
<pre><code class="language-yaml">on:
  issues:
    types: [opened, edited, closed]</code></pre>
<h3 id="필터를-활용한-트리거-구체화">필터를 활용한 트리거 구체화</h3>
<p>일부 트리거 이벤트는 이벤트에 대한 응답으로 워크플로가 실행되는 시기를 더 정확하게 정의하기 위해 필터를 사용할 수 있다. 필터는 필터링할 엔티티 유형을 정의하는 키워드와 특정 이름이나 패턴인 하나 이상의 문자열을 사용하여 지정된다. 문자열은 여러 항목을 일치시키기 위해 표준 글로브 구문(<em>, *</em>, ?, !, + 등)을 사용할 수 있다.</p>
<p>푸시 이벤트가 발생할 때 워크플로를 실행하게 하는 브랜치와 태그를 한정하는 좋은 예는 다음과 같다:</p>
<pre><code class="language-yaml">on:
  push:
    branches:
      - main
      - 'rel-v*'
    tags:
      - 'v1.*'
      - 'beta'</code></pre>
<p><code>branches-ignore</code>, <code>tags-ignore</code>, 또는 <code>paths-ignore</code> 키워드를 통해 제외할 브랜치, 태그 또는 경로 집합을 지정할 수도 있다:</p>
<pre><code class="language-yaml">on:
  push:
    branches-ignore:
      - 'prod*'
    tags-ignore:
      - 'rc*'</code></pre>
<p>패턴 사용에 관한 주의사항:</p>
<ul>
<li>이러한 포함/제외 옵션은 pull_request와 같은 다른 이벤트에도 적용된다.</li>
<li>글로브 패턴과 충돌하는 특수 문자를 패턴에 사용하려면 그 문자 앞에 <code>\</code>를 붙인다.</li>
<li>태그를 푸시하는 이벤트에는 경로 필터를 적용할 수 없다.</li>
<li>브랜치 및 태그에 대한 패턴은 Git 구조의 refs/heads에 대해 평가된다.</li>
<li>동일한 이벤트에 대해 포함 키워드와 제외 키워드를 동시에 사용할 수 없다.</li>
</ul>
<p>깃허브에서는 필터 패턴에 대한 유용한 <a href="https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet">치트시트</a>를 제공한다.</p>
<p>더 넓은 집합에서 상속되는 하위 항목을 필터링하려면 <code>!</code> 글로브 패턴을 활용할 수 있다:</p>
<pre><code class="language-yaml">on:
  push:
    branches:
      - 'rel-*'      
      - '!rel-beta*' # 패턴을 선언하는 순서에 주의하자.</code></pre>
<p>브랜치나 태그 옵션과 마찬가지로, 특정 파일 경로의 변경 사항에 대해서만 트리거하도록 푸시나 풀 리퀘스트 이벤트를 더 정밀하게 설정할 수 있다:</p>
<pre><code class="language-yaml">on:
  push:
    paths:
      - '*.go'</code></pre>
<p>다음 코드는 저장소 루트에서 data/**을 제외한 경로에 변경된 파일이 하나 이상 있는 경우에만 실행된다:</p>
<pre><code class="language-yaml">on:
  push:
    paths-ignore:
      - 'data/**'</code></pre>
<hr />
<h2 id="변경-없는-워크플로-트리거">변경 없는 워크플로 트리거</h2>
<p>GitHub Actions 워크플로는 저장소에서 변경 사항이 발생하는 것 외에도 다른 방식으로 트리거될 수 있다. 저장소에서 변경이 발생하지 않고도 호출되는 트리거 세트에는 <code>workflow_dispatch</code>, <code>repository_dispatch</code>, <code>workflow_call</code> 및 <code>workflow_run</code> 이벤트가 포함된다.</p>
<p>디스패치 이벤트는 GitHub 외부에서 발생하는 활동을 기반으로 하나 이상의 워크플로를 트리거해야 하는 경우 사용할 수 있다. 예를 들어, 프로세스가 실패할 때 특정 데이터로 GitHub 이슈를 생성하는 데 사용되는 <code>create-failure-issue</code>라는 저장소에 워크플로가 있다고 가정해보자:</p>
<pre><code class="language-yaml"># 이슈를 생성하기 위한 재사용 가능한 워크플로
name: create-failure-issue

# 워크플로가 실행될 시기를 제어
on:
  # 다른 워크플로에서 이 워크플로를 실행할 수 있도록 함
  workflow_call:
    inputs:
      title:
        required: true
        type: string
      body:
        required: true
        type: string
  # Actions 탭에서 수동으로 호출할 수 있도록 함
  workflow_dispatch:
    inputs:
      title:
        description: &quot;Issue title&quot;
        required: true
      body:
        description: &quot;Issue body&quot;
        required: true

jobs:
  create-issue-on-failure:
    runs-on: ubuntu-latest
    permissions:
      issues: write
    steps:
      - name: Create issue using REST API
        run: |
          curl --request POST \
          --url https://api.github.com/repos/${{ github.repository }}/issues \
          --header &quot;authorization: Bearer ${{ secrets.GITHUB_TOKEN }}&quot; \
          --header &quot;content-type: application/json&quot; \
          --data '{&quot;title&quot;: &quot;Failure: ${{ inputs.title }}&quot;, &quot;body&quot;: &quot;Details: ${{ inputs.body }}&quot;}' \
          --fail</code></pre>
<p>이 워크플로에는 트리거로 <code>workflow_call</code>과 <code>workflow_dispatch</code> 이벤트가 모두 포함되어 있다. <code>workflow_call</code> 트리거를 사용하면 이 워크플로를 다른 워크플로에서 호출할 수 있는 재사용 가능한 워크플로로 만들 수 있다. 다음은 이 워크플로를 호출하는 작업의 예시이다:</p>
<pre><code class="language-yaml">create-issue-on-failure:
  needs: [test-run, count-args]
  if: always() &amp;&amp; failure()
  uses: ./.github/workflows/create-failure-issue.yml
  with:
    title: &quot;Automated workflow failure issue for commit ${{ github.sha }}&quot;
    body: &quot;This issue was automatically created by the GitHub Action workflow ${{ github.workflow }}&quot;</code></pre>
<p><code>workflow_dispatch</code> 트리거를 사용하면 Actions 탭, GitHub CLI 또는 REST API 호출을 통해 워크플로를 실행할 수 있다. 워크플로에 워크플로 디스패치 트리거가 있고 해당 워크플로 파일이 기본 브랜치에 있는 경우, 워크플로를 선택하면 Actions 탭에서 &quot;Run workflow&quot; 버튼이 표시된다.</p>
<p><code>workflow_run</code> 이벤트 트리거를 사용하면 별도의 워크플로 실행을 기반으로 하나의 워크플로 실행을 트리거할 수 있다. 다음 예제의 워크플로는 &quot;Pipeline&quot;이라는 이름의 다른 워크플로가 실행을 완료하면 rel/로 시작하는 브랜치의 액션을 트리거한다:</p>
<pre><code class="language-yaml">on:
  workflow_run:
    workflows: [Pipeline]
    types: [completed]
    branches:
      - 'rel-*'
      - '!rel-*-preprod'</code></pre>
<p>여기서 <code>completed</code> 상태는 파이프라인 실행이 완료됐음을 의미할 뿐, 성공 또는 실패 여부는 알지 못한다. 또는 다른 워크플로가 트리거되었음을 의미하는 <code>requested</code> 상태를 사용할 수도 있다.</p>
<hr />
<h2 id="동시성-처리">동시성 처리</h2>
<p>일반적으로 한 번에 하나의 워크플로 인스턴스만 실행되도록 해야 할 수 있다. 이를 위해 워크플로 구문은 <code>concurrency</code> 키워드를 제공한다. 이는 작업 수준이나 전체 워크플로 수준에서 지정할 수 있다.</p>
<p>작업이나 전체 워크플로의 한 인스턴스만 실행되도록 지정하려면 <code>concurrency</code> 절의 일부로 동시성 그룹을 지정한다. 동시성 그룹은 모든 문자열이나 표현식이 될 수 있다:</p>
<pre><code class="language-yaml">concurrency: release-build</code></pre>
<p>동일한 동시성 그룹으로 다른 인스턴스가 진행 중인 경우 새 인스턴스는 대기 중으로 표시된다. 동일한 동시성 그룹으로 이전에 대기 중인 인스턴스가 있었다면 취소된다.</p>
<p>이미 실행 중인 인스턴스가 있을 때 새 인스턴스가 대기하는 대신 취소되기를 원한다면, <code>cancel-in-progress: true</code>를 동시성 절의 일부로 지정할 수 있다:</p>
<pre><code class="language-yaml">jobs:
  build:
    runs-on: ubuntu-latest
    concurrency:
      group: ${{ github.ref }}
      cancel-in-progress: true</code></pre>
<p>저장소의 다른 워크플로를 실수로 취소하지 않도록 동시성 그룹이 워크플로에 고유하도록 하려면 github 컨텍스트의 workflow 속성을 추가할 수 있다:</p>
<pre><code class="language-yaml">concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true</code></pre>
<p>일부 컨텍스트 값은 특정 유형의 이벤트에 대해서만 정의되므로, 트리거 이벤트가 해당 값을 제공하지 않는 경우 구문 오류가 발생할 수 있다. 이를 방지하기 위해 논리적 OR 연산을 사용하여 대체 동시성 그룹을 가질 수 있다:</p>
<pre><code class="language-yaml">concurrency:
  group: ${{ github.head_ref || github.ref }}
  cancel-in-progress: true</code></pre>
<hr />
<h2 id="매트릭스로-워크플로-실행">매트릭스로 워크플로 실행</h2>
<p>때로는 데이터의 다양한 차원에 따라 동일한 워크플로를 여러 번 실행해야 할 수 있다. 예를 들어, 여러 브라우저에서 동일한 테스트 케이스를 실행해야 할 수 있다. 또는 여러 운영 체제의 각각에서 여러 브라우저에 걸쳐 테스트 케이스를 실행해야 할 수 있다. 이러한 경우에는 GitHub Actions 내에서 매트릭스 전략을 활용할 수 있다.</p>
<p>워크플로의 작업에 대해 이 전략을 지정하고 실행하려는 차원의 매트릭스를 정의한다. 그러면 GitHub Actions는 각 조합에 대한 작업을 생성하고 그에 따라 실행한다:</p>
<pre><code class="language-yaml">name: Create demo issue 3
on:
  push:
jobs:
  create-new-issue:
    strategy:
      matrix:
        prod: [prod1, prod2]
        level: [dev, stage, prod]
    uses: rnd/repos/common/.github/workflows/create-issue.yml@v1
    secrets: inherit
    with:
      title: ${{ matrix.prod }} issue
      body: Update for ${{ matrix.level }}
  report-issue-number:
    runs-on: ubuntu-latest
    needs: create-new-issue
    steps:
      - run: echo ${{ needs.create-new-issue.outputs.issue-num }}</code></pre>
<p>이 예제에서는 두 개의 제품과 세 개의 개발 수준에 걸쳐 처리가 실행된다. 제품과 레벨의 각 조합에 대해 새 GitHub 이슈를 생성하기 위해 재사용 가능한 워크플로를 호출한다. 최종적으로 작업의 6개 인스턴스가 실행되고 6개의 새 이슈가 생성된다.</p>
<p>잡 및 스텝에 대한 <code>continue-on-error</code> 설정은 매트릭스 전략과 함께 사용하여 매트릭스 처리가 정의된 조합을 계속 반복할 수 있도록 할 수 있다. 이것이 지정되면 조합 중 하나가 실패하더라도 워크플로가 나머지 매트릭스를 계속 처리할 수 있다.</p>
<h2 id="워크플로-전용-함수">워크플로 전용 함수</h2>
<p>워크플로에서 사용할 수 있는 여러 내장 함수가 있다. 일부는 문자열이나 다른 값을 검사, 형식 지정 또는 변환하는 편리한 방법을 제공한다. 다음은 사용 가능한 함수의 요약이다:</p>
<table>
<thead>
<tr>
<th>함수</th>
<th>목적</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td>contains</td>
<td>항목이 문자열이나 배열에 포함되어 있는지 확인. 발견되면 true 반환.</td>
<td>contains(search, item)</td>
</tr>
<tr>
<td>startsWith</td>
<td>문자열이 특정 값으로 시작하는지 확인.</td>
<td>startsWith(searchString, searchValue)</td>
</tr>
<tr>
<td>endsWith</td>
<td>문자열이 특정 값으로 끝나는지 확인.</td>
<td>endsWith(searchString, searchValue)</td>
</tr>
<tr>
<td>format</td>
<td>주어진 문자열 내에서 {0}, {1}, {2} 등의 발생을 주어진 순서대로 대체 값으로 바꾼다.</td>
<td>format(string, replaceValue0, replaceValue1, ..., replaceValueN)</td>
</tr>
<tr>
<td>join</td>
<td>배열의 값을 문자열로 연결. 기본 구분자는 쉼표이지만 다른 구분자를 지정할 수 있다.</td>
<td>join(array, optionalSeparator)</td>
</tr>
<tr>
<td>toJSON</td>
<td>지정된 값을 JSON 형식으로 예쁘게 출력.</td>
<td>toJSON(value)</td>
</tr>
<tr>
<td>fromJSON</td>
<td>주어진 값에서 JSON 객체나 JSON 데이터 유형을 반환. 필요한 경우 환경 변수를 문자열에서 부울이나 정수와 같은 다른 데이터 유형으로 변환하는 데 유용.</td>
<td>fromJSON(value)</td>
</tr>
<tr>
<td>hashFiles</td>
<td>지정된 경로와 일치하는 파일 집합에 대한 해시를 반환.</td>
<td>hashFiles(pathFiles)</td>
</tr>
</tbody></table>
<p>이러한 함수는 많은 유틸리티를 제공한다. 예를 들어, <code>hashFiles</code> 함수는 고유한 해시를 생성해 이전 캐시를 사용하는 것이 적절한지 결정하는 데 사용한다. 또한 <code>contains</code> 함수는 특정 브랜치에서 워크플로가 실행 중인지 확인하는 데 사용할 수 있다.</p>
<h3 id="조건부-및-상태-함수">조건부 및 상태 함수</h3>
<p>워크플로 실행 중에 조건부 처리를 위한 특수 함수도 있다. 이러한 함수는 주로 <code>if</code> 절에서 사용되지만 표현식이 허용되는 다른 곳에서도 사용할 수 있다.</p>
<table>
<thead>
<tr>
<th>함수</th>
<th>목적</th>
<th>예시</th>
</tr>
</thead>
<tbody><tr>
<td>success()</td>
<td>이전 단계가 실패하거나 취소되지 않았는지 확인</td>
<td>if: success()</td>
</tr>
<tr>
<td>always()</td>
<td>작업이나 단계가 항상 실행되도록 함</td>
<td>if: always()</td>
</tr>
<tr>
<td>cancelled()</td>
<td>워크플로가 취소되었는지 확인</td>
<td>if: cancelled()</td>
</tr>
<tr>
<td>failure()</td>
<td>작업의 이전 단계가 실패했는지 확인</td>
<td>if: failure()</td>
</tr>
</tbody></table>
<p>이러한 함수는 워크플로 실행의 현재 상태에 대한 정보를 제공한다. 예를 들어, 특정 단계가 실패했을 때만 실행되는 작업을 설정할 수 있다:</p>
<pre><code class="language-yaml">create-issue-on-failure:
  permissions:
    issues: write
  needs: [test-run, count-args]
  if: always() &amp;&amp; failure()
  uses: ./.github/workflows/create-failure-issue.yml</code></pre>
<p>이 예제는 항상 장애가 있는 지 확인해 깃허브 이슈로 장애를 보고한다.</p>
<hr />
<h2 id="결론">결론</h2>
<p>이 장에서는 GitHub Actions 워크플로 실행을 관리하는 다양한 방법을 살펴보았다. 워크플로 트리거를 정밀하게 제어하기 위한 방법부터 동시성 관리, 매트릭스 전략을 사용한 다차원 실행, 그리고 워크플로 내에서 사용할 수 있는 특수 함수까지 다양한 주제를 다루었다.</p>
<p>워크플로 트리거는 활동 유형과 다양한 필터를 통해 더 구체화할 수 있으며, 변경 사항이 없는 경우에도 워크플로를 트리거하는 방법이 있다. 동시성 관리를 통해 동일한 워크플로의 여러 인스턴스가 동시에 실행되는 방식을 제어할 수 있으며, 매트릭스 전략을 사용하면 다양한 조합에 대해 동일한 작업을 효율적으로 실행할 수 있다.</p>
<p>또한 GitHub Actions는 워크플로 내에서 사용할 수 있는 다양한 내장 함수를 제공하여 문자열 조작, JSON 처리, 파일 해싱 등의 작업을 수행할 수 있게 한다. 조건부 및 상태 함수는 워크플로 실행의 현재 상태에 따라 특정 작업이나 단계를 실행할지 여부를 결정하는 데 도움이 된다.</p>