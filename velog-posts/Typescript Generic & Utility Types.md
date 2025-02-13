<p><del>I Love anyscript</del></p>
<h3 id="any-vs-unknown">Any vs unknown</h3>
<blockquote>
</blockquote>
<p><strong>any</strong> - ’어떠한 것이든지, 누구든지’ 라는 뜻으로, 어떠한 타입이 입력되더라도 전부 허용하는 타입이다. 요소에 any 타입을 부여할 경우 <strong>사실상 타입스크립트가 아닌 자바스크립트</strong>를 사용하는 것이나 마찬가지가 된다.</p>
<blockquote>
</blockquote>
<p><strong>unknown</strong> - ’알 수 없다, 모른다’ 라는 뜻으로, 개발자에게 주의를 주는 용도의 타입이다.  타입이 지정되지 않았으므로 연산에 오류가 발생할 수 있음을 경고한다.</p>
<h2 id="generic">Generic</h2>
<p><strong>typescript의 Generic 타입을 사용하면, <code>arguments에 들어오는 type</code>을 그대로 사용할 수 있다.</strong> 제네릭(Generics)은 <code>TypeScript</code>에서 데이터 타입을 유연하게 정의하고, 컴파일 시점에 타입을 안전하게 유지하기 위해 사용되는 <strong>강력한 기능</strong>이다. <strong>Generic</strong>은 함수, 클래스, 인터페이스 등을 작성할 때, 타입을 고정하지 않고 다양한 타입을 처리할 수 있도록 돕는다. 이는 코드 재사용성과 타입 안전성을 높이며, 특히 <code>컬렉션이나 API 요청</code> 등을 다룰 때 유용하다.</p>
<p><strong>Generic</strong>은 <code>코드 재사용성, 타입 안전성, 유연성</code>으로 많은 사람들의 사랑을 받고 있지만 과도하게 사용하게 되면 코드 가독성을 떨어트릴 수 있다. </p>
<pre><code class="language-typescript">// 예시

function identity&lt;T&gt;(value: T): T {
  return value;
}

// 사용 시점에 타입 지정
identity&lt;number&gt;(42); // 타입은 number
identity&lt;string&gt;(&quot;Hello!&quot;); // 타입은 string

function getGeneric&lt;T, U, V&gt;(arg1: T, arg2: U, arg3: V): [V, U, T]
    return [arg3, arg2, arg1];

getGeneric&lt;number, string, boolean&gt;(1, &quot;지승&quot;, true);</code></pre>
<h3 id="generic-interface">Generic Interface</h3>
<p><strong>Generic</strong>은 인터페이스에도 적용할 수 있다. 인터페이스에 제네릭을 사용하면 특정 타입을 기준으로 인터페이스의 구조를 정의할 수 있다.</p>
<pre><code class="language-typescript">interface KeyValuePair&lt;K, V&gt; {
  key: K;
  value: V;
}

const stringNumberPair: KeyValuePair&lt;string, number&gt; = { key: &quot;age&quot;, value: 30 };
const numberBooleanPair: KeyValuePair&lt;number, boolean&gt; = { key: 1, value: true };</code></pre>
<h3 id="generic-constraints">Generic Constraints</h3>
<p><strong>Generic</strong>에 특정 타입이 요구될 때는 <code>extends 키워드</code>를 사용해 제약 조건을 부여할 수 있다. 예를 들어, 객체에 특정 속성이 있을 때만 허용하고 싶다면 제약 조건을 사용한다.</p>
<pre><code class="language-typescript">function getProperty&lt;T extends object, K extends keyof T&gt;(obj: T, key: K) {
  return obj[key];
}

const person = { name: &quot;Alice&quot;, age: 25 };
console.log(getProperty(person, &quot;name&quot;)); // &quot;Alice&quot;
console.log(getProperty(person, &quot;age&quot;)); // 25</code></pre>
<h3 id="generic-basic-setting">Generic basic setting</h3>
<p><strong>Generic</strong>은 기본값을 설정할 수 있다. 기본 타입은 호출 시 특정 타입을 지정하지 않을 때 사용된다.</p>
<pre><code class="language-typescript">function createArray&lt;T = string&gt;(length: number, value: T): T[] {
  return Array(length).fill(value);
}

