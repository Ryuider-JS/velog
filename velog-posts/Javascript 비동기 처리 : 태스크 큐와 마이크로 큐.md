<h2 id="event-loop">event loop</h2>
<h3 id="call-stack">Call Stack</h3>
<p><strong>Call Stack</strong>은 현재 실행 중인 함수들이 쌓이는 구조를 말한다. 함수가 호출될 때마다 그 함수는 <strong>Call Stack</strong>에 쌓이고, 함수가 종료되면 Call Stack에서 제거된다. 자바스크립트 엔진은 <strong>Call Stack</strong>을 사용하여 코드 실행 순서를 관리한다. <strong>실행컨텍스트 스택과 비슷하지만 다른 개념이다.</strong></p>
<h4 id="실행컨텍스트-스택-vs-call-stack">실행컨텍스트 스택 vs Call Stack</h4>
<p><strong>실행 컨텍스트 스택 (Execution Context Stack)</strong>
실행 컨텍스트가 생성될 때마다 실행 컨텍스트 스택에 쌓이게 된다. 이 스택은 <strong>콜 스택(Call Stack)</strong>과 사실상 같은 구조를 공유하지만, 개념적으로는 실행 컨텍스트를 관리하는 용도에 더 가깝다. JavaScript 엔진은 이 스택을 통해 현재 실행 중인 코드 블록이나 함수의 순서를 관리한다.</p>
<ol>
<li>전역 실행 컨텍스트가 생성되고 스택에 쌓인다.</li>
<li>함수가 호출될 때마다 해당 함수의 실행 컨텍스트가 생성되어 스택에 쌓인다.</li>
<li>함수가 완료되면 해당 실행 컨텍스트는 스택에서 제거된다.</li>
</ol>
<p><strong>콜 스택 (Call Stack)</strong>
콜 스택은 JavaScript 엔진이 실제로 코드 실행을 관리하는 데 사용하는 스택 데이터 구조이다. 이 스택에는 현재 실행 중인 함수 호출들이 쌓이며, 실행 컨텍스트 스택과 유사하게 함수를 호출하고 반환하는 순서에 따라 쌓이고 제거된다. 즉, 콜 스택은 실행 컨텍스트를 관리하는 구조라고 할 수 있다.</p>
<ol>
<li>함수가 호출될 때마다 콜 스택에 함수가 쌓인다.</li>
<li>함수 실행이 완료되면 콜 스택에서 함수가 제거된다.</li>
<li>콜 스택에 있는 함수들은 순차적으로 실행되며, 비동기 작업이 완료될 때까지 대기 상태가 된다.</li>
</ol>
<p><strong>차이점 요약</strong>
실행 컨텍스트는 코드가 실행될 때마다 생성되는 특정 환경으로, <code>변수, 스코프, this</code> 등 실행에 필요한 정보를 담고 있다.
<strong>실행 컨텍스트 스택</strong>은 이러한 실행 컨텍스트들이 쌓이는 구조다. 실행 순서를 관리하는데, 콜 스택과 동일한 원리로 작동한다.</p>
<p><strong>콜 스택(Call Stack)</strong>은 JavaScript 엔진이 함수 호출과 반환을 관리하는 실제 스택이다. 함수 호출의 순서를 따라 쌓이고 제거되며, 실행 컨텍스트를 스택에 쌓는 역할을 수행한다.</p>
<p>요약하면, 실행 컨텍스트 스택과 콜 스택은 개념적으로는 구분될 수 있지만, 실제로는 같은 스택 구조를 공유하며, 실행 컨텍스트는 콜 스택에서 실행되는 함수들의 실행 환경을 나타낸다.</p>
<blockquote>
<p><strong>Call Stack의 동작 방식</strong></p>
</blockquote>
<ul>
<li><strong>LIFO(Last In, First Out)</strong> 구조로 동작합니다. 즉, 마지막에 쌓인 함수가 가장 먼저 처리되고, 해당 함수가 완료되면 그 다음으로 쌓인 함수가 처리된다.</li>
<li><strong>함수 호출</strong>: 함수가 호출되면 Call Stack의 맨 위에 추가된다.</li>
<li><strong>함수 완료</strong>: 함수가 실행을 마치면 Call Stack에서 제거된다.<blockquote>
<p><strong><em>FIFO(First In, First Out)</em></strong></p>
</blockquote>
</li>
<li><em>자료 구조*</em> 및 <strong>큐(Queue)</strong>의 동작 원리로, 가장 먼저 들어간 데이터가 가장 먼저 나오는 구조를 의미한다. <strong>FIFO</strong>텍스트는 현실에서 흔히 볼 수 있는 예시로, 줄을 서서 기다리는 대기열과 비슷한 개념이다. 줄을 서서 먼저 온 사람이 먼저 서비스를 받는 것처럼, FIFO는 데이터를 저장한 순서대로 처리하는 방식이다.</li>
</ul>
<p><strong>이벤트 루프(Event Loop)</strong>는 JavaScript의 비동기 프로그래밍을 가능하게 하는 메커니즘으로, 싱글 스레드 언어인 JavaScript가 비동기 작업을 효율적으로 처리할 수 있게 도와준다. 이벤트 루프는 <strong>콜 스택(Call Stack)</strong>과 <code>태스크 큐(Task Queue) (또는 마이크로태스크 큐)</code> 간의 상호작용을 통해 작동하며, 동기 및 비동기 코드를 실행하는 순서를 관리한다.</p>
<p>JavaScript는 <strong>단일 스레드로 동작하기 때문에</strong> 한 번에 하나의 작업만 수행할 수 있지만, <strong>이벤트 루프</strong>를 통해 비동기 작업이 완료될 때 적절한 시점에 실행할 수 있게 한다.</p>
<h3 id="이벤트-루프의-작동-방식">이벤트 루프의 작동 방식</h3>
<p><strong>이벤트 루프</strong>는 콜 스택이 비어 있는지 계속 확인한다. 콜 스택이 비어 있다면 <code>태스크 큐나 마이크로태스크 큐</code>에 있는 콜백들을 <strong>콜 스택</strong>으로 옮겨 실행한다.</p>
<p><strong>콜 스택</strong>: <strong>동기 코드와 현재 실행할 작업을 쌓아두는 곳</strong>이다. 함수가 호출되면 콜 스택에 쌓이고, 완료되면 스택에서 제거된다. 동기 코드는 콜 스택에서 바로 실행된다.</p>
<p><strong>태스크 큐(Task Queue)</strong>: <code>비동기 작업(예: setTimeout 콜백 함수, 이벤트 핸들러)</code>이 완료되면 태스크 큐에 해당 콜백이 들어간다. <strong>이벤트 루프는 콜 스택이 비었을 때 태스크 큐에 있는 콜백을 콜 스택으로 옮겨 실행한다.</strong> <strong>Microtask Queue</strong>와 구분하기 위해 태스크 큐를 <strong>Macrotask Queue</strong>라고도 한다.</p>
<p><strong>마이크로태스크 큐(Microtask Queue)</strong>: <code>주로 Promise의 .then() 콜백</code>과 같은 마이크로태스크들이 들어가는 큐로, <strong>태스크 큐보다 높은 우선순위를 가진다.</strong> <strong>이벤트 루프는 매 루프마다 마이크로태스크 큐를 먼저 확인하고, 비어 있지 않으면 모든 마이크로태스크를 실행한 후에 태스크 큐로 넘어간다.</strong></p>
<blockquote>
<p><strong>이벤트 루프</strong>는 다음과 같은 과정으로 작동한다.</p>
</blockquote>
<ol>
<li><strong>콜 스택</strong>이 비어 있는지 확인한다.</li>
<li>비어 있다면, <strong>마이크로태스크 큐</strong>가 비어 있는지 확인하고, 비어 있지 않으면 모든 마이크로태스크를 실행한다.</li>
<li>그 후 <strong>태스크 큐</strong>에서 하나의 작업을 가져와 <strong>콜 스택</strong>에 쌓고 실행한다.</li>
<li>이 과정을 반복한다.</li>
</ol>
<h3 id="싱글-스레드">싱글 스레드</h3>
<p>자바 스크립트는 <strong><code>싱글 스레드</code></strong> 방식을 가지고 있다. 조금 더 구체적으로 표현하자면 <strong><code>싱글 이벤트 루프 스레드</code></strong>라고도 한다. 핵심은, <strong>CallStack이 비어야만 TaskQueue에 있는 작업을 CallStack으로 가져온다는 것</strong>이다. CallStack이 작업 중이라면 <code>setTimeout</code>에 설정한 0초가 이미 지났다고 하더라도 <strong>TaskQueue 안의 작업이 CallStack으로 들어오지 못한다.</strong></p>
<h4 id="프로세스와-스레드의-차이">프로세스와 스레드의 차이</h4>
<p><strong>프로세스(Process)</strong>: <strong>운영체제</strong>에서 실행 중인 하나의 독립적인 프로그램을 의미한다. 프로세스는 <code>CPU, 메모리, 파일</code> 등의 시스템 자원을 소유한다. 각 프로세스는 서로 독립적이며, 일반적으로 다른 프로세스와 직접적인 자원 공유를 하지 않는다.</p>
<p><strong>스레드(Thread)</strong>: 프로세스 내부에서 실제 작업을 수행하는 실행 단위다. <strong>하나의 프로세스는 여러 스레드를 가질 수 있으며, 이 경우 여러 스레드가 프로세스 내 자원을 공유할 수 있다.</strong> 여러 스레드를 가지는 것을 <code>멀티스레딩</code>이라 한다.</p>
<h4 id="싱글스레드와-멀티스레드의-차이">싱글스레드와 멀티스레드의 차이</h4>
<p><strong>싱글스레드(Single Thread)</strong>: <strong>하나의 스레드</strong>로만 작업을 처리하는 방식이다. 모든 작업이 <strong>동기적</strong>으로 실행되며, 한 번에 하나의 작업만 수행할 수 있다.</p>
<p><strong>멀티스레드(Multi-Thread)</strong>: <strong>여러 스레드</strong>가 동시에 작업을 처리할 수 있는 방식입니다. <strong>병렬 처리가 가능하여 여러 작업을 동시에 수행할 수 있다.</strong> 멀티스레드는 복잡한 작업을 더 효율적으로 수행할 수 있지만, 스레드 간 데이터 경쟁이나 동기화 문제가 발생할 수 있다.</p>
<h2 id="task-queue간의-실행-우선순위">task Queue간의 실행 우선순위</h2>
<pre><code class="language-javascript">// 예시 코드

