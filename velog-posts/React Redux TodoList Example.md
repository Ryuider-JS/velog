<blockquote>
<p><a href="https://react-redux.js.org/tutorials/connect">https://react-redux.js.org/tutorials/connect</a></p>
</blockquote>
<h1 id="example-introduce">Example Introduce</h1>
<h2 id="react-ui-components">React UI Components</h2>
<h3 id="todoapp">TodoApp</h3>
<p><strong>TodoApp</strong>은 <strong>UI Components</strong>를 감싸는 부모 컴포넌트이다. <code>header,AddTodo, TodoList, VisibilityFilters</code>로 구성되어 있다.</p>
<pre><code class="language-jsx">import React from &quot;react&quot;;
import AddTodo from &quot;./components/AddTodo&quot;;
import TodoList from &quot;./components/TodoList&quot;;
import VisibilityFilters from &quot;./components/VisibilityFilters&quot;;
import &quot;./styles.css&quot;;

export default function TodoApp() {
  return (
    &lt;div className=&quot;todo-app&quot;&gt;
      &lt;h1&gt;Todo List&lt;/h1&gt;
      &lt;AddTodo /&gt;
      &lt;TodoList /&gt;
      &lt;VisibilityFilters /&gt;
    &lt;/div&gt;
  );
}</code></pre>
<h3 id="addtodo">AddTodo</h3>
<p><strong>AddTodo</strong>는 사용자가 할 일 항목을 입력하고 <code>Add Todo</code> Button을 클릭하여 목록에 추가할 수 있는 컴포넌트이다. <strong>Controlled input</strong>을 사용해 <code>onChange 이벤트</code>로 상태를 관리하며, 버튼 클릭시 <strong>addTodo Action</strong>을 <code>dispatch</code>하여 새로운 Todo를 store에 추가한다. </p>
<pre><code class="language-jsx">// class component
import React from &quot;react&quot;;
import { connect } from &quot;react-redux&quot;;
import { addTodo } from &quot;../redux/actions&quot;;

class AddTodo extends React.Component {
  constructor(props) {
    super(props);
    this.state = { input: &quot;&quot; };
  }

  updateInput = input =&gt; {
    this.setState({ input });
  };

  handleAddTodo = () =&gt; {
    this.props.addTodo(this.state.input);
    this.setState({ input: &quot;&quot; });
  };

  render() {
    return (
      &lt;div&gt;
        &lt;input
          onChange={e =&gt; this.updateInput(e.target.value)}
          value={this.state.input}
        /&gt;
        &lt;button className=&quot;add-todo&quot; onClick={this.handleAddTodo}&gt;
          Add Todo
        &lt;/button&gt;
      &lt;/div&gt;
    );
  }
}

export default connect(
  null,
  { addTodo }
)(AddTodo);

export default AddTodo;

// function component
import React, { useState } from &quot;react&quot;;
import { connect } from &quot;react-redux&quot;;
import { addTodo } from &quot;../redux/actions&quot;;

const AddTodo = ({ addTodo }) =&gt; {
  const [input, setInput] = useState(&quot;&quot;);

  const updateInput = (input) =&gt; {
    setInput(input);
  };

  const handleAddTodo = () =&gt; {
    addTodo(input);
    setInput(&quot;&quot;);
  };

  return (
    &lt;div&gt;
      &lt;input
        onChange={(e) =&gt; updateInput(e.target.value)}
        value={input}
      /&gt;
      &lt;button className=&quot;add-todo&quot; onClick={handleAddTodo}&gt;
        Add Todo
      &lt;/button&gt;
    &lt;/div&gt;
  );
};

export default connect(
  null,
  { addTodo }
)(AddTodo);

export default AddTodo;</code></pre>
<h3 id="todolist">TodoList</h3>
<p><strong>TodoList</strong>는 할 일 리스트 전체를 보여주는 component로써 <code>VisibilityFilters</code>가 선택되었을 때, 필터된 할 일 리스트를 보여준다. </p>
<pre><code class="language-jsx">import React from &quot;react&quot;;
import { connect } from &quot;react-redux&quot;;
import Todo from &quot;./Todo&quot;;
// import { getTodos } from &quot;../redux/selectors&quot;;
import { getTodosByVisibilityFilter } from &quot;../redux/selectors&quot;;
import { VISIBILITY_FILTERS } from &quot;../constants&quot;;

