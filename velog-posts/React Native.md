<h2 id="cross-platform과-hybrid-app">Cross Platform과 Hybrid App</h2>
<h3 id="cross-platform">Cross Platform</h3>
<p><strong>Cross Platform</strong>이란 하나의 코드 베이스로 여러 플랫폼(<strong>iOS / Android</strong>)을 동시에 개발하는 것을 의미한다. 코드를 재사용할 수 있어서 <strong>개발 비용이나 시간</strong>을 줄일 수 있다. <strong>ex) React Native / Flutter</strong></p>
<blockquote>
<p><strong>모바일 개발 언어</strong>
<strong>Kotlin</strong> : <strong>안드로이드 앱 개발</strong>에 주로 사용되는 언어로 <strong>Java</strong>와의 호환성이 높고 간결한 문법을 제공한다.</p>
</blockquote>
<p><strong>Swift</strong> : <strong>iOS 앱 개발</strong>에 사용되는 언어로 안전성과 성능이 뛰어나며 Apple의 공식 언어이다.</p>
<blockquote>
</blockquote>
<p><strong>Flutter</strong> : 구글이 개발한 <strong>UI tool kit</strong>으로 <strong>Dart 언어</strong>를 사용하여 iOS와 Android 모두 네이티브 성능을 제공하는 앱을 만들 수 있다.</p>
<h3 id="hybrid-app">Hybrid App</h3>
<p><strong>Hybrid App</strong>은 <strong>웹 기술(HTML, CSS, JavaScript)</strong>로 작성된 애플리케이션을 <strong>네이티브 앱 컨테이너(WebView)</strong> 안에서 실행하도록 만든 앱이다. 이 앱은 <strong>웹 앱과 네이티브 앱</strong>의 특성을 모두 갖춘 형태로, 다양한 플랫폼(iOS, Android 등)에서 동작할 수 있다. 네이티브 앱과 웹 앱의 장점을 결합한 형태의 어플리케이션이다. </p>
<blockquote>
<p><strong>Hybrid App 특징</strong> </p>
</blockquote>
<ol>
<li><strong>크로스 플랫폼 개발</strong> :  하나의 코드 베이스로 iOS와 Andtroid 등 다양한 플랫폼에서 실행할 수 있다. 이를 통해 개발 시간과 비용을 절감할 수 있다.</li>
<li><strong>네이티브 기능 접근</strong> : 카메라, GPS, 알림 등 네이티브 기능에 접근할 수 있어 사용자 경험을 향상 시킨다.</li>
<li><strong>빠른 업데이트</strong> : 웹 기술을 사용하기 때문에 앱의 콘텐츠를 서버에서 직접 업데이트 할 수 있어 빠른 배포가 가능하다.</li>
<li><strong>성능</strong> : 네이티브 앱에 비해 성능이 떨어질 수 있지만, 최신 기술을 사용하면 성능을 개선할 수 있다.</li>
</ol>
<p>앱은 <strong>React Native</strong> / 웹은 <strong>React</strong>을 사용하며 <strong>React Native</strong>의 프레임워크인 <strong>Expo</strong> / <strong>React</strong>의 프레임워크인 <strong>Next</strong>를 사용할 예정이다. <strong>Notch 등 native에서만 구동 가능한 기능</strong>을 제외한 나머지 부분은 우리가 평상시 익숙한 <strong>Next React</strong>로 개발한다는 얘기다.</p>
<h2 id="react-native--expo">React Native &amp; Expo</h2>
<h3 id="react-native">React Native</h3>
<p><strong>React Native</strong>는 Facebook에서 개발한 오픈 소스 프레임워크로, <strong>JavaScript와 React</strong>를 사용해 <strong>네이티브 모바일 애플리케이션(iOS, Android)</strong>을 개발할 수 있다. <strong>React Native</strong>는 <strong>Cross Platform 개발</strong>을 지원하며, 한 번 작성한 코드를 <strong>iOS와 Android</strong>에서 동시에 사용할 수 있다.</p>
<blockquote>
<p><strong>React Native 특징</strong>
<strong>네이티브 렌더링</strong> : <strong>React Native</strong>는 <strong>WebView</strong>를 사용하지 않고 Native UI Component를 직접 렌더링한다. 이로 인해 <strong>준수한 성능과 사용자 경험</strong>을 user들에게 제공한다.</p>
</blockquote>
<p><strong>React 문법 사용</strong> : <strong>React</strong>의 Component 기반 UI설계 방식과 동일한 패턴을 사용해 기존 <strong>React 개발자</strong>라면 러닝 커브가 낮다</p>
<blockquote>
</blockquote>
<p><strong>코드 재사용성</strong> : 하나의 코드베이스로 <strong>iOS와 Android</strong>를 동시에 지원하기 때문에 코드 재사용성이 높다. </p>
<blockquote>
</blockquote>
<p><strong>Bridge</strong> : <strong>React Native</strong>는 <strong>Javascript</strong>와 <strong>Native API</strong>를 연결하기 위해 <strong>bridge</strong>를 사용한다. 이 <strong>bridge</strong>를 통해 Native 기능에 접근할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>Hot Reload</strong> : 코드 변경 시 앱을 재시작하지 않아도 변경 사항을 즉시 확인할 수 있다.</p>
<h3 id="expo">Expo</h3>
<p><strong>Expo</strong>는 <strong>React Native</strong> 개발 환경을 간소화하고 더 빠르게 작업할 수 있도록 돕는 도구 및 <strong>프레임워크</strong>이다. <strong>Expo</strong>는 <strong>React Native</strong>의 상위 레이어로 동작하며, 초보자부터 고급 개발자까지 쉽게 모바일 앱을 개발할 수 있게 한다.</p>
<blockquote>
<p><strong>Expo 특징</strong>
<strong>빠른 시작</strong> - <strong>Expo CLI</strong>를 사용하면 설정 없이도 빠르게 프로젝트를 시작할 수 있다.</p>
</blockquote>
<p><strong>다양한 API 제공</strong> - <code>카메라 , 위치서비스, 푸시알림</code> 등 다양한 기능을 사용할 수 있는 API를 미리 구성하여 제공한다. 이를 통해 복잡한 네이티브 코드 없이도 필요한 기능을 쉽게 구현할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>Expo App</strong> - 개발중인 앱을 실제 기기에서 쉽게 테스트할 수 있도록 해주는 <strong>Expo App</strong>이 있다. QR코드를 스캔하여 실시간으로 앱을 실행하고, 변경사항을 즉시 확인할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>빌드 서비스</strong> - <strong>Expo</strong>는 클라우드에서 앱을 빌드할 수 있는 서비스를 제공한다. 이를 통해 복잡한 네이티브 환경 설정 없이도 <strong>iOS와 Android 앱</strong>을 쉽게 배포할 수 있다.</p>
<blockquote>
</blockquote>
<p><strong>OTA 업데이트(Over-The-Air Update)</strong>
앱을 배포한 후에도 코드 수정이 가능하며, 사용자가 앱을 업데이트 하지 않고도 새로운 버전을 제공할 수 있다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/484c39f8-28ca-4c0d-a956-6642684e6343/image.png" />
다음은 <strong>React Native</strong>에서 일부 발취한 내용이다. <strong>React Native에서도 Expo</strong>를 권장하고 있기 때문에  <strong>Expo</strong>를 사용하는 편이 낫다. Expo가 나오기 이전 <strong>React Native CLI</strong>를 사용했다. 간단하게 설명하자면 <strong>React Natvie CLI</strong>는 성능이 상대적으로 좋고 높은 러닝 커브가 존재하는 반면 <strong>Expo</strong>는 성능이 상대적으로 나쁘지만 러닝커브가 낮다는 장점이 있다.</p>
<table>
<thead>
<tr>
<th><strong>항목</strong></th>
<th><strong>React Native CLI</strong></th>
<th><strong>Expo</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>설치 및 설정</strong></td>
<td>직접 iOS와 Android 개발 환경 설정 필요 (Xcode, Android Studio)</td>
<td>자동 설정 제공. Expo CLI로 바로 프로젝트 시작 가능</td>
</tr>
<tr>
<td><strong>네이티브 코드 접근</strong></td>
<td>Swift, Objective-C, Java, Kotlin과 같은 네이티브 코드 수정 가능</td>
<td>Managed Workflow에서는 네이티브 코드 수정 불가</td>
</tr>
<tr>
<td><strong>플랫폼 지원</strong></td>
<td>iOS, Android</td>
<td>iOS, Android (Expo Go 앱을 통해 미리보기 제공)</td>
</tr>
<tr>
<td><strong>네이티브 API</strong></td>
<td>네이티브 API 직접 연결 필요</td>
<td>Expo SDK를 통해 주요 네이티브 API(카메라, GPS 등) 제공</td>
</tr>
<tr>
<td><strong>OTA 업데이트</strong></td>
<td>직접 설정해야 함</td>
<td>기본적으로 제공 (Over-the-Air 업데이트 지원)</td>
</tr>
<tr>
<td><strong>앱 크기</strong></td>
<td>필요한 라이브러리만 포함하여 상대적으로 작음</td>
<td>Expo SDK 전체가 포함되어 앱 크기가 더 커질 수 있음</td>
</tr>
<tr>
<td><strong>앱 스토어 배포</strong></td>
<td>직접 빌드 및 배포</td>
<td>Expo EAS(Build)를 통해 빌드 및 배포 가능</td>
</tr>
<tr>
<td><strong>플러그인 확장</strong></td>
<td>네이티브 라이브러리를 직접 추가 가능</td>
<td>Managed Workflow에서는 Expo에서 지원하는 기능에 제한됨</td>
</tr>
<tr>
<td><strong>초기 학습 곡선</strong></td>
<td>높은 편 (환경 설정 및 네이티브 작업 필요)</td>
<td>낮은 편 (JavaScript만으로 작업 가능)</td>
</tr>
<tr>
<td><strong>프로토타이핑</strong></td>
<td>설정이 복잡하여 프로토타이핑에 시간이 걸릴 수 있음</td>
<td>빠르게 시작 가능, 프로토타이핑에 적합</td>
</tr>
<tr>
<td><strong>Hot Reloading</strong></td>
<td>지원</td>
<td>지원</td>
</tr>
<tr>
<td><strong>커스터마이징</strong></td>
<td>완전한 유연성 제공</td>
<td>Managed Workflow에서는 제한적, Bare Workflow로 전환 가능</td>
</tr>
</tbody></table>
<blockquote>
<p><strong>create-expo-app</strong></p>
</blockquote>
<pre><code class="language-bash">npx create-expo-app —-template tabs@sdk-51</code></pre>
<h2 id="android-emulator--ios-simulator">Android Emulator &amp; iOS Simulator</h2>
<p><strong>Android Emulator와 iOS Simulator</strong>는 각각 <strong>Android와 iOS</strong> 앱을 개발, 디버깅, 테스트하기 위해 제공되는 <strong>Virtual Machine</strong>다. 이들은 실제 기기를 대신하여 앱을 실행하고, 다양한 환경에서 앱이 어떻게 동작하는지 확인할 수 있도록 도와준다. 두 도구는 기능과 작동 방식에서 차이가 있으며, 각 플랫폼에 최적화된 개발 환경을 제공한다.</p>
<h3 id="android-emulator">Android Emulator</h3>
<p>Android Emulator는 <strong>Android Virtual Device (AVD)</strong>를 실행하는 소프트웨어다. Google에서 제공하며, 다양한 Android 버전, 해상도, 하드웨어 구성에서 앱을 테스트할 수 있다. 안드로이드용 다양한 디바이스 크기, 해상도, API 레벨, CPU 아키텍처를 설정할 수 있으며, 하드웨어 에뮬레이션(<strong>GPS / 카메라 / 배터리 상태 / 전화 / 메세지</strong>)를 실제 기기에서 동작하는 하드웨어 기능을 가상화하여 사용할 수 있다. 하지만, 비교적 높은 시스템 <strong>Resource</strong>를 요구하며 가상 머신 기반이라 <strong>iOS simulator</strong>에 비해 초기 실행 속도가 느릴 수 있다.</p>
<h3 id="ios-simulator">iOS Simulator</h3>
<p><strong>iOS Simulator</strong>는 Apple에서 제공하며, Xcode의 일부로 동작한다. iOS 앱을 가상으로 실행하고 디버깅할 수 있는 환경을 제공한다. <strong>macOS</strong>에서만 실행 가능하며, <strong>iPhone 뿐만 아니라 iPad, Apple Watch</strong>등 다양한 device simulation이 가능하다. 하지만 하드웨어 기능 테스트를 완전히 에뮬레이션하지 않아 <strong>Android Emulator</strong>보단 지원 범위가 적다. </p>
<p><strong>React Native</strong>에선 <strong>Android Emulator &amp; iOS Simulator</strong>를 사용하여 안드로이드 개발과 ios 개발을 동시에 진행한다. <strong>React Native</strong>에 내장되어 있는 <code>Platform</code> 을 이용하여 <strong>Android와 iOS</strong>별 코드를 작성할 수 있다.</p>
<pre><code class="language-javascript">import { Platform } from 'react-native';

