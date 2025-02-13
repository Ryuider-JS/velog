<h2 id="hydration">Hydration</h2>
<p><strong>Hydration 과정</strong>은 <strong>Next.js</strong>를 사용하는 애플리케이션에서 서버가 미리 생성한 <code>정적인 HTML(프리렌더링)</code>을 <strong>클라이언트(브라우저)</strong>로 전송하고, 브라우저가 이 <strong>HTML</strong>을 렌더링한 후 JavaScript가 추가되어 상호작용 가능한 상태로 만드는 과정을 의미한다. 이 과정을 통해 브라우저는 <code>정적인 HTML</code>을 기반으로 <strong>JavaScript</strong>를 실행하고, 동적인 기능과 이벤트 처리를 추가하여 애플리케이션을 활성화하게 된다.</p>
<h3 id="hydration-과정의-단계">Hydration 과정의 단계</h3>
<ol>
<li><strong>서버에서 HTML 제공</strong><ul>
<li>서버는 브라우저로 <strong>정적인 HTML</strong>을 먼저 전달한다. 이 HTML은 <strong>React와 같은 JavaScript 라이브러리</strong>를 사용하지 않아도 화면을 표시할 수 있으며, 초기 로딩 속도가 빠르다.</li>
<li>이 HTML은 미리 서버에서 렌더링되었기 때문에 브라우저는 아직 상호작용할 수 없는 정적인 콘텐츠를 보여주게 됩니다.</li>
</ul>
</li>
<li><strong>브라우저에서 HTML 렌더링</strong><ul>
<li>브라우저는 서버로부터 받은 HTML을 화면에 렌더링합니다. 이 단계에서는 단순히 <strong>HTML만</strong>을 표시하고 있으며, <strong>JavaScript 코드</strong>가 실행되지 않은 상태다.</li>
<li>이때까지는 사용자와의 <code>상호작용(클릭, 입력 등)</code>은 동작하지 않는다.</li>
</ul>
</li>
<li><strong>React의 Hydration 시작</strong><ul>
<li>브라우저가 HTML을 렌더링한 후, <strong>JavaScript 파일이 로드</strong>되면서 React가 실행된다.</li>
<li>React는 서버에서 받은 프리렌더링 된 HTML과 브라우저의 Hydration을 위해 받아온 JavaScript를 비교한다. 이 과정을 통해 React는 HTML 구조와 상태가 일치하는지 확인한다.</li>
</ul>
</li>
<li><strong>HTML과 브라우저 동기화</strong><ul>
<li>서버에서 전달된 HTML과 브라우저가 <strong>동기화(hydrate)</strong> 된다. 이 단계에서는 서버에서 미리 렌더링된 정적인 HTML에 React의 <strong>이벤트 리스너</strong>와 같은 동적 기능이 추가된다.</li>
<li>예를 들어, 사용자가 클릭 가능한 버튼이나 폼을 상호작용할 수 있도록, React는 해당 요소에 대한 이벤트 핸들러를 등록한다.</li>
</ul>
</li>
<li><strong>완전한 상호작용 가능 상태</strong><ul>
<li>Hydration이 완료되면 클라이언트는 서버에서 전달된 HTML과 일치하는 브라우저를 기반으로 동작하며, <strong>완전한 상호작용이 가능한 상태</strong>가 된다.</li>
<li>이 시점부터 사용자는 페이지에서 <code>동적인 동작(버튼 클릭, 데이터 입력, 폼 제출 등)</code>을 수행할 수 있다.</li>
</ul>
</li>
</ol>
<h4 id="hydration이-필요한-이유">Hydration이 필요한 이유</h4>
<p><strong>SSR</strong>을 통해 서버에서 HTML을 미리 렌더링하면 브라우저는 초기 콘텐츠를 빠르게 표시할 수 있다. 하지만 정적인 HTML로만 구성된 페이지는 상호작용이 불가능하므로, <strong>React와 같은 JavaScript 라이브러리</strong>를 통해 동적 기능을 추가하는 <strong>Hydration</strong>이 필수적이다. 만약 <strong>Hydration</strong>을 사용하지 않는다면, 클라이언트 측에서 새로 HTML을 다시 렌더링해야 하므로 성능이 저하되고 초기 로딩 시간이 길어질 수 있다.</p>
<h4 id="hydration과-관련된-문제">Hydration과 관련된 문제</h4>
<p><code>Hydration 과정</code> 중에 클라이언트에서 서버의 <strong>HTML과 React</strong>의 가상 DOM이 불일치하면 Hydration mismatch 오류가 발생할 수 있다. 일반적인 예는 다음과 같다.</p>
<ol>
<li><strong>동일하지 않은 초기 상태</strong>: 서버와 클라이언트의 초기 상태가 다를 경우 HTML이 불일치합니다.</li>
<li><strong>브라우저 전용 코드</strong>: window, document와 같은 브라우저 전용 객체를 SSR에서 사용하면 오류가 발생할 수 있다. 이런 코드는 <code>useEffect</code>에서 실행하여 클라이언트에서만 동작하도록 해야한다.</li>
<li><strong>비동기 데이터</strong>: 서버에서 동기화되지 않은 비동기 데이터를 사용하면 클라이언트에서 다른 결과가 나타날 수 있다.</li>
</ol>
<blockquote>
<h4 id="hydration-최적화-방법">Hydration 최적화 방법</h4>
<p><strong>동적 import</strong>: 특정 컴포넌트를 클라이언트에서만 렌더링하도록 <code>next/dynamic의 ssr: false 옵션</code>을 사용하는 것이 유용하다.
<strong>상태 초기화 통일</strong>: 서버와 클라이언트의 초기 상태가 일관되게 유지되도록 주의해야 한다. 동일한 초기 상태를 유지하면, <strong>Hydration mismatch</strong>를 줄일 수 있다.
<strong>데이터 Fetching</strong>: <code>getServerSideProps나 getStaticProps</code>를 사용해 서버에서 데이터를 받아 SSR 시 필요한 데이터를 미리 준비할 수 있다.</p>
</blockquote>
<h3 id="ttvtime-to-view--ttitime-to-interactive">TTV(Time to view) / TTI(Time to Interactive)</h3>
<ul>
<li><p><strong>TTV(Time to view)</strong></p>
<p>  TTV는 프리렌더링이 된 서버에 있는 HTML 파일이 브라우저에 보여지기까지의 시간을 의미한다.</p>
</li>
<li><p><strong>TTI(Time to Interactive)</strong></p>
<p>  TTI는 TTV까지 완료된 후 브라우저에 JavaScript를 입혀주는 시간을 의미한다.</p>
</li>
</ul>
<pre><code class="language-javascript">export default function TtvTtiExample() {
    return (
        &lt;div&gt;
            &lt;div&gt;브라우저+서버 철수&lt;/div&gt;
            {process.browser &amp;&amp; &lt;div&gt;브라우저 영희&lt;/div&gt;}
            {typeof window !== &quot;undefined&quot; &amp;&amp; &lt;div&gt;브라우저 훈이&lt;/div&gt;}
            &lt;div&gt;브라우저+서버 맹구&lt;/div&gt;
        &lt;/div&gt;
    );
}</code></pre>
<ul>
<li><strong>TTV: 철수와 맹구</strong><ul>
<li>위 코드에서 보면 프리렌더링된 HTML이 브라우저에 로드되면 사용자는 즉시 &quot;철수&quot;와 &quot;맹구&quot;라는 내용을 볼 수 있다.</li>
</ul>
</li>
<li><strong>TTI: 영희와 훈이</strong><ul>
<li>서버에서는 <strong>프리렌더링</strong>되지 않으며, <strong>클라이언트 측에서 JavaScript</strong>가 실행된 후에만 보여진다.</li>
<li>클라이언트가 JavaScript를 로드하고 실행한 후에야 <code>&quot;영희&quot;</code>와 <code>&quot;훈이&quot;</code>라는 내용이 화면에 추가된다. 즉, 브라우저가 HTML을 표시한 후, JavaScript가 로드되고 실행되어 상호작용이 가능해지는 시점이 TTI다.</li>
</ul>
</li>
</ul>
<blockquote>
<p><strong>TTV와 TTI 최적화 전략</strong>
<strong>코드 분할(Code Splitting)</strong>: <code>React.lazy나 dynamic import</code>를 통해 필요한 JavaScript 코드만 로드하여 TTI를 줄일 수 있다.
<strong>중요 요소 우선 로딩</strong>: 가장 중요한 콘텐츠를 먼저 렌더링하고 나머지 콘텐츠는 <code>지연 로딩(Lazy Loading)</code>하여 TTI를 개선할 수 있다.
<strong>성능 측정 도구 사용</strong>: <code>Lighthouse나 React Profiler</code>를 통해 TTV와 TTI를 측정하고 병목 지점을 파악하는 것이 유용하다.</p>
</blockquote>
<h2 id="critical-rendering-path">Critical Rendering Path</h2>
<p><strong>Critical Rendering Path (CRP)</strong>는 broswer가 <code>HTML, CSS, Javascript</code>를 받아 웹페이지를 사용자에게 렌더링하는 과정을 의미한다. 이 경로는 브라우저가 최종적으로 화면을 표시하기 위해 따라야 하는 일련의 단계로, 웹 성능 최적화를 위해 중요한 개념이다.</p>
<blockquote>
<p><strong>CRP 단계</strong></p>
</blockquote>
<ul>
<li>HTML 파싱 및 DOM(Document Object Model) 생성</li>
<li>CSS 파싱 및 CSSOM(CSS Object Model) 생성</li>
<li>Render Tree 생성(<strong>DOM과 CSSOM 병합</strong>)</li>
<li>Layout 단계 (Reflow)</li>
<li>Painting 단계(Repaint)</li>
</ul>
<h3 id="render-tree">Render Tree</h3>
<h4 id="html-파싱-및-dom-생성">HTML 파싱 및 DOM 생성</h4>
<p><strong>browser</strong>는 서버로부터 <strong>HTML 파일</strong>을 받는다. 이 <strong>HTML 파일</strong>을 파싱하여 <strong>DOM (Document Object Model) Tree</strong>를 생성한다. DOM은 HTML 문서의 구조를 트리 형태로 표현한 것이다.</p>
<pre><code class="language-css">Document
  └─ html
      └─ body
          ├─ h1
          └─ p