const TodoList = ({ todos }) =&gt; (
  &lt;ul className=&quot;todo-list&quot;&gt;
    {todos &amp;&amp; todos.length
      ? todos.map((todo, index) =&gt; {
          return &lt;Todo key={`todo-${todo.id}`} todo={todo} /&gt;;
        })
      : &quot;No todos, yay!&quot;}
  &lt;/ul&gt;
);

// const mapStateToProps = state =&gt; {
//   const { byIds, allIds } = state.todos || {};
//   const todos =
//     allIds &amp;&amp; state.todos.allIds.length
//       ? allIds.map(id =&gt; (byIds ? { ...byIds[id], id } : null))
//       : null;
//   return { todos };
// };

const mapStateToProps = state =&gt; {
  const { visibilityFilter } = state;
  const todos = getTodosByVisibilityFilter(state, visibilityFilter);
  return { todos };
  //   const allTodos = getTodos(state);
  //   return {
  //     todos:
  //       visibilityFilter === VISIBILITY_FILTERS.ALL
  //         ? allTodos
  //         : visibilityFilter === VISIBILITY_FILTERS.COMPLETED
  //           ? allTodos.filter(todo =&gt; todo.completed)
  //           : allTodos.filter(todo =&gt; !todo.completed)
  //   };
};
// export default TodoList;
export default connect(mapStateToProps)(TodoList);</code></pre>
<h3 id="todo">Todo</h3>
<p><strong>Todo</strong>는 <strong>TodoList</strong>에서의 할 일 중 하나이다. 할 일을 완료되었으면 취소선이 그어지며 <code>onClick</code>시 해당 할 일을 완료 상태를 토글하는 <strong>toggleTodo action을 dispatch</strong>합니다.</p>
<pre><code class="language-jsx">import React from &quot;react&quot;;
import { connect } from &quot;react-redux&quot;;
import cx from &quot;classnames&quot;;
import { toggleTodo } from &quot;../redux/actions&quot;;

const Todo = ({ todo, toggleTodo }) =&gt; (
  &lt;li className=&quot;todo-item&quot; onClick={() =&gt; toggleTodo(todo.id)}&gt;
    {todo &amp;&amp; todo.completed ? &quot;👌&quot; : &quot;👋&quot;}{&quot; &quot;}
    &lt;span
      className={cx(
        &quot;todo-item__text&quot;,
        todo &amp;&amp; todo.completed &amp;&amp; &quot;todo-item__text--completed&quot;
      )}
    &gt;
      {todo.content}
    &lt;/span&gt;
  &lt;/li&gt;
);

// export default Todo;
export default connect(
  null,
  { toggleTodo }
)(Todo);</code></pre>
<h3 id="visibilityfilters">VisibilityFilters</h3>
<p><strong>VisibilityFilters</strong>는 TodoList를 필터링하는 버튼을 포함하는 컴포넌트이다. <code>all / completed / incomplete</code> 필터 버튼을 클락하여 할 일 목록을 필터링할 수 있다. 선택한 필터를 <code>activeFilter prop</code>를 통해 부모 컴포넌트로부터 받으며 선택된 필터는 밑줄로 강조 표시된다. <strong>setFilter action을 dispatch</strong>하여 filter를 업데이트한다.</p>
<pre><code class="language-jsx">import React from &quot;react&quot;;
import cx from &quot;classnames&quot;;
import { connect } from &quot;react-redux&quot;;
import { setFilter } from &quot;../redux/actions&quot;;
import { VISIBILITY_FILTERS } from &quot;../constants&quot;;

const VisibilityFilters = ({ activeFilter, setFilter }) =&gt; {
  return (
    &lt;div className=&quot;visibility-filters&quot;&gt;
      {Object.keys(VISIBILITY_FILTERS).map(filterKey =&gt; {
        const currentFilter = VISIBILITY_FILTERS[filterKey];
        return (
          &lt;span
            key={`visibility-filter-${currentFilter}`}
            className={cx(
              &quot;filter&quot;,
              currentFilter === activeFilter &amp;&amp; &quot;filter--active&quot;
            )}
            onClick={() =&gt; {
              setFilter(currentFilter);
            }}
          &gt;
            {currentFilter}
          &lt;/span&gt;
        );
      })}
    &lt;/div&gt;
  );
};

