<h2 id="서론">서론</h2>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/2f2f20e4-c512-4ece-a6ae-33ad48660773/image.png" /></p>
<p>React에서는 데이터 패칭을 기본적으로 <code>useEffect와 useState</code> 훅을 사용하여 처리한다. 하지만 이 방식은 가독성을 떨어뜨리고, <strong>Boilerplate</strong> 코드가 많아질 수 있다.</p>
<pre><code class="language-jsx">import { useEffect, useState } from 'react';

const UserList = () =&gt; {
  const [users, setUsers] = useState([]);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() =&gt; {
    const fetchUsers = async () =&gt; {
      try {
        const response = await fetch('https://api.example.com/users');
        if (!response.ok) {
          throw new Error('Failed to fetch users');
        }
        const data = await response.json();
        setUsers(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    };

    fetchUsers();
  }, []);

  if (isLoading) return &lt;p&gt;Loading...&lt;/p&gt;;
  if (error) return &lt;p&gt;{error}&lt;/p&gt;;

  return (
    &lt;ul&gt;
      {users.map((user) =&gt; (
        &lt;li key={user.id}&gt;{user.name}&lt;/li&gt;
      ))}
    &lt;/ul&gt;
  );
};

export default UserList;</code></pre>
<blockquote>
<h4 id="useeffect를-이용하여-data-fetch를-하는-이유"><code>useEffect</code>를 이용하여 Data Fetch를 하는 이유</h4>
</blockquote>
<ol>
<li><strong>렌더링 시점과 데이터 패칭 분리</strong></li>
</ol>
<p><strong>React의 function component</strong>는 렌더링될 때마다 함수를 다시 실행한다. 만약 <code>useEffect</code> 없이 <code>fetchUsers()</code>를 직접 호출하면, 컴포넌트가 렌더링될 때마다 API 요청이 실행될 수 있다. 반면 <code>useEffect</code>를 사용하면 특정 시점(예: 마운트 시)에만 실행되도록 제어할 수 있다.</p>
<blockquote>
</blockquote>
<ol start="2">
<li><strong>비동기 작업과 렌더링 흐름 관리</strong></li>
</ol>
<p><strong>fetch</strong>는 비동기 함수이므로, 데이터를 가져오기 전에 먼저 초기 렌더링이 이루어진다. <strong>useEffect 내부</strong>에서 데이터를 불러온 후 <code>setUsers</code>를 통해 상태를 업데이트하면 React가 데이터를 받아서 다시 렌더링한다. 만약 <code>useEffect</code> 없이 <strong>fetch 요청</strong>을 실행하면, <strong>비동기 작업이 완료되기 전에 화면이 먼저 그려지면서 불완전한 UI가 노출될 수 있다.</strong></p>
<ul>
<li><strong>data fetch</strong>를 할 때 3가지의 state를 직접 관리해야한다. <code>state / loading / error</code> </li>
<li>캐싱 기능이 존재하지 않기 때문에, 동일한 데이터를 <strong>여러 컴포넌트</strong>에서 요청하면 중복 요청이 발생할 수 있다. </li>
<li>자동 리패치 기능이 존재하지 않아, <strong>창이 다시 활성화되거나 네트워크가 재연결될 때</strong> 자동으로 데이터를 새로고침하지 않는다. </li>
<li>또한, 에러가 발생할 때마다 <strong>try-catch 블록</strong>을 수동으로 작성해야 한다. </li>
<li>마지막으로 컴포넌트가 <strong>unmount</strong> 되더라도 promise는 중단되지 않고 실행되어 비효율적이다.</li>
</ul>
<h2 id="tanstack-query---tanstack-query-공식-홈페이지">Tanstack Query - <a href="https://tanstack.com/query/latest">Tanstack Query 공식 홈페이지</a></h2>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/020157a4-a333-4fc3-9b67-3f90005a88db/image.png" /></p>
<blockquote>
<p><del>v3까지 React Query라고 불렸지만 다른 라이브러리에도 적용시키기위해 v4부턴 tanstack query로 바뀌게 되었다.</del>
tanstack query == react query</p>
</blockquote>
<p><strong>Tanstack Query</strong>는 자기 자신을 <strong>강력한 비동기 상태 관리</strong>라고 소개하고 있다. <strong>React Query</strong>는 리액트 애플리케이션에서 <strong>서버 상태를 쉽게 관리하고, 데이터를 fetching, caching, syncing하는 라이브러리다.</strong> 이 라이브러리는 API 호출을 간소화하고, 서버 상태를 효율적으로 관리하는 데 도움을 준다.</p>
<blockquote>
<p><strong>React Query 톺아보기</strong>
<strong>Data Fetching</strong>: <strong>React Query</strong>는 서버에서 데이터를 가져오고, 상태를 관리한다. <code>useQuery와 useMutation</code> 을 사용하여 데이터 <strong>fetching과 mutation</strong>을 할 수 있다.</p>
</blockquote>
<p><strong>Caching</strong>: 서버에서 가져온 데이터를 캐시하여 불필요한 네트워크 요청을 줄이고, 앱의 성능을 향상시킨다. 데이터는 기본적으로 백그라운드에서 최신 상태로 유지된다.</p>
<blockquote>
</blockquote>
<p><strong>Auto Refetch</strong>: <strong>React Query</strong>는 데이터를 일정 시간마다 자동으로 <strong>refetch</strong>할 수 있으며, <code>focut 변화</code>나 <code>네트워크가 복구</code>되면 자동으로 데이터를 새로고침한다.</p>
<blockquote>
</blockquote>
<p><strong>Seperate Server State and Client State</strong>: <strong>React Query</strong>는 서버 상태를 관리하는 데만 집중하여, 클라이언트 상태는 다른 상태 관리 라이브러리(예: Recoil, Redux)로 관리할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>Data Synchronization &amp; Update</strong>: <code>mutate</code>를 통해 데이터를 변경하고, 변경된 데이터가 자동으로 캐시되어 <strong>UI</strong>에 반영된다.</p>
<blockquote>
</blockquote>
<p><strong>Parallel and Dependent Queries</strong>: 여러 개의 쿼리를 병렬로 요청하거나(<code>useQueries</code>), 하나의 쿼리가 완료된 후 다른 쿼리를 실행(<code>option / enabled</code>)하는 등의 동작을 지원한다.</p>
<pre><code class="language-tsx">import {
  useQuery,
  useMutation,
  useQueryClient,
  QueryClient,
  QueryClientProvider,
} from '@tanstack/react-query'
import { getTodos, postTodo } from '../my-api'

// Create a client
const queryClient = new QueryClient()

function App() {
  return (
    // Provide the client to your App
    &lt;QueryClientProvider client={queryClient}&gt;
      &lt;Todos /&gt;
    &lt;/QueryClientProvider&gt;
  )
}

function Todos() {
  // Access the client
  const queryClient = useQueryClient()

  // Queries
  const query = useQuery({ queryKey: ['todos'], queryFn: getTodos })

  // Mutations
  const mutation = useMutation({
    mutationFn: postTodo,
    onSuccess: () =&gt; {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ['todos'] })
    },
  })

  return (
    &lt;div&gt;
      &lt;ul&gt;{query.data?.map((todo) =&gt; &lt;li key={todo.id}&gt;{todo.title}&lt;/li&gt;)}&lt;/ul&gt;

      &lt;button
        onClick={() =&gt; {
          mutation.mutate({
            id: Date.now(),
            title: 'Do Laundry',
          })
        }}
      &gt;
        Add Todo
      &lt;/button&gt;
    &lt;/div&gt;
  )
}