const isAndroid = Platform.OS === 'android';
const isIOS = Platform.OS === 'ios';</code></pre>
<h2 id="react-native-tag">React Native Tag</h2>
<p><strong>React Native는 HTML 태그와 유사한 컴포넌트</strong>를 제공하지만, <strong>네이티브 플랫폼(Android, iOS)</strong>을 기반으로 동작하기 때문에 사용하는 태그의 성격과 동작이 다소 다르다. 아래는 <strong>React Native</strong>에서 자주 사용하는 주요 태그이다.</p>
<h3 id="view">View</h3>
<p><strong>View에서 React Native</strong>의 가장 기본적인 컨테이너 컴포넌트이다. HTML의 <code>&lt;div&gt;</code>와 유사하지만, 더 강력한 레이아웃과 스타일링 기능 제공한다. <strong>Flexbox</strong>를 통해 기본 레이아웃을 구성한다. <code>flexDirection, justifyContent, alignItems</code>와 같은 속성으로 <strong>수직/수평 정렬</strong>이 간단하며, <code>flex</code> 속성을 사용하면 화면 크기에 따라 자동으로 요소가 배치되고 크기가 조정할 수 있다. </p>
<pre><code class="language-jsx">&lt;View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}&gt;
    &lt;Text&gt;Hello World&lt;/Text&gt;