const arr1 = createArray(3, &quot;hi&quot;); // string[] 타입
const arr2 = createArray&lt;number&gt;(3, 5); // number[] 타입</code></pre>
<h2 id="generic을-이용하여-hoc-type-유연하게-처리하기">Generic을 이용하여 HOC Type 유연하게 처리하기</h2>
<h3 id="usestate">useState</h3>
<p><code>useState</code> 함수에 제네릭을 적용함으로써 초기값에 따른 상태의 타입을 유추하고, 그에 맞는 타입의 값을 설정할 수 있게 된다.</p>
<ul>
<li><code>S</code>는 제네릭 타입으로, 사용자가 이 함수 호출 시 제공한 타입을 받는다.</li>
<li><code>state</code>는 <code>S</code> 타입으로 초기값을 받아서 상태로 관리한다.</li>
<li><code>setState</code> 함수는 새로운 값을 받아 상태를 변경할 때 사용되며, 해당 값 또한 <code>S</code> 타입을 따른다.</li>
</ul>
<pre><code class="language-typescript">function useState&lt;S = undefined&gt;(): [S | undefined, Dispatch&lt;SetStateAction&lt;S | undefined&gt;&gt;];</code></pre>
<h3 id="withlogin">withLogin</h3>
<pre><code class="language-jsx">import { useRouter } from 'next/navigation';
import { ReactElement, useEffect } from 'react';

export const withLogin = &lt;P extends Record&lt;string, unknown&gt;&gt;(Component: React.ComponentType&lt;P&gt;) =&gt; {
  const WrappedComponent = (props: P): ReactElement =&gt; {
    const router = useRouter();

    useEffect(() =&gt; {
      if (localStorage.getItem('accessToken') === null) {
        alert('login fail');
        void router.push('/login');
      }
    }, []);

    return &lt;Component {...props} /&gt;;
  };

  return WrappedComponent;
};

</code></pre>
<p>HOC를 사용할 떄 네이밍 컨벤션으로 with를 접두사로 붙여준다. </p>
<p><code>Record&lt;K, T&gt;</code>는 <strong>타입스크립트에서 제네릭 타입</strong>을 나타내는 방법 중 하나로, 특정 키(<code>K</code>)와 값(<code>T</code>)의 타입을 정의하는 데 사용된다.</p>
<p>그리고 props에 반드시 객체가 들어가야 스프레드 연산자를 사용할 수 있기 때문에, extends를 사용해서 P라는 Generic 타입이 객체라는 사실을 명시해준다.</p>
<h4 id="routerpush-앞에-void를-사용하는-이유">router.push 앞에 void를 사용하는 이유</h4>
<p>router.push 같은 경우 반환값으로 <code>promise</code>를 받는다. <code>void</code>를 붙이면 이 반환값을 무시하고 더 이상 신경 쓰지 않겠다는 의도를 명확히 할 수 있다. 이 방법은 <code>Next.js 코드베이스</code>에서 종종 권장되며, 프로미스 처리를 명시적으로 하지 않으면서도 코드가 깔끔하게 유지된다.</p>
<h2 id="utility-types">Utility Types</h2>
<p><strong>TypeScript</strong>에서 <code>유틸리티 타입(Utility Types)</code>은 기존 타입을 기반으로 새 타입을 정의할 수 있게 해 주는 도구다. TypeScript는 다양한 유틸리티 타입을 제공하여 타입 정의를 간편하게 만들어주며, 타입 변환이나 특정 속성만 선택하는 등 다양한 상황에 유용하게 사용할 수 있다. 유틸리티 타입은 코드의 재사용성을 높이고, 타입을 안전하게 관리하는 데 도움을 준다.</p>
<h3 id="type-alias-vs-interface">Type Alias vs Interface</h3>
<h4 id="type-alias"><code>Type Alias</code></h4>
<ul>
<li><strong>정의 방법</strong>: <strong>type keyword</strong>를 사용하여 새 타입을 정의한다.</li>
<li><strong>사용 목적</strong>: <code>원시 타입, Object, union type, intersection type</code> 등을 정의할 때 사용한다.</li>
<li><strong>기능</strong>: 함수 타입, 유니온 타입, 튜플 등 다양한 타입을 정의할 수 있으며, 특정 값에 대한 별칭을 줄 때 유용하다.</li>
<li><strong>특징</strong>: 다양한 타입 조합에 유리하다. 예를 들어 <code>유니온 타입(type ID = string | number)</code>을 정의할 때 사용 / <strong>확장 불가능</strong>: 동일한 이름으로 추가 속성을 병합할 수 없다.</li>
</ul>
<h4 id="interface"><code>interface</code></h4>
<ul>
<li><strong>정의 방법</strong>: <strong>interface keyword</strong>를 사용하여 새 타입을 정의한다.</li>
<li><strong>사용 목적</strong>: 주로 <code>Obejct</code>를 정의하며, <code>class와 관련된 구조</code>를 정의할 때 유용하다.</li>
<li><strong>기능</strong>: 객체의 속성과 구조를 정의하며, <code>class</code>에서 <strong>implements</strong>로 구현할 수 있다.</li>
</ul>
<h3 id="combinable-types">Combinable Types</h3>
<h4 id="union-type"><code>Union Type</code></h4>
<p><strong>Union Type</strong>은 여러 타입 중 하나의 타입을 가질 수 있음을 의미한다. 각 타입 중 하나로 정의된 변수를 생성할 수 있어, 하나의 변수에 여러 타입이 허용되도록 할 때 유용하다.</p>
<pre><code class="language-typescript">type Status = &quot;success&quot; | &quot;error&quot; | &quot;loading&quot;;

