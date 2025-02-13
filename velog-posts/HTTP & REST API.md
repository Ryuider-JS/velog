<h2 id="httphyper-text-transfer-protocol">HTTP(Hyper Text Transfer Protocol)?</h2>
<p><strong>HTTP</strong>란 클라이언트와 서버가 통신하는 방법 중 하나로 클라이언트가 요청을 보내면 서버가 응답을 반환한다. 요청과 응답의 구조화된 데이터를 보낼 때 일반적으로 <strong>JSON</strong> 구조를 사용한다. </p>
<h3 id="json">JSON</h3>
<p><strong>JSON</strong>이란 데이터 교환을 위한 <strong>문자열 기반의 경량 테이터 포맷</strong>으로써 <strong>JS</strong> 객체 또는 <strong>Map 자료구조</strong>와 비슷한 형태이다. <strong>JSON</strong>는 주로 <strong>API</strong>(특히, REST API)통신할 때 사용된다.</p>
<p><strong>REST API</strong> 요청과 응답의 body에 사용되는 구조이다. <strong>key / value</strong>로 이루어져 있고, <strong>colon</strong>을 기준으로 왼쪽이 <strong>key</strong> 오른쪽이 <strong>value</strong>이다. <strong>key</strong>는 string만 허용하고 <strong>value</strong>는 <code>number, string, 중첩된 JSON, List</code>가 허용된다.</p>
<pre><code class="language-json">// JSON 예시
{
  'name' : &quot;류지승&quot;,
  'age' : 26,
  'occupation' : 'sofrware engineer',
  'school' : [
    {
      'name' : '류지승',
      'address' : &quot;서울시 강동구 고덕로 98길&quot;
    }
  ]
}</code></pre>
<h2 id="http-request">HTTP Request</h2>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/9b370952-6859-4f26-bf89-05debfa2ccfe/image.png" /></p>
<blockquote>
<p>** HTTP Request 구성 요소**</p>
</blockquote>
<ul>
<li><strong>URL</strong> : 요청을 보내는 모든 주소(실제 웹사이트 주소)<ul>
<li>protocol :  <code>http / https / ftp</code></li>
<li>host : <code>www.naver.com</code></li>
<li>port : <code>3000, 8080, 5173, 5432, 80, 443</code></li>
<li>path : <code>/board/1/comment</code></li>
<li>query parameter: <code>?key1=value1&amp;key2=value2</code></li>
<li>anchor : <code>#documentlocation</code><ul>
<li><strong>Method</strong> : 요청을 보내는 종류 / 타입<strong>(GET / POST / PUT / PATCH / DELETE / OPTIONS)</strong></li>
<li><strong>Header</strong> : 요청의 메타데이터(<strong>body</strong>에 대한 데이터)</li>
<li><strong>Body</strong> : 요청과 관련된 데이터</li>
</ul>
</li>
</ul>
</li>
</ul>
<h3 id="query-parameter--anchor">query parameter &amp; anchor</h3>
<p><strong>Query Parameter</strong>는 URL에서 리소스에 대한 추가 정보를 전달하기 위해 사용된다. <strong>Path 뒤에 ?key=value</strong> 형식으로 작성되며, 여러 파라미터는 <strong>&amp;</strong>로 구분된다. anchor와 다르게 query parameter는 <strong>client뿐만 아니라 server 모두 처리한다.</strong></p>
<pre><code class="language-plaintext">https://example.com/product?category=electornics&amp;sort=popular</code></pre>
<p>주로 <strong>필터링 / 정렬, 검색 기능, 페이징</strong>에 쓰인다.</p>
<blockquote>
</blockquote>
<pre><code class="language-plaintext">// 전자제품을 가격 내림차순으로 조회
/products?category=electronics&amp;sort=price_desc
&gt;
// laptop을 검색
/search?query=laptop
&gt;
// 2페이지 &amp; 5개씩 조회
/article?page=2&amp;limit=5</code></pre>
<pre><code class="language-jsx">'use client';