const mapStateToProps = state =&gt; {
  return { activeFilter: state.visibilityFilter };
};
// export default VisibilityFilters;
export default connect(
  mapStateToProps,
  { setFilter }
)(VisibilityFilters);</code></pre>
<h2 id="the-redux-store">The Redux Store</h2>
<h3 id="store">Store</h3>
<p><code>todos</code>는 할 일 목록을 관리하는 <strong>store</strong>로써 <code>byIds</code>와 <code>allIds</code>의 state가 존재한다., <strong>byIds</strong>는 할 일 내용을 담은 <strong>content</strong>와 할 일을 완료했는 지 확인하는 <strong>complete(boolean)</strong>으로 구성되어 있는 <strong>object이다.</strong> <strong>allIds</strong>는 현재 할 일 리스트에 존재하는 모든 <strong>id</strong>를 담은 <strong>array</strong>이다. </p>
<pre><code class="language-jsx">const initialState: { 
  allIds: number[], 
  byIds: { [key: number]: { content: string, completed: boolean } } 
} = {
  allIds: [],
  byIds: {}
};</code></pre>
<p><code>visibilityFilters</code>는 현재 어떤 <strong>filter</strong>가 focus 되었는 지 확인하는 <strong>store</strong>이다. </p>
<pre><code class="language-jsx">export const VISIBILITY_FILTERS = {
  ALL: &quot;all&quot;,
  COMPLETED: &quot;completed&quot;,
  INCOMPLETE: &quot;incomplete&quot;
};

const initialState = VISIBILITY_FILTERS.ALL;</code></pre>
<p><strong>store</strong>는 <strong>rootReducer</strong>에서 <code>combineReducers</code>를 통해서 선언된 reducer를 통합하고 이후 rootReducer를 createStore에 인자로 넣어 store를 생성한다. </p>
<pre><code class="language-jsx">// rootReducer
import { combineReducers } from &quot;redux&quot;;
import visibilityFilter from &quot;./visibilityFilter&quot;;
import todos from &quot;./todos&quot;;

export default combineReducers({ todos, visibilityFilter });

// store
import { createStore } from &quot;redux&quot;;
import rootReducer from &quot;./reducers&quot;;

export default createStore(rootReducer);</code></pre>
<h3 id="action-creators">Action Creators</h3>
<pre><code class="language-jsx">// actionTypes.js
export const ADD_TODO = &quot;ADD_TODO&quot;;
export const TOGGLE_TODO = &quot;TOGGLE_TODO&quot;;
export const SET_FILTER = &quot;SET_FILTER&quot;;</code></pre>
<p><code>addTodo</code> 새로운 할 일을 <strong>TodoList</strong>에 추가하는 <strong>action creator</strong>. 콘텐츠와 함께 id++를 payload로 추가한다. </p>
<pre><code class="language-jsx">import { ADD_TODO } from &quot;./actionTypes&quot;;

let nextTodoId = 0;

export const addTodo = content =&gt; ({
  type: ADD_TODO,
  payload: {
    id: ++nextTodoId,
    content
  }
});</code></pre>
<p><code>toggleTodo</code> 특정 할 일의 완료 상태를 토글하는 <strong>action creator</strong> <code>id</code>를 받아서 해당 할 일을 토글한다. </p>
<pre><code class="language-jsx">import { TOGGLE_TODO } from &quot;./actionTypes&quot;;

export const toggleTodo = id =&gt; ({
  type: TOGGLE_TODO,
  payload: { id }
});</code></pre>
<p><code>setFilter</code> 필터를 설정하는 <strong>action creator</strong> 필터 값(<code>all / completed / incomplete</code>)으로 payload를 받는다.</p>
<pre><code class="language-jsx">import { SET_FILTER } from &quot;./actionTypes&quot;;