render(&lt;App /&gt;, document.getElementById('root'))</code></pre>
<p><strong>react query</strong>는 <code>context.provider / redux / recoil / apollo client</code>와 같이 최상단에 <code>QueryClientProvider</code>를 layer로써 둬야한다. 이는 전역에서 <strong>Data fetching state</strong>를 관리함으로써 강력한 <strong>react query</strong>의 장점을 사용하기 위해서이다. </p>
<blockquote>
<h4 id="ssr에서-queryclientprovider를-사용하는-방법">SSR에서 QueryClientProvider를 사용하는 방법</h4>
<p><strong>NextJS</strong>에서는 Parent Component가 <strong>RCC</strong>면 Children Components들은 전부 <strong>RCC</strong>가 된다. 따라서 최상단 <strong>RootLayout</strong>에서 <strong>QueryClientProvider</strong>를 사용하려면 <strong>RCC</strong>가 되어야할 것이고, 이는 전역컴포넌트에서 <strong>SSR</strong>을 사용하지 못하는 것을 의미한다.</p>
</blockquote>
<p><strong>NextJS</strong>에서는 이 문제점을 해결하기 위해 <a href="https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns">Component Composition</a>을 제안했는데, 쉽게 설명하자면, 부모컴포넌트가 <strong>RCC</strong>여도 자식컴포넌트가 <strong>RSC</strong>가  가능하게 해주는 기술이다.</p>
<blockquote>
</blockquote>
<pre><code class="language-jsx">// app/layout.tsx
import { Providers } from './providers';
&gt;
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    &lt;html lang=&quot;en&quot;&gt;
      &lt;body&gt;
        &lt;Providers&gt;{children}&lt;/Providers&gt;
      &lt;/body&gt;
    &lt;/html&gt;
  );
}
&gt;
// app/providers.tsx
&quot;use client&quot;
&gt;
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
&gt;
const queryClient = new QueryClient();
&gt;
export function Providers({ children }: { children: React.ReactNode }) {
  return &lt;QueryClientProvider client={queryClient}&gt;{children}&lt;/QueryClientProvider&gt;;
}</code></pre>
<blockquote>
</blockquote>
<p>여기서 주의할 사항은 <strong>Client Component에서 Server Component를 호출할 수 없다는 것이다.</strong></p>
<h3 id="usequery">useQuery</h3>
<p><code>useQuery</code>는 <strong>React Query</strong>에서 데이터를 가져오기(fetching) 위해 사용하는 hook이다. 주로 <code>Get</code> method를 통해 데이터를 가져올 때 사용되며 나머지 <code>Post / Put / Patch / Delete</code> 메서드는 <code>useMutation</code>을 이용하여 처리한다. </p>
<p><code>useQuery</code>는 <strong>Promise 객체</strong>를 처리하고, <strong>Promise status(loading, error, success 등)</strong>를 관리하며, caching, auto refetching 등을 제공한다.</p>
<pre><code class="language-jsx">const { data, error, isLoading } = useQuery({
  queryKey: unknown[],
  queryFn: (context: QueryFunctionContext) =&gt; Promise&lt;TData&gt;
}) </code></pre>
<p><code>useQuery</code>는 기본적으로 <strong>required</strong> <code>queryKey, queryFn</code>을 arugment로 받는다. </p>
<ul>
<li>queryKey : <strong>쿼리 데이터를 고유하게 식별하는 키다.</strong> 이 값은 배열로 제공된다.(v4 기준) 배열을 사용하는 경우에는 쿼리를 고유하게 식별할 수 있도록 여러 개의 인자를 결합할 수 있다.</li>
<li>queryFn : <strong>데이터를 가져오는 비동기 함수다.</strong> 이 함수는 <strong>Promise</strong>를 반환해야 하며, 서버에서 데이터를 가져오거나 비즈니스 로직을 처리하는 데 사용된다. <strong>context 인자</strong>는 선택적으로 받을 수 있으며, 쿼리 함수에 대한 추가적인 정보가 포함된다. 예를 들어, 쿼리 키가 어떻게 구성되어 있는지, 리패칭과 관련된 정보 등을 포함할 수 있다.<pre><code class="language-jsx">const fetchUserData = async (context: QueryFunctionContext) =&gt; {
const { queryKey } = context;
const userId = queryKey[1]; // 예: ['user', userId]
const res = await fetch(`/api/user/${userId}`);
if (!res.ok) {
  throw new Error('Failed to fetch user data');
}
return res.json();
};</code></pre>
또한 <code>QueryFucntionContext</code>로 <strong>pageParam</strong>를 호출할 수 있는데 <strong>pageParam은 pagination</strong>을 처리할 때 유용하게 사용된다. 예를 들어, 쿼리가 추가적인 페이지를 요청할 때(infinite scroll)마다 <strong>pageParam</strong>을 전달받아 해당 페이지 데이터를 요청할 수 있다.<pre><code class="language-jsx">import { useInfiniteQuery } from '@tanstack/react-query';
</code></pre>
</li>
</ul>
<p>// 데이터를 가져오는 함수
const fetchPosts = async ({ pageParam = 1 }) =&gt; {
  const res = await fetch(<code>/api/posts?page=${pageParam}</code>);
  if (!res.ok) {
    throw new Error('Failed to fetch posts');
  }
  return res.json(); // posts와 nextPage를 반환한다고 가정
};</p>
<p>function Posts() {
  const {
    data,
    error,
    isLoading,
    fetchNextPage,
    hasNextPage,
    isFetchingNextPage,
  } = useInfiniteQuery({
    queryKey: ['posts'], // queryKey
    queryFn: fetchPosts,  // 데이터 가져오는 함수
    getNextPageParam: (lastPage) =&gt; {
      // lastPage는 이전 페이지 데이터로부터 nextPage를 추출
      return lastPage.nextPage || undefined;  // 다음 페이지 번호를 반환
    },
  });</p>
<p>  if (isLoading) return <div>Loading...</div>;
  if (error instanceof Error) return <div>Error: {error.message}</div>;</p>
<p>  return (
    <div>
      {data?.pages.map((page, index) =&gt; (
        <div>
          {page.posts.map((post) =&gt; (
            <div>{post.title}</div>
          ))}
        </div>
      ))}
      {hasNextPage &amp;&amp; (
        <button>
          {isFetchingNextPage ? 'Loading more...' : 'Load More'}
        </button>
      )}
    </div>
  );
}</p>
<pre><code>### useMutation
`useMutation`은 CRUD 메서드중 **Read**를 제외한 모든 메서드를 처리할 때 사용된다.
```jsx
const { mutate, isLoading, isError, isSuccess, error, data } = useMutation({
  mutationFn: (variables: TVariables) =&gt; Promise&lt;TData&gt;,
  onMutate: (variables) =&gt; {
    // 뮤테이션이 시작될 때 호출
  },
  onSuccess: (data) =&gt; {
    // 뮤테이션 성공 후 호출
  },
  onError: (error) =&gt; {
    // 뮤테이션 실패 후 호출
  },
  onSettled: (data, error) =&gt; {
    // 성공/실패 후 항상 호출
  },
});</code></pre><p><code>useQuery</code>와 다르게 <code>useMutation</code>은 required로 <strong>mutationFn</strong>만 받는다. <strong>mutationFn</strong>은 mutation을 처리하는 함수로써 서버에 데이터를 보내거나 수정하는 등의 작업을 처리한다. <strong>Promise 객체</strong>를 반환한다. 또한 <strong>Optimistic Update</strong>를 적용하기 위해 <code>onMutate</code>를 이용하여 ui를 먼저 업데이트할 수 있다. </p>
<p>반환값으로 <code>mutate, isLoading, isError, isSuccess, error, data</code>를 받을 수 있는데 <strong>mutate</strong>는 mutationFn에 정의된 서버 요청을 실행하며 <code>useMutation</code>에서 데이터를 보낼 때 사용되는 트리거다. </p>
<h2 id="react-query-options--returns">React Query Options &amp; returns</h2>
<h3 id="usequery-options---링크-httpstanstackcomqueryv4docsframeworkreactreferenceusequery">useQuery Options - [링크] (<a href="https://tanstack.com/query/v4/docs/framework/react/reference/useQuery">https://tanstack.com/query/v4/docs/framework/react/reference/useQuery</a>)</h3>
<h4 id="querykey-unknown">queryKey: unknown[]</h4>
<p><strong>필수 옵션으로</strong>, 해당 query를 식별하는 <strong>고유한 식별자 역할</strong>을 하며, query가 업데이트될 때 queryKey를 기준으로 캐시가 업데이트된다. queryKey를 해싱하여 고유 키를 생성하며 이 키를 기반으로 캐시를 관리하며, 쿼리가 실행될 때마다 해당 키로 캐시된 데이터를 찾아 응답을 제공하거나 새 데이터를 가져온다. </p>
<h4 id="queryfn-context-queryfunctioncontext--promisetdata">queryFn: (context: QueryFunctionContext) =&gt; Promise<code>&lt;TData&gt;</code></h4>
<p><strong>필수 옵션으로</strong>, 실제 데이터를 요청하는 함수로써 네트워크 요청 데이터베이스 쿼리 등과 같은 작업을 처리한다. <code>context</code>라는 매개변수를 받으며 <strong>Promise</strong>를 반환해야한다. 반드시 <strong>Promise</strong>를 반환해야하며 유효한 데이터를 resolve해야한다. 즉, <strong>undefined</strong>가 반환되서는 안되며, <strong>reject</strong> 시 적절한 에러를 던져야한다. </p>
<h4 id="enabled-booleandefault-true">enabled: boolean(default: true)</h4>
<p>쿼리가 자동으로 실행될 지 여부를 결정한다. <code>false</code>로 설정하면 해당 쿼리는 자동으로 실행되지 않으며 수동으로 트리거할 수 있다. </p>
<pre><code class="language-jsx">import { useQuery } from '@tanstack/react-query';

const fetchUser = async (userId) =&gt; {
  const res = await fetch(`/api/user/${userId}`);
  if (!res.ok) {
    throw new Error('Failed to fetch user');
  }
  return res.json();
};

const fetchBoard = async (userId) =&gt; {
  const res = await fetch(`/api/board?userId=${userId}`);
  if (!res.ok) {
    throw new Error('Failed to fetch board');
  }
  return res.json();
};

function UserBoard({ userId }) {
  // 1. user 데이터 불러오기
  const { data: user, isLoading: userLoading, error: userError } = useQuery(
    ['user', userId],
    () =&gt; fetchUser(userId)
  );

  // 2. user 데이터가 준비되었을 때 board 데이터 불러오기
  const { data: board, isLoading: boardLoading, error: boardError } = useQuery(
    ['board', user?.id],
    () =&gt; fetchBoard(user.id),
    {
      enabled: !!user, // user 데이터가 준비되기 전에 board 데이터 요청하지 않음
    }
  );</code></pre>
<h4 id="staletime-number--infinitydefault-0">staleTime: number | Infinity(default: 0)</h4>
<p>데이터를 얼마나 오래 <strong>캐시할지를 설정한다.</strong> 지정된 시간 동안 데이터는 만료되지 않고 캐시된 상태로 유지된다. 기본값은 0으로 설정되어 있기 때문에 즉시 만료되며, 이를 늘려주면 해당 데이터는 캐시된 상태로 더 오래 유지된다. staleTime이 지나면 즉시 리패치를 시키는 것이 아니라, 어떤 트리거에 의해 데이터 요청이 일어날 때 staleTime이 지나면 data가 리패치 되는 것이다.</p>
<h4 id="cachetime-number--infinitydefault-5--60--1000v5-cachetime--gctime">cacheTime: number | Infinity(default: 5 * 60 * 1000)(v5 cacheTime =&gt; gcTime)</h4>
<p>데이터가 <code>unused/inactive cache data</code>일 때 해당 데이터는 <strong>garbage collect</strong>에 들어가게 되는데, 이 때, 이 데이터를 얼마나 보관할 건지에 대한 시간이다. 만약 데이터를 사용하는 컴포넌트가 렌더링 되었을 때, cacheTime을 지나지 않았다면 <strong>garbage collect</strong>에서 데이터를 그대로 가져와 사용할 수 있다. </p>
<blockquote>
<h4 id="staletime-vs-cachetime">staleTime vs cacheTime</h4>
<p><strong>staleTime</strong> 같은 경우 data가 <strong>caching</strong> 되어 있지만, 이 cache된 데이터가 <code>fresh</code> 상태인지, <code>stale</code> 상태인 지 확인할 수 있는 시간이다. 만약 stale 상태이면 해당 caching 데이터를 새로 refetch를 시켜줘야한다. 데이터가 최신 상태가 아님을 나타내며, 자동으로 refetch가 된다.</p>
</blockquote>
<p>반면, <strong>cacheTime</strong> 같은 경우 data가 <strong>caching</strong> 되어 있는 시간을 의미한다. 만약 <strong>cacheTime</strong>이 지난다면, 해당 데이터는 사라지게 된다. 데이터가 사용되지 않거나 접근되지 않음을 의미하며, 자동으로 refetch되지 않는다.</p>
<h4 id="refetchonwindowfocus-boolean--always--query-query--boolean--alwaysdefault-true">refetchOnWindowFocus: boolean | &quot;always&quot; | ((query: Query) =&gt; boolean | &quot;always&quot;)(default: true)</h4>
<p>브라우저 창이 포커스를 받을 때 쿼리를 다시 실행할지 여부를 결정한다. 데이터가 최신 상태여야 할 때 유용하다. <strong>false</strong>로 설정하면, 윈도우 포커스 시 쿼리를 다시 호출하지 않는다. </p>
<blockquote>
<h4 id="always-vs-true">always vs true</h4>
<p>공통점: 둘 다 새로 focus될 때마다 data를 <strong>refetch</strong>를 시킨다.</p>
</blockquote>
<ul>
<li><strong>always</strong> : focus할 때마다, 무조건 refetch를 시킨다. data status가 <strong>fresh</strong>여도 refetch를 시킨다.</li>
<li><strong>true</strong> : focus할 때, 무조건 refetch를 시키는 것이 아닌, data status가 <strong>stale</strong> 상태일 때만 refetch를 시킨다.</li>
</ul>
<h4 id="refetchinterval-number--false--data-tdata--undefined-query-query--number--falsedefault-false">refetchInterval: number | false | ((data: TData | undefined, query: Query) =&gt; number | false)(default: false)</h4>
<p>데이터를 주기적으로 <strong>refetch</strong>하도록 설정한다. 숫자인 경우 밀리초마다 refetch를 시킨다. <strong>false</strong>인 경우 자동 refetch를 비활성화시키며, 함수인 경우, 동적으로 refetch 간격을 계산할 수 있게 해준다.</p>
<h4 id="retry-boolean--number--failurecount-number-error-terror--booleandefault-3">retry: boolean | number | (failureCount: number, error: TError) =&gt; boolean(default: 3)</h4>
<p>만약 query 요청이 실패했을 때 자동으로 재시도하는 횟수를 설정한다. <strong>false</strong>로 설정하면 재시도를 하지 않는다.</p>
<h4 id="onsuccess--onerror--onsettled-data-tdata-error-terror--voiddeprecated">onSuccess / onError / onSettled: (data?: TData, error?: TError) =&gt; void(deprecated)</h4>
<p>query가 성공하거나 실패하거나 또는 성공 &amp; 실패한 이후 콜백함수로 추가작업을 실행시킬 때  주로 사용한다. v5부턴 해당 콜백함수들이 <strong>deprecated</strong>가 되었는데, data fetch와 side effect를 분리하여 역할을 명확하게 나누기 위함이다. error는 주로 전역<code>querproviderclient</code>에서 처리하며, onSuccess 같은 경우 <code>useEffect 또는 status</code> 로 처리하라고 권장한다.</p>
<pre><code class="language-jsx">// error
const queryClient = new QueryClient({
  queryCache: new QueryCache({
    onError: (error, query) =&gt; {
      if (query.meta.errorMessage) {
        toast.error(query.meta.errorMessage)
      }
    },
  }),
})

export function useTodos() {
  return useQuery({
    queryKey: ['todos', 'list'],
    queryFn: fetchTodos,
    meta: {
      errorMessage: 'Failed to fetch todos',
    },
  })
}</code></pre>
<h4 id="placeholderdata-tdata----tdata">placeholderData: TData | () =&gt; TData</h4>
<p><strong>status</strong>가 <strong>loading(pending)</strong> 상태일 때 표시할 데이터를 설정한다. 이 데이터를 로딩중에 보여주며, placeholderData 같은 경우 caching되어 있지 않아, 계속 설정을 해주거나, <code>keepPreviousData</code> option을 true로 설정해줘야한다.</p>
<h4 id="select-data-tdata--unknown">select: (data: TData) =&gt; unknown</h4>
<p>query data가 <strong>성공적으로 얻은 이후</strong> 반환된 데이터를 <strong>변형하거나 필터링</strong>하는 데 사용할 수 있는 함수이다. </p>
<pre><code class="language-jsx">const { data } = useQuery(['posts'], fetchPosts, {
  select: (data) =&gt; data.filter((post) =&gt; post.isActive),
});</code></pre>
<h4 id="suspense-booleandefault-false">Suspense: boolean)(default: false)</h4>
<p><strong>suspense</strong>은 <strong>React Suspense</strong>를 사용하여 비동기 데이터 로딩을 처리하는 기능이다. suspense 옵션을 활성화하면 React Query는 데이터를 로딩하는 동안 Suspense boundary로 감싸서 로딩 상태를 관리할 수 있게 된다.</p>
<h3 id="usequery-returns">useQuery returns</h3>
<h4 id="status-stringloading--error--success">status: string(loading | error | success)</h4>
<ul>
<li><strong>loading</strong> : 캐시된 데이터가 없고 쿼리 시도도 아직 끝나지 않은 경우</li>
<li><strong>error</strong> : 쿼리 시도가 에러로 끝났을 경우, <code>error</code> 프로퍼티는 에러메세지를 반환한다. 즉, status에서의 error는 상태를 의미하고, error property는 error msg를 의미한다.</li>
<li><strong>success</strong> : 쿼리가 에러 없이 응답을 받았고, 데이터를 화면에 표시할 준비가 되었을 경우. 이 때, <code>data</code> property에는 성공적으로 가져온 데이터가 들어가며, <code>option</code> 에서 <code>enabled: false</code>로 설정되어 있고 쿼리 요청이 가지 않았다면, data에는 <strong>initial data</strong>가 들어가있다.</li>
</ul>
<h4 id="isloading--issuccess--iserror-boolean">isLoading / isSuccess / isError: boolean</h4>
<p><strong>status</strong> 기반으로 자동으로 설정되는 <strong>boolean</strong> 값이며 <code>status</code> 를 비교할 필요 없이 간단히 해당 return으로 비교하면 된다.</p>
<h4 id="data-tdata">data: TData</h4>
<p>쿼리 요청이 성공적으로 갖고왔다면 data에 값을 저장한다. 최초에는 <code>undefined</code>값이 할당되어 있다.</p>
<h4 id="error-null--terror">error: null | TError</h4>
<p>default 값으로 null이 들어오며, error가 발생했을 시, 해당 에러 객체가 이 변수에 저장된다.</p>
<blockquote>
<h4 id="isloading-vs-isfetching">isLoading vs isFetching</h4>
</blockquote>
<ul>
<li><strong>isLoading</strong> : 최초 데이터를 가져올 때를 의미한다. 즉, 쿼리요청이 처음일 때, data가 <code>undefined</code>일 때를 의미한다. </li>
<li><strong>isFetching</strong> : 쿼리 요청이 실행될 때를 의미한다. 즉, 한 번 이상의 데이터 쿼리 요청이 이뤄지면, <strong>isLoading</strong>은 <code>false</code>이지만, <strong>isFetching</strong>은 <code>true</code>이다.  </li>
</ul>
<h3 id="usemutation-options---링크">useMutation options - <a href="https://tanstack.com/query/v4/docs/framework/react/reference/useMutation">링크</a></h3>
<h4 id="mutationfn-variables-tvariables--promisetdata">mutationFn: (variables: TVariables) =&gt; Promise<code>&lt;TData&gt;</code></h4>
<p><strong>필수 옵션으로</strong>, api 요청을 처리하는 함수로써, <strong>반드시 <code>promise</code> 객체를 반환해야한다.</strong> <code>mutate(variable)</code>을 호출하면, <strong>mutationFn parameter</strong>로 variable이 들어온다.</p>
<h4 id="onmutate--onsuccess--onerror--onsettled-data-tdata-error-terror-variables-tvariables-context-tcontext--promiseunknown--unknown">onMutate / onSuccess / onError / onSettled: (data: TData, error: TError, variables: TVariables, context?: TContext) =&gt; Promise<code>&lt;unknown&gt;</code> | unknown</h4>
<blockquote>
<p>mutation.mutate(variables)  // → 1️⃣ onMutate 실행<br />    ⬇<br />    요청 진행 중... (isLoading: true, isFetching: true)<br />    ⬇<br />✅ 성공하면 → 2️⃣ onSuccess 실행<br />❌ 실패하면 → 3️⃣ onError 실행<br />    ⬇<br />4️⃣ onSettled (성공/실패와 관계없이 무조건 실행) </p>
</blockquote>
<p><code>useMuation</code>의 콜백함수로써, <code>useQuery</code>와 다르게 <strong>depercated</strong>가 되지 않고 v5에도 사용되고 있다. <code>onMuate</code>같은 경우 <strong>Optimistic Update</strong>에서 주로 사용되며 error가 발생 시 <strong>rollback</strong>할 수 있다. 또한 <code>onSettled</code>는 성공하든 실패하든 실행되며 주로 <strong>cleanup</strong>처리할 때 사용된다.</p>
<h4 id="mutationkey-unknown">mutationKey: unKnown[]</h4>
<p>queryKey와 같이 해당 mutation의 <strong>고유 식별자 역할</strong>을 한다. 특이하게도 <strong>required가 아닌 optional</strong>이다. <strong>queryKey</strong>가 required인 이유는 <strong>데이터를 캐싱하기 위함이다.</strong> 해당 queryKey를 가지고 caching을 진행하기 때문에 <strong>key</strong>는 필수적이며, mutation 같은 경우 단순히 식별자 역할만 하기 때문에 다시말해, <strong>mutation status</strong>를 추적하는데만 사용되기 때문에 optional인 것이다.</p>
<h3 id="usemutation-returns">useMutation returns</h3>
<h4 id="mutate-variables-tvariables--onsuccess-onsettled-onerror---void">mutate: (variables: TVariables, { onSuccess, onSettled, onError }) =&gt; void</h4>
<p><strong>mutate 함수</strong>는 mutation을 실행할 때 사용된다. 주로 변경할 데이터를 인수로 받는다. <strong>mutate 함수</strong>는 동기적으로 실행되며, 선택적으로 성공, 실패, 완료 시 실행할 콜백 함수들 <code>(onSuccess, onError, onSettled)</code>을 설정할 수 있다. 해당 variables는 <strong>mutateFn</strong>에서 parameter로 사용할 수 있다. <code>useMutation</code>의 <strong>options callback</strong> 과 비교하자면 useMutation에서는 해당 mutation이 성공했을 때 전부 실행되는 것이며, mutate argument로 존재하는 <strong>callback</strong>들은 해당 mutate에서 만족했을 때만 실행되는 것이다.</p>
<h4 id="mutateasync-variables-tvariables--onsuccess-onsettled-onerror---promisetdata">mutateAsync: (variables: TVariables, { onSuccess, onSettled, onError }) =&gt; Promise<code>&lt;TData&gt;</code></h4>
<p><code>mutate</code>와 동일한 역할을 한다. 다만, <code>mutate</code>같은 경우 동기적으로 처리되는 대신, <code>mutateAsync</code>같은 경우 비동기로 처리할 때 사용한다. 주로 순차적으로 mutateion을 호출해야하는 경우에 사용된다. </p>
<h4 id="status-stringidle--loading--error--success">status: string(idle | loading | error | success)</h4>
<ul>
<li><strong>idle</strong> : 초기상태 / 요청이 시작되지 않은 상태</li>
<li><strong>loading</strong> : 변경 작업이 진행 중일 때 상태</li>
<li><strong>error</strong> : 에러가 발생했을 때 상태</li>
<li><strong>success</strong> : 요청이 성공적으로 완료된 상태</li>
</ul>
<h4 id="data-undefined--unknown">data: undefined | unKnown</h4>
<p><strong>status가 success</strong>일 때 받은 data가 할당된다. 최초 data같은 경우 <code>undefined</code>로 할당되어있다.</p>
<h4 id="error-null--terror-1">error: null | TError</h4>
<p><strong>status가 error</strong>일 때 error object가 할당된다. </p>
<h4 id="reset---void">reset: () =&gt; void</h4>
<p><strong>reset</strong>같은 경우 내부 상태를 초기화시킬 때 사용된다. 주로, 다시 시도할 때 내부 상태를 초기화하거나, 에러 처리후 mutation을 초기화할 때 사용된다.</p>
<h2 id="swr---swr-공식-홈페이지">SWR - <a href="https://swr.vercel.app/ko">SWR 공식 홈페이지</a></h2>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/f125aa41-d87f-443f-9a2d-ebc3d9e26017/image.png" /></p>
<p>&quot;SWR&quot;이라는 이름은 <strong>HTTP RFC 5861(opens in a new tab)</strong>에 의해 알려진 HTTP 캐시 무효 전략인 <strong>stale-while-revalidate</strong>에서 유래되었다. <strong>SWR</strong>은 먼저 캐시(stale)로부터 데이터를 반환한 후, <strong>fetch 요청(revalidate)</strong>을 하고, 최종적으로 최신화된 데이터를 가져오는 전략이다. SWR을 사용하면 컴포넌트는 지속적이며 자동으로 데이터 업데이트 스트림을 받게 된다.</p>
<blockquote>
<h4 id="swr-톺아보기">SWR 톺아보기</h4>
</blockquote>
<ul>
<li><strong>Stale-While-Revalidate</strong> :  <strong>오래된 데이터(stale data)</strong>를 먼저 보여주고, 백그라운드에서 새로운 데이터를 가져와 최신 상태로 갱신하는 방식이다. 이렇게 함으로써 사용자는 빠른 응답을 받을 수 있으며, 데이터가 새로고침될 때까지 기다릴 필요가 없다.<blockquote>
</blockquote>
</li>
<li><strong>Revalidation</strong> : SWR은 <strong>자동 재요청 기능</strong>을 제공하여, 일정 시간이 지난 후(<strong>stale status</strong>) 데이터의 유효성을 다시 검증한다. 이를 통해 캐시된 데이터가 일정 시간이 지나면 자동으로 업데이트된다. 기본적으로 서버에 재요청을 보내고, 그 결과로 받은 최신 데이터를 다시 캐시에 저장한다.<blockquote>
</blockquote>
</li>
<li><strong>Low Learning Curve</strong> : <code>useSWR 훅</code>을 사용하여 데이터를 쉽게 가져오고, 로딩 상태나 에러 상태 등을 손쉽게 관리할 수 있다. 설정이 간단하고, 불필요한 옵션이 거의 없기 때문에 바로 적용해 볼 수 있는 장점이 있다.<blockquote>
</blockquote>
</li>
<li><strong>Auto Error Handling &amp; Retry</strong> : <strong>SWR</strong>은 자동 에러 핸들링을 지원한다. 요청이 실패하면 자동으로 재시도할 수 있도록 설정할 수 있다. 이를 통해 네트워크 오류나 서버의 일시적인 오류로 인한 요청 실패를 자동으로 복구할 수 있다.<blockquote>
</blockquote>
</li>
<li><strong>Asynchronous Data Processing</strong> : <strong>SWR</strong>은 비동기 데이터 처리를 위해 <code>fetcher 함수</code>를 사용한다. 이 함수는 데이터를 가져오는 역할을 하며, <strong>fetch나 axios</strong>를 사용해 HTTP 요청을 보낼 수 있다. <strong>SWR</strong>은 이를 통해 비동기적으로 데이터를 처리하고, 해당 데이터를 캐시하고, 백그라운드에서 최신 데이터를 다시 요청하는 방식을 사용한다.</li>
</ul>
<p><strong>react query의 <code>QueryClientProvider</code></strong>와 동일하게 <strong>SWR에도 <code>SWRConfig</code></strong>가 존재한다. 역할은 비슷하면서 다른게, 전역 설정 같은 경우 동일하게 진행되지만, <strong>react query</strong>는 중앙집중형을 채택하여 모든 <strong>query &amp; status</strong>를 처리할 수 있다. 즉, <strong>SWRConfig</strong> 같은 경우 <strong>middleware</strong> 역할을 하며, <strong>QueryClientProvider</strong> 같은 경우 <strong>middleware + state management</strong> 역할을 한다. </p>
<p><strong>SWR의 <code>SWRConfig</code></strong>는 주로 <code>fetcher 함수</code>를 전역에서 관리하거나, <strong>재시도 설정</strong>을 일괄적으로 설정한다. 이로인해 전역적으로 일관된 데이터 요청과 에러 처리를 할 수 있으며, 애플리케이션의 효율성을 높일 수 있다.</p>
<h3 id="useswr---링크">useSWR - <a href="https://swr.vercel.app/ko/docs/api">링크</a></h3>
<p><strong>React Query의 <code>useQuery / useMutation</code></strong> 과 다르게 <strong>SWR은 <code>useSWR</code></strong> hook 으로만 처리한다.</p>
<blockquote>
<pre><code class="language-jsx">// SWR Structure 
const { data, error, isLoading, isValidating, mutate } = useSWR(key, fetcher, options)</code></pre>
</blockquote>
<pre><code>
### useSWR Argument
**arugment**로 총 세가지 인자가 들어온다. 
- **key** : 요청을 식별하는 고유한 키 문자열로, 주로 url을 보내준다. `fetcher`에서 **parameter**로 key를 받을 수 있는데, `fetcher`를 공통적으로 사용하기 위해선 url을 key로 등록한다.
- **fetcher** : 데이터를 가져오기 위한 함수를 반환하는 **Promise**이다. 전역에서 fetcher를 제공하고 있으면, argument를 설정할 필요가 없다.
&gt;```jsx
import React from 'react';
import { SWRConfig } from 'swr';
&gt;
const fetcher = (url) =&gt; fetch(url).then((res) =&gt; res.json());
&gt;
function App() {
  return (
    &lt;SWRConfig value={{ fetcher }}&gt;
      &lt;YourComponent /&gt;
    &lt;/SWRConfig&gt;
  );
}
&gt;
export default App;


