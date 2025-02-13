<h2 id="erdentity-relationship-diagram">ERD(Entity-Relationship Diagram)</h2>
<p><strong>ERD</strong>는 데이터베이스 설계를 시각화하는 도구입니다. 시스템 내 데이터 구조를 나타내며, <code>엔티티(Entity), 속성(Attribute), 관계(Relationship)</code> 간의 연관성을 표현한다. <strong>ERD</strong>는 <code>One-to-One(한 Entity가 다른 Entity에 고유하게 매칭) / One to Many(한 Entity가 여러 Entity에 연관) / Many to Many(여러 Entity가 여러 Entity에 연관)</code></p>
<blockquote>
<p><strong>ERD의 구성 요소</strong>
<strong>Entity (엔티티)</strong>
정의: 관리하려는 데이터의 객체를 나타낸다.
예: 사용자(User), 동영상(Video), 좋아요(Like).</p>
</blockquote>
<p><strong>Attribute (속성)</strong>
정의: 엔티티가 가진 특성을 나타낸다.
예: 사용자 엔티티의 속성 → 이름, 이메일, 비밀번호.</p>
<blockquote>
</blockquote>
<p><strong>Relationship (관계)</strong>
정의: 엔티티 간의 연관성을 나타낸다.
예: 사용자가 동영상을 &quot;좋아요&quot; → User와 Video 간의 관계.
표현: 다이아몬드형으로 표시.</p>
<h2 id="정규화normalization">정규화(Normalization)</h2>
<p><strong>정규화(Normalization)</strong>는 데이터베이스 설계 과정에서 데이터 중복을 최소화하고, 데이터 무결성을 유지하며, 효율적인 데이터 구조를 만들기 위해 데이터를 여러 테이블로 분리하는 방법이다. 정규화는 <strong>중복된 데이터를 제거하고 데이터 무결성을 보장하며 효율적인 데이터를 관리</strong>하면서 데이터 베이스 향상시킨다.</p>
<h3 id="1nf">1NF</h3>
<p>모든 속성의 값이 <strong>원자값(Atomic Value)</strong>여야한다. 즉 각 <strong>column</strong>에는 하나의 값만 저장되어야만 한다.</p>
<p><strong>비정규화</strong>
<strong>다가 속성(Multivalued Attributed)</strong> 하나의 엔티티(Entity)나 관계(Relationship)가 여러 개의 값을 가질 수 있는 속성을 말한다. 즉 한 <strong>Column</strong>에 여러 데이터가 존재하는 것을 의미한다. 이는 <strong>1NF</strong>의 위배된다. </p>
<table>
<thead>
<tr>
<th>주문번호</th>
<th>날짜</th>
<th>이름</th>
<th>나이</th>
<th>주문 상품</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>10/23</td>
<td>철수</td>
<td>13</td>
<td>A001 레고 1개, A002 미니카 2개</td>
</tr>
<tr>
<td>P002</td>
<td>08/13</td>
<td>영희</td>
<td>15</td>
<td>A003 인형 1개, A001 레고 2개</td>
</tr>
<tr>
<td>P003</td>
<td>09/07</td>
<td>훈이</td>
<td>12</td>
<td>A003 인형 2개, A002 미니카 1개</td>
</tr>
</tbody></table>
<hr />
<p>다가 속성을 해결하기 위해 여러 데이터를 각각 <strong>row</strong>으로 나누었다. </p>
<table>
<thead>
<tr>
<th>주문번호</th>
<th>날짜</th>
<th>이름</th>
<th>나이</th>
<th>주문 상품</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>10/23</td>
<td>철수</td>
<td>13</td>
<td>A001 레고 1개</td>
</tr>
<tr>
<td>P001</td>
<td>10/23</td>
<td>철수</td>
<td>13</td>
<td>A002 미니카 2개</td>
</tr>
<tr>
<td>P002</td>
<td>08/13</td>
<td>영희</td>
<td>15</td>
<td>A003 인형 1개</td>
</tr>
<tr>
<td>P002</td>
<td>08/13</td>
<td>영희</td>
<td>15</td>
<td>A001 레고 2개</td>
</tr>
<tr>
<td>P003</td>
<td>09/07</td>
<td>훈이</td>
<td>12</td>
<td>A003 인형 2개</td>
</tr>
<tr>
<td>P003</td>
<td>09/07</td>
<td>훈이</td>
<td>12</td>
<td>A002 미니카 1개</td>
</tr>
</tbody></table>
<p><strong>복합 속성(Composite Attrubute)</strong> - 여러 하위 속성으로 구성된 속성을 말한다. 하나의 속성이 여러 구성 요소를 포함한다는 뜻이다. 현재 주문상품 <strong>Column</strong>을 보면 <code>상품 번호(A001), 상품명(레고), 상품수량(1개)</code>이 <strong>주문 상품</strong>에 들어가 있는데 이는 여전히 <strong>1NF</strong>에 위배된다. </p>
<hr />
<table>
<thead>
<tr>
<th>주문번호</th>
<th>날짜</th>
<th>이름</th>
<th>나이</th>
<th>상품번호</th>
<th>상품명</th>
<th>상품수량</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>10/23</td>
<td>철수</td>
<td>13</td>
<td>A001</td>
<td>레고</td>
<td>1개</td>
</tr>
<tr>
<td>P001</td>
<td>10/23</td>
<td>철수</td>
<td>13</td>
<td>A002</td>
<td>미니카</td>
<td>2개</td>
</tr>
<tr>
<td>P002</td>
<td>08/13</td>
<td>영희</td>
<td>15</td>
<td>A003</td>
<td>인형</td>
<td>1개</td>
</tr>
<tr>
<td>P002</td>
<td>08/13</td>
<td>영희</td>
<td>15</td>
<td>A001</td>
<td>레고</td>
<td>2개</td>
</tr>
<tr>
<td>P003</td>
<td>09/07</td>
<td>훈이</td>
<td>12</td>
<td>A003</td>
<td>인형</td>
<td>2개</td>
</tr>
<tr>
<td>P003</td>
<td>09/07</td>
<td>훈이</td>
<td>12</td>
<td>A002</td>
<td>미니카</td>
<td>1개</td>
</tr>
</tbody></table>
<p>계속 분리하다보니 <strong>중복 데이터</strong>가 발생한 사실을 알 수 있다. 만약 이름을 변경할 경우가 발생하면 이는 대규모 공사가 될 것이다. 이를 해결 하기 위해 우리는 테이블을 2개로 분리해야한다.</p>
<hr />
<h4 id="-주문--테이블"><code>[ 주문 ]</code> 테이블</h4>
<table>
<thead>
<tr>
<th>주문번호</th>
<th>날짜</th>
<th>이름</th>
<th>나이</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>10/23</td>
<td>철수</td>
<td>13</td>
</tr>
<tr>
<td>P002</td>
<td>08/13</td>
<td>영희</td>
<td>15</td>
</tr>
<tr>
<td>P003</td>
<td>09/07</td>
<td>훈이</td>
<td>12</td>
</tr>
</tbody></table>
<h4 id="-주문_상품--테이블"><code>[ 주문_상품 ]</code> 테이블</h4>
<table>
<thead>
<tr>
<th>주문번호</th>
<th>상품번호</th>
<th>상품명</th>
<th>상품수량</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>A001</td>
<td>레고</td>
<td>1개</td>
</tr>
<tr>
<td>P001</td>
<td>A002</td>
<td>미니카</td>
<td>2개</td>
</tr>
<tr>
<td>P002</td>
<td>A003</td>
<td>인형</td>
<td>1개</td>
</tr>
<tr>
<td>P002</td>
<td>A001</td>
<td>레고</td>
<td>2개</td>
</tr>
<tr>
<td>P003</td>
<td>A003</td>
<td>인형</td>
<td>2개</td>
</tr>
<tr>
<td>P003</td>
<td>A002</td>
<td>미니카</td>
<td>1개</td>
</tr>
</tbody></table>
<blockquote>
<p><strong>Key</strong>
<strong>Primary Key (PK, 기본 키)</strong> - 테이블 내의 각 행(row)을 고유하게 식별할 수 있는 속성(컬럼) 또는 속성들의 조합으로 중복 불가하며, NULL 값을 허용하지 않으며 <strong>반드시 하나의 PK</strong>만 존재해야한다.</p>
</blockquote>
<p><strong>Candidate Key (CK, 후보 키)</strong> - 테이블에서 <strong>PK</strong>로 선택될 수 있는 모든 속성 또는 속성 조합을 의미한다. <strong>PK</strong>이기 때문에 <strong>고유성(유일하게 식별 가능)과 최소성(불필요한 컬럼 포함하지 않음)</strong>을 만족해야한다. </p>
<blockquote>
</blockquote>
<p><strong>Composite Key (복합 키)</strong> - 두 개 이상의 칼럼을 조합하여 고유성을 보장하는 <strong>기본 키</strong> 단일 칼럼만으로는 고유하지 않지만 컬럼 조합으로 유일하게 식별 가능하다. 주로 <strong>다대다 관계</strong>를 표현하는 중간 테이블에서 사용한다. </p>
<blockquote>
</blockquote>
<p><strong>Foreign Key (FK, 외래 키)</strong> - 다른 테이블의 <strong>PK</strong>를 참조하는 속성으로 두 테이블 간의 관계를 정의하며 참조 무결성을 유지한다. <strong>FK</strong> 다른 테이블의 PK를 참조하여 관계를 형성하며 참조된 값이 삭제되거나 변경될 때, 조건(CASCADE / SET NULL)에 따라 동작한다. 주로 <strong>일대다 관계</strong>에서 사용된다. </p>
<blockquote>
</blockquote>
<p><strong>Unique Key (UK, 유니크 키)</strong> - 테이블에서 <strong>유일성</strong>을 보장하는 속성으로써 <strong>PK</strong>와 다르게 NULL을 허용하며 하나의 테이블에는 여러 가지의 <strong>Unique Key</strong>가 존재할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>Alternate Key (대체 키)</strong> - <strong>후보 키(Candidate Key)</strong> 중에서 <strong>PK</strong>로 선택되지 않은 키로써 PK를 제외한 CK를 <strong>Alternate Key</strong>라고 부른다. </p>
<h3 id="2nf">2NF</h3>
<p>1NF를 만족하면서 <strong>부분 함수 종속성 제거(기본 키의 일부만으로 결정되는 컬럼이 없어야 한다)</strong>인 경우를 제 2 정규화라고 한다. 테이블의 모든 <strong>비기본속성(Non-Prime Attribute - 기본 키에 포함되지 않은 나머지 속성)</strong>이 <strong>PK</strong>의 모든 속성에 완전 함수 종속되어야 한다. 먼저 헷갈릴만한 용어부터 정리를 해보겠다. <strong>부분 함수 종속성</strong>은 <strong>기본 키(PK)</strong>의 일부 속성에만 의존하는 비기본 속성을 의미한다.</p>
<p>예를 들어보겠다 위에 테이블을 기준으로 말하자면 주문번호(PK)와 상품명은 실질적으로 의존하지 않는다. 즉 <strong>상품명은 상품 번호</strong>에만 의존한다는 사실을 알 수 있을 것이다. 즉 <strong>2NF</strong>가 되기 위해서는 <strong>1NF를 만족하면서 비기본 속성이 PK의 전체에 의존하는 경우(완전 함수 종속성)를 의미한다.</strong> 주로 2NF는 <strong>PK가 복합키</strong>로 구성되어 있으며 <strong>PK</strong>의 일부에만 종속된 비기본속성이 있을 때 사용한다. </p>
<h4 id="-주문--테이블-1"><code>[ 주문 ]</code> 테이블</h4>
<table>
<thead>
<tr>
<th>주문번호</th>
<th>날짜</th>
<th>이름</th>
<th>나이</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>10/23</td>
<td>철수</td>
<td>13</td>
</tr>
<tr>
<td>P002</td>
<td>08/13</td>
<td>영희</td>
<td>15</td>
</tr>
<tr>
<td>P003</td>
<td>09/07</td>
<td>훈이</td>
<td>12</td>
</tr>
</tbody></table>
<h4 id="-상품--테이블"><code>[ 상품 ]</code> 테이블</h4>
<table>
<thead>
<tr>
<th>상품번호</th>
<th>상품명</th>
</tr>
</thead>
<tbody><tr>
<td>A001</td>
<td>레고</td>
</tr>
<tr>
<td>A002</td>
<td>미니카</td>
</tr>
<tr>
<td>A003</td>
<td>인형</td>
</tr>
</tbody></table>
<h4 id="-주문_상품--테이블-1"><code>[ 주문_상품 ]</code> 테이블</h4>
<p>실질적으로 <strong>상품 수량</strong>은 <strong>주문번호와 상품번호</strong>에 완전히 종속되어 있다. 하지만 <strong>상품 명</strong> 같은 경우 <strong>상품 번호</strong>만 종속되어 있기 때문에 테이블을 분리해야한다. </p>
<table>
<thead>
<tr>
<th>주문번호</th>
<th>상품번호</th>
<th>상품수량</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>A001</td>
<td>1개</td>
</tr>
<tr>
<td>P001</td>
<td>A002</td>
<td>2개</td>
</tr>
<tr>
<td>P002</td>
<td>A003</td>
<td>1개</td>
</tr>
<tr>
<td>P002</td>
<td>A001</td>
<td>2개</td>
</tr>
<tr>
<td>P003</td>
<td>A003</td>
<td>2개</td>
</tr>
<tr>
<td>P003</td>
<td>A002</td>
<td>1개</td>
</tr>
</tbody></table>
<hr />
<p>이렇게 되면 각 <strong>column</strong>에는 하나의 값만 존재하며, <strong>완전 함수 종속성</strong>을 갖는 테이블 구조가 될 것이다. </p>
<h3 id="3nf">3NF</h3>
<p><strong>2NF</strong>를 만족하면서 <strong>이행성 종속성(Transitive Dependency)</strong>를 제거한 상태를 말한다. <strong>이행성 종속성(Transitive Dependency)</strong>란 비기본 속성(Non-Prime Attribute)이 다른 비기본 속성을 통해 기본 키에 종속되는 경우를 의미한다. </p>
<p>예를 들자면 <strong>주문 테이블</strong>에서 <strong>이름과 나이</strong>는 <strong>PK</strong>인 주문번호를 통해 종속되어 있지만, 사실상 <strong>고객</strong>이라는 데이터에 종속되어 있다. 즉, <strong>이름과 나이</strong>는 고객 정보이며 <strong>주문 정보와 독립적이여야 한다.</strong></p>
<h4 id="-주문--테이블-2">[ 주문 ] 테이블</h4>
<table>
<thead>
<tr>
<th>주문번호</th>
<th>날짜</th>
<th>고객번호</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>10/23</td>
<td>U001</td>
</tr>
<tr>
<td>P002</td>
<td>08/13</td>
<td>U002</td>
</tr>
<tr>
<td>P003</td>
<td>09/07</td>
<td>U003</td>
</tr>
</tbody></table>
<h4 id="-고객--테이블">[ 고객 ] 테이블</h4>
<table>
<thead>
<tr>
<th>고객번호</th>
<th>이름</th>
<th>나이</th>
</tr>
</thead>
<tbody><tr>
<td>U001</td>
<td>철수</td>
<td>13</td>
</tr>
<tr>
<td>U002</td>
<td>영희</td>
<td>15</td>
</tr>
<tr>
<td>U003</td>
<td>훈이</td>
<td>12</td>
</tr>
</tbody></table>
<h4 id="-상품--테이블-1">[ 상품 ] 테이블</h4>
<table>
<thead>
<tr>
<th>상품번호</th>
<th>상품명</th>
</tr>
</thead>
<tbody><tr>
<td>A001</td>
<td>레고</td>
</tr>
<tr>
<td>A002</td>
<td>미니카</td>
</tr>
<tr>
<td>A003</td>
<td>인형</td>
</tr>
</tbody></table>
<h4 id="-주문_상품--테이블-2">[ 주문_상품 ] 테이블</h4>
<table>
<thead>
<tr>
<th>주문번호</th>
<th>상품번호</th>
<th>상품수량</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>A001</td>
<td>1개</td>
</tr>
<tr>
<td>P001</td>
<td>A002</td>
<td>2개</td>
</tr>
<tr>
<td>P002</td>
<td>A003</td>
<td>1개</td>
</tr>
<tr>
<td>P002</td>
<td>A001</td>
<td>2개</td>
</tr>
<tr>
<td>P003</td>
<td>A003</td>
<td>2개</td>
</tr>
<tr>
<td>P003</td>
<td>A002</td>
<td>1개</td>
</tr>
</tbody></table>
<hr />
<h2 id="테이블-정규화-예시">테이블 정규화 예시</h2>
<table>
<thead>
<tr>
<th>표</th>
</tr>
</thead>
<tbody><tr>
<td>상품명</td>
</tr>
<tr>
<td>상품내용</td>
</tr>
<tr>
<td>상품가격</td>
</tr>
<tr>
<td>상품거래주소</td>
</tr>
<tr>
<td>상품거래상세주소</td>
</tr>
<tr>
<td>상품거래위도</td>
</tr>
<tr>
<td>상품거래경도</td>
</tr>
<tr>
<td>상품거래예정시각</td>
</tr>
<tr>
<td>상품판매여부</td>
</tr>
<tr>
<td>카테고리-도서</td>
</tr>
<tr>
<td>카테고리-의류</td>
</tr>
<tr>
<td>카테고리-가전</td>
</tr>
<tr>
<td>카테고리-기타</td>
</tr>
<tr>
<td>상품태그</td>
</tr>
<tr>
<td>상품판매자이름</td>
</tr>
<tr>
<td>상품판매자이메일</td>
</tr>
</tbody></table>
<hr />
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품명</th>
<th>상품내용</th>
<th>상품가격</th>
<th>상품판매 여부</th>
<th>카테고리</th>
<th>상품태그</th>
<th>상품판매자 이름</th>
<th>상품판매자 이메일</th>
<th>상품거래주소</th>
<th>상품거래상세주소</th>
<th>상품거래위도</th>
<th>상품거래경도</th>
<th>상품거래예정시각</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>마우스</td>
<td>좋은 마우스</td>
<td>1000</td>
<td>FALSE</td>
<td>가전</td>
<td>전자제품, 영등포마우스</td>
<td>철수</td>
<td><a href="mailto:chulsoo@naver.com">chulsoo@naver.com</a></td>
<td>영등포</td>
<td>영등포역</td>
<td>10.24</td>
<td>30.1</td>
<td>10시</td>
</tr>
<tr>
<td>P002</td>
<td>키보드</td>
<td>잘쳐지는 키보드</td>
<td>1500</td>
<td>FALSE</td>
<td>가전</td>
<td>굿키보드, 전자제품</td>
<td>영희</td>
<td><a href="mailto:hee@gmail.com">hee@gmail.com</a></td>
<td>잠실</td>
<td>잠실역</td>
<td>10.44</td>
<td>30.21</td>
<td>5시</td>
</tr>
<tr>
<td>P003</td>
<td>데이터베이스</td>
<td>데이터베이스책</td>
<td>2000</td>
<td>FALSE</td>
<td>도서</td>
<td>책</td>
<td>훈이</td>
<td><a href="mailto:hun@naver.com">hun@naver.com</a></td>
<td>신도림</td>
<td>신도림역</td>
<td>10.33</td>
<td>30.66</td>
<td>7시</td>
</tr>
<tr>
<td>P004</td>
<td>운영체제</td>
<td>핵심운영체제</td>
<td>3000</td>
<td>FALSE</td>
<td>도서</td>
<td>책사랑</td>
<td>맹구</td>
<td><a href="mailto:mang@naver.com">mang@naver.com</a></td>
<td>구로</td>
<td>구로역</td>
<td>10.37</td>
<td>30.99</td>
<td>3시</td>
</tr>
<tr>
<td>---</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody></table>
<p>먼저 <strong>1NF 정규화</strong>를 해보겠다. <strong>상품 태그 부분</strong>을 보면 한 <strong>column</strong>에 여러 값이 있는 것을 확인할 수 있다. <strong>1FA</strong>에 위배되므로 </p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품명</th>
<th>상품내용</th>
<th>상품가격</th>
<th>상품판매여부</th>
<th>카테고리</th>
<th>상품태그</th>
<th>상품판매자 이름</th>
<th>상품판매자 이메일</th>
<th>상품거래주소</th>
<th>상품거래상세주소</th>
<th>상품거래위도</th>
<th>상품거래경도</th>
<th>상품거래예정시각</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>마우스</td>
<td>좋은 마우스</td>
<td>1000</td>
<td>FALSE</td>
<td>가전</td>
<td>전자제품</td>
<td>철수</td>
<td><a href="mailto:chulsoo@naver.com">chulsoo@naver.com</a></td>
<td>영등포</td>
<td>영등포역</td>
<td>10.24</td>
<td>30.1</td>
<td>10시</td>
</tr>
<tr>
<td>P001</td>
<td>마우스</td>
<td>좋은 마우스</td>
<td>1000</td>
<td>FALSE</td>
<td>가전</td>
<td>영등포마우스</td>
<td>철수</td>
<td><a href="mailto:chulsoo@naver.com">chulsoo@naver.com</a></td>
<td>영등포</td>
<td>영등포역</td>
<td>10.24</td>
<td>30.1</td>
<td>10시</td>
</tr>
<tr>
<td>P002</td>
<td>키보드</td>
<td>잘쳐지는 키보드</td>
<td>1500</td>
<td>FALSE</td>
<td>가전</td>
<td>굿키보드</td>
<td>영희</td>
<td><a href="mailto:hee@gmail.com">hee@gmail.com</a></td>
<td>잠실</td>
<td>잠실역</td>
<td>10.44</td>
<td>30.21</td>
<td>5시</td>
</tr>
<tr>
<td>P002</td>
<td>키보드</td>
<td>잘쳐지는 키보드</td>
<td>1500</td>
<td>FALSE</td>
<td>가전</td>
<td>전자제품</td>
<td>영희</td>
<td><a href="mailto:hee@gmail.com">hee@gmail.com</a></td>
<td>잠실</td>
<td>잠실역</td>
<td>10.44</td>
<td>30.21</td>
<td>5시</td>
</tr>
<tr>
<td>P003</td>
<td>데이터베이스</td>
<td>데이터베이스책</td>
<td>2000</td>
<td>FALSE</td>
<td>도서</td>
<td>책</td>
<td>훈이</td>
<td><a href="mailto:hun@naver.com">hun@naver.com</a></td>
<td>신도림</td>
<td>신도림역</td>
<td>10.33</td>
<td>30.66</td>
<td>7시</td>
</tr>
<tr>
<td>P004</td>
<td>운영체제</td>
<td>핵심운영체제</td>
<td>3000</td>
<td>FALSE</td>
<td>도서</td>
<td>책사랑</td>
<td>맹구</td>
<td><a href="mailto:mang@naver.com">mang@naver.com</a></td>
<td>구로</td>
<td>구로역</td>
<td>10.37</td>
<td>30.99</td>
<td>3시</td>
</tr>
</tbody></table>
<hr />
<p>한 <strong>column</strong>에는 한 개의 데이터만 존재하도록 <strong>1FA</strong>로 변경하였지만 중복되는 데이터가 있다는 것을 확인할 수 있다. </p>
<p><strong><code>[상품_상품태그] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품태그</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>전자제품</td>
</tr>
<tr>
<td>P001</td>
<td>영등포마우스</td>
</tr>
<tr>
<td>P002</td>
<td>굿키보드</td>
</tr>
<tr>
<td>P002</td>
<td>전자제품</td>
</tr>
<tr>
<td>P003</td>
<td>책</td>
</tr>
<tr>
<td>P004</td>
<td>책사랑</td>
</tr>
<tr>
<td>---</td>
<td></td>
</tr>
</tbody></table>
<p><strong><code>[상품] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품명</th>
<th>상품내용</th>
<th>상품가격</th>
<th>상품판매여부</th>
<th>카테고리</th>
<th>상품판매자이름</th>
<th>상품판매자이메일</th>
<th>상품거래주소</th>
<th>상품거래상세주소</th>
<th>상품거래위도</th>
<th>상품거래경도</th>
<th>상품거래예정시각</th>
</tr>
</thead>
<tbody><tr>
<td><strong>P001</strong></td>
<td>마우스</td>
<td>좋은 마우스</td>
<td>1000</td>
<td>FALSE</td>
<td>가전</td>
<td>철수</td>
<td><a href="mailto:chulsoo@naver.com">chulsoo@naver.com</a></td>
<td>영등포</td>
<td>영등포역</td>
<td>10.24</td>
<td>30.1</td>
<td>10시</td>
</tr>
<tr>
<td><strong>P002</strong></td>
<td>키보드</td>
<td>잘쳐지는 키보드</td>
<td>1500</td>
<td>FALSE</td>
<td>가전</td>
<td>영희</td>
<td><a href="mailto:hee@gmail.com">hee@gmail.com</a></td>
<td>잠실</td>
<td>잠실역</td>
<td>10.44</td>
<td>30.21</td>
<td>5시</td>
</tr>
<tr>
<td><strong>P003</strong></td>
<td>데이터베이스</td>
<td>데이터베이스책</td>
<td>2000</td>
<td>FALSE</td>
<td>도서</td>
<td>훈이</td>
<td><a href="mailto:hun@naver.com">hun@naver.com</a></td>
<td>신도림</td>
<td>신도림역</td>
<td>10.33</td>
<td>30.66</td>
<td>7시</td>
</tr>
<tr>
<td><strong>P004</strong></td>
<td>운영체제</td>
<td>핵심운영체제</td>
<td>3000</td>
<td>FALSE</td>
<td>도서</td>
<td>맹구</td>
<td><a href="mailto:mang@naver.com">mang@naver.com</a></td>
<td>구로</td>
<td>구로역</td>
<td>10.37</td>
<td>30.99</td>
<td>3시</td>
</tr>
</tbody></table>
<hr />
<p>이렇게 되면 <strong>상품 테이블</strong>의 중복은 없어졌지만, <strong>상품 태그 테이블</strong> 같은 경우 <strong>전자제품</strong>이 중복되었음을 알 수 있다. </p>
<p>** <code>[상품태그] 테이블</code>**</p>
<table>
<thead>
<tr>
<th>상품태그ID</th>
<th>상품태그</th>
</tr>
</thead>
<tbody><tr>
<td>T001</td>
<td>전자제품</td>
</tr>
<tr>
<td>T002</td>
<td>영등포마우스</td>
</tr>
<tr>
<td>T003</td>
<td>굿키보드</td>
</tr>
<tr>
<td>T004</td>
<td>책</td>
</tr>
<tr>
<td>T005</td>
<td>책사랑</td>
</tr>
</tbody></table>
<p><strong><code>[상품_상품태그]</code> 테이블</strong></p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품태그ID</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>T001</td>
</tr>
<tr>
<td>P001</td>
<td>T002</td>
</tr>
<tr>
<td>P002</td>
<td>T003</td>
</tr>
<tr>
<td>P002</td>
<td>T001</td>
</tr>
<tr>
<td>P003</td>
<td>T004</td>
</tr>
<tr>
<td>P004</td>
<td>T005</td>
</tr>
</tbody></table>
<p><strong><code>[상품]</code> 테이블</strong></p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품명</th>
<th>상품내용</th>
<th>상품가격</th>
<th>상품판매여부</th>
<th>카테고리</th>
<th>상품판매자이름</th>
<th>상품판매자이메일</th>
<th>상품거래주소</th>
<th>상품거래상세주소</th>
<th>상품거래위도</th>
<th>상품거래경도</th>
<th>상품거래예정시각</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>마우스</td>
<td>좋은 마우스</td>
<td>1000</td>
<td>FALSE</td>
<td>가전</td>
<td>철수</td>
<td><a href="mailto:chulsoo@naver.com">chulsoo@naver.com</a></td>
<td>영등포</td>
<td>영등포역</td>
<td>10.24</td>
<td>30.1</td>
<td>10시</td>
</tr>
<tr>
<td>P002</td>
<td>키보드</td>
<td>잘쳐지는 키보드</td>
<td>1500</td>
<td>FALSE</td>
<td>가전</td>
<td>영희</td>
<td><a href="mailto:hee@gmail.com">hee@gmail.com</a></td>
<td>잠실</td>
<td>잠실역</td>
<td>10.44</td>
<td>30.21</td>
<td>5시</td>
</tr>
<tr>
<td>P003</td>
<td>데이터베이스</td>
<td>데이터베이스책</td>
<td>2000</td>
<td>FALSE</td>
<td>도서</td>
<td>훈이</td>
<td><a href="mailto:hun@naver.com">hun@naver.com</a></td>
<td>신도림</td>
<td>신도림역</td>
<td>10.33</td>
<td>30.66</td>
<td>7시</td>
</tr>
<tr>
<td>P004</td>
<td>운영체제</td>
<td>핵심운영체제</td>
<td>3000</td>
<td>FALSE</td>
<td>도서</td>
<td>맹구</td>
<td><a href="mailto:mang@naver.com">mang@naver.com</a></td>
<td>구로</td>
<td>구로역</td>
<td>10.37</td>
<td>30.99</td>
<td>3시</td>
</tr>
</tbody></table>
<hr />
<p>이제 다음은 <strong>상품 테이블에서의 카테고리</strong>이다. 가전과 도서가 중복되어 있다는 사실을 알 수 있다. </p>
<p><strong><code>[ 카테고리 ] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품카테고리ID</th>
<th>카테고리</th>
</tr>
</thead>
<tbody><tr>
<td><strong>C001</strong></td>
<td>가전</td>
</tr>
<tr>
<td><strong>C002</strong></td>
<td>도서</td>
</tr>
</tbody></table>
<p><strong><code>[ 상품 ]</code> 테이블</strong></p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품명</th>
<th>상품내용</th>
<th>상품가격</th>
<th>상품판매여부</th>
<th>상품카테고리ID</th>
<th>상품판매자이름</th>
<th>상품판매자이메일</th>
<th>상품거래주소</th>
<th>상품거래상세주소</th>
<th>상품거래위도</th>
<th>상품거래경도</th>
<th>상품거래예정시각</th>
</tr>
</thead>
<tbody><tr>
<td>P001</td>
<td>마우스</td>
<td>좋은 마우스</td>
<td>1000</td>
<td>FALSE</td>
<td>C001</td>
<td>철수</td>
<td><a href="mailto:chulsoo@naver.com">chulsoo@naver.com</a></td>
<td>영등포</td>
<td>영등포역</td>
<td>10.24</td>
<td>30.1</td>
<td>10시</td>
</tr>
<tr>
<td>P002</td>
<td>키보드</td>
<td>잘쳐지는 키보드</td>
<td>1500</td>
<td>FALSE</td>
<td>C001</td>
<td>영희</td>
<td><a href="mailto:hee@gmail.com">hee@gmail.com</a></td>
<td>잠실</td>
<td>잠실역</td>
<td>10.44</td>
<td>30.21</td>
<td>5시</td>
</tr>
<tr>
<td>P003</td>
<td>데이터베이스</td>
<td>데이터베이스책</td>
<td>2000</td>
<td>FALSE</td>
<td>C002</td>
<td>훈이</td>
<td><a href="mailto:hun@naver.com">hun@naver.com</a></td>
<td>신도림</td>
<td>신도림역</td>
<td>10.33</td>
<td>30.66</td>
<td>7시</td>
</tr>
<tr>
<td>P004</td>
<td>운영체제</td>
<td>핵심운영체제</td>
<td>3000</td>
<td>FALSE</td>
<td>C002</td>
<td>맹구</td>
<td><a href="mailto:mang@naver.com">mang@naver.com</a></td>
<td>구로</td>
<td>구로역</td>
<td>10.37</td>
<td>30.99</td>
<td>3시</td>
</tr>
</tbody></table>
<hr />
<p>여기까지가 <strong>2NF</strong> 정규화를 해결한 것이다. 
다음은 <strong>3NF</strong> 정규화이다. 먼저 <strong>상품 테이블</strong>을 확인하면 <strong>상품 판매자 이름과 상품 판매자 이메일</strong>은 현재 <strong>PK</strong>와 이행성 종속성이 있는 상태이다.</p>
<p><strong><code>[상품판매자] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품판매자ID</th>
<th>상품판매자이름</th>
<th>상품판매자이메일</th>
</tr>
</thead>
<tbody><tr>
<td><strong>U001</strong></td>
<td>철수</td>
<td><a href="mailto:chulsoo@naver.com">chulsoo@naver.com</a></td>
</tr>
<tr>
<td><strong>U002</strong></td>
<td>영희</td>
<td><a href="mailto:hee@gmail.com">hee@gmail.com</a></td>
</tr>
<tr>
<td><strong>U003</strong></td>
<td>훈이</td>
<td><a href="mailto:hun@naver.com">hun@naver.com</a></td>
</tr>
<tr>
<td><strong>U004</strong></td>
<td>맹구</td>
<td><a href="mailto:mang@naver.com">mang@naver.com</a></td>
</tr>
</tbody></table>
<p><strong><code>[ 상품 ] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품명</th>
<th>상품내용</th>
<th>상품가격</th>
<th>상품판매여부</th>
<th>상품카테고리ID</th>
<th>상품판매자ID</th>
<th>상품거래주소</th>
<th>상품거래상세주소</th>
<th>상품거래위도</th>
<th>상품거래경도</th>
<th>상품거래예정시각</th>
</tr>
</thead>
<tbody><tr>
<td><strong>P001</strong></td>
<td>마우스</td>
<td>좋은 마우스</td>
<td>1000</td>
<td>FALSE</td>
<td><strong>C001</strong></td>
<td><strong>U001</strong></td>
<td>영등포</td>
<td>영등포역</td>
<td>10.24</td>
<td>30.1</td>
<td>10시</td>
</tr>
<tr>
<td><strong>P002</strong></td>
<td>키보드</td>
<td>잘쳐지는 키보드</td>
<td>1500</td>
<td>FALSE</td>
<td><strong>C001</strong></td>
<td><strong>U002</strong></td>
<td>잠실</td>
<td>잠실역</td>
<td>10.44</td>
<td>30.21</td>
<td>5시</td>
</tr>
<tr>
<td><strong>P003</strong></td>
<td>데이터베이스</td>
<td>데이터베이스책</td>
<td>2000</td>
<td>FALSE</td>
<td><strong>C002</strong></td>
<td><strong>U003</strong></td>
<td>신도림</td>
<td>신도림역</td>
<td>10.33</td>
<td>30.66</td>
<td>7시</td>
</tr>
<tr>
<td><strong>P004</strong></td>
<td>운영체제</td>
<td>핵심운영체제</td>
<td>3000</td>
<td>FALSE</td>
<td><strong>C002</strong></td>
<td><strong>U004</strong></td>
<td>구로</td>
<td>구로역</td>
<td>10.37</td>
<td>30.99</td>
<td>3시</td>
</tr>
</tbody></table>
<hr />
<p>또한 <strong>상품 거래 관련 테이블</strong> 또한 이행성 종속성이 있는 상태이기 때문에 분리해주는 게 좋다. </p>
<p><strong><code>[상품거래위치] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품거래위치ID</th>
<th>상품거래주소</th>
<th>상품거래상세주소</th>
<th>상품거래위도</th>
<th>상품거래경도</th>
<th>상품거래예정시각</th>
</tr>
</thead>
<tbody><tr>
<td><strong>L001</strong></td>
<td>영등포</td>
<td>영등포역</td>
<td>10.24</td>
<td>30.1</td>
<td>10시</td>
</tr>
<tr>
<td><strong>L002</strong></td>
<td>잠실</td>
<td>잠실역</td>
<td>10.44</td>
<td>30.21</td>
<td>5시</td>
</tr>
<tr>
<td><strong>L003</strong></td>
<td>신도림</td>
<td>신도림역</td>
<td>10.33</td>
<td>30.66</td>
<td>7시</td>
</tr>
<tr>
<td><strong>L004</strong></td>
<td>구로</td>
<td>구로역</td>
<td>10.37</td>
<td>30.99</td>
<td>3시</td>
</tr>
</tbody></table>
<p><strong><code>[ 상품 ] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품명</th>
<th>상품내용</th>
<th>상품가격</th>
<th>상품판매여부</th>
<th>상품카테고리ID</th>
<th>상품판매자ID</th>
<th>상품거래위치ID</th>
</tr>
</thead>
<tbody><tr>
<td><strong>P001</strong></td>
<td>마우스</td>
<td>좋은 마우스</td>
<td>1000</td>
<td>FALSE</td>
<td><strong>C001</strong></td>
<td><strong>U001</strong></td>
<td><strong>L001</strong></td>
</tr>
<tr>
<td><strong>P002</strong></td>
<td>키보드</td>
<td>잘쳐지는 키보드</td>
<td>1500</td>
<td>FALSE</td>
<td><strong>C001</strong></td>
<td><strong>U002</strong></td>
<td><strong>L002</strong></td>
</tr>
<tr>
<td><strong>P003</strong></td>
<td>데이터베이스</td>
<td>데이터베이스책</td>
<td>2000</td>
<td>FALSE</td>
<td><strong>C002</strong></td>
<td><strong>U003</strong></td>
<td><strong>L003</strong></td>
</tr>
<tr>
<td><strong>P004</strong></td>
<td>운영체제</td>
<td>핵심운영체제</td>
<td>3000</td>
<td>FALSE</td>
<td><strong>C002</strong></td>
<td><strong>U004</strong></td>
<td><strong>L004</strong></td>
</tr>
</tbody></table>
<hr />
<h2 id="최종-정리">최종 정리</h2>
<p><strong><code>[ 상품 ] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품명</th>
<th>상품내용</th>
<th>상품가격</th>
<th>상품판매여부</th>
<th>상품카테고리ID</th>
<th>상품판매자ID</th>
<th>상품거래위치ID</th>
</tr>
</thead>
<tbody><tr>
<td><strong>P001</strong></td>
<td>마우스</td>
<td>좋은 마우스</td>
<td>1000</td>
<td>FALSE</td>
<td><strong>C001</strong></td>
<td><strong>U001</strong></td>
<td><strong>L001</strong></td>
</tr>
<tr>
<td><strong>P002</strong></td>
<td>키보드</td>
<td>잘쳐지는 키보드</td>
<td>1500</td>
<td>FALSE</td>
<td><strong>C001</strong></td>
<td><strong>U002</strong></td>
<td><strong>L002</strong></td>
</tr>
<tr>
<td><strong>P003</strong></td>
<td>데이터베이스</td>
<td>데이터베이스책</td>
<td>2000</td>
<td>FALSE</td>
<td><strong>C002</strong></td>
<td><strong>U003</strong></td>
<td><strong>L003</strong></td>
</tr>
<tr>
<td><strong>P004</strong></td>
<td>운영체제</td>
<td>핵심운영체제</td>
<td>3000</td>
<td>FALSE</td>
<td><strong>C002</strong></td>
<td><strong>U004</strong></td>
<td><strong>L004</strong></td>
</tr>
</tbody></table>
<p><strong><code>[ 상품거래위치 ] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품거래위치ID</th>
<th>상품거래주소</th>
<th>상품거래상세주소</th>
<th>상품거래위도</th>
<th>상품거래경도</th>
<th>상품거래예정시각</th>
</tr>
</thead>
<tbody><tr>
<td><strong>L001</strong></td>
<td>영등포</td>
<td>영등포역</td>
<td>10.24</td>
<td>30.1</td>
<td>10시</td>
</tr>
<tr>
<td><strong>L002</strong></td>
<td>잠실</td>
<td>잠실역</td>
<td>10.44</td>
<td>30.21</td>
<td>5시</td>
</tr>
<tr>
<td><strong>L003</strong></td>
<td>신도림</td>
<td>신도림역</td>
<td>10.33</td>
<td>30.66</td>
<td>7시</td>
</tr>
<tr>
<td><strong>L004</strong></td>
<td>구로</td>
<td>구로역</td>
<td>10.37</td>
<td>30.99</td>
<td>3시</td>
</tr>
</tbody></table>
<p><strong><code>[ 카테고리 ] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품카테고리ID</th>
<th>카테고리</th>
</tr>
</thead>
<tbody><tr>
<td><strong>C001</strong></td>
<td>가전</td>
</tr>
<tr>
<td><strong>C002</strong></td>
<td>도서</td>
</tr>
</tbody></table>
<p><strong><code>[ 상품판매자 ] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품판매자ID</th>
<th>상품판매자이름</th>
<th>상품판매자이메일</th>
</tr>
</thead>
<tbody><tr>
<td><strong>U001</strong></td>
<td>철수</td>
<td><a href="mailto:chulsoo@naver.com">chulsoo@naver.com</a></td>
</tr>
<tr>
<td><strong>U002</strong></td>
<td>영희</td>
<td><a href="mailto:hee@gmail.com">hee@gmail.com</a></td>
</tr>
<tr>
<td><strong>U003</strong></td>
<td>훈이</td>
<td><a href="mailto:hun@naver.com">hun@naver.com</a></td>
</tr>
<tr>
<td><strong>U004</strong></td>
<td>맹구</td>
<td><a href="mailto:mang@naver.com">mang@naver.com</a></td>
</tr>
</tbody></table>
<p><strong><code>[ 상품태그 ] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품 태그ID</th>
<th>상품 태그</th>
</tr>
</thead>
<tbody><tr>
<td><strong>T001</strong></td>
<td>전자제품</td>
</tr>
<tr>
<td><strong>T002</strong></td>
<td>영등포마우스</td>
</tr>
<tr>
<td><strong>T003</strong></td>
<td>굿키보드</td>
</tr>
<tr>
<td><strong>T004</strong></td>
<td>책</td>
</tr>
<tr>
<td><strong>T005</strong></td>
<td>책사랑</td>
</tr>
</tbody></table>
<p><strong><code>[ 상품_상품태그 ] 테이블</code></strong></p>
<table>
<thead>
<tr>
<th>상품ID</th>
<th>상품 태그ID</th>
</tr>
</thead>
<tbody><tr>
<td><strong>P001</strong></td>
<td><strong>T001</strong></td>
</tr>
<tr>
<td><strong>P001</strong></td>
<td><strong>T002</strong></td>
</tr>
<tr>
<td><strong>P002</strong></td>
<td><strong>T003</strong></td>
</tr>
<tr>
<td><strong>P002</strong></td>
<td><strong>T001</strong></td>
</tr>
<tr>
<td><strong>P003</strong></td>
<td><strong>T004</strong></td>
</tr>
<tr>
<td><strong>P004</strong></td>
<td><strong>T005</strong></td>
</tr>
</tbody></table>
<hr />
<h2 id="erd">ERD</h2>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/8c418f36-f693-4876-b459-c58c4f753c79/image.png" /></p>
<h3 id="일대일-관계-11-one-to-one">일대일 관계 (1:1 One to One)</h3>
<p>한 테이블의 한 행이 다른 테이블의 정확히 하나의 행과 연관되는 관계를 <strong>일대일 관계</strong>라고 한다. 두 테이블 간에 데이터가 1:1로 매핑되고, <strong>외래 키(Foreign Key)</strong>를 사용하여 두 테이블을 연결하며 보통 테이블을 분리해야 하는 논리적 이유가 있을 때 사용한다. 현재 데이터베이스 설계로 <strong>한 상품에는 하나의 상품 거래 위치</strong>라고 설계했기 때문에 <strong>상품 테이블과 상품거래위치 테이블</strong>은 일대일 관계이다. </p>
<h3 id="일대다-관계1n-one-to-many">일대다 관계(1:N, One to Many)</h3>
<p>한 테이블의 한 행이 다른 테이블의 여러 행과 연관되는 관계를 <strong>일대다 관계</strong>라고 한다. 주로 <strong>부모-자식</strong> 관계를 표현할 때 사용하며 부모 테이블의 <strong>기본 키(PK)</strong>가 자식 테이블의 <strong>외래 키(FK)</strong>로 사용된다. <strong>카테고리 테이블 - 상품 테이블 / 상품판매자 테이블 - 상품 테이블</strong>은 일대다 관계이다.</p>
<h3 id="다대다-관계nn-many-to-many">다대다 관계(N:N Many to Many)</h3>
<p>한 테이블의 여러 행이 다른 테이블의 여러 행과 연관되는 관계를 <strong>다대다 관계</strong>라고 한다. 두 테이블 간의 관계를 표현하기 위해 <strong>중간 테이블이 필요</strong>하며 중간 테이블은 두 테이블의 <strong>PK</strong>를 <strong>FK</strong>로 가진다. 현재 <strong>상품 테이블 - 상품 태그 테이블</strong>을 보면 상품에는 여러 개의 상품 태그를 달 수 있고, 하나의 태그의 여러개의 상품이 사용될 수 있기 때문에 <strong>상품 테이블 - 상품 태그 테이블</strong>은 다대다 관계이다. </p>