<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/a3c9cd57-a1d5-41f1-85c0-6e2e6249bea3/image.png" /></p>
<h1 id="usage-summary">Usage Summary</h1>
<h3 id="install-redux-toolkit-and-react-redux">Install Redux Toolkit and React Redux</h3>
<pre><code class="language-shell">npm install @reduxjs/toolkit react-redux</code></pre>
<h3 id="common">Common</h3>
<pre><code class="language-javascript">// main.jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import './index.css'
import App from './App'
import store from './app/store'
import { Provider } from 'react-redux'

// As of React 18
const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  &lt;Provider store={store}&gt;
    &lt;App /&gt;
  &lt;/Provider&gt;,
)

// App.js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

const App = () =&gt; {
  // 상태를 useSelector로 읽어옴
  const count = useSelector((state) =&gt; state.counter.count);  // counter 리듀서의 상태
  const user = useSelector((state) =&gt; state.user);  // user 리듀서의 상태
  const dispatch = useDispatch();

  // 카운터 증가/감소
  const increment = () =&gt; {
    dispatch({ type: 'INCREMENT' });
  };

  const decrement = () =&gt; {
    dispatch({ type: 'DECREMENT' });
  };

  // 사용자 설정/초기화
  const setUser = () =&gt; {
    dispatch({ type: 'SET_USER', payload: { name: 'John', email: 'john@example.com' } });
  };

  const clearUser = () =&gt; {
    dispatch({ type: 'CLEAR_USER' });
  };

  return (
    &lt;div&gt;
      &lt;h1&gt;Counter: {count}&lt;/h1&gt;
      &lt;button onClick={increment}&gt;Increment&lt;/button&gt;
      &lt;button onClick={decrement}&gt;Decrement&lt;/button&gt;

      &lt;h2&gt;User Info&lt;/h2&gt;
      &lt;p&gt;Name: {user.name}&lt;/p&gt;
      &lt;p&gt;Email: {user.email}&lt;/p&gt;
      &lt;button onClick={setUser}&gt;Set User&lt;/button&gt;
      &lt;button onClick={clearUser}&gt;Clear User&lt;/button&gt;
    &lt;/div&gt;
  );
};

export default App;</code></pre>
<h2 id="react-redux-vs-rtk">React Redux VS RTK</h2>
<h3 id="reducer">Reducer</h3>
<pre><code class="language-javascript">// React Redux
const initialState = { count: 0 };

const counterReducer = (state = initialState, action) =&gt; {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

export default counterReducer;

// RTK
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { count: 0 },
  reducers: { // immer를 이용하여 불변성
    increment: (state) =&gt; {
      state.count += 1;
    },
    decrement: (state) =&gt; {
      state.count -= 1;
    },
  },
});