let currentStatus: Status;
currentStatus = &quot;success&quot;; // OK
currentStatus = &quot;error&quot;; // OK
currentStatus = &quot;loading&quot;; // OK
currentStatus = &quot;pending&quot;; // Error: &quot;pending&quot;는 Status 타입에 존재하지 않음</code></pre>
<blockquote>
<p><strong>object</strong>의 <strong>key</strong>를 이용하여 <strong>Union Type</strong> 만들기
<code>keyof 연산자</code>를 사용하면, 객체의 키(key) 들을 유니언(Union) type으로 추출할 수 있다.</p>
</blockquote>
<pre><code class="language-typescript">interface IProfile = {
    name : string;
    age : age;
    school: string;
    hobby?: string;
}
&gt;
type keyProfile = keyof IProfile;
&gt;
let myprofile: keyProfile = &quot;hobby&quot; // name, age, school, hobby 중 하나를 선택 가능</code></pre>
<h4 id="intersection-type"><code>Intersection Type</code></h4>
<p><strong>Intersection Type</strong>은 여러 타입을 모두 만족하는 타입을 정의할 때 사용한다. 즉, 두 타입의 속성을 모두 포함하는 타입을 생성하는 데 유용하다. <strong>Intersection Type</strong> 주로 객체의 속성을 합성할 때 사용된다.</p>
<pre><code class="language-typescript">type User = {
  id: number;
  name: string;
};

type Admin = {
  role: string;
};

type AdminUser = User &amp; Admin; // User와 Admin의 속성을 모두 포함

const adminUser: AdminUser = {
  id: 1,
  name: &quot;Alice&quot;,
  role: &quot;admin&quot;, // User와 Admin의 속성을 모두 가져야 함
};
</code></pre>
<h3 id="자주-사용되는-typescript-utility-types">자주 사용되는 Typescript Utility Types</h3>
<h4 id="partialt"><code>Partial&lt;T&gt;</code></h4>
<p>타입 T의 모든 속성을 <strong>선택 사항(optional)</strong>으로 만들어준다. 주로 객체를 부분적으로 업데이트할 때 유용하다.</p>
<pre><code class="language-typescript">interface User {
  id: number;
  name: string;
  age: number;
}

const updateUser = (user: Partial&lt;User&gt;) =&gt; {
  // 모든 필드가 optional
  // 즉 Partial&lt;User&gt; {
  //      id?: number;
  //    name?: string;
  //    age?: number;
  // }
};

updateUser({ name: &quot;Alice&quot; }); // 유효</code></pre>
<h4 id="requiredt"><code>Required&lt;T&gt;</code></h4>
<p>타입 T의 모든 속성을 <strong>필수(required)</strong>로 만든다.</p>
<pre><code class="language-typescript">interface User {
  id?: number;
  name?: string;
  age?: number;
}

