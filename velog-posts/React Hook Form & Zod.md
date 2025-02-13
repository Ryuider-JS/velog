<h2 id="form-라이브러리-react-hook-form">Form 라이브러리 React-Hook-Form</h2>
<p><strong>React</strong>에서 Form을 효율적으로 관리하기 위한 라이브러리이다. 이 라이브러리는 폼 상태 관리와 유효성 검사를 간편하게 처리할 수 있도록 설계되었으며, <code>비제어 컴포넌트(uncontrolled components)</code>를 기반으로 성능을 최적화한다. 특히, <strong>다중 필드를 가진 대규모 폼이나 고성능이 요구되는 상황에서 React-Hook-Form은 매우 유용하다.</strong></p>
<blockquote>
<p><strong>주요기능 및 특징</strong></p>
</blockquote>
<ul>
<li><strong>성능 최적화</strong> : <code>react-hook-form</code>은 폼 요소를 직접 관리하는 ref를 활용하여, 필드 값이 변경될 때 불필요한 리렌더링을 방지한다. 이를 통해 폼의 성능을 최적화하여, 대규모 폼에서도 성능 저하 없이 동작한다.<blockquote>
</blockquote>
</li>
<li><strong>비제어 컴포넌트 기반 설계</strong> :  라이브러리는 비제어 컴포넌트를 사용하여, 기본적인 폼의 상태를 DOM에 저장하고 필요한 시점에만 상태를 가져온다. 이는 컨트롤된 컴포넌트보다 코드가 간결해지고 성능이 향상되는 장점이 있다.<blockquote>
</blockquote>
</li>
<li><strong>간편한 유효성 검사</strong> : <code>react-hook-form</code>은 HTML5 기본 유효성 검사와 커스텀 유효성 검사를 함께 지원한다. 또한, <code>Yup이나 Zod</code>와 같은 유효성 검사 라이브러리와도 쉽게 통합할 수 있어 복잡한 검증 로직도 간편하게 구현할 수 있다.</li>
<li><strong>작고 가벼운 라이브러리</strong> : <code>react-hook-form</code>은 의존성이 거의 없고 크기가 작아, 프로젝트에 추가해도 번들 크기 증가가 적다.</li>
</ul>
<h4 id="비제어-컴포넌트-vs-제어-컴포넌트">비제어 컴포넌트 vs 제어 컴포넌트</h4>
<ul>
<li><strong>비제어 컴포넌트</strong> : 바닐라 자바스크립트처럼 submit함수를 실행할 때 ref 로 input값을 한번에 변경한다.</li>
<li><strong>제어 컴포넌트</strong> : 사용자의 입력을 기반으로 state를 실시간으로 관리(setState 사용). 쉽게 말해 입력이 될 때마다 state값이 실시간으로 변경된다.</li>
</ul>
<h3 id="react-hook-form-예시">React-Hook-Form 예시</h3>
<pre><code class="language-javascript">export default function GraphqlMutationPage() {
    const {register , handleSubmit} = useForm()

    const onClickSubmit = (data)=&gt;{
        console.log(data)
    }

    return (
    &lt;form onSubmit={handleSubmit(onClickSubmit)}&gt;
      작성자: &lt;input type=&quot;text&quot; {...register(&quot;writer&quot;)} /&gt;&lt;br /&gt;
      제목: &lt;input type=&quot;text&quot; {...register(&quot;title&quot;)} /&gt;&lt;br /&gt;
      내용: &lt;input type=&quot;text&quot; {...register(&quot;contents&quot;)} /&gt;&lt;br /&gt;
      주소: &lt;input type=&quot;text&quot; {...register(&quot;boardAddress.addressDetail&quot;)} /&gt;&lt;br /&gt;
      &lt;button type=&quot;submit&quot;&gt;클릭&lt;/button&gt;
    &lt;/form&gt;
  );
}</code></pre>
<blockquote>
</blockquote>
<p><strong>register</strong> : state를 등록하는데 필요한 모든 기능이 들어있다.
<strong>handleSubmit</strong> : register에 적힌 state를 등록해주는 함수.
<strong>formState</strong> : 폼 상태를 포함하며, <code>errors, isDirty, isSubmitting</code>과 같은 속성을 제공한다. 각 속성은 폼의 상태를 나타내며 유효성 검사와 에러 처리를 쉽게 해준다.
<strong>reset</strong> : 폼의 상태를 초기화하거나 특정 값을 설정하여 리셋할 수 있다.</p>
<h3 id="react-hook-form-주요-메서드-및-속성">React Hook Form 주요 메서드 및 속성</h3>
<h4 id="useform">useForm</h4>
<p><strong>React Hook Form</strong>의 기본 훅으로, 폼 상태와 메서드들을 제공한다. </p>
<blockquote>
<p><strong>defaultValues</strong> - form 초기값 설정 / API를 통해 데이터를 초기값으로 설정하거나 폼 초기 상태를 기본값으로 설정하고 싶을 때 사용한다. 
<strong>mode</strong> - 유효성 검사가 실행되는 타이밍을 설정한다. <code>onSubmit / onChange / onBlur / all</code> , 실시간으로 유효성 검사를 해야하는 경우 <code>onChange</code>를 사용하고 form 성능이 중요하거나 간단한 유효성 검사를 할 때는 <code>onSubmit</code>을 사용한다. 
<strong>resolver</strong> - <strong>Zod, Yup</strong> 등과 같은 유효성 라이브러리와 통합할 때 사용한다. 주로 <strong>복잡한 유효성 검사</strong>를 처리하거나 데이터 스키마와 타입 정의를 함께 사용하고 싶을 때 사용한다. 
<strong>shouldFocusError</strong> - 유효성 검사 실패 시, 오류가 발생한 필드로 포커스를 이동할지 여부를 설정한다. default 값으로 <strong>true</strong>이며 주로 다중 필드 폼에서 오류가 난 필드로 자동으로 포커스 이동을 원치 않는 경우 <strong>false</strong>로 설정한다. 
<strong>shouldUnregister</strong> - 폼에서 필드가 <strong>UnMount</strong>되었을 때, 해당 필드를 상태에서 제거할지 여부를 설정한다. default 값으로 <strong>false</strong>이며 주로 동적으로 추가/삭제되는 필드가 많을 때, 상태를 초기화하려는 경우 <strong>true</strong>로 설정한다.
<strong>reValidateMode</strong> - 이미 유효성 검사를 받은 필드가 다시 검사를 받을 조건을 설정한다. <code>onChange, onBlur</code> , 입력값 수정 중에는 검사를 피하고 싶을 때 <strong>onBlur</strong>를 사용한다.
<strong>defaultValuesFromResolver</strong> - <strong>resolve</strong>에서 가져온 값을 초기값으로 설정할지 여부를 지정한다. default 값으로 <strong>false</strong>이며 외부 유효성 검사 라이브러리에서 초기값을 자동으로 가져오고 싶을 때 true로 설정한다
<strong>criteriaMode</strong> - 에러 메세지를 수집하는 방식 설정으로 default로는 <strong>firstError</strong>이지만 모든 에러를 반환하고 싶으면 <strong>all</strong> 설정하면 된다.
<strong>delayError</strong> - 에러 메시지를 일정 시간 지연 후 표시한다. 주로 에러 메세지가 실시간으로 나타나 사용자를 방해하지 않도록 조정할 때 사용한다. 
<strong>shouldUseNativeValidation</strong> - 브라우저의 기본 HTML5 유효성 검사를 활성화할 때 사용한다. default로 <strong>false</strong>이며 기본 유효성 검사를 활용해 간단한 유효성 검사를 구현할 때 사용한다. </p>
</blockquote>
<pre><code class="language-jsx">import React from &quot;react&quot;;
import { useForm } from &quot;react-hook-form&quot;;
import { z } from &quot;zod&quot;;
import { zodResolver } from &quot;@hookform/resolvers/zod&quot;;

