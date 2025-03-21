<p>GitHub Actions의 워크플로우를 실행하는 시스템인 러너(Runner)에 대해 설명하는 챕터이다. 러너는 워크플로우의 작업(job)을 실행하는 물리적 또는 가상 시스템으로, GitHub에서 호스팅하는 러너와 사용자가 직접 호스팅하는 러너로 구분된다.</p>
<hr />
<h2 id="github-호스팅-러너">GitHub 호스팅 러너</h2>
<p>GitHub에서 제공하는 러너는 다음과 같은 특징을 가진다:</p>
<ul>
<li>GitHub에서 관리하고 유지하는 가상 머신으로, 워크플로우 실행을 위한 환경을 제공한다.</li>
<li>여러 운영 체제(Linux, Windows, macOS)를 지원하며, 각 운영 체제별로 다양한 버전을 제공한다.</li>
</ul>
<h3 id="러너-이미지-내-지원-소프트웨어">러너 이미지 내 지원 소프트웨어</h3>
<p>GitHub 호스팅 러너에는 다양한 소프트웨어가 사전 설치되어 있다:</p>
<pre><code class="language-yaml">runs-on: ubuntu-latest</code></pre>
<p>위 코드는 최신 Ubuntu 이미지를 사용하는 러너를 지정한다. 이 이미지에는 다양한 프로그래밍 언어, 빌드 도구, 데이터베이스 등이 사전 설치되어 있다.</p>
<h3 id="러너에-소프트웨어-추가">러너에 소프트웨어 추가</h3>
<p>필요한 소프트웨어가 러너에 없는 경우, 다음과 같은 방법으로 추가할 수 있다:</p>
<ol>
<li><code>apt-get</code>이나 <code>brew</code> 같은 패키지 관리자 사용</li>
<li>직접 다운로드하여 설치</li>
<li>Docker 컨테이너 사용</li>
</ol>
<hr />
<h2 id="자체-호스팅-러너-시스템의-요구-사항">자체 호스팅 러너 시스템의 요구 사항</h2>
<p>자체 호스팅 러너를 설정하기 위해서는 다음과 같은 요구 사항을 충족해야 한다:</p>
<ol>
<li><p><strong>지원되는 아키텍처 및 운영 체제</strong>: <a href="https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners#supported-architectures-and-operating-systems-for-self-hosted-runners">애플리케이션을 지원하는 아키텍처 및 운영체제</a>를 기반으로 한다. 기본적인 사양은 최신 버전의 Linux, Windows, macOS 운영 체제와 x86-64 또는 ARM 프로세서 아키텍처다.</p>
</li>
<li><p><strong>러너 애플리케이션 설치 및 실행 능력</strong>: <a href="https://github.com/actions/runner">자체 호스팅 러너 애플리케이션</a>을 설치하고 실행할 수 있어야 한다.</p>
</li>
<li><p><strong>네트워크 연결</strong>: <a href="https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners#communication-between-self-hosted-runners-and-github">GitHub 서버</a>와 통신하여 작업을 수신하고 러너 업데이트를 다운로드할 수 있어야 한다.</p>
</li>
<li><p><strong>충분한 하드웨어 리소스</strong>: 실행하려는 워크플로 유형에 맞는 CPU, 메모리, 스토리지 등의 충분한 하드웨어 리소스가 있어야 한다.</p>
</li>
<li><p><strong>적절한 소프트웨어</strong>: 실행하려는 워크플로 및 작업 유형에 적합한 소프트웨어가 설치되어 있어야 한다. Docker 컨테이너 액션을 사용하는 워크플로의 경우, Docker가 설치된 Linux 머신이 필요하다.</p>
</li>
<li><p><strong>적절한 네트워크 액세스</strong>: 필요하거나 승인된 리소스 및 엔드포인트에 대한 적절한 네트워크 액세스가 있어야 한다.</p>
</li>
</ol>
<hr />
<h2 id="자체-호스팅-러너의-제한-사항">자체 호스팅 러너의 제한 사항</h2>
<p>자체 호스팅 러너를 사용할 때는 다음과 같은 제한 사항이 적용된다:</p>
<table>
<thead>
<tr>
<th>종류</th>
<th>제한 범위</th>
<th>제한 초과 시 조치</th>
</tr>
</thead>
<tbody><tr>
<td>워크플로 런타임</td>
<td>35일</td>
<td>워크플로 취소</td>
</tr>
<tr>
<td>잡 대기열 시간</td>
<td>24시간</td>
<td>시작되지 않은 잡 전부 종료</td>
</tr>
<tr>
<td>API 요청</td>
<td>저장소의 모든 액션에 대해 시간당 1,000개</td>
<td>추가 API 호출 실패</td>
</tr>
<tr>
<td>잡 매트릭스</td>
<td>워크플로 실행당 256개 작업</td>
<td>허용되지 않음</td>
</tr>
<tr>
<td>워크플로 실행 대기열</td>
<td>저장소당 10초 간격으로 500개 워크플로 실행</td>
<td>워크플로 실행이 즉각 실패로 종료됨</td>
</tr>
<tr>
<td>GitHub Actions에 의한 대기열</td>
<td>트리거 후 30분 이내</td>
<td>워크플로 처리되지 않음(GitHub Actions 서비스를 장시간 사용할 수 없는 경우에만 발생할 가능성이 높음)</td>
</tr>
</tbody></table>
<hr />
<h3 id="자체-호스팅-러너-보안-고려-사항">자체 호스팅 러너 보안 고려 사항</h3>
<p>자체 호스팅 러너는 보안 위험이 있으므로 다음 사항을 고려해야 한다:</p>
<ul>
<li>포크된 저장소에서 오는 워크플로우 실행 시 주의</li>
<li>공개 저장소에서는 가능한 한 자체 호스팅 러너 사용 자제</li>
<li>권한이 제한된 계정으로 러너 실행</li>
</ul>
<hr />
<h3 id="자체-호스팅-러너-설정">자체 호스팅 러너 설정</h3>
<p>자체 호스팅 러너를 설정하는 과정은 다음과 같다:</p>
<ol>
<li>GitHub 저장소 또는 조직 설정에서 &quot;Actions&quot; 메뉴 선택</li>
<li>&quot;Runners&quot; 섹션에서 &quot;New runner&quot; 클릭 후 &quot;New self-hosted runner&quot; 클릭</li>
<li>운영 체제 선택 및 설치 명령어 복사</li>
<li>러너를 설치할 시스템에서 명령어 실행</li>
<li>러너 구성 및 시작</li>
</ol>
<hr />
<h3 id="자체-호스팅-러너-사용">자체 호스팅 러너 사용</h3>
<p>워크플로우에서 자체 호스팅 러너를 사용하려면 <code>runs-on: self-hosted</code> 를 지정한다:</p>
<pre><code class="language-yaml">jobs:
  build:
    runs-on: self-hosted
    steps:
     - name: Install tree
       run: |
         brew update
         brew install tree
     - name: Execute tree
       run: time tree | tee filetreelist.txt</code></pre>