// 동기작업
console.log(&quot;시작!!!&quot;);

// 비동기작업(macrotask Queue -&gt; setTimeout)
setTimeout(() =&gt; {
  // 비동기작업(microtask Queue -&gt; Promise.then())
  new Promise((resolve) =&gt; resolve(&quot;철수&quot;)).then(() =&gt; {
    console.log(&quot;저는 Promise(setTimeout 안에서 실행될 거예요!!!)&quot;);
  });
  console.log(&quot;저는 setTimeout!! macroQueue!! 0초마다 실행될 거예요!!!&quot;);
}, 0);

// 비동기작업(microtask Queue -&gt; Promise.then())
new Promise((resolve) =&gt; resolve(&quot;철수&quot;)).then(() =&gt; {
  console.log(&quot;저는 Promise(1)!! microQueue!!&quot;);
});

// 비동기작업(macrotask Queue -&gt; setInterval)
setInterval(() =&gt; {
  console.log(&quot;저는 setInterval!! macroQueue!! 1초마다 실행될 거예요!!!&quot;);
}, 1000);

// 동기작업
let sum = 0;
for (let i = 0; i &lt;= 100; i++) {
  sum += 1;
}

// 비동기작업(micro Queue -&gt; Promise.then())
new Promise((resolve) =&gt; resolve(&quot;철수&quot;)).then(() =&gt; {
  console.log(&quot;저는 Promise(2)!! microQueue!!&quot;);
});

