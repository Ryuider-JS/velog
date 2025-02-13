<h2 id="shallow-routing">Shallow Routing</h2>
<p><strong>Shallow Routing</strong>은 <strong>Next.js에서 URL</strong>을 변경하면서도 페이지를 리렌더링하지 않고 상태를 유지할 수 있는 기법이다. 이를 통해 페이지의 URL만 변경하고, 페이지 내에서 컴포넌트나 상태는 그대로 유지하는 방식으로 동작한다. 즉, 페이지 간 이동 시 리소스를 다시 로드하지 않고 URL만 변경하기 때문에 <strong>상태 유지와 성능 최적화</strong>에 유리하다. 또한, 페이지가 다시 로드되지 않으므로, 리렌더링 없이 URL 변경과 데이터 요청만 새로 할 수 있다. 이는 주로 <strong>Query Parameter나 URL Path</strong>를 변경할 때, 페이지를 리로드하지 않고 URL만 바꾸고자 할 때 유용합니다. 주로 <strong>검색 필터링, 탭 전환, 모달 상태 관리, 정렬</strong> 등에서 사용된다. 이를 사용하면 빠르고 부드러운 사용자 경험을 제공하며, URL을 상태와 동기화하는 데 유용한다.</p>
<blockquote>
<p><strong>Shallow Routing의 특징</strong>
<strong>페이지 리렌더링을 하지 않고 URL만 변경</strong>: <strong>Shallow Routing</strong>을 사용하면, URL만 변경하고 페이지는 리렌더링되지 않다. 따라서 URL에 의존하는 컴포넌트만 리렌더링되며, 다른 페이지 내용은 그대로 유지된다.</p>
</blockquote>
<p><strong>상태 유지</strong>: 페이지 상태(예: 폼 입력값, 탭 상태, 필터링 상태)는 URL을 변경하더라도 유지된다. 이는 사용자가 브라우저 뒤로 가기/앞으로 가기를 하더라도 페이지 상태가 유지된다는 의미다.</p>
<blockquote>
</blockquote>
<p><strong>동적 페이지 관리</strong>: <strong>Shallow Routing</strong>은 주로 URL 쿼리 파라미터나 URL 경로를 변경할 때 유용하며, 페이지를 새로 로드하지 않고 URL 상태만 업데이트한다.</p>
<blockquote>
</blockquote>
<p><strong>URL에 상태 반영</strong>: URL의 쿼리 파라미터나 경로에 상태를 반영할 수 있으므로, 사용자가 URL을 직접 수정하거나 북마크할 수 있다. 예를 들어, 필터나 검색어를 URL에 반영하여 다른 페이지로 공유할 수 있다.</p>
<h3 id="nextjs-14-이전의-shallow-route와-nextjs-14-이후의-shallow-route">NextJS 14 이전의 shallow route와 NextJS 14 이후의 shallow route</h3>
<p><strong>기존 방식</strong> : 예전에는 <code>router.push()</code> 또는 <code>router.replace()</code>에서 <strong>shallow: true 옵션</strong>을 사용하여 <strong>Shallow Routing</strong>을 적용했었다. 이 방식은 <strong>페이지 리렌더링 없이 URL</strong>을 변경하는데 사용되었으나, <strong>App Router와 React 18</strong>의 도입으로 라우팅 방식이 개선되었다.</p>
<p><strong>새 방식 (Next.js 14, React 18)</strong>: 현재는 <strong>window.history.pushState()</strong>와 <strong>window.history.replaceState()</strong>를 통해 URL을 변경하면서 Next.js의 라우터와 동기화가 자동으로 이루어진다. 이제 <code>shallow: true 옵션</code>을 별도로 설정할 필요 없이 URL을 변경하면서도 페이지 리렌더링 없이 상태를 유지할 수 있다.</p>
<p>바뀐 이유는 다음과 같다. 먼저 <strong>브라우저 기본 History API와의 통합이다.</strong> <strong>window.history.pushState()</strong>와 <strong>window.history.replaceState()</strong>는 브라우저 내장 API로, URL 변경을 페이지 리렌더링 없이 처리할 수 있게 해준다. 추가적으로 <strong>window.history</strong>를 사용하게 되면, <strong>NextJS 라우터와 Browser History</strong>에 자동으로 동기화된다.이러면서 좀 더 직관적이고 일관된 방식으로 사용할 수 있게 되었다.
또한, <strong>React 18 Concurrent Mode / Server Component</strong>와 호환성이 좋으며 비동기적 <strong>UI</strong> 업데이트와 성능 최적화를 위해 기존 방식보단 <strong>window.history</strong>를 사용하는 게 더 적합하다.</p>
<h3 id="shallow-route-예시">Shallow Route 예시</h3>
<pre><code class="language-jsx">'use client';