<hr />
<h3 id="자체-호스팅-러너와-레이블-사용">자체 호스팅 러너와 레이블 사용</h3>
<p>자체 호스팅 러너에 사용자 지정 레이블을 적용하려면 초기 구성 스크립트를 실행할 때 전달하면 된다:</p>
<pre><code>./config.sh --url &lt;REPO_URL&gt; --token &lt;REG_TOKEN&gt; --labels ssd,gpu</code></pre><p>레이블을 사용하여 특정 러너를 선택할 수 있다:</p>
<pre><code class="language-yaml">runs-on: [self-hosted, linux, ssd]</code></pre>
<p>위 코드는 'self-hosted', 'linux', 'ssd' 레이블을 모두 가진 러너를 선택한다.</p>
<hr />
<h3 id="자체-호스팅-러너-트러블슈팅">자체 호스팅 러너 트러블슈팅</h3>
<p>깃허브와 셀프 호스팅 러너가 통신할 수 없는 경우, 잡이 예약되지  않고 러너를 기다리는 것처럼 보인다. 러너 연결 문제나 작업 실행 오류가 발생할 경우 검사 옵션을 사용하면 합격 및 불합격 검사 결과가 모두 출력된다.:</p>
<pre><code>./run.sh --check --url https://github.com/brentlaster/greetings-actions --pat ghp_**</code></pre><ul>
<li>URL: 사용할 깃허브 리포지터리의 URL</li>
<li><a href="https://github.com/settings/tokens">개인 엑세스 토큰(PAT)</a>: 개발자 설정을 통해 생성된 토큰. 워크플로 스코프가 필요하다.</li>
</ul>
<hr />
<h3 id="자체-호스팅-러너-제거">자체 호스팅 러너 제거</h3>
<p>자체 호스팅 러너를 제거하는 방법은 요구 사항과 엑세스 권한에 따라 방법이 다르다. 
일반적으로 더 이상 필요하지 않은 러너는 다음 단계로 제거할 수 있다:</p>
<ol>
<li>GitHub 저장소 또는 조직 설정에서 &quot;Actions&quot; 메뉴 선택</li>
<li>&quot;Runners&quot; 섹션에서 제거할 러너 선택</li>
<li>&quot;Remove&quot; 버튼 클릭</li>
<li>엑세스 권한이 있는 경우 제거 절차에 대한 지침 따르기</li>
</ol>
<hr />
<h2 id="자체-호스팅된-러너-오토스케일링">자체 호스팅된 러너 오토스케일링</h2>
<p>워크로드에 따라 자동으로 러너를 확장하고 축소하는 기능으로, 필요할 때만 리소스를 사용할 수 있게 해준다.</p>
<hr />
<h2 id="저스트-인-타임-러너">저스트 인 타임 러너</h2>
<p>작업이 필요할 때만 생성되고 작업이 완료되면 자동으로 제거되는 임시 러너로, 보안과 리소스 효율성을 높일 수 있다.</p>