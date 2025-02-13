<h2 id="memoization">Memoization</h2>
<p><strong>Memoization</strong>이란? 컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, <strong>이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복 수행을 제거하여 프로그램 실행 속도를 빠르게 하는 기술</strong>이다. </p>
<p><strong>React</strong>는 컴포넌트 기반 라이브러리로, <strong>데이터 변경에 따라 UI를 자동으로 갱신하는 특징</strong>을 가지고 있다. 이는 <strong>User Interface</strong>를 일관되게 유지하면서, 동적으로 변경되는 데이터를 반영할 수 있도록 도와준다. 그러나 <strong>React</strong>의 이러한 자동 렌더링 방식은 때로는 불필요한 컴포넌트 재렌더링을 유발하여 성능 문제를 일으킬 수 있다. 이 문제를 해결하기 위해 <strong>메모이제이션(Memoization)</strong>이 필요합니다.</p>
<p><strong>React</strong>에서 <strong>memoization</strong>을  지원하는데 변수의 값을 저장하고 싶을 땐 <code>useMemo()</code> / 함수의 값을 저장하고 싶을 땐 <code>useCallback()</code> 을 사용한다. </p>
<h3 id="react-memoization의-필요성">React Memoization의 필요성</h3>
<p><strong>Memoization</strong>을 사용하지 않을 경우, 다음과 같은 불필요한 리렌더링이 발생할 수 있다.</p>
<blockquote>
</blockquote>
<ul>
<li>같은 데이터를 받는 자식 컴포넌트가 부모의 상태 변화에 따라 계속해서 <strong>리렌더링</strong>된다.</li>
<li>특히, 자식 컴포넌트가 무거운 연산을 수행하거나 복잡한 컴포넌트 구조를 가지고 있는 경우, 전체 애플리케이션 성능에 큰 영향을 미칠 수 있다.</li>
<li>이러한 불필요한 렌더링은 사용자 경험을 저하시킬 수 있으며, 페이지 로딩 속도 및 반응 속도에 영향을 미친다.</li>
</ul>
<p>만약 <strong>ChildComponent</strong>가 <code>데이터 fetching, 대규모 연산, 또는 복잡한 렌더링 로직</code>을 포함하고 있다고 가정해봅시다. <strong>ParentComponent</strong>가 사소한 상태 변경만으로도 자식 컴포넌트들이 다시 렌더링된다면, 이는 전체 애플리케이션의 성능을 크게 저하시킬 수 있다.</p>
<h3 id="memoization을-사용한-최적화">Memoization을 사용한 최적화</h3>
<p><strong>React</strong>에서는 불필요한 렌더링을 방지하고 성능을 최적화하기 위해 메모이제이션을 사용할 수 있다. <code>React.memo, useMemo, useCallback</code>을 통해 다음과 같은 방식으로 성능을 개선할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>React.memo</strong>: <strong>Children Component</strong>를 메모이제이션하여, 같은 <code>props</code>를 받는 경우 <strong>리렌더링</strong>을 방지한다.
<strong>useMemo</strong>: 값이나 연산 결과를 메모이제이션하여 비용이 큰 연산을 캐싱한다.
<strong>useCallback</strong>: 함수의 참조를 메모이제이션하여 불필요한 함수 재생성을 방지한다.</p>
<h4 id="reactmemo---component-memoization">React.memo - Component Memoization</h4>
<pre><code class="language-jsx">const ChildComponent = React.memo(({ value }) =&gt; {
  console.log('Child component rendered');
  return &lt;div&gt;{value}&lt;/div&gt;;
});

function ParentComponent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  return (
    &lt;div&gt;
      &lt;button onClick={() =&gt; setCount(count + 1)}&gt;Increase Count&lt;/button&gt;
      &lt;input onChange={(e) =&gt; setText(e.target.value)} /&gt;
      &lt;ChildComponent value={count} /&gt;
    &lt;/div&gt;
  );
}</code></pre>
<p><strong>React.memo</strong>를 사용하면 <strong>ChildComponent</strong>는 value가 변경되지 않는 한 <strong>리렌더링</strong>되지 않는다. <strong>React.memo</strong>는 <strong>얕은 비교(Shallow Compare)</strong>를 수행하기 때문에, <code>object / array</code>을 props로 전달할 경우 예상과 다르게 동작할 수 있다.</p>
<h4 id="usememo---value-memoization">useMemo - Value Memoization</h4>
<pre><code class="language-jsx">import React, { useMemo, useState } from 'react';