import { useSearchParams, useRouter } from 'next/navigation';
import { useEffect, useState } from 'react';

export default function ProductsPage() {
  const searchParams = useSearchParams();
  const router = useRouter();
  const [products, setProducts] = useState([]);

  useEffect(() =&gt; {
    const category = searchParams.get('category') || 'all';
    const page = searchParams.get('page') || 1;

    // 서버로 쿼리 파라미터를 포함한 요청
    fetch(`/api/products?category=${category}&amp;page=${page}`)
      .then((res) =&gt; res.json())
      .then((data) =&gt; setProducts(data));
  }, [searchParams]);

  const updateFilter = (category) =&gt; {
    // 쿼리 파라미터 업데이트
    router.push(`/products?category=${category}&amp;page=1`);
  };

  return (
    &lt;div&gt;
      &lt;button onClick={() =&gt; updateFilter('electronics')}&gt;Electronics&lt;/button&gt;
      &lt;button onClick={() =&gt; updateFilter('fashion')}&gt;Fashion&lt;/button&gt;
      &lt;ul&gt;
        {products.map((product) =&gt; (
          &lt;li key={product.id}&gt;{product.name}&lt;/li&gt;
        ))}
      &lt;/ul&gt;
    &lt;/div&gt;
  );
}</code></pre>
<p><strong>Anchor(Fragment)</strong>는 URL에서 특정 리소스 내의 특정 위치를 참조하기 위해 사용된다. <strong>#</strong>으로 시작하며, <code>client(browser)</code>에서만 처리된다. 주로 <strong>HTML 내 특정 위치</strong>를 찾는데 사용한다. </p>
<blockquote>
</blockquote>
<pre><code class="language-plaintext">// HTML 문서 내 특정 ID(예: &lt;div id=&quot;installation&quot;&gt;)로 스크롤 이동.
https://example.com/docs#installation</code></pre>
<pre><code class="language-jsx">&quot;use client&quot;
import Link from 'next/link';

