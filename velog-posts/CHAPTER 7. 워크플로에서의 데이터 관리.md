<p>오늘날 대부분의 작업은 단일 작업이나 프로젝트로 완료되지 않는다. 일반적인 CI/CD 파이프라인을 생각해보면, 빌드 작업, 패키징 작업, 테스트 작업 등 여러 작업이 필요하다. 이러한 개별 작업들은 서로 데이터와 파일을 주고받을 수 있어야 한다. GitHub Actions는 워크플로에서 입력과 출력을 캡처, 공유, 액세스하기 위한 구문을 제공하며, 중간 파일이나 모듈(아티팩트라고 함)을 관리하기 위한 기능도 제공한다.</p>
<hr />
<h2 id="워크플로의-입출력-사용">워크플로의 입출력 사용</h2>
<p>워크플로 내에서 워크플로 자체의 입력이나 단계 간 또는 작업 간에 공유할 입력과 출력에 액세스해야 할 수 있다. 이를 위해 적절한 구문을 알아야 한다.</p>
<h3 id="워크플로-입력-정의-및-참조">워크플로 입력 정의 및 참조</h3>
<p>여기서 입력이란 사용자나 프로세스가 워크플로에 명시적으로 제공하는 값을 의미한다. 컨텍스트나 기본 환경 변수에서 얻는 값과는 다르다.</p>
<p>입력이 명시적으로 정의되면 <code>${{inputs.&lt;input-name&gt;}}</code> 구문으로 참조할 수 있다. 다음은 두 가지 다른 트리거(<code>workflow_call</code>과 <code>workflow_dispatch</code>)에서 제공된 입력에 액세스하는 작업의 예시이다:</p>
<pre><code class="language-yaml"># 다른 워크플로에서 이 워크플로를 실행할 수 있게 함
on:
  workflow_call:
    inputs:
      title:
        required: true
        type: string
      body:
        required: true
        type: string
  # Actions 탭에서 수동으로 호출할 수 있게 함
  workflow_dispatch:
    inputs:
      title:
        description: &quot;Issue title&quot;
        required: true
      body:
        description: &quot;Issue body&quot;
        required: true

jobs:
  create_issue_on_failure:
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
<p><code>workflow_call</code> 트리거는 이 워크플로를 다른 워크플로에서 호출할 수 있게 하여 재사용 가능한 워크플로로 만든다. <code>workflow_dispatch</code> 트리거는 GitHub 저장소의 Actions 인터페이스에서 직접 워크플로를 호출하는 방법을 만든다.</p>
<h3 id="스텝에서-출력-확인">스텝에서 출력 확인</h3>
<p>스텝에서 출력을 캡처하려면 환경 변수로 출력을 정의하고 <code>GITHUB_OUTPUT</code>에 작성해야 한다. 이를 위해서는 스텝에 <code>id</code> 값을 추가해야 한다. 이 <code>id</code> 필드의 값은 다른 단계에서 환경 변수 값을 참조하는 경로의 일부가 된다.</p>
<pre><code class="language-yaml">jobs:
  setup:
    runs-on: ubuntu-latest
    steps:
      - name: Set debug
        id: set-debug-stage
        run: echo &quot;BUILD_STAGE=debug&quot; &gt;&gt; $GITHUB_OUTPUT
      - name: Get stage
        run: echo &quot;The build stage is ${{ steps.set-debug-stage.outputs.BUILD_STAGE }}&quot;</code></pre>