export const setFilter = filter =&gt; ({ type: SET_FILTER, payload: { filter } });</code></pre>
<h3 id="reducers">Reducers</h3>
<h4 id="todos-reducer">todos reducer</h4>
<p><code>ADD_TODO</code> action을 dispatch로 받으면 <code>allIds</code>에 ID를 추가하고 <code>byIds</code>에 해당 ID의 할 일<strong>(content)</strong>와 완료 여부<strong>(completed = default false)</strong> 저장한다.
<code>TOGGLE_TODO</code> action을 dispatch로 받으면 해당 ID에 <code>completed</code>의 boolean을 <strong>논리 NOT</strong> 연산자를 이용하여 반전시킨다.</p>
<pre><code class="language-jsx">export default function(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO: {
      const { id, content } = action.payload;
      return {
        ...state,
        allIds: [...state.allIds, id],
        byIds: {
          ...state.byIds,
          [id]: {
            content,
            completed: false
          }
        }
      };
    }
    case TOGGLE_TODO: {
      const { id } = action.payload;
      return {
        ...state,
        byIds: {
          ...state.byIds,
          [id]: {
            ...state.byIds[id],
            completed: !state.byIds[id].completed
          }
        }
      };
    }
    default:
      return state;
  }
}</code></pre>
<h4 id="visibilityfilters-reducer">visibilityFilters reducer</h4>
<p><code>SET_FILTER</code> action을 dispatch하면 <strong>action.payload</strong>에 있는 filter를 새로운 값으로 설정한다.</p>
<pre><code class="language-jsx">import { SET_FILTER } from &quot;../actionTypes&quot;;

const visibilityFilter = (state = initialState, action) =&gt; {
  switch (action.type) {
    case SET_FILTER: {
      return action.payload.filter;
    }
    default: {
      return state;
    }
  }
};