const schema = z.object({
  username: z.string().min(3, &quot;Username must be at least 3 characters long&quot;),
  email: z.string().email(&quot;Invalid email address&quot;),
  password: z.string().min(6, &quot;Password must be at least 6 characters long&quot;),
});

const FormExample = () =&gt; {
  const {
    register,
    handleSubmit,
    formState: { errors },
    watch,
    setValue,
  } = useForm({
    mode: &quot;onChange&quot;, // 실시간 유효성 검사
    reValidateMode: &quot;onBlur&quot;, // 필드 수정 후 blur 시 재검사
    defaultValues: {
      username: &quot;defaultUser&quot;, // 초기값 설정
      email: &quot;&quot;,
      password: &quot;&quot;,
    },
    resolver: zodResolver(schema), // Zod를 사용한 유효성 검사
    shouldFocusError: true, // 에러가 발생한 필드로 포커스 이동
  });

  const onSubmit = (data) =&gt; {
    console.log(&quot;Submitted Data:&quot;, data);
  };

  const username = watch(&quot;username&quot;); // 특정 필드 관찰
  const setDefaultEmail = () =&gt; setValue(&quot;email&quot;, &quot;default@example.com&quot;); // 필드값 설정

  return (
    &lt;form onSubmit={handleSubmit(onSubmit)}&gt;
      &lt;div&gt;
        &lt;label&gt;Username&lt;/label&gt;
        &lt;input {...register(&quot;username&quot;)} /&gt;
        {errors.username &amp;&amp; &lt;p&gt;{errors.username.message}&lt;/p&gt;}
        &lt;p&gt;Currently Typed: {username}&lt;/p&gt;
      &lt;/div&gt;

      &lt;div&gt;
        &lt;label&gt;Email&lt;/label&gt;
        &lt;input type=&quot;email&quot; {...register(&quot;email&quot;)} /&gt;
        {errors.email &amp;&amp; &lt;p&gt;{errors.email.message}&lt;/p&gt;}
        &lt;button type=&quot;button&quot; onClick={setDefaultEmail}&gt;
          Set Default Email
        &lt;/button&gt;
      &lt;/div&gt;

      &lt;div&gt;
        &lt;label&gt;Password&lt;/label&gt;
        &lt;input type=&quot;password&quot; {...register(&quot;password&quot;)} /&gt;
        {errors.password &amp;&amp; &lt;p&gt;{errors.password.message}&lt;/p&gt;}
      &lt;/div&gt;

      &lt;button type=&quot;submit&quot;&gt;Submit&lt;/button&gt;
    &lt;/form&gt;
  );
};