&lt;/View&gt;</code></pre>
<h3 id="text">Text</h3>
<p><strong>Text</strong>는 텍스트를 렌더링하는 컴포넌트로써 HTML의 <code>&lt;p&gt; 또는 &lt;span&gt; 태그</code>와 유사하다. <strong>Style 및 Text Event Handling</strong>이 가능하다. <strong>React Native</strong>에서 모든 텍스트는 반드시 <code>&lt;Text&gt; 태그</code>로 감싸야 스타일을 적용할 수 있다.</p>
<pre><code class="language-jsx">&lt;Text style={{ fontSize: 18, color: 'blue' }}&gt;This is a text&lt;/Text&gt;</code></pre>
<h3 id="button">Button</h3>
<p><strong>Button</strong> 클릭 이벤트를 처리하는 기본 버튼 컴포넌트이다. 간단한 스타일링이 가능하지만, 고급 커스터마이징에는 제한적이며 만약 복잡한 버튼을 만들고 싶다면, <code>&lt;Pressable&gt; 또는 &lt;TouchableOpacity&gt;</code>를 사용하는 것이 좋다. 간단한 <strong>click event</strong>는 <strong><code>onPress</code></strong> 속성으로 처리하고, 기본 Button으로 <strong>Android에서는 Material Design 스타일 버튼으로 렌더링되며, iOS에서는 기본 UIButton 스타일로 렌더링된다.</strong> </p>
<table>
<thead>
<tr>
<th><strong>속성</strong></th>
<th><strong>설명</strong></th>
<th><strong>타입</strong></th>
<th><strong>필수 여부</strong></th>
</tr>
</thead>
<tbody><tr>
<td><code>title</code></td>
<td>버튼에 표시될 텍스트</td>
<td><code>string</code></td>
<td>✔️</td>
</tr>
<tr>
<td><code>onPress</code></td>
<td>버튼이 눌렸을 때 호출될 콜백 함수</td>
<td><code>function</code></td>
<td>✔️</td>
</tr>
<tr>
<td><code>color</code></td>
<td>버튼의 텍스트 또는 배경 색상 (Android: Background-Color / iOS: text-color)</td>
<td><code>string</code></td>
<td>❌</td>
</tr>
<tr>
<td><code>disabled</code></td>
<td>버튼을 비활성화 상태로 설정</td>
<td><code>boolean</code></td>
<td>❌</td>
</tr>
<tr>
<td><code>accessibilityLabel</code></td>
<td>버튼의 접근성 라벨로, 화면 읽기 앱(Screen Reader)이 읽을 텍스트</td>
<td><code>string</code></td>
<td>❌</td>
</tr>
<tr>
<td>```jsx</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>&lt;Button</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>title=&quot;Click Me&quot;</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>onPress={() =&gt; alert('Button Pressed!')}</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>color=&quot;#841584&quot;</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>/&gt;</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>```</td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody></table>
<h4 id="pressable">Pressable</h4>
<p><strong>Pressable은 React Native</strong>에서 더 세밀하게 터치 이벤트를 처리할 수 있는 컴포넌트로써 다양한 상태에 따라 스타일 변경 가능하다.
<strong>터치 상태(onPressIn, onPressOut)나 포커스 상태(onFocus, onBlur)</strong>를 기반으로 스타일링 가능하며, 버튼 내부에 텍스트, 아이콘, 이미지 등을 자유롭게 배치 가능하다. 주로 눌림 상태, 포커스 상태 등을 제어하며 세밀한 사용자 인터랙션을 처리할 때 사용한다. </p>
<pre><code class="language-jsx">import { Pressable, Text, StyleSheet } from 'react-native';