// 동기작업
console.log(&quot;끝!!!&quot;);</code></pre>
<blockquote>
<p><strong>순서</strong></p>
</blockquote>
<ol>
<li>동기작업
<code>시작</code>이 먼저 실행되고 비동기 작업은 각자의 맞는 <strong>task queue</strong>에 저장된다. 이후 <strong>for 반복문</strong>이 실행되고 반복문이 종료되면 <code>끝!!!</code> 이 실행된다.<blockquote>
</blockquote>
<ol start="2">
<li>비동기 작업 - <strong>microtask queue</strong></li>
</ol>
<strong>event loop</strong>가 <strong>call stack</strong>이 비어있는 지 확인하고 비어 있다면 <strong>microtask queue</strong>을 실행시킨다.
queue의 구조에 따라 <code>FIFO</code>로 첫번째로 들어간 <strong>Promise</strong>를 실행시킨다. 그에 따라 <code>저는 Promise(1)!! microQueue!!</code> 가 실행된다.
다음 <strong>microtask queue</strong>에 존재하는 <strong>Promise</strong>를 실행하고 <code>저는 Promise(2)!! microQueue!!</code> 가 실행된다.<blockquote>
</blockquote>
<ol start="3">
<li>비동기 작업 - <strong>macrotask queue</strong></li>
</ol>
<strong>microtask queue</strong>가 비어있다면 그 다음 <strong>macrotask queue</strong>가 실행된다. 첫 번째 <code>setTimeout</code>이 먼저 실행되고 <code>setTimeout</code>에 있는 <strong>Promise</strong>가 <strong>microtask queue</strong>에 들어가고 이후 <code>저는 setTimeout!! macroQueue!! 0초뒤에 실행될 거예요!!!</code> 가 실행된다. <blockquote>
</blockquote>
<ol start="4">
<li>비동기 작업 - <strong>microtask queue</strong>
<code>setTimeout</code>에 의해 <strong>Promise</strong>가 <strong>microtask queue</strong>에 들어갔으므로, <strong>macrotask queue</strong>에 존재하는 <code>setInterval</code>을 실행하는 게 아닌, <strong>microtask queue</strong>에 존재하는 <strong>promise</strong>를 먼저 실행시킨다. 따라서 <code>저는 Promise(setTimeout 안에서 실행될 거예요!!!)</code>가 실행된다.<blockquote>
</blockquote>
</li>
<li>비동기 작업 - <strong>macrotask queue</strong></li>
</ol>
<strong>microtask queue</strong>가 비었으므로, <strong>event loop</strong>는 <strong>macrotask queue</strong>를 실행시킨다. <code>setInterval</code>를 실행되고 <code>저는 setInterval!! macroQueue!! 1초마다 실행될 거예요!!!</code>라고 1초마다 <code>setInterval</code>를 실행된다.</li>
</ol>
<h4 id="실제-console-창">실제 console 창</h4>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/102c95da-364d-433e-8168-9059a79ce63d/image.png" /></p>
<h2 id="async-await와-microtask-queue의-관계">Async-Await와 Microtask Queue의 관계</h2>
<pre><code class="language-javascript">console.log(&quot;=======시작!!!!=======&quot;);

  function aaa() {
    console.log(&quot;aaa-시작&quot;);
    bbb();
    console.log(&quot;aaa-끝&quot;);
  }

  async function bbb() {
    console.log(&quot;bbb-시작&quot;);
    await ccc();
    console.log(&quot;bbb-끝&quot;);
  }

  async function ccc() {
    console.log(&quot;ccc-시작&quot;);
    const friend = await fetch('https://api.example.com/data'); // 철수
    console.log(friend);
  }