<p>두 번째 스텝은 <code>steps.step-id.outputs.env-var-name</code> 계층 경로를 참조하여 다른 단계의 값을 가져온다.</p>
<h3 id="잡의-출력-확인">잡의 출력 확인</h3>
<p>잡에서 출력을 캡처하는 것은 스텝의 출력을 기반으로 할 수 있다. 이전 예제에서 두 번째 &quot;Get stage&quot; 스텝이 별도의 잡에 있다고 가정해보자. 첫 번째 잡에서 정보를 다시 전달하려면 해당 잡에 대한 새로운 <code>outputs</code> 섹션을 정의하고 스텝에서 출력 값을 가져와야 한다.</p>
<pre><code class="language-yaml">jobs:
  setup:
    runs-on: ubuntu-latest
    outputs:
      build-stage: ${{ steps.set-debug-stage.outputs.BUILD_STAGE }}
    steps:
      - name: Set debug
        id: set-debug-stage
        run: echo &quot;BUILD_STAGE=debug&quot; &gt;&gt; $GITHUB_OUTPUT

  report:
    runs-on: ubuntu-latest
    needs: setup
    steps:
      - name: Get stage
        run: echo &quot;The build stage is ${{ needs.setup.outputs.build-stage }}&quot;</code></pre>
<p>첫 번째 잡의 값을 가져오기 위해 두 번째 잡의 스텝은 <code>needs.job.outputs.output-value-name</code> 구문을 사용한다. 이는 다른 작업의 출력을 캡처하는 <code>needs</code> 컨텍스트를 활용한다.</p>
<p>출력에 액세스하는 구문이 복잡할 수 있으므로, 잡에서 환경 변수를 통해 참조하는 것이 더 간단할 수 있다:</p>
<pre><code class="language-yaml">report:
  runs-on: ubuntu-latest
  needs: setup
  steps:
    - name: Get stage
      env:
        BUILD_STAGE: ${{ needs.setup.outputs.build-stage }}
      run: echo &quot;The build stage is $BUILD_STAGE&quot;</code></pre>
<h3 id="스텝에서-캡처하는-액션의-출력">스텝에서 캡처하는 액션의 출력</h3>
<p>스텝이 액션을 사용하고 해당 액션이 <code>action.yml</code> 메타데이터를 통해 출력을 정의한 경우, 동일한 방식으로 해당 출력을 캡처할 수 있다.</p>
<pre><code class="language-yaml">jobs:
  build:
    runs-on: ubuntu-latest
    # 작업 출력에 단계 출력 매핑
    outputs:
      artifact-tag: ${{ steps.changelog.outputs.version }}
    steps:
      - name: Conventional Changelog Action
        id: changelog
        uses: TriPSs/conventional-changelog-action@v3.14.0</code></pre>
<hr />
<h2 id="아티팩트-정의">아티팩트 정의</h2>
<p>아티팩트는 GitHub Actions에서 작업이나 워크플로 실행 결과로 생성된 파일 또는 파일 모음으로, GitHub에 저장된다. 아티팩트는 일반적으로 동일한 워크플로의 다른 작업과 공유하기 위해 유지되지만, UI나 REST API 호출을 통해 실행 후에도 접근할 수 있다.</p>
<p>아티팩트는 무기한으로 유지될 수 없다. 기본적으로 GitHub는 공개 저장소의 경우 아티팩트와 빌드 로그를 90일 동안 보관한다. 이 기간은 조정 가능하다. 공개 저장소의 경우 최대 90일, 비공개 저장소의 경우 1~400일 사이로 설정할 수 있다.</p>
<hr />
<h2 id="아티팩트-업로드-및-다운로드">아티팩트 업로드 및 다운로드</h2>
<p>잡에서 생성된 아티팩트를 워크플로의 잡끼리 액세스하려면 아티팩트를 유지해야 한다. 각 잡은 <code>runs-on</code> 절을 사용하여 GitHub에 작업 코드를 실행할 위치를 알려야 한다. 이는 각 잡에 대해 별도의 러너 인스턴스를 사용하는 경우, 환경이 스핀업되고 사용된 다음 제거되며, 이 과정에서 생성된 아티팩트도 함께 제거됨을 의미한다.</p>
<p>아티팩트를 업로드하기 위해 GitHub는 <code>upload-artifact</code> 액션을 제공한다. 다음은 Gradle을 사용하여 Java 소스 프로그램을 빌드하고 아티팩트를 업로드하는 워크플로 예시이다:</p>
<pre><code class="language-yaml">name: Simple Pipe
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 1.8
        uses: actions/setup-java@v1
        with:
          java-version: 1.8
      - name: Grant execute permission for gradlew
        run: chmod +x gradlew
      - name: Build with Gradle
        run: ./gradlew build
      - name: Upload Artifact
        uses: actions/upload-artifact@v3
        with:
          name: greetings-jar
          path: build/libs</code></pre>