export default function App() {
  return (
    &lt;Pressable
      onPress={() =&gt; console.log('Pressed!')}
      onPressIn={() =&gt; console.log('Press Started')}
      onPressOut={() =&gt; console.log('Press Released')}
      style={({ pressed }) =&gt; [
        styles.button,
        pressed &amp;&amp; { backgroundColor: 'darkblue' },
      ]}
    &gt;
      &lt;Text style={styles.text}&gt;Custom Button&lt;/Text&gt;
    &lt;/Pressable&gt;
  );
}

const styles = StyleSheet.create({
  button: {
    padding: 10,
    backgroundColor: 'blue',
    borderRadius: 5,
  },
  text: {
    color: 'white',
    fontSize: 16,
  },
});</code></pre>
<h4 id="touchableopacity">TouchableOpacity</h4>
<p><strong>TouchableOpacity</strong>는 버튼을 누를 때 투명도가 변경되어 사용자가 클릭 동작을 인지할 수 있고, 내부에 텍스트, 아이콘, 이미지 등을 배치 가능하다. 주로 버튼 내부에 텍스트, 아이콘 등 다양한 콘텐츠를 추가하거나, 세밀한 상태 관리가 필요하지 않을 때 적합하다.</p>
<pre><code class="language-jsx">import { TouchableOpacity, Text, StyleSheet } from 'react-native';

