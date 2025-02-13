<h2 id="real-time-communication이란">Real-Time Communication이란?</h2>
<p><strong>Real-Time Communication(실시간 통신)</strong>은 클라이언트와 서버, 또는 클라이언트 간 데이터를 지연 없이 실시간으로 주고받는 기술과 프로토콜의 총칭이다. <strong>RTC</strong>는 웹 애플리케이션, 모바일 애플리케이션, IoT 등에서 실시간 상호작용이 필요한 다양한 분야에 사용된다.</p>
<h2 id="polling">Polling</h2>
<p><strong>Socket</strong>이 등장하기 전, 웹 브라우저는 주로 <code>HTTP 프로토콜</code>을 이용해 데이터를 송수신했다. <strong>HTTP는 요청-응답 모델</strong>을 기반으로 동작하며, <strong>Client</strong>가 서버에 요청을 보내면 서버는 요청에 대한 응답을 반환한다. 이 방식은 간단하지만, 실시간 데이터 전송에는 한계가 있었다. 기본적으로 <strong>HTTP</strong> 프로토콜은 <strong>Stateless</strong> 상태를 유지하기 때문이다. </p>
<p><strong>Stateless(무상태성)</strong>은 서버가 클라이언트의 이전 요청 상태를 유지하지 않는 것을 의미한다. 즉, 서버와 클라이언트 간의 연결은 각 요청마다 새롭게 시작된다는 얘기다. 이러한 특성으로 인해, 서버는 클라이언트의 상태를 기억할 수 없으므로, <strong>지속적인 상태 업데이트를 위한 실시간 데이터 교환이 어렵다.</strong></p>
<p>결과적으로 <strong>Client</strong>는 서버의 최신 데이터를 확인하기 위해 <strong>Polling(주기적인 요청) 방식</strong>을 사용할 수밖에 없었습니다. <strong>Polling은 클라이언트가 정해진 간격으로 서버에 데이터를 요청하는 방식</strong>으로, 간단하게 실시간성을 흉내낼 수 있었다. </p>
<blockquote>
<p><strong>Client Polling</strong></p>
</blockquote>
<pre><code class="language-javascript">// 클라이언트에서 일정 간격으로 서버 데이터를 요청하는 Polling 예제
const POLLING_INTERVAL = 5000; // 5초 간격
&gt;
const fetchData = async () =&gt; {
  try {
    const response = await fetch('/api/data'); // 서버에 요청
    const data = await response.json(); // 응답 데이터를 JSON으로 변환
    console.log('Server Response:', data); // 데이터 출력
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};
&gt;
// 일정 간격으로 fetchData 호출
setInterval(fetchData, POLLING_INTERVAL);</code></pre>
<blockquote>
</blockquote>
<p><strong>Server Polling</strong></p>
<pre><code class="language-javascript">const express = require('express');
const app = express();
&gt;
let count = 0;
&gt;
// 간단한 API 엔드포인트
app.get('/api/data', (req, res) =&gt; {
  count++;
  res.json({ message: `Polling response ${count}`, timestamp: new Date().toISOString() });
});
&gt;
app.listen(3000, () =&gt; console.log('Server running on http://localhost:3000'));</code></pre>
<p><strong>Polling</strong>의 장점으로 구현이 간단하고 <strong>Javascript setInterval 을 통해서 Fetching이 일어나기 때문에</strong> 모든 브라우저에서 동작하며, 서버 변경 없이 <strong>기존 HTTP</strong> 구조에서 사용이 가능하다. 하지만 데이터가 변경되지 않아도 주기적으로 요청이 발생해 <strong>네트워크 낭비</strong>가 발생하고 요청 간격에 따라 실시간성이 제한될 수 있다. </p>
<h2 id="long-polling">Long Polling</h2>
<p><strong>Polling</strong>의 문제점으로 <strong>네트워크 낭비와 실시간성 부족</strong>이다. 이를 해결하기 위해 <strong>Long Polling</strong>이라는 기법이 도입되었다. <strong>HTTP 프로토콜</strong>을 기반으로 하며, 클라이언트가 요청을 보내고 서버가 데이터가 준비될 때까지 요청을 유지(대기) 하는 방식으로 동작한다.</p>
<blockquote>
<p>** Long Polling ** 작동 원리</p>
</blockquote>
<ol>
<li>client가 서버에 <strong>HTTP Request</strong>를 보낸다.</li>
<li>server는 데이터가 들어올 때까지 연결을 유지한다.</li>
<li>데이터가 준비되면 응답을 보내고 연결을 종료한다.</li>
<li>client 다시 요청을 보낸다. </li>
</ol>
<blockquote>
<p><strong>Client Long Polling</strong></p>
</blockquote>
<pre><code class="language-javascript">function startLongPolling() {
  fetch('/poll')
    .then((response) =&gt; response.json())
    .then((data) =&gt; {
      console.log('Received data:', data);
      // 새로운 요청 시작
      startLongPolling();
    })
    .catch((error) =&gt; {
      console.error('Polling error:', error);
      // 재연결 시도
      setTimeout(startLongPolling, 1000);
    });
}
&gt;
// Long Polling 시작
startLongPolling();</code></pre>
<blockquote>
</blockquote>
<p><strong>Server Long Polling</strong></p>
<pre><code class="language-javascript">const express = require('express');
const app = express();
&gt;
app.get('/poll', (req, res) =&gt; {
  // 5초 후 응답
  setTimeout(() =&gt; {
    res.json({ message: &quot;New data from server&quot; });
  }, 5000);
});
&gt;
app.listen(3000, () =&gt; {
  console.log('Server is running on port 3000');
});</code></pre>
<p><strong>Long Polling</strong>은 <strong>Polling</strong>에 비해 실시간 반응성이나, 트래픽 측면에서 더 좋은 효율을 보이지만, 데이터가 들어올 때까지 서버에서 계속 대기를 해야하기 때문에 서버 내의 리소스가 증가해 <strong>서버 부하</strong>를 일으키며, 연결이 끊이질 경우 클라이언트가 새로운 요청을 보내야한다. </p>
<h2 id="streaming">Streaming</h2>
<p><strong>Streaming(스트리밍)</strong>은 클라이언트와 서버 간의 지속적인 연결을 통해 데이터가 준비되는 즉시 클라이언트로 연속적으로 전송하는 방식이다. 일반적으로 <strong>HTTP 스트리밍</strong>으로 구현되며, 실시간 데이터 전송을 위한 더 효율적이고 발전된 방법으로 사용된다.</p>
<blockquote>
<p><strong>Streaming의 동작 방식</strong></p>
</blockquote>
<ol>
<li><strong>클라이언트가 서버에 연결 요청</strong> : 클라이언트는 서버에 지속적인 연결을 요청하며, 이는 일반적으로 <code>HTTP/1.1 또는 HTTP/2</code> 기반으로 이루어진다.<blockquote>
</blockquote>
</li>
<li><strong>서버에서 지속적으로 데이터 전송</strong> : 서버는 데이터가 준비될 때마다 끊기지 않는 연결을 통해 데이터를 스트림 형태로 전송.<blockquote>
</blockquote>
</li>
<li><strong>클라이언트가 데이터 수신 및 처리</strong> : 클라이언트는 서버에서 전송받은 데이터를 실시간으로 처리하며, 새 데이터가 들어올 때까지 연결은 계속 유지된다.<blockquote>
</blockquote>
</li>
<li><strong>연결 종료</strong> : 클라이언트 또는 서버에서 필요에 따라 연결을 종료할 수 있다.</li>
</ol>
<p><strong>Streaming</strong>은 데이터가 생성되는 즉시 클라이언트로 전달하며, 비디오, 오디오, 텍스트 등 모든 데이터 형태를 실시간으로 처리 가능하다. 또한 <strong>Polling이나 Long Polling</strong>과 달리 <code>요청-응답</code> 반복 없이 지속적인 연결을 유지한다. 또한 연결이 유지되므로 반복적인 요청/응답 오버헤드가 제거되며, <strong>기본적으로 서버 → 클라이언트 방향의 단방향 데이터 전송</strong>으로 통신한다.</p>
<blockquote>
<p><strong>Client Streaming</strong></p>
</blockquote>
<pre><code class="language-javascript">fetch('http://localhost:3000')
    .then((response) =&gt; response.body.getReader())
    .then((reader) =&gt; {
        function read() {
            reader.read().then(({ done, value }) =&gt; {
                if (done) {
                    console.log('Stream complete');
                    reader.releaseLock(); // 스트림 잠금 해제
                    return;
                }
                console.log(new TextDecoder().decode(value));
                read();
            });
        }
        read();
    });</code></pre>
<blockquote>
</blockquote>
<p><strong>Server Streaming</strong></p>
<pre><code class="language-javascript">const http = require('http');
&gt;
http.createServer((req, res) =&gt; {
    res.writeHead(200, {
        'Content-Type': 'text/plain',
        'Transfer-Encoding': 'chunked',
    });
&gt;
    let count = 0;
    const interval = setInterval(() =&gt; {
        count++;
        res.write(`Chunk ${count}\n`);
        if (count === 5) {
            clearInterval(interval);
            res.end('End of stream');
        }
    }, 1000);
}).listen(3000, () =&gt; {
    console.log('Server is running on port 3000');
});</code></pre>
<h3 id="client-streaming-코드-분석">Client Streaming 코드 분석</h3>
<ol>
<li><strong>fetch 요청</strong>
client는 서버에 데이터 요청을 하는데, response가 <strong>Streaming</strong> 형식이므로 <strong>response.body.getReader()</strong>를 통해 데이터를 <strong>chuck</strong> 단위로 읽어올 준비를 한다. Streaming은 주로 반환값으로 <strong>ReadableStream</strong> 객체이다. </li>
</ol>
<h4 id="readablestream">ReadableStream</h4>
<p><strong>ReadableStream</strong>은 데이터가 서버에서 클라이언트로 스트리밍되는 동안 <strong>데이터를 읽을 수 있도록 제공하는 인터페이스다.</strong> 이는 <code>네트워크 요청, 파일 읽기, 또는 다른 데이터소스로부터 데이터</code>를 점진적으로 읽는 데 사용된다. </p>
<blockquote>
</blockquote>
<ol>
<li><strong>스트림 기반 데이터 처리</strong></li>
</ol>
<ul>
<li>데이터를 한꺼번에 모두 읽지 않고 <strong>Chuck</strong> 단위로 읽어서 처리한다.</li>
<li>데이터를 처리하면서 새로운 데이터가 추가적으로 들어오는 상황에 적합하다.</li>
</ul>
<ol start="2">
<li><strong>유연한 데이터 소비</strong></li>
</ol>
<ul>
<li>데이터를 점진적으로 읽어 메모리 사용을 줄일 수 있다. </li>
<li>대규모 데이터를 처리하거나 실시간 데이터를 다룰 때 유용하다. </li>
</ul>
<ol start="3">
<li><strong>비동기적 데이터 처리</strong></li>
</ol>
<ul>
<li>스트림은 데이터가 도착할 때 비동기적을 처리되므로 빠르고 효율적이다. </li>
</ul>
<p><strong>ReadableStream</strong> 구조
ReadableStream은 <strong>Producer(데이터 생성)</strong>와 <strong>Consumer(데이터 소비)</strong>의 관계를 기반으로 동작한다. <strong>Producer</strong>는 데이터를 생성하고 Stream에 push한다. <code>서버 / 파일 시스템 / 네트워크 요청</code> 등이 생산자 역할을 한다. <strong>Consumer</strong>는 <strong>Stream</strong>에서 데이터를 읽고 처리하는 역할을 한다. 주로 <code>클라이언트, 브라우저, 애플리케이션</code>이 소비자 역할을 한다. </p>
<pre><code class="language-javascript">const stream = new ReadableStream({
  start(controller) {
    // 스트림 시작 시 호출
    controller.enqueue('Hello, World!'); // 데이터를 스트림에 추가
    controller.close(); // 스트림 종료
  },
  pull(controller) {
    // 소비자가 데이터를 요청할 때 호출
    console.log('Consumer is requesting data');
  },
  cancel(reason) {
    // 스트림이 취소될 때 호출
    console.log(`Stream cancelled: ${reason}`);
  },
});</code></pre>
<blockquote>
<p><strong>주요 메서드</strong>
<strong>start(controller)</strong>: 스트림이 생성될 때 초기화 코드를 실행한다.</p>
</blockquote>
<ul>
<li>controller.enqueue(): 데이터를 스트림에 추가.</li>
<li>controller.close(): 스트림 종료.<blockquote>
</blockquote>
</li>
<li><em>pull(controller)*</em>: 소비자가 데이터를 요청할 때 실행된다. 추가 데이터를 스트림에 푸시하거나 특별한 처리를 수행.<blockquote>
</blockquote>
</li>
<li><em>cancel(reason)*</em> : 스트림이 취소될 때 호출된다.</li>
</ul>
<p><strong>ReadableStream</strong> 같은 경우 직접 데이터를 노출하지 않는다. <strong>Reader</strong>를 통해 데이터를 받아오는데 <strong>ReadableStream</strong> 객체 같은 경우 <strong>실시간</strong>으로 <strong>chuck</strong> 단위로 데이터가 끊임없이 들어온다. 데이터 순서와 스트림 종료를 관리를 해야하는데 <strong>Reader</strong>가 <strong>chuck</strong>를 관리한다고 생각하면 된다. 심지어 <strong>Stream</strong> 데이터는 순차적으로 도착하므로 데이터를 받을 때 마다 처리하기 위한 비동기 작업이 필요하다. 이를 <strong>Reader</strong>가 관리한다.</p>
<ol start="2">
<li><strong>.then((response) =&gt; response.body.getReader())</strong>
데이터를 받아오면 <strong>ReadableStream</strong> 객체로 넘어오는데 현재 이상태에서는 데이터를 읽지 못한다. <strong>Reader</strong> 객체를 통해 데이터를 받아와야하는데 이는 <strong>getReader</strong>를 통해 <strong>ReadeableStreamDefaultReader</strong> 객체로 변환하여 reader를 받아와야한다. <h4 id="readablestream-vs-readablestreamdefaultreader"><strong>ReadableStream vs ReadableStreamDefaultReader</strong></h4>
</li>
</ol>
<p><strong>ReadableStream</strong> - 데이터를 <strong>Stream</strong> 형태로 제공하는 객체로 스트림 데이터에 바로 접근할 수 없으며, <strong>getReader()</strong>를 통해 데이터를 소비하고 읽을 수 있다. <strong>Reader</strong>이 Stream과 연결되어 있으면 잠긴상태가 된다.</p>
<p><strong>ReadableStreamDefaultReader</strong> - <strong>Stream</strong> 데이터를 읽고 소비하는 역할을 한다. <strong>read()</strong> 를 통해 <strong>chuck</strong> 데이터를 읽을 수 있다. <strong>releaseLock</strong>을 통해 <strong>Reader와 Stream</strong> 연결을 해제한다.</p>
<pre><code class="language-scss">ReadableStream
   └── getReader()
        └── ReadableStreamDefaultReader
               └── read()</code></pre>
<ol start="3">
<li><strong>reader.read()</strong></li>
</ol>
<p><strong>reader.read() 메서드</strong>는 스트림 데이터를 읽는 데 사용된다. 이 메서드는 <strong>Promise</strong>를 반환하며, <code>결과는 { done, value }</code> 객체 형태
<strong>done: 스트림이 종료되었는지 여부를 나타낸다. / true → 스트림 종료. false → 아직 데이터가 남아 있음.</strong>
<strong>value</strong>: 현재 읽은 데이터의 청크<strong>(Uint8Array 형식)</strong></p>
<ol start="4">
<li><strong>reader.releaseLock();</strong></li>
</ol>
<p><strong>done</strong>이 true면 stream이 종료되었다는 얘기이므로, <strong>relaseLock</strong>을 통해서 <strong>Reader</strong>와 <strong>Stream</strong>을 해제하여 스트림을 잠금 해제해준다.</p>
<ol start="5">
<li><strong>new TextDecoder().decode(value)</strong>
서버가 반환한 데이터는 <strong>바이너리 데이터(Uint8Array)</strong>로 제공된다. <strong>TextDecoder</strong>를 사용하여 이를 사람이 읽을 수 있는 문자열로 변환한다. <strong>decode(value) : Uint8Array</strong>를 UTF-8 문자열로 변환.</li>
</ol>
<h3 id="server-stream-코드-분석">Server Stream 코드 분석</h3>
<pre><code class="language-javascript">res.writeHead(200, {
    'Content-Type': 'text/plain',
    'Transfer-Encoding': 'chunked',
});</code></pre>
<p>일반적으로 우리는 <strong>Content-Type : &quot;application/Json&quot;</strong> 형식으로 사용했을 것이다. 우리는 <strong>HTTP</strong> 통신을 통해 데이터를 송수신하고 있고, <strong>HTTP</strong>는 <strong>JSON</strong> 객체를 이용하여 데이터를 전달받기 때문이다. 하지만 우리는 json 객체가 아닌 text를 전달하기 때문에 <strong>'Content-Type': 'text/plain'</strong>이다. 또한  <strong>Chunk</strong> 단위로 데이터를 전달할 예정이기 때문에 <strong>'Transfer-Encoding': 'chunked'</strong> header에 명시해줘야 한다. </p>
<h3 id="streaming-장단점">Streaming 장단점</h3>
<p><strong>Streaming 기법</strong>은 데이터를 한꺼번에 전달하지 않고 <strong>chuck</strong> 단위로 전달하기 때문에 메모리를 효율적으로 관리할 수 있고, 실시간 데이터를 즉각적으로 처리할 수 있다. 하지만 <strong>Stream</strong>은 서버 -&gt; 클라이언트 단방향 데이터 전송에 특화되어 있어 클라이언트가 서버로 데이터를 보내거나 양방향 실시간 통신이 필요한 경우에 적합하지 않는다. 또한 <strong>Network</strong> 의존도가 높아 <strong>Network</strong> 상태가 좋지 않으면 UX적으로 좋지 않다. </p>
<h2 id="websocket">WebSocket</h2>
<p><strong>WebSocket</strong>은 <strong>HTML5의 등장(2011년)</strong>과 함께 웹 애플리케이션의 실시간성을 높이기 위해 표준화된 <strong>Full-Duplex 양방향 통신 프로토콜</strong>이다. 이전에는 <strong>Polling이나 Long Polling</strong> 같은 기술로 실시간 통신을 구현했지만, 이 방식에는 다음과 같은 문제가 있었다</p>
<blockquote>
</blockquote>
<p><strong>Polling</strong>: 클라이언트가 주기적으로 서버에 요청을 보내 데이터를 확인 → 네트워크 및 서버 리소스 낭비.
<strong>Long Polling</strong>: 클라이언트가 요청을 보내고, 서버가 새로운 데이터가 준비될 때까지 연결을 유지 → 지연 발생 및 비효율적.</p>
<p><strong>WebSocket</strong>은 이러한 문제를 해결하며 HTTP 기반의 비효율성을 극복하기 위해 탄생했다.</p>
<blockquote>
<p><strong>WebSocket 동작 방식</strong>
<strong>1.초기 Handshake</strong> : <strong>WebSocket 연결</strong>은 HTTP 프로토콜을 사용하여 초기 Handshake를 수행한 후 WebSocket 프로토콜로 업그레이드한다. 클라이언트가 <strong>Upgrade 요청</strong>을 보내고, 서버가 이를 승인하면 연결이 설정된다.</p>
</blockquote>
<p><strong>요청 헤더</strong></p>
<pre><code class="language-http">GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13</code></pre>
<blockquote>
</blockquote>
<p><strong>응답 헤더</strong></p>
<pre><code class="language-http">HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=</code></pre>
<p><strong>2. 지속적 연결 유지</strong> : Handshake 이후, 클라이언트와 서버는 WebSocket 프로토콜로 전환되며 지속적인 연결 상태를 유지한다. 연결이 설정되면 양쪽 모두 데이터를 전송할 수 있는 <strong>Full-Duplex 양방향 통신</strong>이 가능하다.</p>
<blockquote>
</blockquote>
<p><strong>3. 데이터 프레임</strong> : WebSocket은 데이터를 전송할 때 프레임 단위로 처리합니다:</p>
<blockquote>
</blockquote>
<ul>
<li><strong>텍스트 프레임</strong>: UTF-8 인코딩된 텍스트 메시지.</li>
<li><strong>바이너리 프레임</strong>: 바이너리 데이터(예: 이미지, 파일).</li>
<li><strong>제어 프레임</strong> : 연결 관리(예: Ping/Pong, Close).</li>
</ul>
<blockquote>
<p><strong>Client Websocket</strong></p>
</blockquote>
<pre><code class="language-javascript">const socket = new WebSocket('ws://localhost:8080');
&gt;
// 서버와의 연결 설정
socket.onopen = () =&gt; {
  console.log('Connected to the server');
  socket.send('Hello Server!'); // 서버로 메시지 전송
};
&gt;
// 서버에서 받은 메시지 처리
socket.onmessage = (event) =&gt; {
  console.log('Received from server:', event.data);
};
&gt;
// 연결 종료 처리
socket.onclose = () =&gt; {
  console.log('Disconnected from the server');
};
&gt;
// 에러 처리
socket.onerror = (error) =&gt; {
  console.error('WebSocket error:', error);
};</code></pre>
<blockquote>
<p><strong>Server WebSocket</strong></p>
</blockquote>
<pre><code class="language-javascript">const WebSocket = require('ws');
&gt;
// WebSocket 서버 생성
const wss = new WebSocket.Server({ port: 8080 });
&gt;
wss.on('connection', (ws) =&gt; {
  console.log('Client connected');
&gt;
  // 클라이언트로 메시지 전송
  ws.send('Welcome to the WebSocket server!');
&gt;
  // 클라이언트 메시지 처리
  ws.on('message', (message) =&gt; {
    console.log(`Received: ${message}`);
    // Echo back the message
    ws.send(`Server: ${message}`);
  });
&gt;
  // 연결 종료 처리
  ws.on('close', () =&gt; {
    console.log('Client disconnected');
  });
});</code></pre>
<h3 id="websocket-method">WebSocket Method</h3>
<h4 id="server"><strong>Server</strong></h4>
<p><strong>WebSocket.Server</strong>: WebSocket 서버를 생성.</p>
<p>이벤트 핸들러:
<strong>connection</strong>: 클라이언트가 연결될 때 실행.
<strong>message</strong>: 클라이언트로부터 메시지를 받을 때 실행.
<strong>close</strong>: 클라이언트 연결이 종료될 때 실행.
<strong>error</strong>: WebSocket 에러가 발생할 때 실행.
<strong>ws.send</strong>(): 서버에서 클라이언트로 메시지를 보냄.</p>
<h4 id="client">Client</h4>
<p><strong>new WebSocket(url)</strong>: WebSocket 서버와 연결.</p>
<p>이벤트 핸들러:
<strong>open</strong>: 서버와 연결이 완료되었을 때 실행.
<strong>message</strong>: 서버로부터 메시지를 받을 때 실행.
<strong>close</strong>: 서버 연결이 종료될 때 실행.
<strong>error</strong>: WebSocket 에러가 발생할 때 실행.
<strong>socket.send()</strong>: 클라이언트에서 서버로 메시지를 보냄.</p>
<h3 id="websocket-장단점">WebSocket 장단점</h3>
<p><strong>WebSocket</strong>의 가장 큰 장점은 <strong>양방향 통신(Full-Duplex)</strong>인 점이다. 기존의 <strong>Polling과 Long Polling</strong>의 문제점을 한꺼번에 해결해 <code>실시간 채팅, 알림, 주식 거래 시스템</code> 에 적합하게 사용할 수 있다. 또한 초기 <strong>handshake</strong>를 제외 나머지 통신은 연결없이 지속적으로 데이터를 주고 받으므로 지연시간이 적고 오버헤드가 적다. 하지만, 초기 <strong>handshake</strong>의 셋팅이 복잡하고 방화벽이나 프록시 서버의 호환성 문제를 겪을 수 있으며 연결이 끊겼을 시 에러 처리(재연결)하기가 복잡하다.</p>
<h2 id="webrtc">WebRTC</h2>
<p><strong>WebRTC(Web Real-Time Communication)</strong>는 Google이 주도하여 2011년에 발표한 기술로, 브라우저 간 <strong>P2P(peer to peer)</strong> 실시간 통신을 가능하게 한다. 이를 통해 <code>오디오, 비디오 스트림, 데이터 파일</code>을 서버를 거치지 않고 클라이언트 간 직접 교환할 수 있다.</p>
<h2 id="sseserver-sent-events">SSE(Server-Sent-Events)</h2>
<p><strong>SSE(Server-Sent Events)</strong>는 서버에서 클라이언트로 데이터를 지속적으로 스트리밍하기 위한 <strong>HTTP 기반 기술</strong>이다. <strong>WebSocket처럼</strong> 실시간 통신을 지원하지만, 단방향 통신만 필요할 때 더 간단하고 효율적인 대안이 된다. <strong>SSE</strong>는 HTML5의 표준으로 정의되어 있으며, 주로 실시간 업데이트<code>(알림, 주식 데이터, 로그 스트리밍 등)</code>에 사용된다.</p>
<p><strong>WebSocket</strong>은 양방향 통신을 지원하지만, 단방향 데이터 전송만 필요한 경우 <strong>WebSocket은 불필요하게 복잡하고 리소스를 더 많이 소모한다.</strong> 또한, <strong>Polling/Long Polling</strong>은 실시간성을 제공하지만, 네트워크 효율성과 성능이 낮다. 근데 <strong>SSE</strong>는 HTTP 기반의 단방향 데이터 스트리밍을 제공하여, 이러한 문제를 해결하기 위해 도입되었다. </p>
<blockquote>
<p><strong>SSE 작동 원리</strong>
<strong>1. 클라이언트가 연결 요청</strong> : 클라이언트는 SSE를 사용하기 위해 <strong>EventSource API</strong>를 통해 서버에 연결을 요청한다. 이 요청은 <strong>일반 HTTP 요청</strong>이며, SSE는 HTTP/1.1에서 동작한다.</p>
</blockquote>
<p><strong>2. 서버가 데이터 스트리밍</strong> : 서버는 요청을 수신한 후 <strong>text/event-stream MIME 타입</strong>으로 데이터를 스트리밍한다. 스트림은 HTTP 연결을 유지하며, 클라이언트는 스트림 데이터를 실시간으로 처리한다.</p>
<blockquote>
</blockquote>
<p><strong>3. 자동 재연결</strong> : 클라이언트와 서버 간의 연결이 끊어지면 <strong>EventSource</strong>는 자동으로 재연결을 시도한다. 이 기능은 별도의 추가 구현 없이 기본적으로 제공된다.</p>
<p><strong>SSE</strong>는 서버 → 클라이언트 데이터 전송만 지원하기때문에 클라이언트가 데이터를 서버로 전송하려면 별도의 HTTP 요청을 사용해야 한다. 또한 연결이 끊어지더라도 클라이언트가 자동으로 재연결을 시도하여 연결 복구한다. <strong>WebSocket</strong>과 달리 재연결 로직을 직접 구현할 필요가 없다. 기존 <strong>HTTP/1.1 프로토콜</strong> 위에서 작동하므로, 네트워크 환경이나 방화벽, 프록시와 호환성이 높다. 브라우저의 기본 기능으로 작동하기 때문에 추가적인 라이브러리나 설정이 필요하지 않는다. </p>
<blockquote>
</blockquote>
<p><strong>Client SSE</strong></p>
<pre><code class="language-javascript">// EventSource 객체 생성
const eventSource = new EventSource('http://localhost:3000/events');
&gt;
// 메시지 수신 처리
eventSource.onmessage = (event) =&gt; {
  console.log('Message from server:', event.data);
};
&gt;
// 연결 에러 처리
eventSource.onerror = () =&gt; {
  console.log('Connection lost. Reconnecting...');
};</code></pre>
<blockquote>
</blockquote>
<p><strong>Server SSE</strong></p>
<p>```javascript
const http = require('http');</p>
<blockquote>
</blockquote>
<p>// SSE 서버 생성
const server = http.createServer((req, res) =&gt; {
  if (req.url === '/events') {
    // SSE 응답 헤더 설정
    res.writeHead(200, {
      'Content-Type': 'text/event-stream',
      'Cache-Control': 'no-cache',
      'Connection': 'keep-alive',
    });</p>
<blockquote>
</blockquote>
<pre><code>// 주기적으로 클라이언트에 데이터 전송
let count = 0;
const interval = setInterval(() =&gt; {
  count++;
  res.write(`data: Event ${count}\n\n`); // SSE 데이터 포맷
}, 1000);</code></pre><blockquote>
</blockquote>
<pre><code>// 연결 종료 처리
req.on('close', () =&gt; {
  clearInterval(interval);
});</code></pre><p>  } else {
    res.writeHead(404);
    res.end('Not Found');
  }
});</p>
<blockquote>
</blockquote>
<p>server.listen(3000, () =&gt; {
  console.log('SSE server running on port 3000');
});</p>
<p>```rer</p>
<h3 id="sseserver-sent-event과-streaming-차이">SSE(Server Sent Event)과 Streaming 차이</h3>
<table>
<thead>
<tr>
<th><strong>특징</strong></th>
<th><strong>SSE(Server-Sent Events)</strong></th>
<th><strong>Streaming</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>통신 방향</strong></td>
<td>단방향 (서버 → 클라이언트)</td>
<td>단방향 (서버 → 클라이언트)</td>
</tr>
<tr>
<td><strong>프로토콜</strong></td>
<td>HTTP/1.1 기반, <code>Content-Type: text/event-stream</code>.</td>
<td>HTTP/1.1 (<code>Transfer-Encoding: chunked</code>) 또는 HTTP/2</td>
</tr>
<tr>
<td><strong>데이터 형식</strong></td>
<td>텍스트 데이터(JSON, 문자열)만 지원</td>
<td>텍스트 및 바이너리 데이터 모두 지원</td>
</tr>
<tr>
<td><strong>재연결 기능</strong></td>
<td>클라이언트가 자동 재연결 지원(EventSource 내장).</td>
<td>별도의 재연결 로직 필요</td>
</tr>
<tr>
<td><strong>사용 사례</strong></td>
<td>실시간 알림, 주식 데이터, 서버 이벤트 푸시</td>
<td>동영상 스트리밍, 오디오 스트리밍, 로그 스트리밍</td>
</tr>
<tr>
<td><strong>브라우저 지원</strong></td>
<td>최신 브라우저에서 기본 지원(EventSource API).</td>
<td>HTTP 요청을 지원하는 모든 클라이언트</td>
</tr>
<tr>
<td><strong>설정 복잡성</strong></td>
<td>간단 (기본 HTTP로 구현 가능)</td>
<td>상대적으로 더 복잡(동영상/오디오 인코딩 필요)</td>
</tr>
</tbody></table>
<h2 id="최종-real-time-communication">최종 Real Time Communication</h2>
<table>
<thead>
<tr>
<th><strong>특징</strong></th>
<th><strong>Polling</strong></th>
<th><strong>Long Polling</strong></th>
<th><strong>Streaming</strong></th>
<th><strong>WebSocket</strong></th>
<th><strong>WebRTC</strong></th>
<th><strong>SSE</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>통신 방향</strong></td>
<td>단방향 (클라이언트 → 서버)</td>
<td>단방향 (서버 → 클라이언트)</td>
<td>단방향 (서버 → 클라이언트)</td>
<td>양방향</td>
<td>양방향</td>
<td>단방향 (서버 → 클라이언트)</td>
</tr>
<tr>
<td><strong>프로토콜</strong></td>
<td>HTTP</td>
<td>HTTP</td>
<td>HTTP/1.1, HTTP/2</td>
<td>WebSocket</td>
<td>ICE, STUN, TURN</td>
<td>HTTP/1.1 (text/event-stream)</td>
</tr>
<tr>
<td><strong>데이터 형식</strong></td>
<td>제한 없음</td>
<td>제한 없음</td>
<td>텍스트/바이너리</td>
<td>텍스트/바이너리</td>
<td>텍스트/바이너리</td>
<td>텍스트</td>
</tr>
<tr>
<td><strong>실시간성</strong></td>
<td>낮음</td>
<td>중간</td>
<td>높음</td>
<td>매우 높음</td>
<td>매우 높음</td>
<td>높음</td>
</tr>
<tr>
<td><strong>재연결 기능</strong></td>
<td>요청마다 연결</td>
<td>별도의 요청</td>
<td>별도의 로직 필요</td>
<td>내장 (WebSocket 프로토콜)</td>
<td>별도의 로직 필요</td>
<td>자동 (EventSource API)</td>
</tr>
<tr>
<td><strong>설정 복잡성</strong></td>
<td>매우 간단</td>
<td>간단</td>
<td>중간</td>
<td>복잡</td>
<td>매우 복잡</td>
<td>간단</td>
</tr>
<tr>
<td><strong>사용 사례</strong></td>
<td>상태 확인</td>
<td>실시간 채팅, 알림</td>
<td>동영상 스트리밍</td>
<td>온라인 게임, 실시간 채팅</td>
<td>화상 통화, 파일 전송</td>
<td>실시간 알림</td>
</tr>
</tbody></table>
<hr />