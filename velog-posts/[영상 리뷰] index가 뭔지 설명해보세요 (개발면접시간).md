<p><a href="https://www.youtube.com/watch?v=iNvYsGKelYs&amp;ab_channel=%EC%BD%94%EB%94%A9%EC%95%A0%ED%94%8C"><img alt="movie" src="https://img.youtube.com/vi/iNvYsGKelYs/sddefault.jpg" /></a></p>
<h2 id="인덱스">인덱스</h2>
<ul>
<li>데이터베이스 테이블의 특정 컬럼을 복사하여 정렬해 둔 것.</li>
<li>데이터 검색 속도를 크게 향상시키는 것이 주요 목적.</li>
<li>이진 탐색 트리(Binary Search Tree) 구조를 기반으로 함.</li>
</ul>
<hr />
<h2 id="인덱스의-종류">인덱스의 종류</h2>
<ol>
<li><p><strong>B-트리 (B-Tree)</strong></p>
<ul>
<li>노드마다 여러 개의 데이터를 저장할 수 있어 효율적.</li>
</ul>
</li>
<li><p><strong>B+트리 (B+ Tree)</strong></p>
<ul>
<li>데이터는 리프 노드에만 저장하고, 내부 노드는 탐색 가이드만 제공.</li>
<li>범위 검색에 특히 유리함.</li>
</ul>
</li>
</ol>
<hr />
<h2 id="인덱스의-장단점">인덱스의 장단점</h2>
<p><strong>장점:</strong></p>
<ul>
<li>데이터 검색 속도가 크게 향상됨.</li>
<li>대량의 데이터에서도 빠른 검색이 가능함.</li>
</ul>
<p><strong>단점:</strong></p>
<ul>
<li>추가적인 저장 공간이 필요함.</li>
<li>데이터 변경 시 인덱스도 함께 업데이트해야 하므로 성능 저하가 있을 수 있음.</li>
</ul>
<hr />
<h2 id="주의사항">주의사항</h2>
<ul>
<li>검색 작업을 안 하는 칼럼은 인덱스를 만들 필요 없음.</li>
<li>프라이머리 키는 자동으로 정렬되어 있어 별도의 인덱스가 필요 없음.</li>
</ul>
<p>-&gt; 인덱스는 데이터베이스 성능 최적화에 중요한 역할을 하지만, 적절히 사용해야 최대의 효과를 얻을 수 있음.</p>