export default function App() {
  return (
    &lt;TouchableOpacity
      onPress={() =&gt; alert('Button Pressed')}
      style={styles.button}
    &gt;
      &lt;Text style={styles.text}&gt;Touchable Button&lt;/Text&gt;
    &lt;/TouchableOpacity&gt;
  );
}

const styles = StyleSheet.create({
  button: {
    padding: 10,
    backgroundColor: 'green',
    borderRadius: 5,
  },
  text: {
    color: 'white',
    fontSize: 16,
  },
});</code></pre>
<h3 id="textinput">TextInput</h3>
<p><strong>TextInput</strong>은 입력 필드를 제공하는 컴포넌트다. <strong>HTML의 <code>&lt;input&gt;</code></strong> 태그와 유사하다. 주로 사용자가 텍스트를 입력하거나 데이터를 입력받는 데 사용되며 다양한 속성 제공한다. <strong>ex) placeholder, secureTextEntry</strong></p>
<pre><code class="language-jsx">&lt;TextInput
    style={{ height: 40, borderColor: 'gray', borderWidth: 1 }}
    placeholder=&quot;Enter text&quot;
    onChangeText={(text) =&gt; console.log(text)}
/&gt;
</code></pre>
<h3 id="flatlist">FlatList</h3>
<p><strong>FlatList</strong>는 대량의 데이터 리스트를 효율적으로 렌더링하는 컴포넌트이다. <strong>Virtualization</strong> 기술을 사용해 성능을 최적화하며 <strong>scroll</strong> 내 보이는 항목만 렌더링되어 자동 최적화가 되어있다. </p>
<pre><code class="language-jsx">&lt;FlatList
    data={[{ key: '1', name: 'John' }, { key: '2', name: 'Jane' }]}
    renderItem={({ item }) =&gt; &lt;Text&gt;{item.name}&lt;/Text&gt;}
    keyExtractor={(item) =&gt; item.key}
