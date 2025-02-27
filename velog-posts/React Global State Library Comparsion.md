<h2 id="서론">서론</h2>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/86a12809-be83-4288-a25e-272bc95ebe6e/image.png" /></p>
<p>다들 react를 개발하면서 <strong>state를 관리하는 라이브러리</strong>를 사용하지 않는 경우는 거의 없을 것이다. 필자 또한 init setting할 때 자연스레 상태관리 라이브러리를 설치한다. <code>useState / useContext</code>로만으로 react를 처리하기엔 한계가 명확하기 때문이다. </p>
<p>필자는 주로 <strong>Zustand</strong>를 이용하여 전역 상태 관리를 하였다. 문뜩 다른 상태관리 라이브러리가 궁금했었고 이번 기회에 좀 더 깊게 공부하고자 블로그를 정리한다. <del>재미있겠다.</del></p>
<h1 id="상태관리-라이브러리">상태관리 라이브러리</h1>
<p><strong>Data Fetching</strong>을 관리하는 상태관리 라이브러리(<code>react query / swr / apollo client</code>)가 아닌 <strong>Global State</strong>를 관리하는 상태관리 라이브러리는 크게 <code>zustand / redux / mobx / jotai / recoil</code>이 있다. </p>
<p>라이브러리를 설명하기 앞서 필자가 왜 <code>context API</code>를 선호하지 않은 이유에 대해서 말해보겠다. </p>
<h3 id="context-api">context API</h3>
<p>react는 유지보수뿐만 아니라 데이터 흐름을 예측 가능하고 직관적으로 만들기 위해 기본적으로 <strong>양방향 데이터 바인딩</strong>이 아닌 <strong>단방향 데이터 바인딩</strong>이다. 따라서 서로 다른 컴포넌트에서 데이터를 사용하고 싶을 땐 공통으로 묶이는 부모 컴포넌트에서 <strong>state</strong>를 관리하게 된다. 또한 서로 다른 컴포넌트에서 해당 state를 사용하기 위해선 <strong>props drilling</strong>을 통해 state를 받는다. </p>
<p>다만 이 방식은 <strong>가독성</strong>에서 문제가 있다. props로 2개 ~ 3개정도의 layer를 넘겨준다면 문제가 없겠지만 그 이상의 layer로 state를 넘겨준다고 가정해보자. 그럼 state를 계속계속 내려야해서 중복 코드도 늘어날 것이며, 가독성 측면에서 보기 힘들 것이다.</p>
<p>react팀도 이를 판단하고 새로 만든 훅이 <strong>useContext</strong>이다. 해당 staet를 사용하는 공통 컴포넌트에서 <code>context.provider</code>를 선언하여 value에 state를 넣어주면 proivder layer로 묶인 모든 컴포넌트에서 해당 state를 사용할 수 있다. </p>
<pre><code class="language-jsx">import { useContext } from &quot;react&quot;;
import { UserContext } from &quot;./UserContext&quot;;

function Child() {
  const user = useContext(UserContext);

  return &lt;div&gt;{user.name}&lt;/div&gt;;
}</code></pre>
<p>useContext를 이용하여 해당 state가 필요한 컴포넌트에서 state를 호출하여 사용할 수 있게 하는것이다. <strong>useState + props drilling</strong>을 이용하는 것과 비교하자면 가독성 + 유지보수 측면에서 훨씬 개선되었다는 것을 확인할 수 있을 것이다. </p>
<p>다만 이 또한 문제가 있다. <code>provider</code>로 묶인 모든 component는 state의 변경이 일어났다면 전부 리렌더링이 되는 것이다. 해당 state가 필요없는 부분 조차 전부 리렌더링이 된다는 것이다. 이는 한 두번 일어나면 문제가 없겠지만 수십번 수백번이 일어나면 문제가 발생할 것이다. </p>
<p>이를 해결하기위해 <strong>React.Memo</strong>가 탄생하였는데, 해당 useEffect의 Dependency Array처럼 state가 변경될 때만 해당 컴포넌트를 리렌더링 시키게 하는 것이다. 리렌더링 최적화 측면에서 좋겠지만 이 또한 문제가 있다.. 가독성이 이라는 것이다. 극단적인 예시긴 하지만 예를 들어보자면, 제일 최상위에서 state를 선언해 provider로 넘겨준다고 가정해보자 해당 state가 필요한 component는 제일 하단 (100번째)의 컴포넌트들이라고 가정하면 나머지 컴포넌트들은 <strong>전부 React Memo</strong>를 달아줘야하는 것이다.</p>
<p>따라서 이러한 문제들 때문에 필자는 useContext를 선호하지 않는다. 당연히 한 5개단위정도의 layer에서의 props drlling이 발생한다면 사용하겠지만 서로 멀리 있는 컴포넌트에서의 state 관리는 <strong>반드시 상태관리 라이브러리</strong>를 사용한다.</p>
<h2 id="pattern">Pattern</h2>
<p>상태를 관리함에 있어 다양한 라이브러리가 존재하고 각 라이브러리 별로 state를 관리하는 특징들이 있다.</p>
<h4 id="flux-패턴">Flux 패턴</h4>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/d845ba13-d885-4a4e-bf7c-791a4379b25c/image.png" />
flux 패턴이란, 2014년 페이스북 컨퍼런스에서 발표된 아키텍처로, <strong>Client-Side 웹 애플리케이션</strong>을 만들기 위해 사용하는 디자인 패턴이다. </p>
<blockquote>
<p><strong>기존 client-side에서의 MVC 패턴의 문제점</strong></p>
</blockquote>
<pre><code class="language-javascript">// vue.js
&lt;template&gt;
  &lt;input v-model=&quot;message&quot; /&gt;
  &lt;p&gt;{{ message }}&lt;/p&gt;