const newUser: Required&lt;User&gt; = {
  id: 1,
  name: &quot;Alice&quot;,
  age: 30,
}; // 모든 필드를 채워야 함
// 즉, Required&lt;User&gt; {
//         id: number;
//        name: string;
//        age: number
//     }
</code></pre>
<h4 id="readonlyt"><code>Readonly&lt;T&gt;</code></h4>
<p>타입 T의 모든 속성을 <strong>읽기 전용(read-only)</strong>으로 만들어, 값을 변경할 수 없게 한다.</p>
<pre><code class="language-typescript">interface User {
  id: number;
  name: string;
}

const user: Readonly&lt;User&gt; = { id: 1, name: &quot;Alice&quot; };
user.name = &quot;Bob&quot;; // 오류: 읽기 전용 속성은 수정할 수 없습니다.</code></pre>
<h4 id="recordk-t"><code>Record&lt;K, T&gt;</code></h4>
<p><code>키 타입 K와 값 타입 T</code>를 가진 객체 타입을 생성한다. 주로 객체에 특정 키와 값의 타입을 지정할 때 사용된다.</p>
<pre><code class="language-typescript">type Roles = &quot;admin&quot; | &quot;user&quot; | &quot;guest&quot;;
const userRoles: Record&lt;Roles, string&gt; = {
  admin: &quot;Admin User&quot;,
  user: &quot;Regular User&quot;,
  guest: &quot;Guest User&quot;,
};
// Record&lt;Roles, string&gt; 
// =&gt; key = Roles / value = string</code></pre>
<h4 id="pickt-k"><code>Pick&lt;T, K&gt;</code></h4>
<p>타입 T에서 <strong>특정 속성 K</strong>만 골라서 선택한다.</p>
<pre><code class="language-typescript">interface User {
  id: number;
  name: string;
  age: number;
}

const userIdAndName: Pick&lt;User, &quot;id&quot; | &quot;name&quot;&gt; = {
  id: 1,
  name: &quot;Alice&quot;,
}; // age는 포함되지 않음
// interface Pick&lt;User, 'id' | 'name'&gt; {
//    id: number;
//    name: string;
// }</code></pre>
<h4 id="omitt-k"><code>Omit&lt;T, K&gt;</code></h4>
<p>타입 T에서 특정 속성 K를 <strong>제외한 타입</strong>을 만든다.</p>
<pre><code class="language-typescript">interface User {
  id: number;
  name: string;
  age: number;
}

const userWithoutId: Omit&lt;User, &quot;id&quot;&gt; = {
  name: &quot;Alice&quot;,
  age: 30,
}; // id는 제외됨
// interface Omit&lt;User, 'id'&gt; {
//    name: string;
//    age: number;
// }</code></pre>
<h4 id="excludet-u"><code>Exclude&lt;T, U&gt;</code></h4>
<p>T 타입에서 U 타입에 해당하는 요소만 <strong>추출</strong>하여 <strong>Union Type</strong>을 생성한다.</p>
<pre><code class="language-typescript">type Status = &quot;success&quot; | &quot;error&quot; | &quot;loading&quot;;
type OnlyLoading = Extract&lt;Status, &quot;loading&quot;&gt;; // &quot;loading&quot;</code></pre>
<h4 id="nonnullablet"><code>NonNullable&lt;T&gt;</code></h4>
<p>T 타입에서 <code>null과 undefined</code>를 제외한 <strong>Union Type</strong>을 생성한다.</p>
<pre><code class="language-typescript">type Status = string | null | undefined;
type NonNullableStatus = NonNullable&lt;Status&gt;; // string</code></pre>
<h4 id="returntypet"><code>ReturnType&lt;T&gt;</code></h4>
<p>함수 T의 반환 타입이 <strong>Union Type</strong>이라면 해당 <strong>Union Type</strong> 추출한다.</p>
<pre><code class="language-typescript">function getUser() {
  return { id: 1, name: &quot;Alice&quot; };
}

type User = ReturnType&lt;typeof getUser&gt;; // { id: number; name: string; }
</code></pre>