/&gt;
</code></pre>
<h3 id="statusbar">StatusBar</h3>
<p><strong>StatusBar</strong>는 모바일 디바이스 화면 상단에 있는 상태 표시줄을 제어하는 컴포넌트다. 상태 표시줄은 <strong>배터리 상태, 네트워크 신호, 알림</strong>등이 표시된다. <strong>React Native</strong>는 StatusBar는 <strong>iOS와 Android</strong>의 상태 표시줄 속성을 변경할 수 있다. iOS는 기본적으로 투명한 상태 표시줄을 제공하며, Android에서는 상태 표시줄의 배경색과 텍스트 색상을 설정 가능하다. 따라서 <strong>StatusBar</strong>같은 경우 <strong>Android의 상태 표시줄</strong>을 처리할 때 사용한다. </p>
<pre><code class="language-jsx">import { StatusBar, View, Text } from 'react-native';

export default function App() {
  return (
    &lt;View style={{ flex: 1 }}&gt;
      {/* 상태 표시줄 설정 */}
      &lt;StatusBar
        backgroundColor=&quot;#6200EE&quot; // Android 전용: 상태 표시줄 배경색
        barStyle=&quot;light-content&quot;  // 아이콘과 텍스트를 밝게 표시
        translucent={true}        // 상태 표시줄을 반투명으로 설정
      /&gt;
      &lt;Text style={{ marginTop: 50 }}&gt;Hello, World!&lt;/Text&gt;
    &lt;/View&gt;
  );
}</code></pre>
<h3 id="safeareaview">SafeAreaView</h3>
<p><strong>SafeAreaView</strong>는 모바일 디바이스의 안전 영역<strong>(Safe Area)</strong>을 고려하여 콘텐츠를 표시하는 컨테이너 컴포넌트다. <strong>iOS의 노치(Notch), Android의 상태 표시줄 및 소프트 키 영역</strong> 등을 자동으로 피해서 UI를 배치한다. <strong>iOS</strong>같은 경우 노치가 까다롭기 때문에, 주로 <strong>SafeAreaView</strong>같은 경우 <strong>iOS</strong>를 controll할 때 사용된다.</p>
<h3 id="statusbar와-safeareaview의-차이">StatusBar와 SafeAreaView의 차이</h3>
<table>
<thead>
<tr>
<th><strong>특징</strong></th>
<th><strong>StatusBar</strong></th>
<th><strong>SafeAreaView</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>역할</strong></td>
<td>상태 표시줄의 스타일, 색상, 표시 여부 등을 제어.</td>
<td>안전 영역을 감지하고 콘텐츠를 배치.</td>
</tr>
<tr>
<td><strong>주요 사용 사례</strong></td>
<td>상태 표시줄을 숨기거나 스타일 변경.</td>
<td>노치 또는 상태 표시줄을 피해서 UI 구성.</td>
</tr>
<tr>
<td><strong>적용 범위</strong></td>
<td>상태 표시줄에만 영향.</td>
<td>View 내부 콘텐츠 레이아웃에 영향을 미침.</td>
</tr>
<tr>
<td><strong>플랫폼 지원</strong></td>
<td>Android와 iOS 모두에서 동작 (단, 일부 속성은 Android 전용).</td>
<td>주로 iOS에서 사용되며, Android에서는 추가적인 패딩이 필요할 수 있음.</td>
</tr>
</tbody></table>
<h3 id="image">Image</h3>
<p><strong>Image</strong>는 이미지를 렌더링하는 컴포넌트로써 <strong>HTML의 <code>&lt;img&gt;</code>와 유사하다.</strong></p>
<pre><code class="language-jsx">&lt;Image
    source={{ uri: 'https://example.com/image.png' }}
    style={{ width: 100, height: 100 }}
/&gt;</code></pre>
<h3 id="modal">Modal</h3>
<p><strong>Modal</strong>은 앱 화면 위에 <strong>overlay</strong> 형태로 화면을 띄우는 컴포넌트로써 사용자 동작에 대한 피드백이나 추가 정보를 제공하는 데 유용하다.</p>
<pre><code class="language-jsx">&lt;Modal visible={true} transparent={true}&gt;
    &lt;View style={{ backgroundColor: 'rgba(0,0,0,0.5)', flex: 1 }}&gt;
        &lt;Text&gt;Hello Modal&lt;/Text&gt;
    &lt;/View&gt;
