<p>GitHub Actions의 워크플로를 효과적으로 구성하고 관리하기 위한 환경 설정 방법에 대해 설명하는 챕터이다. 이 챕터에서는 워크플로의 환경을 정의하고 관리하는 다양한 요소들을 다룬다.</p>
<hr />
<h2 id="워크플로-이름과-워크플로-실행-이름">워크플로 이름과 워크플로 실행 이름</h2>
<p>워크플로와 워크플로 실행에 이름을 지정하는 방법을 설명한다.</p>
<pre><code class="language-yaml"># 워크플로 이름 설정
name: Pipeline

# 워크플로 실행 이름 패턴 설정
run-name: Pipeline run by ${{ github.actor }}</code></pre>
<p>워크플로 이름은 Actions 탭에 표시되며, 이름을 설정하지 않으면 워크플로 파일 이름이 사용된다. 워크플로 실행 이름은 GitHub에서 제공하는 데이터를 기반으로 패턴을 지정할 수 있다.</p>
<hr />
<h2 id="컨텍스트">컨텍스트</h2>
<p><a href="https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#context-availability">컨텍스트</a>는 워크플로 실행 중에 접근할 수 있는 정보 집합이다. 다양한 컨텍스트를 통해 워크플로 실행에 관련된 데이터에 접근할 수 있다.</p>
<ul>
<li>GitHub Actions는 여러 내장 컨텍스트를 제공한다 (github, env, vars, secrets 등)</li>
<li>컨텍스트는 ${{ context.property }} 형식으로 참조한다</li>
<li>신뢰할 수 없는 입력이 포함된 컨텍스트 사용 시 주의해야 한다</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/e1a84c91-ea02-4486-8ec7-71bc85706007/image.png" /></p>
<hr />
<h2 id="환경-변수">환경 변수</h2>
<p>환경 변수는 워크플로 실행 중에 값을 저장하고 참조하는 방법을 제공한다. 워크플로, 잡, 스텝 수준에서 환경 변수를 정의할 수 있다.</p>
<pre><code class="language-yml">env:
  GLOBAL_VAR: &quot;전체 워크플로우에 적용되는 변수&quot;    # 워크플로우 수준 환경 변수

jobs:
  example-job:
    runs-on: ubuntu-latest
    env:
      JOB_VAR: &quot;잡 수준 변수&quot;                  # 잡 수준 환경 변수
    steps:
      - name: 환경 변수 출력
        env:
          STEP_VAR: &quot;스텝 수준 변수&quot;             # 스텝 수준 환경 변수
        run: |
          echo &quot;전역 변수: $GLOBAL_VAR&quot;
          echo &quot;${{ env.GLOBAL_VAR }}&quot;
          echo &quot;작업 변수: $JOB_VAR&quot;
          echo &quot;단계 변수: $STEP_VAR&quot;</code></pre>
<hr />
<h3 id="기본-환경-변수">기본 환경 변수</h3>
<p>GitHub Actions는 모든 워크플로에서 사용할 수 있는 <a href="https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables#default-environment-variables">기본 환경 변수</a>를 제공한다.</p>
<hr />
<h2 id="비밀-변수-및-구성-변수">비밀 변수 및 구성 변수</h2>
<p>민감한 정보와 구성 데이터를 안전하게 관리하기 위한 방법으로 비밀 변수와 구성 변수를 사용할 수 있다.</p>
<h3 id="비밀-변수">비밀 변수</h3>
<p>비밀 변수는 API 키, 비밀번호 등 민감한 정보를 저장하는 데 사용된다. 이 값들은 로그에 표시되지 않도록 마스킹 처리된다.</p>
<ul>
<li>저장소 설정 → Secrets and variables → Actions → New repository secret</li>
</ul>
<p>워크플로에서 비밀 변수 사용:</p>
<pre><code class="language-yaml">steps:
  - name: 비밀 변수 사용
    run: |
      echo &quot;API에 연결 중...&quot;
    env:
      API_TOKEN: ${{ secrets.API_TOKEN }}</code></pre>
<h3 id="구성-변수">구성 변수</h3>
<p>구성 변수는 환경별 설정이나 비민감 구성 데이터를 저장하는 데 사용된다. 비밀 변수와 달리 로그에 표시될 수 있다.</p>
<ul>
<li>저장소 설정 → Secrets and variables → Actions → Variables 탭 → New repository variable</li>
</ul>
<p>워크플로에서 구성 변수 사용:</p>
<pre><code class="language-yaml">steps:
  - name: 구성 변수 사용
    if: ${{ vars.ENABLE_FEATURE == 'true' }}
    run: echo &quot;기능 활성화: ${{ vars.FEATURE_CONFIG }}&quot;</code></pre>
<hr />
<h2 id="워크플로-권한-관리">워크플로 권한 관리</h2>
<p>워크플로가 가지는 권한을 제어하여 보안을 강화할 수 있다. GitHub Actions는 <code>GITHUB_TOKEN</code>을 통해 자동으로 인증을 제공하며, 이 토큰의 권한을 세밀하게 제어할 수 있다.</p>
<p>아래 코드를 워크플로의 본문에 추가하면 모든 잡에 엑세스 권한을 제공하며, 개별 잡에 추가하면 해당 잡에만 액세스 권한을 제공한다.</p>
<pre><code class="language-yaml">permissions:
  issues: write</code></pre>
<p>각 권한에 대해 <code>read</code>, <code>write</code>, <code>none</code> 값을 설정할 수 있다. 최소 권한 원칙을 따라 필요한 권한만 부여하는 것이 보안 관점에서 바람직하다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/ekdeon/post/65c18e54-ff86-4a9a-adcf-0fac0d455d39/image.png" /></p>
<hr />
<h2 id="배포-환경">배포 환경</h2>
<p>배포 환경은 워크플로가 코드를 배포할 수 있는 대상을 정의한다. 환경을 사용하면 환경별로 다른 보호 규칙과 비밀 변수를 설정할 수 있다.</p>
<p>환경 생성 및 구성:</p>
<ol>
<li>저장소 설정 → Environments → New environment</li>
<li>환경 이름 지정 (예: production, staging, development)</li>
<li>필요한 보호 규칙 구성</li>
</ol>
<p>환경 보호 규칙:</p>
<ul>
<li>필수 검토자: 워크플로 잡을 승인하는 검토자로 최대 6명의 사람 또는 팀을 지정</li>
<li>대기 타이머: 잡이 처음 트리거된 후 지연할 시간 지정</li>
<li>배포 브랜치: 특정 브랜치에서만 배포 허용</li>
</ul>
<p>워크플로에서 환경 사용:</p>
<pre><code class="language-yaml">jobs:
  deploy:
    environment:
      name: production
      url: https://production.example.com
    runs-on: ubuntu-latest
    steps:
      - name: 배포
        run: ./deploy.sh
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}</code></pre>
<p>환경을 사용하면 다음과 같은 이점이 있다:</p>
<ul>
<li>환경별로 다른 비밀 변수 및 구성 변수 사용</li>
<li>환경별 보호 규칙으로 중요 환경에 대한 추가 보안 제공</li>
<li>배포 이력 및 상태 추적 가능</li>
<li>환경별 URL을 통한 배포 결과 확인 용이</li>
</ul>
<p>환경을 활용하면 개발, 테스트, 스테이징, 프로덕션 등 다양한 단계에 맞는 배포 파이프라인을 안전하게 구성할 수 있다. 특히 프로덕션 환경에는 더 엄격한 보호 규칙을 적용하여 실수로 인한 문제를 방지할 수 있다.</p>