export const { increment, decrement } = counterSlice.actions; // 액션 생성자 자동
export default counterSlice.reducer; // 리듀서 자동</code></pre>
<blockquote>
<p><strong>React Redux vs RTK</strong></p>
</blockquote>
<p><strong>리듀서 선언 방식</strong>
<strong>React Redux</strong>에서는 리듀서를 수동으로 작성한다. state를 기본값으로 초기화하며, 액션 타입에 따라 <strong>switch 문</strong>을 사용하여 각 액션에 맞는 상태 변화를 처리한다.
<strong>RTK</strong>는 <code>createSlice</code>를 사용하여 리듀서와 액션 생성자를 자동으로 생성한다. reducers 객체에서 각 액션을 정의하며, 각 액션에 대한 상태 업데이트를 간단하게 함수를 사용하여 처리한다.</p>
<blockquote>
<p><strong>state 불변성</strong>
<strong>React Redux</strong>는 state는 직접 수정하지 않고, 새로운 객체를 반환하여 상태를 변경한다. 즉, <code>state.count + 1 또는 state.count - 1</code>을 계산하고 그 결과로 새로운 객체를 반환하는 방식이다.
<strong>RTK</strong>는 <strong>Immer</strong>라는 라이브러리를 사용하여 상태를 직접 변경할 수 있도록 한다. 즉, <code>state.count += 1</code>처럼 상태를 직접 수정할 수 있지만, 내부적으로는 불변성을 유지한 채 상태가 변환된다.</p>
</blockquote>
<p><strong>액션 타입</strong>
<strong>React Redux</strong>는 <code>액션 타입(INCREMENT, DECREMENT)</code>을 문자열로 하드코딩하여 사용한다. 액션 타입을 <code>switch 문</code>에서 직접 비교해야 하기 때문에 액션 타입 관리가 수동으로 이루어진다.
<strong>RTK</strong>에서는 액션 타입을 문자열로 하드코딩할 필요 없이, <strong>createSlice</strong>가 자동으로 액션 생성자와 액션 타입을 생성해준다. 이를 통해 액션 타입을 별도로 관리할 필요가 없다. <code>increment와 decrement 액션</code>을 <strong>createSlice</strong>에서 자동으로 생성하며, 이를 바로 사용할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong><code>createSlice</code> vs <code>createReducer</code> 차이점</strong></p>
<blockquote>
</blockquote>
<table>
<thead>
<tr>
<th><strong>특징</strong></th>
<th><strong><code>createSlice</code></strong></th>
<th><strong><code>createReducer</code></strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>자동 생성되는 것</strong></td>
<td>리듀서 + 액션 생성자</td>
<td>리듀서만 자동 생성</td>
</tr>
<tr>
<td><strong>액션 생성자</strong></td>
<td>자동으로 생성되며, <code>counterSlice.actions</code>에서 사용할 수 있음</td>
<td>별도로 액션 생성자 정의 필요 (수동으로 작성)</td>
</tr>
<tr>
<td><strong>구현 방식</strong></td>
<td>리듀서를 함수 형식으로 작성하고, 액션 타입은 자동으로 생성됩니다.</td>
<td><code>switch</code>문을 사용하지 않고, <strong>맵 객체</strong>로 리듀서를 정의</td>
</tr>
<tr>
<td><strong>불변성 처리</strong></td>
<td><code>Immer</code>를 통해 <strong>불변성</strong> 자동 처리</td>
<td><code>Immer</code>를 사용하여 불변성 관리 자동 처리</td>
</tr>
<tr>
<td><strong>사용 목적</strong></td>
<td><strong>리듀서와 액션 생성자</strong>를 동시에 관리하고 싶을 때 적합</td>
<td><strong>리듀서만</strong>을 관리하고 싶을 때, 액션을 별도로 정의하고 싶을 때 적합</td>
</tr>
<tr>
<td><strong>예시</strong></td>
<td><code>increment</code>, <code>decrement</code> 액션과 리듀서를 동시에 정의함</td>
<td><code>INCREMENT</code>, <code>DECREMENT</code> 액션을 별도로 정의하고, 리듀서만 관리</td>
</tr>
</tbody></table>
<blockquote>
</blockquote>
<ul>
<li><strong><code>createSlice</code></strong>는 <strong>리듀서와 액션을 한 번에 관리</strong>할 수 있어 간단하고 직관적이다.</li>
<li><strong><code>createReducer</code></strong>는 리듀서만을 <strong>세밀하게 정의</strong>할 수 있으며, 액션을 별도로 정의하고 싶을 때 유용하다.</li>
</ul>
<h3 id="store">Store</h3>
<pre><code class="language-javascript">// React Redux
import { createStore, applyMiddleware, combineReducers } from 'redux';
import thunk from 'redux-thunk';
import { composeWithDevTools } from 'redux-devtools-extension';
import counterReducer from './counterReducer';
import userReducer from './userReducer';

// combineReducers를 사용해서 리듀서 결합
const rootReducer = combineReducers({
  counter: counterReducer,
  user: userReducer,
});

// 스토어 생성
const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(thunk)) // 미들웨어와 Redux DevTools 설정
);

export default store;

// RTK
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';
import userReducer from './userSlice';

const store = configureStore({
  reducer: {
    counter: counterReducer,
    user: userReducer,
  },
});