&lt;/template&gt;
&gt;
&lt;script&gt;
export default {
  data() {
    return {
      message: &quot;Hello, World!&quot;
    };
  }
};
&lt;/script&gt;</code></pre>
<p>message (Model)가 변경되면 <code>&lt;p&gt; (View)</code>가 자동 업데이트됨.
<code>&lt;input&gt;(View)</code>에서 사용자가 값을 변경하면 message (Model)도 자동 업데이트됨.</p>
<blockquote>
</blockquote>
<p>현재 간단한 입력 단계에선 큰 문제가 없지만 점차 규모가 커지면 서로 의존하고 있는 view - model이 생겨날 것이고, <strong>state 변화에 대해 예측하기 힘들것이다.</strong>
<img alt="" src="https://velog.velcdn.com/images/rjs8833/post/8b321ce2-9aa1-4482-8a5f-ee68876271ff/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/c8ccb4dc-caa3-4fb0-b0ba-5092adaa0b3e/image.png" /></p>
<p>따라서 기존 <strong>MVC</strong>패턴의 문제를 타파하고자 facebook에서 <strong>Flux</strong> 패턴을 제안하였다. 이는 데이터 흐름을 양방향으로 컨트롤하는 게 아닌 <strong>단방향</strong>으로 처리함으로써 예측 가능한 어플리케이션을 만들자고 제안하였다.</p>
<h4 id="action">action</h4>
<p><strong>action</strong>이란 사용자의 event 또는 서버 액션을 나타낸다. 예를 들어서, user가 A 값을 입력한다든지, server에서 어떤 데이터를 받았다는 지. 모든 행동을 <strong>action</strong>이라고 칭한다. <strong>action</strong>은 <code>type</code>으로 어떤 행동인지 정의하고, <code>payload</code>로 데이터를 넘겨주는 형태로 구성되어 있다. <code>type</code>은 <strong>required</strong>지만 <code>payload</code>는 <strong>optional</strong>로 필수가 아니다.</p>
<pre><code class="language-javascript">const action = {
  type: &quot;ADD_TODO&quot;,
  payload: { id: 1, text: &quot;Redux 공부하기&quot; }
};</code></pre>
<h4 id="dispatch">dispatch</h4>
<p>flux 패턴의 특징으로 <strong>모든 이벤트 처리를 한 곳에 하는 것</strong>이다. 이벤트를 한 곳에서 처리함으로써, 예측 가능한 상태관리 및 미들웨어 적용이 용이해진다. 
<strong>dispatch</strong>는 <strong>모든 action</strong>을 한 곳에서 처리하고 알맞은 <strong>store</strong>로 전달하는 역할을 한다. 여기서 핵심은 <strong>store의 업데이트는 반드시 dispatch</strong>로만 가능하다는 것이다. </p>
<h4 id="store">store</h4>
<p><strong>store</strong>는 어플리케이션에서의 state를 저장하는 곳이다. <strong>dispatch</strong>를 통해 <strong>action</strong>을 받아 <strong>state</strong>를 업데이트하며 업데이트한 <strong>state</strong>를 <strong>view</strong>로 전달한다.</p>
<h4 id="view">view</h4>
<p><strong>view</strong>는 실제 UI를 담당한다. <strong>store</strong>에서 필요한 <strong>state</strong>를 불러와 ui를 그리며, event가 발생하면 <strong>action</strong>이 실행되어서 dispatch를 호출한다.</p>
<p><strong>Flux</strong> 패턴은 위와 같이 예측 가능한 상태관리가 가능하지만, 단점으로는 <strong>Boilerplate</strong>가 존재한다는 것이다. state를 관리하기 위해서, <code>action / dispatch / store</code>라는 형태가 강제되기 때문이다. 따라서 단순 state를 관리한다면 flux 패턴을 기반으로 둔 상태관리 라이브러리는 부적합하다. </p>
<h3 id="atomic-패턴">Atomic 패턴</h3>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/d2202ded-8b8e-49aa-88a1-cdb579a98945/image.png" /></p>
<p><strong>atom</strong>이란 원자를 의미한다. 즉 flux 패턴과 다르게 한 곳에서 모든 state를 관리하는 것이 아닌, <strong>각 분자별로 state를 분리하여 관리한다는 것이다.</strong></p>
<p><strong>Atomic 패턴</strong>은 상태 관리 시스템을 설계하는 방식으로, 상태를 독립적이고 작은 단위로 분리하여 관리하는 패턴이다. 이렇게 하면 각 상태가 독립적으로 변경되고, 다른 상태에 영향을 주지 않으며 필요한 컴포넌트에서만 상태를 구독할 수 있게 된다. </p>
<blockquote>
<p>** Atomic 패턴 장점**</p>
</blockquote>
<p><strong>독립적인 상태 관리</strong>
각 상태는 서로 독립적으로 동작하고, 다른 상태에 영향을 미치지 않는다. 필요한 상태만 구독하고 관리할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>컴포넌트의 세밀한 업데이트</strong>
상태가 변경될 때, 해당 상태를 구독하고 있는 컴포넌트만 리렌더링되기 때문에 불필요한 렌더링을 줄일 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>모듈화와 확장성</strong>
상태 관리가 독립적으로 이루어지므로, 애플리케이션의 규모가 커져도 상태 관리가 훨씬 더 효율적이고 확장 가능해진다.</p>
<h2 id="global-state-library">Global State Library</h2>
<h3 id="zustand---zustand-공식홈페이지">Zustand - <a href="https://zustand-demo.pmnd.rs/">zustand 공식홈페이지</a></h3>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/5b70f4f3-f94b-4e86-b74a-e322913e923e/image.png" /></p>
<p><del>zustand의 가장 큰 포인트는 <strong>귀여운 곰</strong>이다..</del> </p>
<blockquote>
<p><em>Zustand는 독일어로 '상태'</em></p>
</blockquote>
<p><strong>zustand</strong>는 작고 빠르며 확장 가능한 React 프로젝트에서 사용하는 상태 관리 라이브러리이다. zustand의 핵심은 <strong>'작고, 빠르다'</strong>이다. 일단 패키지 번들 사이즈 <code>5.0.3</code>기준 <strong>87.1KB</strong>이다. 다른 상태관리 라이브러리와 비교했을 때 꽤나 큰 차이를 보인다. </p>
<p><strong>zustand</strong>가 각광받고 있는 이유는 다음과 같다. 먼저 가볍다. 너무 가볍다. <strong>RTK</strong>와 비교했을 때 약 80배 정도된다. 최근 frontend 트렌드를 보면 성능 최적화가 대두되고 있는데 이 상황에서의 <strong>zustand</strong>의 번들 크기는 무시하지 못할 것이다. </p>
<p>또한, 러닝커브가 상당히 낮다. <strong>RTK</strong>만 봐도, <code>action / dispatch / store / selector</code> 등 배워야할 것이 많은데, zustand 같은 경우, <code>create()</code>를 통해 state를 정의하고 <strong>정의한 store명</strong> <code>ex)useStore()</code>를 통해 state만 호출하면 끝이다. </p>
<p>zustand는 <strong>flux</strong> 패턴이나 <strong>action</strong> 로직이 없기 때문에, redux에 비해 상대적으로 구조화가 되어 있지않다. 이는, <code>비즈니스 로직과 상태 업데이트 로직</code>이 섞일 가능성이 높다. 또한, 불필요한 <strong>re-render</strong>를 유발할 수도 있는데, store 전체를 구독하고 있으면, 모든 변경 시 리렌더링을 발생하기 때문이다. 따라서 store를 구독할 때는 필요한 state만 구독하는 편이 re-render를 덜 유발할 것이다. </p>
<h4 id="store-1">Store</h4>
<blockquote>
<pre><code class="language-jsx">import { create } from 'zustand'
</code></pre>
</blockquote>
<p>const useStore = create((set) =&gt; ({
  count: 1,
  inc: () =&gt; set((state) =&gt; ({ count: state.count + 1 })),
}))</p>
<blockquote>
</blockquote>
<p>function Counter() {
  const count = useStore((state) =&gt; state.count)
  const inc = useStore((state) =&gt; state.inc)</p>
<blockquote>
</blockquote>
<p>  return (
    <div>
      <span>{count}</span>
      <button>one up</button>
    </div>
  )
}</p>
<pre><code>
#### 사용법
```jsx
import { create } from 'zustand'

export const use__Store = create&lt;{
    state: type
    action: () =&gt; void
}&gt;((set, get) =&gt; {
  return {
    state: init,
    action: function
  }
})</code></pre><p><strong>store naming convention</strong>은 접두사 <code>use</code> / 접미사 <code>Store</code>이다. 
<strong>ex) useTodoListStore</strong> 
<code>create</code>를 통해 store를 생성하며, callback으로 <code>set과 get</code>을 받는다. </p>
<blockquote>
<p><strong>set, get</strong> 정의
<strong>set</strong>
현재 상태를 기반으로 <strong>새로운 상태를 설정하는 함수다</strong>. 상태 변경 로직이 간단하고, 현재 상태를 바로 읽고 업데이트하는 경우에 주로 사용된다. set에서 callback함수로 state를 호출할 수 있는데, store 내부에 있는 state를 직접적으로 접근하여 값을 업데이트할 수 있다. 이때, 얕은 복사형태로 값을 업데이트하는 게 아닌 <strong>새로운 객체를 생성하여 state를 업데이트한다.</strong></p>
</blockquote>
<pre><code class="language-jsx">import create from 'zustand';
&gt;
const useStore = create&lt;{
  count: number, 
  increase: () =&gt; void 
}&gt;((set) =&gt; ({
  count: 0, 
  increase: () =&gt; set((state) =&gt; ({ count: state.count + 1 })),  
}));</code></pre>
<blockquote>
</blockquote>
<p><strong>get</strong> 
현재 상태를 <strong>읽을 수 있게 해주는 함수다</strong>. get을 사용하면 상태를 읽고 그 값을 기반으로 더 복잡한 상태 업데이트를 처리할 수 있다. get은 상태를 비동기적으로 읽어야 할 때나, 상태 값을 명시적으로 조회해야 할 때 유용하다.</p>
<pre><code class="language-jsx">import create from 'zustand';
&gt;
const useStore = create&lt;{
  count: number, 
  increase: () =&gt; void 
}&gt;((set, get) =&gt; ({
  count: 0, 
  increase: () =&gt; {
    const { count } = get()
    set({ count: count + 1 })
  } 
}));</code></pre>
<h3 id="redux-toolkit---rtk-공식홈페이지">Redux-Toolkit - <a href="https://redux-toolkit.js.org/">RTK 공식홈페이지</a></h3>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/35f1e339-e2af-4c75-98f1-96aff207996b/image.png" /></p>
<blockquote>
<p><em>Redux는 reduce + flux</em></p>
</blockquote>
<p><strong>NPM Trends기준으로</strong> Redux만 봤을 때, 이를 이길 수 있는 라이브러리는 없을 것이다. 그만큼 <strong>근본</strong>있는 라이브러리이다. <strong>Flux</strong> 패턴 기반으로 <strong>state</strong>를 업데이트하며, 중앙 집중식 데이터 흐름, 강력한 <code>devtools</code>, 다양한 <code>middleware</code>, 풍부한 커뮤니티, etc</p>
<p>다만,, <strong>Boilerplate</strong>가 많다... 가볍게 state 처리할 때는 비효율적이다. <code>distpatch action store selector</code>... </p>
<blockquote>
<ul>
<li><a href="https://velog.io/@rjs8833/Redux-Overview-and-Concepts">redux 정리</a></li>
</ul>
</blockquote>
<ul>
<li><a href="https://velog.io/@rjs8833/React-Redux-RTK">react-redux</a></li>
<li><a href="https://velog.io/@rjs8833/React-Redux-TodoList-Example">redux-example</a></li>
</ul>
<h3 id="jotai---jotai-공식홈페이지">Jotai - <a href="https://jotai.org/">Jotai 공식홈페이지</a></h3>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/f421ffd1-59c5-4419-8c94-9277967b79ce/image.png" /></p>
<blockquote>
<p><em>Jotai는 일본어로 '상태'</em></p>
</blockquote>
<p>jotai에서 소개하듯이 <strong>primitive and flexible state management for React</strong>. 리액트를 위한 <strong>원시적이며 유연한 상태관리 도구</strong>이다. </p>
<p><strong>atom 기반</strong>으로 동작하며 <strong>redux</strong>와 다르게 중앙 한곳에서 관리하는 게 아닌 atom에서 state를 관리한다. <code>useState</code>와 형태가 비슷하여 러닝커브가 상대적으로 낮으며, 한 곳에서 state를 관리하는 게 아니라 <strong>각각 state를 관리하고 있기 때문에</strong> <code>의존성 측면</code>이라던지 <code>성능 최적화</code>측면에서 유리하다. </p>
<p>세밀한 제어가 가능하다는 것은, <strong>각각 atom을 구독</strong>하기 때문에 state간의 영향이 적다. state가 독립적으로 존재하며 state를 사용하는 component만 re-render 되기 때문에 state간의 의존도를 낮춰 리렌더링을 최소화할 수 있다.</p>
<p>하지만 <strong>jotai</strong>는 각각 <strong>atom</strong>에서 관리하기 때문에, 전반적인 application의 state 흐름을 한눈에 파악하기 힘들고, <code>devtool</code>도 갖고 있지 않아 디버깅이 어렵다.</p>
<h4 id="atom">atom</h4>
<pre><code class="language-jsx">import { atom } from 'jotai'

const priceAtom = atom(10)
const messageAtom = atom('hello')
const productAtom = atom({ id: 12, name: 'good stuff' })</code></pre>
<p><strong>atom</strong>은 <strong>React에서 <code>useState</code></strong>와 비슷한 역할을 한다. 다만 컴포넌트 내부에서 사용하는 것이 아닌 전역에서 사용할 수 있다. state를 저장할 때 사용되며 <code>useAtom</code>을 이용하여 atom을 불러와서 component에서 사용한다. </p>
<p>atom은 세가지 <strong>pattern</strong>이 있다. </p>
<blockquote>
<p><strong>Derived Atom</strong></p>
</blockquote>
<ul>
<li><strong>Read-only Atom</strong></li>
<li><em>Read-only Atom*</em>은 상태를 읽을 수만 있고, 수정할 수는 없는 상태를 의미한다. Read-Only Atom은 의존하고 있는 다른 Atom이 업데이트되면, 해당 값도 자동으로 업데이트된다. <pre><code class="language-jsx">const readOnlyAtom = atom((get) =&gt; get(priceAtom) * 2)</code></pre>
<blockquote>
</blockquote>
</li>
<li><strong>Write-only Atom</strong></li>
<li><em>Write-only Atom*</em>은 상태를 읽을 수 없으며 수정만 할 수 있는 상태를 의미한다. 주로 액션을 통해 어떤 변화가 필요할 때 사용된다. <strong>Read-only Atom</strong>과 다르게 get argument의 atom이 변화가 일어나면 자동으로 state가 업데이트 되지 않는다. <code>set</code>을 통해 state를 업데이트하며, <code>update</code>는 상태를 변경할 때 외부에서 전달되는 값으로, 예를 들어 사용자가 입력한 값이나 다른 액션의 결과를 반영할 수 있다.<blockquote>
</blockquote>
특이하게 <strong>첫 번째 인자로 null을 넣는데</strong>, 이는 <strong>read only</strong>에서는 첫번째 인자로 state를 넣기 때문이다. 즉, <strong>write only</strong> 같은 경우 값을 읽지 못하고 수정만 할 수 있기 때문에 <strong>명시적으로 null을 넣으면서, 읽을 값이 없다는 것을 나타낸다.</strong><pre><code class="language-jsx">const writeOnlyAtom = atom(
 null, // it's a convention to pass `null` for the first argument
 (get, set, update) =&gt; {
   // `update` is any single value we receive for updating this atom
   set(priceAtom, get(priceAtom) - update.discount)
   // or we can pass a function as the second parameter
   // the function will be invoked,
   //  receiving the atom's current value as its first parameter
   set(priceAtom, (price) =&gt; price - update.discount)
 },
)</code></pre>
</li>
<li><strong>Read-Write Atom</strong></li>
<li><em>Read-Write Atom*</em>은 상태를 읽음 동시에 수정도 가능하다. <pre><code class="language-jsx">const readWriteAtom = atom(
 (get) =&gt; get(priceAtom) * 2,
 (get, set, newPrice) =&gt; {
   set(priceAtom, newPrice / 2)
   // you can set as many atoms as you want at the same time
 },
)</code></pre>
</li>
</ul>
<h4 id="useatom">useAtom</h4>
<p><strong>useAtom</strong>은 <strong>react component</strong> 내부에서 atom을 접근할 때 사용하는 hook이다. <strong>React의 <code>useState</code></strong>와 같은 형식으로 튜플형태로 atom을 호출할 수 있다. </p>
<p>atom은 <strong>WeakMap 형태</strong>로 저장되는데 <strong>key로 atom으로 넣어줘야 WeakMap에 접근하여 state와 setState를 불러올 수 있는 것이다.</strong> 해당 atom이 업데이트되면 자동으로 컴포넌트를 리렌더링 시켜준다.</p>
<blockquote>
<pre><code class="language-jsx">const [atomValue] = useAtom(atom(0))</code></pre>
</blockquote>
<pre><code>위와 같은 useAtom argument로 atom을 선언하게 되면 무한루프를 야기할 수 있다. 따라서 미리 atom을 선언하여 useAtom에 atom 객체를 넣어줘야한다.

```jsx
const stableAtom = atom(0)
const Component = () =&gt; {
  const [atomValue] = useAtom(stableAtom) // This is fine
  const [derivedAtomValue] = useAtom(
    useMemo(
      // This is also fine
      () =&gt; atom((get) =&gt; get(stableAtom) * 2),
      [],
    ),
  )
}</code></pre><blockquote>
<p><strong>useAtomValue / useSetAtom</strong>
<strong>useAtomValue</strong>은 <strong>atom</strong>을 읽을 때만 사용하는 hook이다. 주로 state만 필요한 경우에 사용한다. <strong>Read-Only Atom</strong> 같은 경우 <code>useAtomValue</code>를 사용한다. </p>
</blockquote>
<pre><code class="language-jsx">const countAtom = atom(0)
&gt;
const Counter = () =&gt; {
  const count = useAtomValue(countAtom)
&gt;
  return &lt;div&gt;count: {count}&lt;/div&gt;
}</code></pre>
<blockquote>
</blockquote>
<p><strong>useSetAtom</strong>은 <strong>atom</strong>을 업데이트할 때만 사용하는 hook이다. 주로 읽기가 필요없고 업데이트만 필요한 경우에 사용한다. <strong>Write-Only Atom</strong> 같은 경우 <code>useSetAtom</code>을 사용한다. </p>
<pre><code class="language-jsx">const switchAtom = atom(false)
&gt;
const SetTrueButton = () =&gt; {
  const setCount = useSetAtom(switchAtom)
  const setTrue = () =&gt; setCount(true)
&gt;
  return (
    &lt;div&gt;
      &lt;button onClick={setTrue}&gt;Set True&lt;/button&gt;
    &lt;/div&gt;
  )
}
&gt;
const SetFalseButton = () =&gt; {
  const setCount = useSetAtom(switchAtom)
  const setFalse = () =&gt; setCount(false)
&gt;
  return (
    &lt;div&gt;
      &lt;button onClick={setFalse}&gt;Set False&lt;/button&gt;
    &lt;/div&gt;
  )
}
&gt;
export default function App() {
  const state = useAtomValue(switchAtom)
&gt;
  return (
    &lt;div&gt;
      State: &lt;b&gt;{state.toString()}&lt;/b&gt;
      &lt;SetTrueButton /&gt;
      &lt;SetFalseButton /&gt;
    &lt;/div&gt;
  )
}</code></pre>
<h3 id="recoil---recoil-공식홈페이지">Recoil - <a href="https://recoiljs.org/ko/">Recoil 공식홈페이지</a></h3>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/cfbf4edd-4b7c-432f-aa68-50bf2c814ff3/image.png" /></p>
<blockquote>
<p><del>recoil은 maintainer가 그만 둔 상태로 2025년 1월 12일 deprecated 상태가 되었다.</del></p>
</blockquote>
<p><strong>Recoil</strong>은 facebook에서 만든 오픈소스로서, 현재 react의 <code>useState / useContext</code>의 문제점을 타파하기 위해 생긴 상태관리 라이브러리이다. <strong>Recoil은 Jotai와 비슷하게 Atomic 패턴으로 state를 관리한다.</strong></p>
<h4 id="atom-1">atom</h4>
<p><strong>Jotai</strong>와 동일하게 <strong>Recoil은 Atom</strong>에 state을 저장하고 관리한다. react의 <code>useState</code>처럼 동작하지만, 전역적으로 공유가 가능하다. 또한 Jotai의 atom과는 다르게, atom 내부에 <code>key, default</code>라는 값이 존재한다. <strong>key</strong>는 내부적으로 state를 고유하게 식별하고 관리하기 위해 필요하며, <code>React Devtools</code>를 통해 디버깅을 확인할 수 있다. </p>
<pre><code class="language-jsx">const fontSizeState = atom({
  key: 'fontSizeState',
  default: 14,
});</code></pre>
<h4 id="selector">selector</h4>
<p><strong>selector</strong>는 <strong>Jotai</strong>의 derrived atom과 유사한 역할을 한다. <strong>selector</strong>는 순수함수이며, atom의 state를 구독하여 atom의 state가 변경되면 리렌더링된다. </p>
<p>atom과 마찬가지로 selector를 구분하는 고유의 key가 존재한다. <code>get</code>를 통해 atom을 불러와 값을 파생시키며, set을 통해 atom을 업데이트 시킬 수 있다. <code>useReducer</code> 와 유사한 역할을 한다고 보면 된다. </p>
<pre><code class="language-jsx">const fontSizeState = atom({
  key: 'fontSizeState',
  default: 14,
});