export default visibilityFilter;
</code></pre>
<h3 id="action-types">Action Types</h3>
<p>`actionType.js``파일에 액션 타입을 상수로 정의해두고 이를 사용하여 action을 dispatch한다. </p>
<pre><code class="language-jsx">export const ADD_TODO = &quot;ADD_TODO&quot;;
export const TOGGLE_TODO = &quot;TOGGLE_TODO&quot;;
export const SET_FILTER = &quot;SET_FILTER&quot;;</code></pre>
<blockquote>
<h4 id="actiontypejs-vs-constantjs">ActionType.js vs constant.js</h4>
<p>공통점으로 둘다 문자열을 변수화 시킨다는 점이다. 
하지만 파일명으로 보시다시피 둘 파일은 차이가 분명히 존재한다. 
<code>ActionType.js</code>같은 경우 <code>Redux Action</code> 에서의 type을 하드코딩 하지 않고 문자열 상수화 시켜 추후 중복코드를 방지하고 유지보수성을 높이는데 사용한다.
<code>constant.js</code> 같은 경우 값들의 집합이다. 예를 들면 <code>VISIBILITY_FILTERS</code>라고 가정해보자. </p>
</blockquote>
<pre><code class="language-jsx">export const VISIBILITY_FILTERS = {
  ALL: &quot;all&quot;,
  COMPLETED: &quot;completed&quot;,
  INCOMPLETE: &quot;incomplete&quot;
};</code></pre>
<p>해당 객체는 filter의 값을 모아둔 객체이다. 하드코딩하지 않고 변수화를 시키면 마찬가지로 유지보수성이든, 오타 방지할 수 있다.
결론적으로 <code>constant</code> 는 단순 상수의 집합 / <code>actionType</code>은 Redux Action 관련된 상수 </p>
<h3 id="selectors">Selectors</h3>
<p><code>getTodoList</code>는 <code>todos</code> store에<code>allIds: Array&lt;number&gt;</code>를 받아온다. </p>
<pre><code class="language-jsx">export const getTodosState = store =&gt; store.todos;

// getTodoState에서 객체가 존재하면 allIds를 반환 아니면 배열
export const getTodoList = store =&gt;
  getTodosState(store) ? getTodosState(store).allIds : [];</code></pre>
<p><code>getTodoById</code> id를 통해 todo를 찾음. 반환값으로 <code>{ content: string, complete: boolean, id: number }</code></p>
<pre><code class="language-jsx">// getTodoState(store)로 todo에 대한 store를 불러오고
// byIds[id]로 접근하여 객체 { content: string, complete: boolean }을 가져오고
// 스프레드 연산자를 이용하여 기존 객체에 id를 추가한다.
export const getTodoById = (store, id) =&gt;
  getTodosState(store) ? { ...getTodosState(store).byIds[id], id } : {};</code></pre>
<p><code>getTodos</code> <code>getTodoList</code>를 통해서 allIds를 받아오고, <code>getTodosState</code>를 통해 store에 존재하는 모든 byIds에 있는 객체를 반환한다.</p>
<pre><code class="language-jsx">export const getTodos = store =&gt;
  getTodoList(store).map(id =&gt; getTodoById(store, id));

// 반환값 예시
// [
//  { content: &quot;Learn Redux&quot;, completed: false, id: 1 },
//  { content: &quot;Build a project&quot;, completed: true, id: 2 },
//  { content: &quot;Deploy the app&quot;, completed: false, id: 3 }
// ]</code></pre>
<p><code>getTodosByVisibilityFilter</code>는 마지막 단계로써 <code>getTodos</code>로 todoList를 불러오고 filter의 값에 따라 <strong>todo</strong>를 필터해주는 함수이다. </p>
<pre><code class="language-jsx">export const getTodosByVisibilityFilter = (store, visibilityFilter) =&gt; {
  const allTodos = getTodos(store);
  switch (visibilityFilter) {
    case VISIBILITY_FILTERS.COMPLETED:
      return allTodos.filter(todo =&gt; todo.completed);
    case VISIBILITY_FILTERS.INCOMPLETE:
      return allTodos.filter(todo =&gt; !todo.completed);
    case VISIBILITY_FILTERS.ALL:
    default:
      return allTodos;
  }
};</code></pre>
<h3 id="providing-the-store">Providing the Store</h3>
<p>처음에는 store를 react에 제공하기 위해서 최상단에 <code>&lt;Provide /&gt;</code>를 감싸준 후, props에 store를 !
등록해줘야한다.</p>
<pre><code class="language-jsx">// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import TodoApp from './TodoApp'

import { Provider } from 'react-redux'
import store from './redux/store'

// As of React 18
const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  &lt;Provider store={store}&gt;
    &lt;TodoApp /&gt;
  &lt;/Provider&gt;,
)</code></pre>
<p><a href="https://velog.velcdn.com/images/rjs8833/post/78e3fbf7-08a7-4529-bb31-edc6ca977e1c/image.png"></a></p>
<h3 id="connecting-the-components">Connecting the Components</h3>
<p><strong>React Redux</strong>는 store에 있는 state를 읽거나 state를 업데이트할 때 다시 읽기 위해 <code>function connect</code>를 제공한다. <code>connect</code>에는 opional인 2개의 argument를 제공한다. </p>
<blockquote>
<p><strong>mapStateToProps / mapDispatchToProps</strong>
<strong>mapStateToProps</strong>는 store에 state가 변경될 때마다 호출되며 전체 <strong>Redux Store</strong>의 state를 받아와서 필요한 데이터만 객체 형태로 반환한다. 즉, 필요한 state를 컴포넌트의 props로 매핑해준다. </p>
</blockquote>
<p><strong>mapDispathchToProps</strong>는 redux의 <strong>dispatch</strong>를 props로 전달하는 역할을 하며, <code>function / object</code> 형태로 전달한다.<code>function</code> 이면 컴포넌트가 생성될 때 한 번만 호출된다. 이 function은 dispatch를 인자로 받고 <code>dispatch</code>를 사용하여 액션을 호출하는 함수들이 포함된 객체를 반환해야한다. </p>
<pre><code class="language-jsx">onst mapStateToProps = (state, ownProps) =&gt; ({
  // ... computed data from state and optionally ownProps
})
&gt;
const mapDispatchToProps = {
  // ... normally is an object full of action creators
}
&gt;
// `connect` returns a new function that accepts the component to wrap:
const connectToStore = connect(mapStateToProps, mapDispatchToProps)
// and that function returns the connected, wrapper component:
const ConnectedComponent = connectToStore(Component)
&gt;
// We normally do both in one step, like this:
connect(mapStateToProps, mapDispatchToProps)(Component)</code></pre>