export default FormExample;</code></pre>
<h4 id="register">register</h4>
<p><strong>Form Fleid</strong>를 <strong>React Hook Form</strong>에 등록할 때 사용한다. </p>
<pre><code class="language-typescript">register(name: string, options?: {
  required?: boolean | string;
  min?: number | { value: number, message: string };
  max?: number | { value: number, message: string };
  maxLength?: number | { value: number, message: string };
  minLength?: number | { value: number, message: string };
  pattern?: RegExp | { value: RegExp, message: string };
  validate?: Validate | Record&lt;string, Validate&gt;;
  valueAsNumber?: boolean;
  valueAsDate?: boolean;
  setValueAs?: (value: any) =&gt; any;
  disabled?: boolean;
})</code></pre>
<blockquote>
<pre><code class="language-javascript">&lt;input {...register(&quot;username&quot;, { required: &quot;Username is required&quot; })} /&gt;</code></pre>
</blockquote>
<pre><code>
#### handleSubmit
**Form Data**를 처리하는 함수로 제출 시 유효성 검사를 실행하고 데이터 전달하는 역할을 한다. 
```javascript
const onSubmit = (data) =&gt; console.log(data);
&lt;form onSubmit={handleSubmit(onSubmit)}&gt;
  &lt;button type=&quot;submit&quot;&gt;Submit&lt;/button&gt;
&lt;/form&gt;</code></pre><h4 id="formstate">formState</h4>
<p><strong>FormState</strong>를 제공하며 유효성 검사와 관련된 여러 속성을 포함한다. 주요 속성으로 <strong>errors</strong>(필드별 에러 정보를 담고 있음) /  <strong>isSubmitting</strong>(제출 중 여부) / <strong>isDirty</strong>(폼 값이 변경되었는지 여부) / <strong>isValid</strong>(모든 유효성 검사가 통과했는지 여부)</p>
<pre><code class="language-javascript">const { formState: { errors, isSubmitting, isValid } } = useForm();</code></pre>
<h4 id="watch">watch</h4>
<p>특정 필드 값이나 전체 폼 데이터를 실시간으로 관찰한다.</p>
<pre><code class="language-javascript">const watchedValue = watch(&quot;username&quot;);
console.log(watchedValue);</code></pre>
<h4 id="reset">reset</h4>
<p>폼 상태를 초기화하거나 특정 값을 설정하여 리셋할 수 있다. </p>
<pre><code class="language-javascript">const { reset } = useForm();

&lt;button onClick={() =&gt; reset({ username: &quot;default&quot; })}&gt;Reset&lt;/button&gt;</code></pre>
<h4 id="setvalue">setValue</h4>
<p>특정 필드의 값을 프로그래밍 방식으로 특정 필드의 값을 설정한다.</p>
<pre><code class="language-javascript">const { setValue } = useForm();
setValue(&quot;username&quot;, &quot;new value&quot;);</code></pre>
<h4 id="getvalues">getValues</h4>
<p>현재 폼 상태에서 특정 필드 값 또는 전체 값을 가져온다.</p>
<pre><code class="language-javascript">const { getValues } = useForm();
console.log(getValues(&quot;username&quot;)); // 특정 필드 값
console.log(getValues()); // 전체 값</code></pre>
<h4 id="clearerrors">clearErrors</h4>
<p>특정 필드 또는 전체 폼의 에러를 제거한다.</p>
<pre><code class="language-javascript">const { clearErrors } = useForm();
clearErrors(&quot;username&quot;);</code></pre>
<h4 id="trigger">trigger</h4>
<p>특정 필드 또는 전체 폼에 대해 유효성 검사를 수동으로 실행한다.</p>
<pre><code class="language-javascript">const { trigger } = useForm();
await trigger(&quot;email&quot;);</code></pre>
<h4 id="seterror">setError</h4>
<p>필드에 수동으로 에러를 설정한다</p>
<pre><code class="language-javascript">const { setError } = useForm();
setError(&quot;username&quot;, { type: &quot;manual&quot;, message: &quot;Custom error&quot; });</code></pre>
<h2 id="유효성-검증-라이브러리-zod">유효성 검증 라이브러리 Zod</h2>
<p><strong>Zod</strong>는 타입스크립트와 함께 유용하게 사용할 수 있는 스키마 기반 유효성 검사 라이브러리다. 주어진 스키마에 맞춰 데이터의 구조와 유효성을 확인할 수 있다. <strong>Zod</strong>는 타입 정의뿐만 아니라 데이터 유효성 검사를 한 번에 처리해주기 때문에, <code>폼 데이터나 API 요청</code>에서 데이터를 다룰 때 안전하게 사용할 수 있다.</p>
<blockquote>
<p><strong>Zod의 장점</strong>
<strong>타입 안전성</strong> : TypeScript와 완벽히 통합되어, 런타임과 컴파일 타임 모두에서 타입을 강제한다.</p>
</blockquote>
<p><strong>간결하고 직관적인 API</strong> : 데이터 스키마와 유효성 검사 코드를 간결하게 작성할 수 있어 가독성이 뛰어난다.</p>
<blockquote>
</blockquote>
<p><strong>유연한 에러 처리 및 커스터마이징</strong> : 유효성 검사 실패 시 사용자 정의 에러 메시지를 제공할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>다양한 데이터 구조 지원</strong> : 단순한 문자열, 숫자뿐 아니라 객체, 배열, 중첩된 구조 등 복잡한 데이터 구조의 유효성 검사도 가능하다.</p>
<h3 id="다양한-유효성-검증-라이브러리">다양한 유효성 검증 라이브러리</h3>
<table>
<thead>
<tr>
<th><strong>라이브러리</strong></th>
<th><strong>주요 특징</strong></th>
<th><strong>장점</strong></th>
<th><strong>단점</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>Zod</strong></td>
<td>스키마 기반 유효성 검사 라이브러리. <br />TypeScript와 완벽히 통합.</td>
<td>- TypeScript 타입 정의와 동시 사용<br />- 직관적 API<br />- 런타임 타입 안전성 제공</td>
<td>- 다른 라이브러리에 비해 상대적으로 간단한 기능</td>
</tr>
<tr>
<td><strong>Yup</strong></td>
<td>선언형 스키마 정의와 유효성 검사 제공. <br />포괄적인 데이터 유형 지원.</td>
<td>- 쉬운 사용법<br />- 널리 알려진 라이브러리<br />- 친숙한 API</td>
<td>- TypeScript와의 통합이 제한적</td>
</tr>
<tr>
<td><strong>Joi</strong></td>
<td>강력하고 유연한 스키마 정의를 지원. <br />Node.js 환경에서 자주 사용.</td>
<td>- 정교한 유효성 검사 가능<br />- 정규식, 조건부 검증 등 고급 기능 지원</td>
<td>- 브라우저 환경에서 사용하려면 번들 크기가 큼</td>
</tr>
</tbody></table>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/43eed271-d597-4042-9681-f086d119cbab/image.png" /></p>
<h3 id="zod에서의-스키마-정의">Zod에서의 스키마 정의</h3>
<p>Zod는 <code>z.object, z.string, z.number</code> 등을 사용해 다양한 필드와 데이터 구조를 정의할 수 있다.</p>
<blockquote>
<p><strong>문자열 필드 유효성 검사</strong>
<strong>min( )</strong>: 문자열 최소 길이 설정
<strong>max( )</strong>: 문자열 최대 길이 설정
<strong>email( )</strong>: 이메일 형식 검사</p>
</blockquote>
<p><strong>숫자 필드 유효성 검사</strong>
<strong>min( )</strong>: 최소 숫자 값을 설정
<strong>max( )</strong>: 최대 숫자 값을 설정
<strong>positive( )</strong>: 양수인지 검사
<strong>int( )</strong>: 정수인지 검사</p>
<blockquote>
</blockquote>
<p><strong>배열과 객체의 유효성 검사</strong>
배열 요소에 대한 유효성 검사 <strong>z.array( )</strong>도 가능하며, 중첩된 객체 <strong>z.object( )</strong>도 쉽게 설정할 수 있다.</p>
<h3 id="react-hook-form와-zod-통합">react-hook-form와 Zod 통합</h3>
<pre><code class="language-javascript">import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const schema = z.object({
  username: z.string().min(3, &quot;Username must be at least 3 characters long&quot;),
  email: z.string().email(&quot;Invalid email address&quot;),
});

const MyForm = () =&gt; {
  const { register, handleSubmit, formState: { errors, isValid } } = useForm({
    resolver: zodResolver(schema),
  });

  const onSubmit = data =&gt; console.log(data);

  return (
    &lt;form onSubmit={handleSubmit(onSubmit)}&gt;
      &lt;input {...register(&quot;username&quot;)} /&gt;
      {errors.username &amp;&amp; &lt;p&gt;{errors.username.message}&lt;/p&gt;}

      &lt;input {...register(&quot;email&quot;)} /&gt;
      {errors.email &amp;&amp; &lt;p&gt;{errors.email.message}&lt;/p&gt;}

      &lt;button type=&quot;submit&quot; disabled={!isValid}&gt;Submit&lt;/button&gt;
    &lt;/form&gt;
  );
};</code></pre>
<p><strong>useForm</strong>을 선언할 때 <strong>options</strong>로 resolver를 <strong>Zod schema</strong>를 넣으면 submit이 될 때 자동으로 유효성 검사가 이뤄진다. </p>