- **options** : **SWR hook**을 위한 옵션 객체로써 자세한 option 내용은 [참조](https://swr.vercel.app/ko/docs/api)
### useSWR returns
- **data** : `fetcher`가 성공적으로 완료되었을 때 서버에서 받은 데이터이다. 만약 **error**가 발생하면 `undefined`가 할당된다. 또한 초기값으로 `undefined`이다.
- **error** : 요청중에 에러가 발생했을 때 해당 에러 객체가 들어온다. 에러가 발생하지 않으면, `undefined`이며, 에러가 발생했을 시 `object` 형태로 저장된다.
- **isLoading** : 데이터 요청이 처음 시작될 때 **true**로 설정되며, 데이터를 로드 중인 상태임을 나타낸다. 캐시된 데이터가 존재하거나, 요청이 완료되면 **false**로 설정된다.
- **isValidating** : 데이터를 새로고침하고 있는 상태일 때 **true**로 설정된다. **isLoading**과 비슷하지만, 이미 캐시된 데이터가 있을 경우에 **true**로 설정될 수 있다.
- **mutate(data?, option?)** : **mutate**는 캐시된 데이터를 수동으로 업데이트하거나 새로고침하는 함수이다. 이를 사용해 데이터 갱신을 트리거할 수 있다.

### React Query vs SWR
| **특징**                        | **React Query**                                          | **SWR**                                                  |
|---------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| **캐싱**                        | 기본적으로 제공, 데이터가 자동으로 캐시되고 관리됨       | 기본적으로 제공, `revalidate`와 `mutate`로 캐시 관리    |
| **리페칭**                       | `refetch` 함수 제공, 자동 리페칭 옵션(`refetchOnWindowFocus`, `refetchInterval`) | `revalidateOnFocus`, `refreshInterval` 등으로 리페칭 관리 |
| **에러 처리**                    | `onError`, `onSuccess`, `onSettled` 콜백 제공             | `onErrorRetry`로 에러 재시도 관리                        |
| **쿼리 함수 (queryFn)**           | 비동기 함수로 데이터를 가져오는 `queryFn` 필수           | `fetcher`로 데이터를 가져오는 함수 필요                   |
| **상태 관리**                    | `isLoading`, `isError`, `isSuccess`, `isFetching` 상태 제공 | `isLoading`, `error`, `data` 상태 제공                    |
| **다중 요청 처리**                | `useQueries` 훅을 통해 다중 쿼리 처리 가능               | `useSWR` 훅으로 다중 데이터 요청을 처리할 수 있음        |
| **동기/비동기 처리**              | `mutateAsync`, `mutate` 함수 제공                        | `useSWR`은 기본적으로 비동기 방식                       |
| **API 호출 함수 추상화**          | `mutationFn`과 `queryFn`으로 분리하여 명확하게 호출 처리   | `fetcher`로 데이터 호출 함수 하나로 처리                  |
| **Suspense 지원**                | Suspense를 지원, `enabled` 옵션으로 활용 가능            | Suspense를 지원, `suspense` 옵션으로 사용                |
| **옵션 설정**                    | `staleTime`, `cacheTime`, `retry`, `enabled`, `initialData` 등 | `revalidateOnFocus`, `refreshInterval`, `mutate` 등        |
| **전역 상태 관리**                | `QueryClient`로 전역 상태 관리                           | 별도 전역 상태 관리 기능은 없음 (직접 구현 필요)           |
| **동시 요청 처리**                | `useMutation` 훅을 사용한 동시 요청 처리 가능            | 기본적으로 `fetcher`를 통해 하나씩 처리                  |
| **인증 및 토큰 관리**              | 쿼리마다 `context`나 `headers`로 인증 정보 관리 가능       | 기본적으로 `fetcher`에 인증 정보를 전달                   |
| **사용자 지정 `queryKey`**        | `queryKey`를 통해 쿼리의 고유 식별자를 설정               | `key`를 통해 고유 식별자 설정                            |
| **서버와 클라이언트의 데이터 동기화**| 서버와 클라이언트 간 데이터 동기화, 자동 캐시 업데이트 가능  | 기본적으로 데이터 동기화 처리, 리페칭 필요                |</code></pre>