const fontSizeSelector = selector({
  key: 'fontSizeSelector',
  get: ({ get }) =&gt; {
    const fontSize = get(fontSizeState);
    return fontSize + 'px';
  },
  set: ({ set }, newValue) =&gt; {
    set(fontSizeState, newValue);
  },
});</code></pre>
<h4 id="userecoilstate">useRecoilState</h4>
<p>React 컴포넌트 내부에서 state를 호출할 때 사용하는 <strong>hook</strong>이다. React의 <code>useState</code>와 유사하다고 보면 된다. <strong>Jotai</strong>에서는 <code>useAtom</code>과 유사한 친구라고 보면 된다. </p>
<pre><code class="language-jsx">function FontButton() {
  const [fontSize, setFontSize] = useRecoilState(fontSizeState);
  return (
    &lt;button onClick={() =&gt; setFontSize((size) =&gt; size + 1)} style={{fontSize}}&gt;
      Click to Enlarge
    &lt;/button&gt;
  );
}</code></pre>
<blockquote>
<p><strong>useSetRecoilState / useRecoilvalue / useResetRecoilState</strong> 
<strong>useSetRecoilState</strong>는 <strong>Jotai의 useSetAtom</strong>과 유사한 역할을 한다. 주로 쓰기전용 recoil hook으로 사용된다.</p>
</blockquote>
<pre><code class="language-jsx">import {atom, useSetRecoilState} from 'recoil';
&gt;
const namesState = atom({
  key: 'namesState',
  default: ['Ella', 'Chris', 'Paul'],
});
&gt;
function FormContent({setNamesState}) {
  const [name, setName] = useState('');
  &gt;
  return (
    &lt;&gt;
      &lt;input type=&quot;text&quot; value={name} onChange={(e) =&gt; setName(e.target.value)} /&gt;
      &lt;button onClick={() =&gt; setNamesState(names =&gt; [...names, name])}&gt;Add Name&lt;/button&gt;
    &lt;/&gt;
)}
&gt;
// This component will be rendered once when mounting
function Form() {
  const setNamesState = useSetRecoilState(namesState);
  &gt;
  return &lt;FormContent setNamesState={setNamesState} /&gt;;
}</code></pre>
<p><strong>useRecoilValue</strong>는 <strong>Jotai의 useAtomValue</strong>와 유사한 역할을 한다. 주로 읽기 전용 recoil hook으로 사용된다.</p>
<pre><code class="language-jsx">import {atom, selector, useRecoilValue} from 'recoil';
&gt;
const namesState = atom({
  key: 'namesState',
  default: ['', 'Ella', 'Chris', '', 'Paul'],
});
&gt;
const filteredNamesState = selector({
  key: 'filteredNamesState',
  get: ({get}) =&gt; get(namesState).filter((str) =&gt; str !== ''),
});
&gt;
function NameDisplay() {
  const names = useRecoilValue(namesState);
  const filteredNames = useRecoilValue(filteredNamesState);
&gt;
  return (
    &lt;&gt;
      Original names: {names.join(',')}
      &lt;br /&gt;
      Filtered names: {filteredNames.join(',')}
    &lt;/&gt;
  );
}</code></pre>
<blockquote>
</blockquote>
<p><strong>Jotai</strong>와 다르게 <strong>Recoil</strong>에는 <strong>useResetRecoilValue</strong>라는 hook이 존재하는데, hook 이름처럼 <strong>default value</strong>로 초기화하고 싶을 때 사용하는 hook이다.</p>
<pre><code class="language-jsx">import {todoListState} from &quot;../atoms/todoListState&quot;;
&gt;
const TodoResetButton = () =&gt; {
  const resetList = useResetRecoilState(todoListState);
  return &lt;button onClick={resetList}&gt;Reset&lt;/button&gt;;
};</code></pre>
<blockquote>
<p><strong>Jotai vs Recoil</strong></p>
</blockquote>
<table>
<thead>
<tr>
<th>Feature</th>
<th>Jotai</th>
<th>Recoil</th>
</tr>
</thead>
<tbody><tr>
<td><strong>Concept</strong></td>
<td>원자(atom)을 기반으로 한 간단한 상태 관리.</td>
<td>원자(atom)와 선택자(selector)를 활용한 상태 관리.</td>
</tr>
<tr>
<td><strong>State Representation</strong></td>
<td>원자(atom)로 상태를 표현.</td>
<td>원자(atom)와 선택자(selector)로 상태를 표현.</td>
</tr>
<tr>
<td><strong>Derived State</strong></td>
<td><code>derived atom</code> 나 <code>useAtom</code> 훅을 사용해 계산된 값을 처리.</td>
<td>선택자를 사용하여 유도된 상태를 처리.</td>
</tr>
<tr>
<td><strong>API Complexity</strong></td>
<td>간단한 API, 최소화된 사용법.</td>
<td>선택자와 비동기 지원을 포함한 더 복잡한 API.</td>
</tr>
<tr>
<td><strong>Performance</strong></td>
<td>작은 앱에 최적화, 최소한의 리렌더링.</td>
<td>복잡한 상태를 처리하는 대규모 앱에 최적화.</td>
</tr>
<tr>
<td><strong>Integration</strong></td>
<td>React 훅과 잘 통합됨.</td>
<td>React 훅과 긴밀하게 통합됨.</td>
</tr>
<tr>
<td><strong>Async State</strong></td>
<td>atom에서 promise를 사용하여 간단히 비동기 상태 관리.</td>
<td>비동기 선택자를 위한 내장 지원.</td>
</tr>
<tr>
<td><strong>Development Experience</strong></td>
<td>최소한의 보일러플레이트, 이해하기 쉬움.</td>
<td>고급 기능을 가진 풍부한 생태계.</td>
</tr>
<tr>
<td><strong>Community Support</strong></td>
<td>작은 커뮤니티, 아직 성장 중.</td>
<td>더 큰 커뮤니티와 성숙한 생태계.</td>
</tr>
<tr>
<td><strong>Size</strong></td>
<td>가볍고 작은 크기.</td>
<td>Jotai보다 큰 크기.</td>
</tr>
<tr>
<td><strong>Use Case</strong></td>
<td>간단한 앱과 가벼운 상태 관리에 적합.</td>
<td>복잡한 상태 의존성이 있는 대규모 앱에 적합.</td>
</tr>
</tbody></table>
<h3 id="mobx---mobx-공식홈페이지">MobX - <a href="https://mobx.js.org/README.html">MobX 공식홈페이지</a></h3>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/3d078254-989a-47b0-943c-a9c6f99f1b29/image.png" /></p>
<h3 id="valtio---valtio-공식홈페이지">Valtio - <a href="https://valtio.dev/docs/introduction/getting-started">Valtio 공식홈페이지</a></h3>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/a88ca661-314a-47ab-a7e6-e2bad09dbfbd/image.png" /></p>
<blockquote>
<p><em>Valtio는 핀란드어로 '상태'</em></p>
</blockquote>