function ExpensiveCalculation(num) {
  console.log('Calculating...');
  return num * 2;
}

export default function App() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  const memoizedValue = useMemo(() =&gt; ExpensiveCalculation(count), [count]);

  return (
    &lt;div&gt;
      &lt;button onClick={() =&gt; setCount(count + 1)}&gt;Increment&lt;/button&gt;
      &lt;input onChange={(e) =&gt; setText(e.target.value)} /&gt;
      &lt;div&gt;Calculated Value: {memoizedValue}&lt;/div&gt;
    &lt;/div&gt;
  );
}</code></pre>
<p><strong>useMemo</strong> 사용하면 count가 변경될 때만 <strong>ExpensiveCalculation 함수</strong>가 호출된다. 주로 대규모 데이터를 필터링하고나 복잡한 계산 또는 렌더링 최적화할 때 사용한다. </p>
<h4 id="usecallback---function-memoization">useCallback - Function Memoization</h4>
<pre><code class="language-jsx">import React, { useState, useCallback } from 'react';

const ChildComponent = React.memo(({ onClick }) =&gt; {
  console.log('Child rendered');
  return &lt;button onClick={onClick}&gt;Click me&lt;/button&gt;;
});

export default function ParentComponent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() =&gt; {
    console.log('Button clicked');
  }, []);

  return (
    &lt;div&gt;
      &lt;button onClick={() =&gt; setCount(count + 1)}&gt;Increment&lt;/button&gt;
      &lt;ChildComponent onClick={handleClick} /&gt;
    &lt;/div&gt;
  );
}</code></pre>
<p><code>onClick</code>이 변하지 않은 이상 <strong>ChildComponent</strong>는 리렌더링 되지 않는다. 주로 <strong>ChildComponent</strong>에 <strong>prop</strong>으로 함수를 전달할 때와 함수가 종속성 배열 내에서 변경되지 않도록 고정하고 싶을 떄 사용한다. </p>
<h3 id="usecallback과-reactmemo">useCallback과 React.memo</h3>
<table>
<thead>
<tr>
<th><strong>특징</strong></th>
<th><strong><code>useCallback</code></strong></th>
<th><strong><code>React.memo</code></strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>목적</strong></td>
<td>함수의 참조를 메모이제이션하여 재생성을 방지</td>
<td>컴포넌트의 재렌더링을 방지</td>
</tr>
<tr>
<td><strong>사용 대상</strong></td>
<td>함수</td>
<td>React 컴포넌트</td>
</tr>
<tr>
<td><strong>주요 사용 시점</strong></td>
<td>자식 컴포넌트에 props로 함수를 전달할 때</td>
<td>부모 컴포넌트가 리렌더링되어도 props가 변경되지 않았을 때</td>
</tr>
<tr>
<td><strong>종속성 배열 필요 여부</strong></td>
<td>있음 (<code>useCallback(fn, [deps])</code>)</td>
<td>없음 (자동으로 props를 비교)</td>
</tr>
<tr>
<td><strong>성능 최적화 방식</strong></td>
<td>함수 참조를 고정하여 불필요한 함수 재생성 방지</td>
<td>props가 변경되지 않으면 컴포넌트 재렌더링 방지</td>
</tr>
</tbody></table>
<p><strong>useCallback과 React.memo</strong>는 서로 보완적인 역할을 한다.</p>
<p><strong>React.memo</strong>를 사용하면 props가 변경되지 않는 한 자식 컴포넌트는 <strong>리렌더링</strong>되지 않지만, 부모 컴포넌트에서 props로 전달되는 함수가 항상 새로 생성되면 React.memo만으로는 방지할 수 없다. 이럴 때 <strong>useCallback</strong>을 사용하여 함수 참조를 고정하면 자식 컴포넌트의 불필요한 렌더링을 더 효과적으로 방지할 수 있다. </p>
<h3 id="memoization-사용-시-발생할-수-있는-문제점">Memoization 사용 시 발생할 수 있는 문제점</h3>
<p><strong>React에서 메모이제이션(memoization) 기법</strong>을 활용하면 성능 최적화에 도움이 되지만, 모든 상황에서 이점을 제공하지는 않는다. 오히려 잘못 사용하면 성능 문제나 코드 복잡성을 초래할 수 있다. </p>
<p><strong>1. 메모리 사용량 증가</strong>
메모이제이션은 이전에 계산된 결과를 메모리에 저장하여 이후 동일한 입력이 있을 때 빠르게 결과를 반환하는 방식이다. 그러나 불필요한 메모이제이션은 메모리 사용량을 증가시키고, 특히 대규모 데이터나 복잡한 객체를 메모이제이션할 경우 메모리 부족 문제를 유발할 수 있다. 오래된 캐시가 계속 쌓이면 <strong>메모리 누수(memory leak)</strong>로 이어질 수 있다.</p>
<p><strong>2. 초기 비용 증가 (Performance Overhead)</strong>
<strong>React.memo, useMemo, useCallback</strong>을 사용할 때, 컴포넌트나 함수의 메모이제이션 자체에 비용이 든다. 메모이제이션을 사용할 때 props나 종속성 배열의 변경을 감지하기 위해 비교 작업을 수행한다. 특히, 객체나 배열의 경우 <strong>얕은 비교(shallow comparison)</strong>만 진행하기 때문에, 복잡한 데이터 구조일 경우 오히려 성능 저하를 초래할 수 있다. 따라서 메모이제이션으로 인한 이득이 비교 비용보다 큰 경우에만 사용하는 것이 바람직하다.</p>
<p><strong>3. 코드 복잡성 증가</strong>
메모이제이션을 남발하면 코드의 가독성이 떨어지고 유지보수성이 낮아질 수 있다. 특히, <strong>useCallback과 useMemo</strong>는 종속성 배열을 정확하게 관리해야 하므로 실수로 종속성 배열을 잘못 설정하면, 예상치 못한 동작을 초래할 수 있다. 이로 인해 버그를 추적하기 어려워지고 코드의 유지보수 비용이 증가합니다.</p>
<p><strong>4. 불필요한 메모이제이션</strong>
성능 최적화가 필요하지 않은 경우에도 메모이제이션을 사용하면 오히려 성능을 저하시킬 수 있다. 예를 들어, 자주 변경되지 않는 값이나 비교적 가벼운 연산에 메모이제이션을 적용하면, 메모이제이션에 드는 비용이 더 커질 수 있다. 메모이제이션은 <strong>비용이 큰 연산(예: 복잡한 계산, 대규모 데이터 처리)</strong> 또는 빈번하게 리렌더링되는 컴포넌트에서만 사용해야 이점이 있다.</p>
<p><strong>5. 의도하지 않은 동작 (Unexpected Behavior)</strong>
<strong>useMemo와 useCallback</strong>은 <strong>종속성 배열(dependency array)</strong>에 포함된 값이 변경되지 않으면 이전 값을 재사용한다. 그러나, 종속성 배열을 잘못 설정하면 의도치 않게 이전 값이 계속 사용되어 업데이트된 값을 반영하지 못할 수 있다.
예를 들어, 종속성 배열을 비워두면 최초 렌더링 시의 값만 계속 재사용하기 때문에, 동적인 값을 제대로 반영하지 못할 수 있다.</p>
<pre><code class="language-jsx">const memoizedValue = useMemo(() =&gt; calculateExpensiveValue(input), []);
// `input`이 변경되어도 memoizedValue는 업데이트되지 않음</code></pre>
<p><strong>6. 객체나 배열의 참조 문제</strong>
<strong>React.memo는 props를 얕은 비교(shallow comparison)</strong>만 하기 때문에, 객체나 배열이 props로 전달되면 항상 새로운 참조로 인식하여 불필요한 리렌더링이 발생할 수 있다. 이 문제를 해결하기 위해 <strong>useMemo나 useCallback</strong>으로 객체나 함수를 메모이제이션해도, 의도치 않은 참조 변경이 발생할 경우 여전히 문제가 발생할 수 있다.</p>