</code></pre>
<h4 id="css-파싱-및-cssom-생성">CSS 파싱 및 CSSOM 생성</h4>
<p><strong>broswer</strong>는 HTML에서 CSS 파일을 발견하면 이를 비동기로 가져온다. <strong>CSS 파일</strong>을 파싱하여 <strong>CSSOM (CSS Object Model)</strong>을 생성한다. <strong>CSSOM</strong>은 CSS 스타일 정보를 트리 구조로 표현한 것이다.</p>
<pre><code class="language-css">h1 { color: blue; }
p { font-size: 16px; }</code></pre>
<h4 id="render-tree-생성">Render Tree 생성</h4>
<p>먼저 <strong>Render Tree</strong>란 최종적으로 browser에 표기될 요소들이라고 생각하면 편하다. 현재 HTML파일을 파싱하여 <strong>DOM Tree</strong>를 구축하고, CSS파일 파싱하여 <strong>CSSOM</strong>을 만들었다. <strong>Render Tree</strong> 단계에서는 <strong>DOM &amp; CSSOM</strong>을 결합하여 최종 <strong>Browser</strong>에서 표기될 요소들을 만든다. 이때, 실제 화면에 표현될 요소만 그려진다. 즉 <strong>display : none</strong>으로 설정된 요소는 <strong>Render Tree</strong>에 보여지지 않는다. </p>
<h3 id="reflow와-repaint">Reflow와 Repaint</h3>
<h4 id="reflowlayout">Reflow(Layout)</h4>
<p><strong>Reflow</strong>는 browser가 요소들의 위치와 크기를 계산하는 과정을 의미한다. 이를 <code>Layout 단계</code>라고도 한다. <strong>Reflow</strong>는 <code>DOM과 CSSOM</code>을 기반으로 요소의 레이아웃을 계산하여, 페이지를 <strong>viewport</strong>에 맞게 배치한다.</p>
<blockquote>
<p><strong>Reflow</strong>가 발생하는 경우</p>
</blockquote>
<ul>
<li>DOM 요소가 추가되거나 삭제될 때</li>
<li>요소의 크기, 위치, 너비, 높이가 변경될 때 <strong>(예: width, height, margin, padding 등)</strong></li>
<li>브라우저 창 크기를 변경할 때 (즉, 뷰포트가 변경될 때)</li>
<li>글꼴 크기 변경이나 텍스트 추가/삭제 시</li>
</ul>
<p><strong>Reflow</strong>는 전체 페이지에 영향을 줄 수 있기 때문에, 성능에 큰 영향을 미친다. 특히, 복잡한 레이아웃 구조를 가진 페이지일수록 <strong>Reflow</strong>는 더 많은 비용이 발생한다. <strong>Reflow</strong>가 진행되면 반드시 <strong>Repaint</strong>가 진행되므로 <strong>Reflow 최적화를 잘 해야한다.</strong></p>
<h4 id="repaintpaint">Repaint(Paint)</h4>
<p>Repaint는 요소의 <strong>모양(appearance)</strong>만 변경될 때 발생한다. 이를 <code>Paint 단계</code>라고 한다. 예를 들어, <strong>요소의 색상, 배경, 그림자</strong> 등이 변경될 때 Repaint가 발생한다. <strong>Repaint</strong>는 레이아웃의 변경 없이 시각적인 요소만 다시 그리는 과정이다.</p>
<blockquote>
<p><strong>Repaint</strong>가 발생하는 경우</p>
</blockquote>
<ul>
<li>요소의 <strong>색상(color)</strong>이 변경될 때</li>
<li>배경 색상이나 테두리 색상이 변경될 때</li>
<li>텍스트 색상이 변경될 때</li>
</ul>
<p><strong>Repaint는 Reflow</strong>보다 비용이 덜 들지만, 여전히 페이지 성능에 영향을 줄 수 있다. 특히, 많은 요소들이 Repaint에 영향을 받을 경우 성능이 저하될 수 있다.</p>
<h3 id="crp-최적화-방법">CRP 최적화 방법</h3>
<p><strong>렌더링 차단 리소스 줄이기</strong></p>
<p>CSS와 자바스크립트 파일은 렌더링을 차단할 수 있다.
CSS 파일은 media 속성을 사용해 필요한 경우에만 로드하거나, 자바스크립트 파일은 async 또는 defer 속성을 사용해 비동기로 로드할 수 있다.</p>
<p><strong>CSS를 상단에 배치하고 자바스크립트는 하단에 배치</strong></p>
<p>브라우저는 CSS를 완전히 파싱하기 전에는 페이지를 렌더링하지 않는다. 자바스크립트는 DOM 생성과 CSSOM 생성을 차단할 수 있으므로, 페이지 하단에 배치하거나 비동기로 로드하는 것이 좋다.</p>
<p><strong>Lazy Loading</strong></p>
<p>이미지나 동영상과 같은 비필수 리소스는 Lazy Loading을 사용해 사용자가 필요할 때 로드하도록 설정한다.</p>
<p><strong>Critical CSS 인라인</strong></p>
<p>페이지 로딩 속도를 개선하기 위해 Critical CSS를 인라인으로 추가하여, 페이지의 첫 번째 렌더링을 빠르게 할 수 있다.</p>
<h3 id="prefetch">Prefetch</h3>
<p><strong>prefetch</strong>란 다음페이지에서 쓰려고 미리 받는 것 이며, 현재페이지를 모두 받아온 이후 제일 나중에 다운로드 하게한다. 이를 통해 사용자가 링크를 클릭하기 전에 <strong>필요한 리소스를 사전에 로드함으로써</strong>, 페이지 전환을 훨씬 더 빠르게 할 수 있다.</p>
<blockquote>
<p><strong>Prefetch의 주요 장점</strong>
<strong>빠른 페이지 전환</strong>: 사용자가 링크를 클릭했을 때, 이미 필요한 리소스가 로드된 상태이기 때문에 즉각적으로 페이지를 표시할 수 있다.
<strong>사용자 경험 개선</strong>: 페이지 전환이 느리면 사용자는 불편함을 느낄 수 있다. <code>prefetch를 사용하면</code> 페이지 전환 속도를 개선하여 사용자 경험을 크게 향상시킬 수 있다.
<strong>브라우저 유휴 시간 활용</strong>: 사용자가 페이지와 상호작용하지 않을 때, 브라우저는 백그라운드에서 리소스를 미리 로드한다. 이를 통해 네트워크 대역폭을 효율적으로 사용할 수 있다.
<strong>SEO에 긍정적인 영향</strong>: 빠른 로딩 속도는 <strong>SEO(검색 엔진 최적화)</strong>에도 긍정적인 영향을 미친다. 페이지 로딩 속도가 빠르면 검색 엔진에서 더 높은 평가를 받을 수 있다.</p>
</blockquote>
<h4 id="nextjs에서-nextlink와-prefetch-활용하기">Next.js에서 next/link와 prefetch 활용하기</h4>
<p><strong>Next.js</strong>는 <strong>next/link 컴포넌트</strong>를 사용하여 내부 페이지 간 링크를 쉽게 관리할 수 있다. 기본적으로 <strong>next/link</strong>는 사용자가 화면에 링크를 볼 때 자동으로 페이지를 <strong>prefetch</strong>한다. 이 기능을 통해 <strong>Next.js</strong> 애플리케이션에서 빠른 페이지 전환을 제공할 수 있다.</p>
<pre><code class="language-jsx">import Link from 'next/link';

