<h2 id="ddldata-definition-language---데이터-정의어">DDL(Data Definition Language) - 데이터 정의어</h2>
<ul>
<li>DB의 구조를 정의하는 데 사용되는 언어</li>
<li>DB의 전체적인 골격을 구성하는 역할을 함</li>
</ul>
<table>
<thead>
<tr>
<th>종류</th>
<th>역할</th>
</tr>
</thead>
<tbody><tr>
<td>CREATE</td>
<td>새로운 데이터베이스, 테이블 생성</td>
</tr>
<tr>
<td>ALTER</td>
<td>기존 테이블 구조 변경</td>
</tr>
<tr>
<td>DROP</td>
<td>데이터베이스, 테이블 삭제</td>
</tr>
<tr>
<td>TRUNCATE</td>
<td>테이블 초기화</td>
</tr>
</tbody></table>
<hr />
<h2 id="dmldata-manipulation-language---데이터-조작어">DML(Data Manipulation Language) - 데이터 조작어</h2>
<ul>
<li>DB 내의 데이터를 조작하는 데 사용되는 언어</li>
<li>데이터를 조회, 삽입, 수정, 삭제하는 작업을 수행</li>
</ul>
<table>
<thead>
<tr>
<th>종류</th>
<th>역할</th>
</tr>
</thead>
<tbody><tr>
<td>SELECT</td>
<td>데이터를 조회하거나 검색</td>
</tr>
<tr>
<td>INSERT</td>
<td>테이블에 새로운 데이터를 삽입</td>
</tr>
<tr>
<td>UPDATE</td>
<td>기존 데이터를 수정</td>
</tr>
<tr>
<td>DELETE</td>
<td>저장된 데이터 삭제</td>
</tr>
</tbody></table>
<hr />
<h2 id="dcldata-control-language---데이터-제어-언어">DCL(Data Control Language) - 데이터 제어 언어</h2>
<ul>
<li>DB에 대한 접근 권한과 보안을 관리하는 데 사용되는 언어</li>
<li>데이터의 보안, 무결성, 회복, 병행 수행 제어 등을 정의하는 데 사용</li>
</ul>
<table>
<thead>
<tr>
<th>종류</th>
<th>역할</th>
</tr>
</thead>
<tbody><tr>
<td>GRANT</td>
<td>사용자에게 특정 권한을 부여</td>
</tr>
<tr>
<td>REVOKE</td>
<td>사용자로부터 특정 권한을 회수</td>
</tr>
<tr>
<td>COMMIT</td>
<td>트랜잭션의 변경사항을 데이터베이스에 영구적으로 반영</td>
</tr>
<tr>
<td>ROLLBACK</td>
<td>트랜잭션의 변경사항을 취소하고 이전 상태로 복구</td>
</tr>
</tbody></table>