<h3 id="매개변수-추가">매개변수 추가</h3>
<p><code>upload-artifact</code> 액션에는 다음과 같은 매개변수가 필요하다:</p>
<table>
<thead>
<tr>
<th>이름</th>
<th>필수</th>
<th>기본값</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>name</td>
<td>예</td>
<td>artifact</td>
<td>아티팩트의 이름</td>
</tr>
<tr>
<td>path</td>
<td>예</td>
<td>none</td>
<td>업로드하려는 파일 시스템 경로</td>
</tr>
<tr>
<td>if-no-files-found</td>
<td>아니오</td>
<td>warn</td>
<td>지정한 경로에 파일이 없을 경우 수행할 작업</td>
</tr>
<tr>
<td>retention-days</td>
<td>아니오</td>
<td>0</td>
<td>아티팩트가 만료되기 전 유지할 일수</td>
</tr>
</tbody></table>
<p>워크플로가 실행된 후, 빌드 중에 생성된 아티팩트는 워크플로 실행 페이지 하단에서 사용할 수 있다. 이 아티팩트는 수동으로 다운로드하거나 삭제하거나 동일한 워크플로의 다른 잡에서 사용할 수 있다.</p>
<p>다른 잡에서 아티팩트를 사용하려면 <code>download-artifact</code> 액션을 사용하여 아티팩트를 다운로드해야 한다:</p>
<pre><code class="language-yaml">test-run:
  runs-on: ubuntu-latest
  needs: build
  steps:
    - name: Download candidate artifacts
      uses: actions/download-artifact@v2
      with:
        name: greetings-jar
    - shell: bash
      run: java -jar greetings-actions.jar</code></pre>
<hr />
<h2 id="github-actions에서-캐시-사용">GitHub Actions에서 캐시 사용</h2>
<p>npm, Yarn, Maven, Gradle과 같은 패키징 및 종속성 관리 도구는 다운로드한 종속성의 캐시를 생성한다. 그러나 깃허브에서 호스팅되는 러너 시스템을 사용하는 경우 액션 안의 각 잡은 종속성을 다시 다운로드한다.GitHub Actions는 이러한 문제를 해결하기 위해 종속성을 캐시하는 기능을 포함한다. </p>
<p>GitHub Actions에서 캐싱 기능을 활성화하는 두 가지 옵션이 있다:</p>
<ol>
<li>명시적 캐시 액션 사용</li>
<li>다양한 <code>setup-*</code> 액션 내에서 활성화</li>
</ol>
<h3 id="명시적-캐시-액션-사용">명시적 캐시 액션 사용</h3>
<p>GitHub 캐시 액션은 많은 프로그래밍 언어와 프레임워크에 적용할 수 있다. 다음은 Go 종속성을 캐싱하고 복원하는 간단한 예시이다:</p>
<pre><code class="language-yaml">- uses: actions/cache@v3
  env:
    cache-name: go-cache
  with:
    path: ~/.cache/go-build
    key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/go.sum') }}
    restore-keys: |
      ${{ runner.os }}-build-${{ env.cache-name }}-
      ${{ runner.os }}-build-
      ${{ runner.os }}-</code></pre>