function HomePage() {
  return (
    &lt;div&gt;
      &lt;Link href=&quot;/about&quot;&gt;
        &lt;a&gt;Go to About Page&lt;/a&gt;
      &lt;/Link&gt;
    &lt;/div&gt;
  );
}

export default HomePage;</code></pre>
<p>위 코드에서 사용된 <strong>next/link</strong>는 자동으로 prefetch 기능을 사용하여 /about 페이지를 백그라운드에서 미리 로드한다.</p>
<h3 id="preload">Preload</h3>
<p><strong>Preload</strong>는 <strong>browser</strong>가 페이지 로딩 시 가장 중요한 리소스를 우선적으로 로드하도록 지시하는 기술이다. <code>HTML &lt;link&gt; 태그</code>를 사용하여 브라우저가 리소스를 비동기로 미리 가져오도록 하지만, 브라우저가 해당 리소스를 최우선으로 로드하도록 한다. Preload의 사용 목적으로 <strong>필수적인 리소스</strong>를 더 빠르게 로드하여 초기 페이지 로딩 시간을 줄이기 위해 사용된다. 또한 <strong>browser</strong>가 <strong>HTML</strong>을 파싱하는 동안 렌더링 차단 리소스를 우선적으로 로드하도록 지시한다.
<strong>async 또는 defer</strong>로 비동기로 로드되는 스크립트보다 우선순위를 높일 수 있어 중요한 리소스를 더 빠르게 불러올 수 있다.</p>
<h3 id="웹-성능-지표">웹 성능 지표</h3>
<h4 id="1-lcp-largest-contentful-paint-가장-큰-콘텐츠-표시-시간">1. LCP (Largest Contentful Paint): 가장 큰 콘텐츠 표시 시간</h4>
<p><strong>LCP</strong>는 사용자가 페이지를 로드할 때 가장 큰 콘텐츠가 표시되는 시간을 측정한다. 페이지 로딩 성능을 평가하는 지표로, 주요 콘텐츠가 얼마나 빨리 화면에 표시되는지를 나타낸다.
예를 들어, Hero 이미지, 대형 배너, 주요 텍스트 블록 등의 가장 큰 시각적 요소가 로딩되기까지 걸리는 시간을 측정한다.
<strong>좋은 LCP 기준: 2.5초 이내: 우수 / 2.5초 ~ 4초: 개선 필요 / 4초 이상: 성능 저하</strong></p>
<blockquote>
<p><strong>LCP 최적화 방법</strong></p>
</blockquote>
<ul>
<li><strong>이미지 최적화</strong>: WebP와 같은 경량 포맷 사용 및 이미지 압축</li>
<li>CSS 및 폰트 파일을 비동기로 로드 <strong>(async, defer).</strong></li>
<li><strong>Lazy Loading</strong>을 사용하여 화면에 보이지 않는 이미지나 비디오를 지연 로드.</li>
<li>서버 응답 시간을 줄이기 위해 CDN 사용 및 캐싱 최적화.</li>
</ul>
<h4 id="2-fid-first-input-delay-첫-입력-지연-시간">2. FID (First Input Delay): 첫 입력 지연 시간</h4>
<p><strong>FID</strong>는 사용자가 페이지와 처음 상호작용했을 때, 브라우저가 반응하는 데 걸리는 시간을 측정한다. 사용자 입력(버튼 클릭, 링크 클릭, 텍스트 필드 입력 등)에 대한 브라우저 반응 속도를 평가한다. 페이지 로딩 후 첫 번째 사용자 입력이 발생하고, 브라우저가 이를 처리하기 시작하기까지의 지연 시간을 측정한다.
<strong>좋은 FID 기준: 100ms 이내: 우수 / 100ms ~ 300ms: 개선 필요 / 300ms 이상: 성능 저하</strong></p>
<blockquote>
<p><strong>FID 최적화 방법</strong></p>
</blockquote>
<ul>
<li>JavaScript 실행 시간을 줄이기: 번들 크기 축소, 코드 스플리팅 사용.</li>
<li>비동기 JavaScript 로드 (async 및 defer 사용).</li>
<li>중복된 스크립트를 제거하고, 필수적인 스크립트만 로드.
Web Worker를 사용하여 메인 스레드 작업 분산.</li>
</ul>
<h4 id="3-cls-cumulative-layout-shift-누적-레이아웃-이동">3. CLS (Cumulative Layout Shift): 누적 레이아웃 이동</h4>
<p><strong>CLS</strong>는 페이지 로딩 중 예상치 못한 레이아웃 변경이 발생하는 정도를 측정한다. 사용자가 페이지를 보는 동안 텍스트, 이미지, 버튼 등의 위치가 갑자기 이동하는 현상을 측정한다.
예를 들어, 이미지가 지연 로드되면서 텍스트가 갑자기 밀리거나 광고가 로드되면서 버튼이 이동하는 경우가 이에 해당한다.
<strong>좋은 CLS 기준: 0.1 이하: 우수 / 0.1 ~ 0.25: 개선 필요 / 0.25 이상: 성능 저하</strong></p>
<blockquote>
<p><strong>CLS 최적화 방법</strong></p>
</blockquote>
<ul>
<li>이미지 및 비디오 요소에 크기(높이 및 너비)를 명시적으로 지정하여, 레이아웃 이동을 방지.</li>
<li><strong>웹 폰트 로딩 중 FOUT(Flash of Unstyled Text)나 FOIT(Flash of Invisible Text)</strong>를 방지하기 위해 font-display: swap 사용.</li>
<li>광고 및 iframe 콘텐츠를 적절한 위치에 고정하고, 로드 후 레이아웃에 영향을 주지 않도록 설정.</li>
<li>애니메이션 사용 시 위치 이동보다는 opacity나 transform 속성을 활용</li>
</ul>