aaa();

console.log(&quot;=======끝!!!!=======&quot;);</code></pre>
<blockquote>
<p><strong>실행 순서</strong></p>
</blockquote>
<ol>
<li><code>시작!!!</code></li>
<li>aaa() 실행</li>
<li><code>aaa-시작</code></li>
<li>bbb() 실행</li>
<li><code>bbb-시작</code></li>
<li>await ccc() 실행</li>
<li><code>ccc-시작</code></li>
<li>await fetch('<a href="https://api.example.com/data'">https://api.example.com/data'</a>) 실행 </li>
<li>microqueue task에 ccc를 저장</li>
<li>ccc가 끝나지 않았으므로 microqueue task에 bbb를 저장</li>
<li><code>aaa-끝</code></li>
<li><code>끝!!!</code></li>
<li>microqueue task에 있는 ccc를 실행</li>
<li>await 철수가 끝나면 <code>철수</code></li>
<li>microqueue task에 있는 bbb를 실행</li>
<li><code>bbb-끝</code></li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/0b6feb4e-faa3-483e-a235-691629c371e5/image.png" /></p>
<p><code>Async-Await</code>는 내부적으로 <strong>Promise</strong>를 사용하기 때문에, await 키워드는 마이크로태스크 큐와 긴밀히 연관된다. <code>await</code>는 비동기 작업을 수행하고 나서, 완료된 후속 작업을 <code>Microtask Queue</code>에 등록한다. <code>await가 있는 async</code> 함수가 중단되면, <code>await 이후의 코드</code>는 <strong>마이크로태스크 큐에 등록되어 이벤트 루프가 다음 순서에서 처리하도록 한다.</strong> 콜 스택이 비어 있으면 이벤트 루프는 Microtask Queue의 작업을 우선 처리한다.</p>
<h3 id="await와-마이크로큐에-기반한-trycatch-동작-원리">await와 마이크로큐에 기반한 try~catch 동작 원리</h3>
<p><code>try-catch</code>는 <strong>동기 코드</strong>에서 오류를 잡기 위한 블록이다. 하지만 비동기 함수(<code>fetch</code> 등)의 오류 처리는 <strong>마이크로태스크 큐</strong>의 개념이 적용되기 때문에 동기 코드와는 다르게 작동한다.</p>
<ol>
<li><strong>비동기 작업을 기다릴 때 <code>await</code></strong>는 <strong>마이크로큐에 대기</strong>한다. 이 대기 중인 작업이 실패하거나 성공하면, 그 결과는 <strong>콜스택으로 돌아온다</strong>.</li>
<li>비동기 작업이 완료된 후에 <code>try-catch</code> 블록이 실행된다. 따라서 <strong><code>await</code>로 비동기 작업의 결과를 받아올 때</strong>, 작업이 실패하면 <code>catch</code> 블록이 그 에러를 잡을 수 있게 된다.</li>
<li>만약 <code>await</code> 없이 <code>fetch</code>를 사용하고 동기적으로 <code>try-catch</code>를 사용하면, <strong>비동기 작업이 실패하더라도 동기 블록은 이미 종료</strong>된 상태라서 오류를 잡지 못한다.</li>
</ol>
<pre><code class="language-javascript">const bbb = async () =&gt; {
  try {
    await fetch(&quot;https://qqqqq.com&quot;);
    console.log(&quot;철수&quot;);
  } catch (error) {
    console.log(&quot;에러가 발생하였습니다!&quot;); // 여기에서 비동기 작업이 실패했을 때 에러를 잡음
  }
};</code></pre>
<h3 id="await와-마이크로큐로-이해하는-promise의-동작-원리"><strong><code>await</code>와 마이크로큐로 이해하는 <code>Promise</code>의 동작 원리</strong></h3>
<p><code>Promise</code>는 자바스크립트의 <strong>비동기 작업</strong>을 처리하는 방식이다. <code>await</code>는 <code>Promise</code>를 사용하여 <strong>비동기 작업이 완료될 때까지 기다리는 역할</strong>을 한다.</p>
<ol>
<li><code>Promise</code>는 <strong>비동기 작업이 완료된 후</strong> <code>resolve</code> 또는 <code>reject</code>로 결과를 반환한다. 이때 <strong><code>await</code>는 <code>Promise</code>가 반환될 때까지 실행을 잠시 멈추고</strong>, <strong>다른 작업들이 실행될 수 있도록 마이크로큐에 들어간다</strong>.</li>
<li>마이크로큐에 등록된 작업이 완료되면 그 결과가 <strong>콜스택</strong>으로 돌아오고, 해당 비동기 작업이 다시 실행된다.</li>
<li>만약 <code>Promise</code>가 성공하면 <code>resolve</code> 값이, 실패하면 <code>reject</code> 값이 반환된다. 비동기 작업에서 오류가 발생하면 <code>catch</code> 블록이 이를 처리하게 된다.</li>
</ol>
<ul>
<li><p><strong>Promise의 동작 순서 예시</strong></p>
<pre><code class="language-jsx">  const fetchData = async () =&gt; {
    try {
      const response = await fetch(&quot;https://api.example.com&quot;);
      console.log(&quot;Data fetched:&quot;, response);
    } catch (error) {
      console.log(&quot;Error occurred:&quot;, error);
    }
  };</code></pre>
<ul>
<li><code>fetch</code>는 기본적으로 <strong>Promise</strong>를 반환한다.</li>
<li><code>await</code>는 이 Promise가 완료될 때까지 기다린 후, 성공 여부에 따라 이후의 로직을 실행하거나 에러를 처리한다.</li>
<li>이 예제에서, <code>fetch</code>는 <strong>비동기 작업(Promise)</strong>을 반환하고, <code>await</code>가 이를 대기한다. 만약 API 호출이 성공하면 <strong>콜스택</strong>에서 그 결과가 처리되고, 실패하면 <code>catch</code> 블록에서 에러를 잡는다.</li>
</ul>
</li>
</ul>