import { useState } from 'react';
import { useSearchParams } from 'next/navigation';

export default function SearchComponent() {
  const [searchTerm, setSearchTerm] = useState(''); 
  const searchParams = useSearchParams(); 

  function handleSearchChange(event: React.ChangeEvent&lt;HTMLInputElement&gt;) {
    const newSearchTerm = event.target.value;
    setSearchTerm(newSearchTerm);

    window.history.pushState(null, '', `?search=${newSearchTerm}`);
  }

  return (
    &lt;div&gt;
      &lt;h1&gt;Search&lt;/h1&gt;
      &lt;input
        type=&quot;text&quot;
        value={searchTerm}
        onChange={handleSearchChange} 
        placeholder=&quot;Search products...&quot;
      /&gt;
      &lt;div&gt;
        &lt;p&gt;Showing results for: {searchTerm}&lt;/p&gt;
      &lt;/div&gt;
    &lt;/div&gt;
  );
}</code></pre>
<p>이 뿐만 아니라 <strong>useSearchParams</strong>를 통해 다른 컴포넌트에서 <strong>Query parameter</strong>를 불러올 수 있다. 이는 장점으로 페이지번호나, 검색어를 url을 통해서 반영할 수 있고, url만 변경되므로, 리렌더링를 최소화시킬 수 있다. 페이지를 새로고침해도 url을 다시 relaod하는 거기 때문에 검색 상태도 유지할 수 있다. 뒤로가기를 눌러도 <strong>history stack</strong>에 쌓이기 때문에, 뒤로가기도 사용가능하다. 만약 뒤로가기 기능을 없애고 싶으면 <strong>window.history.pushState(null, &quot;&quot;, ``?search=${search}`);</strong> 가 아닌 <strong>window.history.replaceState(null, &quot;&quot;, <code>?search=${search}</code>);</strong>를 사용하면 된다. </p>
<h2 id="parallel-routing">Parallel Routing</h2>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/98ad6c2d-d138-43ad-a89f-f2930458fb01/image.png" /></p>
<p><strong>Parallel Routing</strong>은 말 그대로 여러 라우팅 경로를 병렬로 처리할 수 있는 기술이다. 기존의 <strong>NextJS</strong> 라우팅은 단일 경로에 대해 단일 레이아웃을 처리하는 방식이었지만, <strong>Parallel Routing</strong>을 사용하면 다양한 페이지 레벨의 라우트를 동시에 처리할 수 있다. 이를 통해 애플리케이션의 레이아웃을 더 모듈화하고, 성능을 개선할 수 있다.</p>
<blockquote>
<p><strong>Parallel Routing를 사용하는 이유</strong>
<strong>성능 최적화</strong>: <strong>Parallel Routing</strong>을 사용하면, 여러 페이지 레벨의 라우트를 동시에 처리할 수 있어, 렌더링이 더 빠르게 이루어진다. 예를 들어, 다양한 페이지 레벨을 병렬로 로딩하면서 페이지의 주요 콘텐츠는 빨리 렌더링하고, 부가적인 콘텐츠는 뒤따라 렌더링되도록 할 수 있다.</p>
</blockquote>
<p><strong>유연한 레이아웃 구성</strong>: 앱의 레이아웃을 모듈화하고, 여러 컴포넌트나 섹션을 독립적으로 처리할 수 있다. 예를 들어, 네비게이션, 사이드바, 헤더 등의 UI는 한 번만 로드하고, 메인 콘텐츠는 페이지에 맞게 동적으로 로딩할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>더 나은 사용자 경험</strong>: 사용자에게 빠른 로딩 속도를 제공하면서, 느리게 로딩되는 콘텐츠는 비동기적으로 로드할 수 있다. 이렇게 하면 사용자에게 부드러운 탐색을 제공하고, 로딩 스피너를 최소화할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>애플리케이션 유지보수 용이</strong>: 페이지를 모듈화하고 동적으로 로딩하는 방식은, 애플리케이션을 더 효율적으로 유지보수할 수 있도록 도와준다. 예를 들어, 레이아웃을 재사용하거나, 각 페이지 레벨을 독립적으로 관리할 수 있게 된다.</p>
<h3 id="soft-navigation--hard-navigation">Soft Navigation &amp; Hard Navigation</h3>
<h4 id="soft-navigation">Soft Navigation</h4>
<p><strong>Soft Navigation</strong>은 클라이언트 사이드 내비게이션을 의미하며, 페이지 전체를 새로고침하지 않고, URL만 변경하여 일부 콘텐츠만 업데이트하는 방식이다. 이 방식에서는 부분적으로 렌더링이 이루어지며, 페이지의 일부가 바뀌고 다른 부분은 그대로 유지된다. Next.js에서는 클라이언트 사이드 라우팅을 활용해 이 방식이 구현된다.</p>
<blockquote>
<p><strong>Soft Navigation의 특징</strong>
<strong>부분 렌더링</strong>: 현재 URL과 일치하는 세그먼트만 렌더링되고, 나머지 세그먼트는 변경되지 않는다.
<strong>빠르고 부드러운 사용자 경험</strong>: 페이지를 전체적으로 리렌더링하지 않기 때문에, 빠르고 부드러운 전환을 제공한다.
<strong>상태 유지</strong>: 기존 상태(예: 탭 전환, 스크롤 위치 등)는 유지되며, URL만 변경된다.
<strong>경로 전환만 처리</strong>: URL이 변경되지만, 페이지 내용은 부분적으로만 업데이트되기 때문에, 빠른 응답성을 제공한다.</p>
</blockquote>
<pre><code class="language-jsx">'use client';

import { useSelectedLayoutSegments } from 'next/navigation';

export default function TabNavigation() {
  const segment = useSelectedLayoutSegments(); // 현재 활성화된 세그먼트를 추적

  return (
    &lt;div&gt;
      &lt;nav&gt;
        &lt;button className={segment === 'tab1' ? 'active' : ''}&gt;Tab 1&lt;/button&gt;
        &lt;button className={segment === 'tab2' ? 'active' : ''}&gt;Tab 2&lt;/button&gt;
      &lt;/nav&gt;
      {segment === 'tab1' &amp;&amp; &lt;div&gt;Tab 1 Content&lt;/div&gt;}
      {segment === 'tab2' &amp;&amp; &lt;div&gt;Tab 2 Content&lt;/div&gt;}
    &lt;/div&gt;
  );
}</code></pre>
<p>페이지는 새로고침 되지 않고, 탭 콘텐트만 변경된다. 페이지 리렌더링 없이 <strong>부분적인 콘텐츠</strong>만 업데이트되므로 <strong>빠르고 부드러운 사용자 경험</strong>을 제공한다.</p>
<h4 id="hard-navigation">Hard Navigation</h4>
<p><strong>Hard Navigation</strong>은 서버 사이드 내비게이션 또는 전체 페이지 새로 고침을 의미한다. 이 방식에서는 브라우저가 새로 고침을 통해 전체 페이지를 다시 렌더링합니다. <strong>즉, 전체 페이지 로드가 발생하고, URL에 맞는 세그먼트만 렌더링된다.</strong> 매칭되지 않는 세그먼트는 <strong>default.js 파일</strong>을 렌더링하여 기본 상태를 처리하거나 404 페이지를 표시한다.</p>
<blockquote>
<p><strong>Hard Navigation의 특징</strong>
<strong>전체 페이지 리렌더링</strong>: URL에 맞는 콘텐츠가 렌더링되고, 다른 콘텐츠는 기본 상태로 돌아간다.
<strong>default.js 렌더링</strong>: 매칭되지 않는 세그먼트는 <strong>default.js</strong>에서 기본 레이아웃을 렌더링하거나 404 페이지를 반환한다.
<strong>브라우저 새로 고침</strong>: 새로 고침이 발생하면 페이지 전체가 다시 로드된다.
<strong>매칭되지 않는 세그먼트 처리</strong>: URL과 일치하지 않는 부분에 대해 기본 콘텐츠나 404 페이지를 렌더링합니다.</p>
</blockquote>
<h3 id="parallel-routing과의-관계">Parallel Routing과의 관계</h3>
<p><strong>Parallel Routing</strong>은 Next.js 14에서 여러 레이아웃을 병렬로 처리하는 기능이다. <strong>Soft Navigation과 Hard Navigation</strong>은 이러한 병렬 라우팅에서 중요한 역할을 한다.</p>
<h4 id="병렬-라우팅에서-soft-navigation">병렬 라우팅에서 Soft Navigation</h4>
<p><strong>Soft Navigation</strong>을 사용하면, 병렬 라우팅에서 여러 레이아웃을 동시에 로드하면서, 현재 활성화된 세그먼트만 동적으로 렌더링할 수 있다. 이 방식은 빠르고 효율적인 콘텐츠 업데이트를 가능하게 한다. 예를 들어, 탭 전환이나 사이드바 전환 시, 다른 콘텐츠는 그대로 유지되고, 변경된 콘텐츠만 부분적으로 로딩된다.</p>
<h4 id="병렬-라우팅에서-hard-navigation">병렬 라우팅에서 Hard Navigation</h4>
<p><strong>Hard Navigation</strong>에서는 전체 페이지 새로 고침이 발생하고, 병렬 라우팅에서 매칭되지 않는 세그먼트는 <strong>default.js</strong>에서 처리된다. 병렬 라우팅에서 <strong>Hard Navigation</strong>이 발생하면, 세그먼트가 일치하지 않는 경우 <strong>default.js</strong>가 렌더링되어 기본 콘텐츠를 보여주게 됩다. 즉, 페이지 새로 고침 시 기본 상태나 404 페이지를 렌더링하여, 사용자가 올바른 경로로 다시 이동할 수 있게 한다.</p>
<blockquote>
<p><strong>default.js / useSelectedSegment</strong></p>
</blockquote>
<ul>
<li><strong>default.js</strong>는 병렬 라우팅에서 세그먼트가 매칭되지 않을 때 기본적으로 렌더링되는 레이아웃 또는 콘텐츠이다.
Hard Navigation 시, 매칭되지 않는 세그먼트에 대해서는 <strong>default.js</strong>가 렌더링되어 기본 레이아웃을 처리한다. 만약 default.js가 없다면, 404 페이지를 렌더링한다.<blockquote>
</blockquote>
</li>
<li><strong>useSelectedSegment</strong>는 현재 선택된 세그먼트를 추적하는 훅으로, 병렬 라우팅을 사용할 때 여러 레이아웃 세그먼트가 있을 경우 어떤 세그먼트가 활성화되었는지 확인하는 데 유용하다. 탭 그룹이나 모달 등에서 현재 어떤 세그먼트가 활성화되어 있는지 동적으로 처리하는 데 사용된다. 예를 들어, 탭을 클릭하면 해당 탭의 콘텐츠만 렌더링하고, 다른 콘텐츠는 그대로 유지된다.</li>
</ul>
<h3 id="parallel-routing-예시">Parallel Routing 예시</h3>
<h4 id="조건부-라우트conditional-routes">조건부 라우트(Conditional Routes)</h4>
<p>조건부 라우트는 <strong>URL이나 세그먼트</strong>에 따라 동적으로 렌더링할 콘텐츠를 결정하는 방식이다. 예를 들어, 사용자가 로그인 상태에 따라 다른 페이지나 컴포넌트를 렌더링할 수 있다. <strong>useSelectedSegment()</strong>를 이용하여 현재 선택된 segment에 따라 조건에 맞는 컴포넌트를 렌더링 시킨다.</p>
<pre><code class="language-jsx">'use client';

import { useSelectedSegment } from 'next/navigation';

export default function ConditionalRoute() {
  const segment = useSelectedSegment(); // 현재 활성화된 세그먼트 추적

  return (
    &lt;div&gt;
      {segment === 'admin' &amp;&amp; &lt;div&gt;Admin Dashboard&lt;/div&gt;}
      {segment === 'user' &amp;&amp; &lt;div&gt;User Dashboard&lt;/div&gt;}
    &lt;/div&gt;
  );
}</code></pre>
<h4 id="탭-그룹tab-groups">탭 그룹(Tab Groups)</h4>
<p>탭을 사용하는 경우, 여러 개의 탭이 하나의 페이지 내에서 병렬로 로딩되며, 각 탭은 독립적으로 콘텐츠를 렌더링할 수 있다. 탭 전환을 할 때, <strong>각 탭의 콘텐츠만 동적으로 렌더링되고, 다른 탭은 그대로 유지된다.</strong></p>
<pre><code class="language-jsx">'use client';

import { useSelectedSegment } from 'next/navigation';

export default function TabGroup() {
  const segment = useSelectedSegment(); // 현재 활성화된 세그먼트 추적

  return (
    &lt;div&gt;
      &lt;nav&gt;
        &lt;button className={segment === 'tab1' ? 'active' : ''}&gt;Tab 1&lt;/button&gt;
        &lt;button className={segment === 'tab2' ? 'active' : ''}&gt;Tab 2&lt;/button&gt;
      &lt;/nav&gt;
      {segment === 'tab1' &amp;&amp; &lt;div&gt;Tab 1 Content&lt;/div&gt;}
      {segment === 'tab2' &amp;&amp; &lt;div&gt;Tab 2 Content&lt;/div&gt;}
    &lt;/div&gt;
  );
}
</code></pre>
<h4 id="모달modal">모달(Modal)</h4>
<p>모달을 다룰 때도 segment를 사용할 수 있다. 예를 들어, 사용자가 모달을 열거나 닫을 때 <strong>URL의 쿼리 파라미터나 세그먼트</strong>를 통해 모달 상태를 관리할 수 있다. 모달 상태는 URL의 <code>?modal=true</code>와 같은 쿼리 파라미터로 관리된다.
사용자가 모달을 열거나 닫을 때 <strong>window.history.pushState</strong>를 사용해 URL을 업데이트하고, <strong>useSearchParams</strong>를 통해 모달 상태를 추적하여 렌더링한다.</p>
<pre><code class="language-jsx">'use client';

import { useSearchParams } from 'next/navigation';

export default function Modal() {
  const searchParams = useSearchParams();
  const isModalOpen = searchParams.get('modal') === 'true'; // URL에서 modal 파라미터 확인

  return (
    &lt;div&gt;
      {isModalOpen &amp;&amp; &lt;div className=&quot;modal&quot;&gt;This is a modal&lt;/div&gt;}
    &lt;/div&gt;
  );
}</code></pre>