<p>캐시 키는 변수, 정적 문자열, 함수 또는 컨텍스트 값의 조합으로 만들 수 있으며 최대 길이는 512자이다. 키 라인은 다음과 같이 이해할 수 있다:</p>
<ul>
<li><code>runner.os</code>: 러너 컨텍스트에서 제공하는 호스트 OS 유형</li>
<li><code>build</code>: 현재 워크플로가 빌드 워크플로라는 의미</li>
<li><code>env.cache-name</code>: 캐시 이름을 설정하는 환경 변수</li>
<li><code>hashFiles</code>: 지정된 경로에 대해 해시 알고리즘을 실행하여 생성된 고유 값</li>
</ul>
<p>캐시 범위는 워크플로가 특정 브랜치에 연결되는 것처럼 특정 브랜치와 연결된다. 캐시 액션은 먼저 워크플로 실행이 포함된 브랜치에서 생성된 캐시에 대해 키와 복원 키 세트를 일치시켜 캐시 히트를 검색한다.</p>
<p>캐시 수명 주기에 대해, 7일 이상 액세스되지 않은 캐시는 GitHub에 의해 자동으로 제거된다. 저장소의 모든 캐시에 대해 10GB의 저장 공간으로 제한되며, 10GB 제한을 초과하면 GitHub는 오래된 캐시(최근에 액세스한 적이 없는 캐시)를 제거하기 시작한다.</p>
<h3 id="캐시-모니터링">캐시 모니터링</h3>
<p>GitHub Actions 인터페이스의 주요 워크플로 페이지에서 왼쪽의 관리 아래에 있는 캐시 섹션을 클릭하여 생성된 모든 캐시를 볼 수 있다. 특정 지점과 관련된 캐시를 표시하거나 생성일자, 크기 등으로 캐시를 정렬할 수 있다.</p>
<h3 id="설정-액션에-캐시-활성화">설정 액션에 캐시 활성화</h3>
<p>명시적 캐시와 비교하여, 기존 설정 액션(setup류)의 일부인 캐시를 사용하는 것은 무척 간단하다. 이러한 액션에는 캐싱을 생성하고 사용하기 위한 내장 기능이 있다. 다음은 Gradle 파일용 캐시가 있는 JDK를 설정하는 단계이다:</p>
<pre><code class="language-yaml">- name: Set up JDK
  uses: actions/setup-java@v3
  with:
    java-version: 11
    distribution: 'temurin'
    cache: 'gradle'</code></pre>
<p>이 경우 캐시 키는 자동으로 다음 형식으로 구성된다: <code>setup-java-${{platform}}-${{packageManager}}-${{fileHash}}</code></p>
<hr />
<h2 id="결론">결론</h2>
<p>이 장에서는 단계, 작업, 워크플로 및 워크플로 실행 간에 다양한 유형의 데이터를 생성, 관리, 공유 및 유지하는 방법에 대해 논의했다. 이러한 다양한 유형에는 단계 및 작업의 입력과 출력이 포함된다. 워크플로의 부분 간에 전달되는 이러한 값은 기본 데이터를 보존하고 공유하는 데 중요하다.</p>
<p>GitHub Actions의 아티팩트는 워크플로의 작업 간에 그리고 워크플로 실행이 완료된 후에도 콘텐츠를 생성하고 유지하는 방법을 제공한다. 이러한 아티팩트는 액션이 액세스할 수 있는 모든 유형 및 경로의 파일 모음일 수 있다. 커뮤니티 액션을 사용하여 GitHub 환경 내에서 아티팩트를 업로드하고 다운로드할 수 있다.</p>
<p>또한 실행마다 다운로드하거나 재생성하는 데 필요한 시간을 피하고 각 실행에서 불필요한 리소스 사용을 피하기 위해 종속성을 실행 간에 유지하는 것이 유용할 수 있다. Java, Gradle 및 기타 도구에 대한 많은 설정 액션은 표준 종속성에 대한 캐싱을 활용하는 옵션을 제공한다.</p>
<p>GitHub Actions는 캐시를 직접 생성하고 사용하기 위한 전용 캐싱 액션도 제공한다. 이 액션을 사용할 때는 캐시된 콘텐츠의 일치 항목을 찾는 데 사용되는 고유한 키 값을 구성하는 방법을 지정한다. 또한 일치 항목을 확인할 더 넓은 값 목록을 제공할 수도 있다. GitHub Actions 인터페이스는 워크플로에서 생성하고 사용할 수 있는 캐시를 보여주며, 이를 삭제할 수 있는 기능도 제공한다. 일부 기능은 GitHub API를 통해서도 사용할 수 있다.</p>