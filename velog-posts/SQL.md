<h2 id="sql">SQL</h2>
<p><strong>SQL(Structured Query Language)</strong>는 관계형 데이터베이스(<strong>RDBMS</strong>)를 다루는데 사용되는 통합 언어이다. <code>INSERT, SELECT, UPDATE, DELETE</code>  키워드를 통해 <strong>CRUD</strong>를 한다. 데이터베이스 스키마를 변경할 수 있는 <strong>DDL(Data Definition Language(데이터 정의 언어))</strong>를 포함한다. <code>BEGIN, COMMIT, ROOLBACK</code> 키워드를 통한 <strong>ACID(Atomicity - 원자성, Consistency - 일관성, Isolation - 독립성, Durability - 지속성) Transaction</strong>을 지원한다.</p>
<h3 id="sql-주요-분류">SQL 주요 분류</h3>
<h4 id="ddldata-definition-language---데이터-정의-언어">DDL(Data Definition Language) - 데이터 정의 언어</h4>
<p><strong>DDL</strong>은 데이터베이스의 구조(테이블, 인덱스, 뷰, 스키마)를 정의하거나 수정할 때 사용한다. 데이터 자체가 아니라 <strong>데이터를 저장하는 구조</strong>를 관리하고 명령 실행 시 자동으로 <strong>COMMIT</strong>이 수행되어 반경 사항이 즉시 반영된다. </p>
<blockquote>
<p><strong>DDL 명령어</strong></p>
</blockquote>
<p><strong>CREATE</strong> : 테이블, 데이터베이스 등을 새로 생성</p>
<pre><code class="language-sql">CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);</code></pre>
<blockquote>
</blockquote>
<p><strong>ALTER</strong> : 기존 객체의 구조 수정</p>
<pre><code class="language-sql">ALTER TABLE users ADD COLUMN email VARCHAR(100);</code></pre>
<blockquote>
</blockquote>
<p><strong>DROP</strong> : 데이터베이스 객체 삭제</p>
<pre><code class="language-sql">DROP TABLE users;</code></pre>
<blockquote>
</blockquote>
<p><strong>TRUNCATE</strong>: 테이블의 모든 데이터 삭제(구조는 유지).</p>
<pre><code class="language-sql">TRUNCATE TABLE users;</code></pre>
<h4 id="dmldata-manipulation-language---데이터-조작-언어">DML(Data Manipulation Language) - 데이터 조작 언어</h4>
<p><strong>DML</strong>은 데이터 베이스 내 데이터를 <code>삽입, 조회, 수정, 삭제</code> 할 때 사용한다.  주로 데이터를 <strong>조작하거나 변경</strong>할 때 사용하며 트랙잭션의 영향을 받으며 <strong>COMMIT / ROLLBACK</strong>으로 변경 내용을 적용하거나 취소가 가능하다. </p>
<blockquote>
<p><strong>DML 명령어</strong></p>
</blockquote>
<p><strong>SELECT</strong>: 데이터를 조회.</p>
<pre><code class="language-sql">SELECT * FROM users WHERE id = 1;</code></pre>
<blockquote>
</blockquote>
<p><strong>INSERT</strong>: 데이터를 삽입.</p>
<pre><code class="language-sql">INSERT INTO users (id, name) VALUES (1, 'Alice');</code></pre>
<blockquote>
</blockquote>
<p><strong>UPDATE</strong>: 기존 데이터를 수정.</p>
<pre><code class="language-sql">UPDATE users SET name = 'Bob' WHERE id = 1;</code></pre>
<blockquote>
</blockquote>
<p><strong>DELETE</strong>: 데이터를 삭제.</p>
<pre><code class="language-sql">DELETE FROM users WHERE id = 1;</code></pre>
<h4 id="dcldata-control-language---데이터-제어-언어">DCL(Data Control Language) - 데이터 제어 언어</h4>
<p><strong>DCL</strong>은 데이터베이스에 대한 사용자 권한을 부여하거나 취소할 때 사용한다. 주로 <strong>보안과 접근 제어</strong>를 담당하며 데이터베이스 사용 권한을 관리한다. </p>
<blockquote>
<p><strong>DCL 주요 명령어</strong></p>
</blockquote>
<p><strong>GRANT</strong>: 특정 사용자에게 권한 부여.</p>
<pre><code class="language-sql">GRANT SELECT, INSERT ON users TO 'username';</code></pre>
<blockquote>
</blockquote>
<p><strong>REVOKE</strong>: 특정 사용자의 권한 취소.</p>
<pre><code class="language-sql">REVOKE INSERT ON users FROM 'username';</code></pre>
<h4 id="tcltransaction-control-language---트랜잭션-제어-언어">TCL(Transaction Control Language) - 트랜잭션 제어 언어</h4>
<p><strong>TCL</strong>은 데이터베이스에서 트랜잭션의 실행을 관리할 때 사용한다. 주로 여러 작업을 <strong>하나의 트랜잭션 단위</strong>로 묶어 처리하며 데이터의 무결성과 일관성을 유지하는 데 사용한다.</p>
<blockquote>
<p><strong>TCL 주요 명령어</strong></p>
</blockquote>
<p><strong>COMMIT</strong>: 트랜잭션의 변경 사항을 데이터베이스에 저장.</p>
<pre><code class="language-sql">COMMIT;</code></pre>
<blockquote>
</blockquote>
<p><strong>ROLLBACK</strong>: 트랜잭션의 변경 사항을 취소.</p>
<pre><code class="language-sql">ROLLBACK;</code></pre>
<blockquote>
</blockquote>
<p><strong>SAVEPOINT</strong>: 트랜잭션 내 특정 지점으로 롤백 가능하도록 저장.</p>
<pre><code class="language-sql">SAVEPOINT save1;</code></pre>
<blockquote>
</blockquote>
<p><strong>SET TRANSACTION</strong>: 트랜잭션 속성 설정.</p>
<pre><code class="language-sql">SET TRANSACTION READ ONLY;</code></pre>
<h3 id="acid-transaction">ACID Transaction</h3>
<p>먼저 <strong>Transaction</strong>이란 데이터베이스 관리에서 하나의 논리적 작업 단위를 의미하며, 이 단위 내에서 실행되는 작업은 모두 성공적으로 완료되거나 모두 실패해야 하는 특성을 가진다. 즉, <strong>&quot;모두 실행되거나, 아무것도 실행되지 않는&quot;</strong> 원칙을 따르는 작업 그룹이다. </p>
<p><strong>ACID 트랜잭션</strong>은 데이터베이스 관리에서 트랜잭션의 <code>신뢰성과 일관성</code>을 보장하기 위해 필요한 <strong>4가지 핵심 속성</strong>을 의미합니다. 이는 데이터베이스가 장애 상황에서도 <code>일관성과 무결성</code>을 유지할 수 있도록 돕는 중요한 개념이다. 대부분의 <strong>RDBMS(<code>MySQL, PostgreSQL, Oracle, ETC</code>)</strong>는 <strong>ACID</strong>를 지원한다.</p>
<blockquote>
<p><strong>ACID의 4가지 속성</strong></p>
</blockquote>
<p><strong>Atomicity (원자성)</strong>
<strong>Transaction</strong>은 모든 작업이 성공적으로 완료되거나 전혀 수행되지 않은 상태로 복구되어야 한다. 중간에 트랜잭션이 실패하면 모든 작업이 취소되어 데이터베이스는 트랜잭션 이전 상태로 복구된다.</p>
<blockquote>
</blockquote>
<p>ex) 은행 송금 시, 돈이 한 계좌에서 빠져나갔지만 다른 계좌로 입금되지 않으면, 송금 트랜잭션 전체가 취소되어야 함.</p>
<blockquote>
</blockquote>
<p><strong>Consistency (일관성)</strong>
<strong>Transaction</strong> 실행 전후에 데이터베이스는 항상 일관된 상태를 유지해야 한다. 데이터베이스의 규칙(무결성 제약 조건, 외래 키 제약 조건 등)을 트랜잭션이 절대 위반하지 않도록 보장한다.</p>
<blockquote>
</blockquote>
<p>ex) 은행 시스템에서 모든 계좌의 잔액 총합은 트랜잭션 전과 후에 같아야 함.</p>
<blockquote>
</blockquote>
<p><strong>Isolation (독립성)</strong>
여러 <strong>Transaction</strong> 동시에 실행되더라도, 각 트랜잭션은 서로 간섭하지 않고 독립적으로 실행되어야 한다. 한 트랜잭션의 중간 상태를 다른 트랜잭션이 볼 수 없으며, 완료된 트랜잭션만 다른 트랜잭션에서 접근 가능하다.</p>
<blockquote>
</blockquote>
<p>ex) 동시에 실행 중인 두 트랜잭션이 동일한 계좌 데이터를 갱신하려 할 때, 한 트랜잭션이 완료될 때까지 다른 트랜잭션은 대기해야 함.</p>
<blockquote>
</blockquote>
<p><strong>Durability (지속성)</strong></p>
<blockquote>
</blockquote>
<p><strong>Transcation</strong> 이 성공적으로 완료되면, 그 결과는 영구적으로 저장되어야 한다. 시스템 장애(전원 꺼짐, 서버 재부팅 등) 이후에도 트랜잭션 결과는 유지된다.</p>
<blockquote>
</blockquote>
<p>ex) 송금이 완료된 후 서버가 다운되더라도 송금 기록은 데이터베이스에 남아 있어야 함.</p>
<h3 id="transaction-예시">Transaction 예시</h3>
<p><strong>은행 송금 작업</strong></p>
<p>A 계좌에서 100원을 출금한다.
B 계좌에 100원을 입금한다.
이 두 작업은 하나의 트랜잭션으로 처리되어야 한다.</p>
<blockquote>
</blockquote>
<p><strong>Atomicity</strong> : 한 계좌에서 출금되었지만 다른 계좌에 입금되지 않으면, 트랜잭션 전체가 취소된다.
<strong>Consistency</strong> : 트랜잭션 전후로 A 계좌와 B 계좌의 총합은 같아야 한다.
<strong>Isolation</strong> : 다른 트랜잭션은 이 송금 트랜잭션이 완료되기 전까지 중간 상태를 보지 못한다.
<strong>Durability</strong> : 송금이 완료된 후 시스템이 꺼져도 입금/출금 내역은 보존된다.</p>