export default store;</code></pre>
<blockquote>
<p><strong>React Redux vs RTK</strong></p>
</blockquote>
<p><strong>리듀서 설정</strong>
<strong>React Redux</strong>는 <code>combineReducers</code>를 사용하여 여러 리듀서를 결합한다. 이를 통해 <strong>counter와 user</strong> 상태를 관리하는 리듀서를 결합한 후, <strong>rootReducer를 생성하고 이를 createStore</strong>로 전달한다.
<strong>RTK</strong>는 <code>configureStore</code>에서 reducer 속성으로 객체를 전달하여 리듀서를 결합한다. <code>configureStore</code>는 내부적으로 리듀서를 결합하는 작업을 자동으로 처리하므로 <strong>combineReducers</strong>를 별도로 사용할 필요가 없다.</p>
<blockquote>
</blockquote>
<p><strong>Middleware 및 Redux DevTools</strong>
<strong>React Redux</strong>는 <code>applyMiddleware</code>를 사용하여 <strong>thunk 미들웨어</strong>를 설정한다. 또한 <code>composeWithDevTools</code>을 사용하여 개발자 도구를 설정한다.
<strong>RTK</strong>는 <code>configureStore</code>를 사용하면 미들웨어 설정이 자동으로 처리된다. 기본적으로 <strong>redux-thunk</strong>가 내장되어 있어서 별도로 설정할 필요가 없다. 또한 <strong>Redux DevTools</strong>도 기본적으로 활성화되어 있으며, 별도의 설정 없이 사용할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>스토어 생성</strong>
<strong>React Redux</strong>는 <code>createStore</code>를 사용하여 스토어를 생성한다. 또한 applyMiddleware와 composeWithDevTools를 함께 사용하여 미들웨어와 개발자 도구를 설정한다. 더 많은 설정이 있을 경우 추가해야한다.
<strong>RTK</strong>는 <code>configureStore</code>를 사용하여 스토어를 생성한다. 이 함수는 내부적으로 필요한 설정<strong>(미들웨어, Redux DevTools 등)</strong>을 자동으로 처리한다.</p>
<h1 id="redux-toolkit-typescript">Redux ToolKit Typescript</h1>
<h2 id="project-setup">Project Setup</h2>
<h3 id="define-root-state-and-dispatch-types">Define Root State and Dispatch Types</h3>
<p><strong>RTK</strong>의<code>configureStore</code> 에서 별도로 추가적인 Type 정의가 필요하지 않다. 하지만 <code>RootState</code>와 <code>AppDispatch</code> 타입은 추출하는 게 유지보수 측면에서 좋다. </p>
<blockquote>
<p><strong>RootState와 AppDispatch의 타입을 추출하면 유지보수가 좋은 이유</strong>
<strong>RootState</strong>란, redux에서 사용하는 모든 state를 의미한다. 즉, <code>store.getState()</code>를 호출하면 전체 state가 나오는데, 이 때의 타입을 <strong>RootState</strong>라고 한다. </p>
</blockquote>
<pre><code class="language-javascript">export type RootState = ReturnType&lt;typeof store.getState&gt;
&gt;
import { useSelector } from 'react-redux'
import { RootState } from './store'
&gt;
const CounterComponent = () =&gt; {
  const counter = useSelector((state: RootState) =&gt; state.counter.value)
  return &lt;div&gt;Counter: {counter}&lt;/div&gt;
}</code></pre>
<p><strong>RootState</strong>를 <code>configureStore</code>에서 자동추론에 의해 타입 정의를 하게 되면, 전역 <strong>Component</strong>에서의 <code>useSelector</code>에서 state 타입정의를 안전하게 하면서 편리하게 사용할 수 있다. </p>
<blockquote>
</blockquote>
<p><strong>Appdispatch</strong>란 Redux의 <strong>dispatch 함수의 타입</strong>을 의미한다.</p>
<blockquote>
</blockquote>
<p>RTK에서는 <code>store.dispatch</code>도 자동으로 타입을 추론하지만, 컴포넌트에서 <strong>useDispatch</strong>를 사용할 때 정확한 타입을 지정하는 것이 좋다.</p>
<pre><code class="language-typescript">export type AppDispatch = typeof store.dispatch
&gt;
import { useDispatch } from 'react-redux'
import { AppDispatch } from './store'
import { increment } from './counterSlice'
&gt;
const CounterButton = () =&gt; {
  const dispatch = useDispatch&lt;AppDispatch&gt;() 
  return &lt;button onClick={() =&gt; dispatch(increment())}&gt;Increment&lt;/button&gt;
}</code></pre>
<h3 id="define-typed-hooks">Define Typed Hooks</h3>
<p><strong>RootState &amp; AppDispatch</strong>를 사용하면 전역 component에서 타입 정의를 간편하게 할 수 있다는 장점이 존재하지만, <code>useSelector 또는 useDispatch</code>를 선언할 때마다, type을 직접 넣어줘야하기 때문에 (<strong>RootState / AppDispatch</strong>) 개발자의 실수 유발 및 번거로울 수 있다. 따라서 <strong>RTK</strong>에서 이를 해결하기 위해 <strong>store</strong>에서 <strong>RootState &amp; Appdispatch</strong> 타입 선언 이후 hooks를 정의한다.</p>
<pre><code class="language-typescript">const store = configureStore({
  reducer: {
    counter: counterSlice.reducer,
  },
})