&lt;/Modal&gt;</code></pre>
<h3 id="keyboardavoidingview">KeyboardAvoidingView</h3>
<p><strong>KeyboardAvoidingView</strong>는 키보드가 화면을 가리지 않도록 뷰를 조정하는데 사용하는 컴포넌트이다. 주로 <strong>텍스트 입력 필드가 화면 아래 쪽에 있을 때 유용하다.</strong></p>
<pre><code class="language-jsx">&lt;KeyboardAvoidingView behavior=&quot;padding&quot; style={{ flex: 1 }}&gt;
    &lt;TextInput placeholder=&quot;Type here&quot; /&gt;
&lt;/KeyboardAvoidingView&gt;
</code></pre>
<h2 id="webview">WebView</h2>
<p><strong>WebView</strong>는 <strong>React Native</strong>에서 웹 콘텐츠를 앱 내에서 표시할 수 있는 컴포넌트다. 웹 페이지를 네이티브 애플리케이션에 임베드(Embed)할 수 있는 가장 간단한 방법이다. <strong>react-native-webview 패키지</strong>를 사용해 구현하며, 이는 공식적으로 React Native의 WebView 구현체로 제공된다.</p>
<blockquote>
<pre><code class="language-bash">npx expo install react-native-webview</code></pre>
</blockquote>
<pre><code>
### WebView 주요 속성

| **속성**            | **설명**                                                                 | **타입**       | **기본값**    |
|---------------------|-------------------------------------------------------------------------|---------------|---------------|
| `source`            | 로드할 웹 페이지의 URL 또는 HTML.                                        | `object`      | `{}`          |
| `onMessage`         | 웹 페이지에서 네이티브로 메시지를 보낼 때 호출되는 콜백 함수.            | `function`    | `null`        |
| `javaScriptEnabled` | WebView에서 JavaScript를 활성화.                                         | `boolean`     | `true`        |
| `domStorageEnabled` | WebView에서 DOM Storage API를 활성화.                                    | `boolean`     | `true`        |
| `injectedJavaScript`| 웹 페이지가 로드된 후 실행할 JavaScript 코드를 삽입.                     | `string`      | `null`        |
| `onLoadStart`       | 페이지 로드가 시작될 때 호출되는 콜백 함수.                              | `function`    | `null`        |
| `onLoadEnd`         | 페이지 로드가 완료될 때 호출되는 콜백 함수.                              | `function`    | `null`        |
| `onError`           | 페이지 로드 중 에러가 발생했을 때 호출되는 콜백 함수.                    | `function`    | `null`        |
| `style`             | WebView의 스타일 설정.                                                  | `StyleSheet`  | `{}`          |
| `originWhitelist`   | WebView에서 허용할 URL 목록을 정의. 기본적으로 모든 도메인이 허용됨.     | `array`       | `[&quot;*&quot;]`       |


&gt; **WebView의 장점 &amp; 단점**
&gt;
**WebView의 장점**
**웹과 네이티브 통합** : 네이티브 앱에서 웹 기술(HTML, CSS, JS)을 사용할 수 있다.
&gt;
**PWA와의 호환성** : **PWA(Progressive Web App)**를 네이티브 앱처럼 실행 가능하다.
&gt;
**빠른 개발 속도** : 웹 개발자가 기존 웹 코드를 활용해 앱을 빠르게 개발할 수 있다.
&gt;
**유연한 커스터마이징** : **injectedJavaScript**를 통해 웹 페이지의 동작을 제어 가능.
&gt;
&gt;
**WebView의 단점**
**성능 문제** : **WebView는 네이티브 UI 컴포넌트**가 아니므로, 복잡한 애니메이션이나 동작에서 성능 저하된다. 웹 페이지 렌더링 성능은 브라우저 엔진(WebView 엔진)에 의존한다.
&gt;
**네이티브와의 연결** : 네이티브와 웹 간 통신이 필요한 경우 추가적인 설정 및 코드 작성 필요하다.
&gt;
**제한된 플랫폼 지원** : **WebView 엔진은 Android와 iOS**마다 동작 방식이 다를 수 있다.

</code></pre>