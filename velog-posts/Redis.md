<h2 id="redis">Redis</h2>
<p><strong>Redis</strong>란 <strong>Remote Dictionary Server</strong>의 약자로서 <strong>key-value</strong> 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 <strong>비관계형 데이터베이스 관리 시스템(DBMS)</strong>
-&gt; <strong>Redis</strong>는 데이터 처리 속도가 엄청 빠른 <strong>NoSQL DB</strong>이다. </p>
<p><strong>Redis</strong>는 <code>In-Memory</code>에 모든 데이터를 저장한다. 그래서 데이터 처리 성능이 굉장히 빠르다. 대부분의 <strong>RDBMS</strong>의 데이터베이스는 <strong>대부분 DISK</strong>에 데이터를 저장하지만 <strong>Redis</strong> 같은 경우 <strong>RAM</strong>에서의 데이터를 저장한다. </p>
<p><strong>Redis</strong>는 단순한 <code>문자열</code> 뿐만 아니라 <code>Linked-list / Set / Sorted-set / Hash / Stream</code>을 저장할 수 있다. 또한 <strong>Redis</strong>는 기본적으로 메모리에 데이터를 저장하지만, 필요에 따라 <strong>영속성(persistence)</strong>을 지원한다. 데이터를 주기적으로 디스크에 저장하여 서버가 재시작되거나 오류가 발생하더라도 데이터를 복구할 수 있다. </p>
<blockquote>
<ul>
<li><strong>RDB (Redis Database)</strong>: 특정 시점에 데이터를 스냅샷 형태로 디스크에 저장
RDB 방식은 특정 주기마다 Redis의 데이터를 <strong>스냅샷(snapshot)</strong>으로 디스크에 저장하는 방식이다. 이렇게 저장된 파일은 <code>.rdb 파일 형식</code>으로 유지되며, Redis가 재시작되거나 장애가 발생한 경우 이 파일을 사용하여 데이터를 복구할 수 있다.</li>
</ul>
</blockquote>
<ul>
<li><strong>AOF (Append Only File)</strong>: 모든 쓰기 작업을 로그로 기록하여 복구
AOF 방식은 Redis의 모든 쓰기 작업을 로그 파일에 순차적으로 기록하여 영속성을 유지한다. 모든 SET, INCR 등의 명령어가 AOF 파일에 저장되며, Redis가 다시 시작될 때 이 파일을 재생하여 데이터 상태를 복원한다.</li>
</ul>
<p><strong>Redis</strong> 같은 경우 영속성을 유지하기 위해서 <strong>AOF</strong> 파일을 통해 최신 상태를 복구하고, <strong>RDB</strong>로 정기적인 백업 역할을 수행한다. 이렇게 병행 설정하면 데이터의 안정성을 높이면서 복구 성능도 균형 있게 유지할 수 있다.</p>
<blockquote>
<p><strong>Redis 사용 예시</strong></p>
</blockquote>
<ul>
<li>캐싱(<strong>Caching</strong>)</li>
<li>세션 관리(<strong>Session Management</strong>)</li>
<li>실시간 분석 및 통계(<strong>Real-time Analystics</strong>)</li>
<li>메시지 큐(<strong>Message Queue</strong>)</li>
<li>지리공간 인덱싱(<strong>Geospatial Indexing</strong>)</li>
<li>실시간 채팅 및 메시징(<strong>Real-time Chat And Messaging</strong>)</li>
</ul>
<p>필자는 현재 캐싱(<strong>데이터 검색엔진 조회 성능 향상</strong>) 그리고 세션 관리(<strong>refresh token</strong>)를 <strong>focus</strong>로 사용할 예정이다. </p>
<h4 id="캐싱caching">캐싱(Caching)</h4>
<p><strong>Redis</strong>는 주로 캐싱 용도로 사용된다. 예를 들어, 데이터베이스에서 자주 조회하는 데이터를 <strong>Redis</strong>에 저장하여 반복적인 조회 요청을 메모리에서 처리함으로써 성능을 크게 향상시킬 수 있다. 웹 애플리케이션에서 페이지 로딩 속도를 개선하고, API 응답 시간을 단축하는 데 Redis 캐시가 효과적이다.</p>
<h4 id="세션-저장소session-storage">세션 저장소(Session Storage)</h4>
<p><strong>Redis</strong>는 사용자 세션 데이터를 저장하는 데 자주 사용된다. 세션 데이터는 <code>로그인 정보와 같은 사용자 상태 정보</code>를 포함하며, <code>빠른 읽기 및 쓰기 속도</code>가 요구된다. <strong>Redis</strong>는 세션 저장소로 사용하기에 적합하며, 만료 시간을 설정할 수 있어 오래된 세션 데이터를 자동으로 제거하는 데 유용하다.</p>
<h4 id="실시간-데이터-분석real-time-data-analysis">실시간 데이터 분석(Real-time Data Analysis)</h4>
<p><strong>Redis</strong>는 실시간 데이터 분석을 위한 도구로도 유용하다. <strong>Redis</strong>는 초당 수십만 건의 요청을 처리할 수 있어 실시간으로 사용자 활동을 분석하거나 통계 데이터를 생성하는 데 활용된다. 예를 들어, <code>실시간 채팅, 게임 순위, 실시간 피드</code> 등의 기능에서 <strong>Redis</strong>를 사용할 수 있다.</p>
<h3 id="message-queue">Message Queue</h3>
<p><strong>메시지 큐(Message Queue)</strong>는 시스템 내에서 <code>다양한 컴포넌트나 서비스</code>가 서로 비동기적으로 통신할 수 있도록 지원하는 방식이다. 이는 두 서비스가 서로 직접적인 연결 없이도 데이터를 주고받을 수 있게 해주는 중간 저장소 역할을 한다. <strong>메시지 큐에 데이터를 보내면, 다른 서비스가 그 데이터를 가져가 처리할 수 있다.</strong></p>
<h4 id="프로듀서producer와-컨슈머consumer">프로듀서(Producer)와 컨슈머(Consumer)</h4>
<p><strong>프로듀서(Prodcuer)</strong>는 메시지를 생성하고 메시지 큐에 넣는 역할을 한다. <strong>컨슈머(Consumer)</strong>는 큐에 들어있는 메시지를 읽어와서 처리하는 역할을 한다.</p>
<h4 id="큐queue">큐(Queue)</h4>
<p><strong>메시지 큐(Message Queue)</strong>는 <code>FIFO(First In, First Out)</code> 구조로 되어 있어, 먼저 들어온 메시지가 먼저 처리된다. message가 queue에 저장되면, consumer가 해당 <strong>message</strong>를 가져가기 전까지 대기 상태에 놓인다.</p>
<h4 id="비동기-처리">비동기 처리</h4>
<p><strong>메시지 큐(Message Queue)</strong>는 비동기 통신을 지원하므로, producer가 message를 queue에 넣으면 바로 다음 작업을 수행할 수 있고, consumer는 필요할 때 message를 가져와 처리할 수 있다. 이 비동기성 덕분에 서비스 간의 결합도가 낮아지고, 각 서비스가 독립적으로 동작할 수 있다.</p>
<h3 id="redis-pub--subpublish--subscribe">Redis Pub / Sub(Publish / Subscribe)</h3>
<p><strong>Redis</strong>는 <strong>message queue</strong> 기능도 제공한다. <code>Pub/Sub</code>는 게시자-구독자 패턴을 통해 메시지를 송수신하는 방식으로, 한 곳에서 메시지를 발행하면 해당 메시지를 구독하고 있는 모든 클라이언트에 전달된다. 이 기능은 <code>실시간 알림이나 채팅 시스템</code>에 유용하게 사용할 수 있다. 채팅 애플리케이션에서 각 사용자에게 실시간으로 메시지 전송, 게임에서 사용자에게 실시간 이벤트 정보 전송, 분산 시스템에서 여러 서버가 특정 상태 변화를 실시간으로 감지하도록 구현에서 주로 사용된다.</p>
<h3 id="redis-stream">Redis Stream</h3>
<p><strong>Redis Streams</strong>는 <code>로그 및 이벤트 스트리밍 기능</code>을 제공한다. <strong>Kafka와 같은 message queue</strong> 서비스처럼 이벤트 스트리밍을 지원하며, 데이터의 순차적 저장 및 실시간 분석에 매우 유용하다. 특히 <code>IoT 데이터 처리나 로그 수집, 실시간 분석</code>에 많이 활용된다. 데이터가 순차적으로 저장되며, 각 항목에 <code>고유 ID</code>가 할당된다. <strong>각 소비자 그룹(Consumer Group)</strong>이 독립적으로 데이터를 처리할 수 있다. 또한, message queue 기능을 강화한 구조로, 데이터 손실 없이 안정적인 처리가 가능하다.</p>
<h2 id="redis-pubsub와-redis-streams의-주요-차이점-요약">Redis Pub/Sub와 Redis Streams의 주요 차이점 요약</h2>
<table>
<thead>
<tr>
<th>기능</th>
<th>Redis Pub/Sub</th>
<th>Redis Streams</th>
</tr>
</thead>
<tbody><tr>
<td><strong>메시지 저장</strong></td>
<td>실시간 전송 후 저장되지 않음</td>
<td>메시지가 저장되어 나중에 다시 조회 가능</td>
</tr>
<tr>
<td><strong>소비자 모델</strong></td>
<td>단순 발행-구독</td>
<td>소비자 그룹 지원, 여러 소비자가 분산 처리 가능</td>
</tr>
<tr>
<td><strong>주요 사용 사례</strong></td>
<td>실시간 알림, 채팅</td>
<td>로그 처리, 실시간 분석, 이벤트 스트리밍</td>
</tr>
<tr>
<td><strong>메시지 전달 보장</strong></td>
<td>구독 중일 때만 전달</td>
<td>메시지가 스트림에 저장되어 복구 가능</td>
</tr>
<tr>
<td><strong>순차 데이터 처리</strong></td>
<td>지원하지 않음</td>
<td>메시지를 순차적으로 저장하고 조회 가능</td>
</tr>
</tbody></table>
<p><strong>Redis Pub/Sub</strong>는 실시간 알림이나 일회성 메시지를 전송할 때 적합한다. 데이터 영속성이 필요 없고, 메시지가 즉시 전달되어야 하는 경우(예: 채팅 메시지 전송)에 사용된다.</p>
<p><strong>Redis Streams</strong>는 데이터의 영속성이 필요하며, 메시지를 순차적으로 처리하거나 다시 조회해야 하는 경우(예: 로그 수집, 데이터 분석) 적합한다. Redis Streams는 Redis를 메시지 큐처럼 사용할 수 있게 해주는 고급 기능으로, 비슷한 기능을 가진 Kafka와 같은 시스템을 대체할 수 있다.</p>