export type RootState = ReturnType&lt;typeof store.getState&gt;
export type AppDispatch = typeof store.dispatch

// react-redux v8 미만

export const useAppSelector: TypedUseSelectorHook&lt;RootState&gt; = useSelector
export const useAppDispatch = () =&gt; useDispatch&lt;AppDispatch&gt;()

// react-redux v8 이상
export const useAppSelector = useSelector.withTypes&lt;RootState&gt;()
export const useAppDispatch = useDispatch.withTypes&lt;AppDispatch&gt;()

export default store</code></pre>
<h2 id="application-usage">Application Usage</h2>
<h3 id="define-slice-state-and-action-types">Define Slice State and Action Types</h3>
<p><strong>RTK</strong> 기준 <strong>initial Slice</strong>를 선언할 때, 초기 상태의 타입에 대해서 정의해야한다. <strong>action</strong> 같은 경우 <code>PayloadAction&lt;T&gt;</code>를 사용하여 타입을 정의해야한다. </p>
<pre><code class="language-typescript">import { createSlice, PayloadAction } from '@reduxjs/toolkit'
import type { RootState } from '../../app/store'

// slice에서 사용하는 value의 interface 정의
interface CounterState {
  value: number
}

// 초기 state를 interface CounterState 타입 할당
const initialState: CounterState = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) =&gt; {
      state.value += 1
    },
    decrement: (state) =&gt; {
      state.value -= 1
    },
    // PayloadAction을 이용하여 action 타입 정의
    incrementByAmount: (state, action: PayloadAction&lt;number&gt;) =&gt; {
      state.value += action.payload
    },
  },
})

export const { increment, decrement, incrementByAmount } = counterSlice.actions</code></pre>
<p>생성된 액션 생성자는 <code>PayloadAction&lt;T&gt;</code>타입을 기반으로 <strong>payload 인자</strong>를 정확하게 타입화한다. <strong>incrementByAmount</strong>같은 경우 <code>action.payload</code> 타입으로 number만 가능하다는 얘기다. 경우에 따라 typescript 명시적 타입 할당을 해야하는 경우가 있다. <a href="https://github.com/reduxjs/redux-toolkit/pull/827">예시</a> 그런 경우에는 <code>as</code>를 이요하여 타입을 명시적으로 할당하면 된다. </p>
<pre><code class="language-typescript">// Workaround: cast state instead of declaring variable type
const initialState = {
  value: 0,
} as CounterState</code></pre>
<h3 id="use-typed-hooks-in-components">Use Typed Hooks in Components</h3>
<p>컴포넌트에서 redux에서 제공하는 훅을 사용하는 게 아닌 <code>useAppSelector, useAppDispatch</code>를 호출하여 사용해야한다.</p>
<pre><code class="language-typescript">import React, { useState } from 'react'

import { useAppSelector, useAppDispatch } from 'app/hooks'

import { decrement, increment } from './counterSlice'

export function Counter() {
  // The `state` arg is correctly typed as `RootState` already
  const count = useAppSelector((state) =&gt; state.counter.value)
  const dispatch = useAppDispatch()

  // omit rendering logic
}
```![](https://velog.velcdn.com/images/rjs8833/post/f6a33eeb-9640-41e0-bf9c-1deba48de5e3/image.png)
</code></pre>