export default function Home() {
  return (
    &lt;div&gt;
      {/* Navigation */}
      &lt;nav className=&quot;fixed top-0 right-4 flex gap-4&quot;&gt;
        &lt;Link href=&quot;#section1&quot;&gt;Go to Section 1&lt;/Link&gt;
        &lt;Link href=&quot;#section2&quot;&gt;Go to Section 2&lt;/Link&gt;
        &lt;Link href=&quot;#section3&quot;&gt;Go to Section 3&lt;/Link&gt;
      &lt;/nav&gt;

      {/* Sections */}
      &lt;section id=&quot;section1&quot; style={{ height: '100vh', border: '1px solid black' }}&gt;
        Section 1
      &lt;/section&gt;
      &lt;section id=&quot;section2&quot; style={{ height: '100vh', border: '1px solid black' }}&gt;
        Section 2
      &lt;/section&gt;
      &lt;section id=&quot;section3&quot; style={{ height: '100vh', border: '1px solid black' }}&gt;
        Section 3
      &lt;/section&gt;
    &lt;/div&gt;
  );
}</code></pre>
<h2 id="rest-api">REST API</h2>
<p><strong>REST API(Representational State Transfer Application Programming Interface)</strong>는 웹에서 자원을 활용하고 관리하기 위한 표준적인 방식을 정의한 API 설계 아키텍처다. <strong>REST는 HTTP 프로토콜</strong>을 기반으로 클라이언트와 서버 간의 데이터를 교환하는 데 사용되며, 간단하고 확장 가능한 설계를 제공한다. </p>
<p>REST는 <strong>Stateless(무상태성)</strong>를 준수한다. 클라이언트 요청 간에 서버가 상태 정보를 저장하지 않는다. 모든 요청은 필요한 정보를 독립적으로 포함해야 한다. 서버는 <strong>client의 상태</strong>를 저장하지 않는다. !!!
EX) 클라이언트가 인증되었는지를 판단하려면 요청에 인증 토큰이 항상 포함되어야 한다.</p>
<blockquote>
<p><strong>REST</strong>에서 무상태성을 준수하는 이유</p>
</blockquote>
<ol>
<li>확장성(<strong>Scalability</strong>) : 서버는 클라이언트의 상태를 저장하지 않기 때문에, 여러 서버를 쉽게 추가하여 확장할 수 있다. 클라이언트의 요청은 어떤 서버로 전달되어도 동일하게 처리된다. 만약 상태를 저장하는 서버라면? 해당 클라이언트는 <strong>항상 해당 서버에만</strong> 전달되어야 하므로 확장성 측면에서 분리하다. <blockquote>
</blockquote>
</li>
<li>단순성(<strong>Simplicity</strong>) : 서버는 상태를 저장하지 않기 떄문에 <strong>특정 사용자나 클라이언트 상태를 추적할 필요가 없다.</strong> 서버는 각 요청별로 독립적으로 처리하면 서버의 복잡도를 줄일 수 있다.<blockquote>
</blockquote>
</li>
<li>안정성(<strong>Reliability</strong>) : 실질적으로 서버는 클라이언트에 비해 문제가 많이 발생할 수 있다. 예를 들어서 서버가 다운되면 해당 클라이언트에 대한 상태는 전부 사라지는 것이다. 또한, <strong>client</strong>에서는 필요한 경우 반복적으로 보낼 수 있기 때문에 서버가 실패해도 클라이언트에서 요청 처리가 가능하다. <blockquote>
</blockquote>
</li>
<li><strong>클라이언트와 서버 분리</strong> : 클라이언트와 서버는 서로 상태를 공유하지 않기 때문에 서로 독립적인 설계가 가능하다.</li>
</ol>
<h3 id="http-method">HTTP Method</h3>
<p><strong>HTTP Method</strong>를 사용하여 자원에 대한 작업을 수행한다. <strong>REST의 핵심</strong>은 자원의 상태를 변경하거나 읽는 작업을 HTTP 메서드로 표현하는 것이다.</p>
<table>
<thead>
<tr>
<th><strong>HTTP 메서드</strong></th>
<th><strong>설명</strong></th>
<th><strong>사용 예시</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>GET</strong></td>
<td>자원 읽기</td>
<td><code>/users</code> → 사용자 목록 조회</td>
</tr>
<tr>
<td><strong>POST</strong></td>
<td>자원 생성</td>
<td><code>/users</code> → 새 사용자 생성</td>
</tr>
<tr>
<td><strong>PUT</strong></td>
<td>자원 전체 수정</td>
<td><code>/users/123</code> → 사용자 전체 정보 수정</td>
</tr>
<tr>
<td><strong>PATCH</strong></td>
<td>자원 부분 수정</td>
<td><code>/users/123</code> → 사용자 일부 정보 수정</td>
</tr>
<tr>
<td><strong>DELETE</strong></td>
<td>자원 삭제</td>
<td><code>/users/123</code> → 특정 사용자 삭제</td>
</tr>
</tbody></table>
<h3 id="method-특징">Method 특징</h3>
<h4 id="같은-url에-여러-개의-method가-존재">같은 URL에 여러 개의 Method가 존재</h4>
<p><strong>GET <code>https://localhost:8080/board</code></strong>
<strong>POST <code>https://localhost:8080/board</code></strong>
둘은 <strong>같은 URL</strong>이지만 완전하게 다른 요청이다.</p>
<h4 id="get-요청은-데이터-조회할-때-사용read">GET 요청은 데이터 조회할 때 사용(READ)</h4>
<p><strong>GET <code>https://localhost:8080/board</code></strong></p>
<h4 id="post-요청은-데이터-생성할-때-사용create">POST 요청은 데이터 생성할 때 사용(CREATE)</h4>
<p><strong>POST <code>https://localhost:8080/board</code></strong></p>
<h4 id="put--patch-요청은-데이터-업데이트할-때-사용-update">PUT &amp; PATCH 요청은 데이터 업데이트할 때 사용 (UPDATE)</h4>
<p><strong>PUT <code>https://localhost:8080/board</code></strong>
<strong>PATCH <code>https://localhost:8080/board</code></strong>
PUT -&gt; 데이터 전부 업데이트 만약 데이터가 존재하지 않으면 생성
PATCH -&gt; 데이터 일부 업데이트 </p>
<h4 id="delete-요청은-데이터-삭제할-때-사용delete">DELETE 요청은 데이터 삭제할 때 사용(DELETE)</h4>
<p><strong>DELETE <code>https://localhost:8080/board</code></strong></p>
<h3 id="header">Header</h3>
<p><strong>Header</strong>는 <strong>metadata(요청에 대한 정보)</strong> 자동 생성되는 경우가 많고 실질적으로 개발자가 직접 header를 건드는 일은 거의 없다. 다만 !! <strong>Authorization Token</strong>을 처리할 떄 <strong>Bearer 000</strong>를 넣어줘서 넘겨준다.</p>
<h3 id="body">Body</h3>
<p><strong>Body</strong>는 요청에 대한 로직 수행에 직접적으로 필요한 정보를 정의한다. <strong>GraphQL에서의 useMutation</strong>을 예로 들면 <strong>variable</strong>과 같은 존재이다. 일반적으로 <strong>JSON</strong> 구조를 가지고 전달한다. 쉽게 생각하면 실제 데이터!!</p>
<h2 id="http-response">HTTP Response</h2>
<p><strong>HTTP Response</strong>는 <strong>Status Code, Header, Body</strong> 형태로 넘겨준다. Header와 Body는 위에서 설명했기 때문에 <strong>Status Code</strong>만 간략하게 설명</p>
<blockquote>
<p><strong>HTTP Status Code(상태 코드)</strong></p>
</blockquote>
<p>REST API는 클라이언트가 요청의 결과를 쉽게 이해할 수 있도록 <strong>HTTP 상태 코드</strong>를 반환한다.</p>
<blockquote>
</blockquote>
<table>
<thead>
<tr>
<th><strong>상태 코드</strong></th>
<th><strong>설명</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>200(OK)</strong></td>
<td>요청 성공</td>
</tr>
<tr>
<td><strong>201(Created)</strong></td>
<td>리소스 생성 성공</td>
</tr>
<tr>
<td><strong>204(No Content)</strong></td>
<td>요청 성공(콘텐츠 없음)</td>
</tr>
<tr>
<td><strong>400(Bad Request)</strong></td>
<td>잘못된 요청</td>
</tr>
<tr>
<td><strong>401(Unauthorized)</strong></td>
<td>인증 실패</td>
</tr>
<tr>
<td><strong>403(Forbidden)</strong></td>
<td>권한 없음</td>
</tr>
<tr>
<td><strong>404(Not Found)</strong></td>
<td>자원을 찾을 수 없음</td>
</tr>
<tr>
<td><strong>500(Internal Server Error)</strong></td>
<td>서버 내부 오류</td>
</tr>
</tbody></table>
<blockquote>
<p>REST API 설계 원칙</p>
</blockquote>
<ol>
<li>URI는 동사보단 <strong>명사</strong> / 대문자보단 <strong>소문자</strong>를 사용해야한다.</li>
<li>마지막에는 <strong>슬래쉬(/)</strong>를 포함하지 않는다.</li>
<li>언더바(_) 대신 하이픈(-)을 사용한다.</li>
<li>파일확장자는 <strong>URI</strong>에 포함하지 않는다.</li>
<li>url에는 <strong>method</strong>를 포함하면 안된다.</li>
</ol>
<h1 id="결론-http를-설계된-의도대로-규격화해서-api를-만드는-게-rest-api">결론 HTTP를 설계된 의도대로 규격화해서 API를 만드는 게 REST API</h1>