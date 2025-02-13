<p><del>주니어 개발자의 우당탕탕 배포기 입니다.!! 보안측면이 많이 부실할 수 있습니다!!!! 참고용으로만 읽어주시면 감사하겠습니다.</del></p>
<h2 id="서론">서론</h2>
<p>최근에 새로운 프로젝트를 시작했다. 프로젝트 핵심 개념은 <strong>실시간 스트리밍 사이트 개발</strong>이다. 프로젝트를 진행하면 할 수록 느꼈던 게 있다. <strong>프로젝트 시작할 때 배포 및 CI-CD</strong>를 구축하는 거 가장 에러를 잡기 편하다는 것이다. 그래서 이번 프로젝트는 <strong>배포 및 CICD</strong>를 먼저 구축해보자 한다. 우리 프로젝트에서는 <strong>NextJS</strong>를 사용할 예정이다. 즉 정적 페이지를 배포하는 게 아니여서 s3에 올려놓는 것이 아닌 ec2에 올려놔 <strong>동적 페이지</strong>를 구축했다.<strong>(정적 페이지는 s3에 배포할 예정)</strong> 우당탕탕 주니어 개발자의 프론트엔드 배포를 시작하겠다. </p>
<h3 id="다른vercel--netlify--github-pages-배포-전략보다-ec2에-배포를-한-이유">다른(Vercel / Netlify / GitHub Pages) 배포 전략보다 EC2에 배포를 한 이유</h3>
<p>먼저 현재 <strong>백엔드</strong> 배포를 <strong>AWS EC2</strong>에 배포할 예정이다. 인프라를 통일함으로써 <strong>IAM, 보안 그룹, 네트워크, VPC</strong>를 한 번에 관리할 수 있다. 또한 같은 리전에 배포되기 때문에 <strong>네트워크 지연시간</strong>을 최소화할 수 있다. 또한 우리 서비스 같은 경우 <strong>실시간 스트리밍</strong>을 구축할 예정이기 때문에, 언제든 <strong>Auto Scaling이나 로드밸런스</strong>가 자유로워야 한다고 생각한다. 위와 같은 이유로 다른 배포 전략보다는 <strong>EC2</strong>를 사용하였다. 또한 <strong>가장 중요한 점 ** 현재 난 개발자로써 끊임없이 배워야한다고 생각한다. 한국에선 대부분의 회사에선 **AWS</strong>를 사용할 것이다. 이번 기회에 제대로 <strong>AWS</strong>에 대해서 공부하고 싶어서 <strong>EC2</strong>에 배포를 진행하게 되었다.</p>
<h2 id="ec2amazon-elastic-compute-cloud">EC2(Amazon Elastic Compute Cloud)</h2>
<p><strong>EC2</strong>는 가상의 컴퓨터라고 생각하면 편하다. 우린 <strong>AWS</strong>에서 컴퓨터를 빌려서 여기다가 프론트엔드를 배포한다고 생각하면 편하다. </p>
<h3 id="인스턴스-생성">인스턴스 생성</h3>
<p>인스턴스 생성같은 경우 어렵지 않다. 그냥.. 딸각 딸각 생성하면 된다. </p>
<blockquote>
<p><strong>인스턴스 생성 옵션</strong></p>
</blockquote>
<ul>
<li><strong>애플리케이션 및 OS 이미지(Amazon Machine Image)</strong> : 주로 <strong>인스턴스</strong>를 실행할 때 기반이 되는 운영체제와 소프트웨어 스택을 정의한다. 주로 <strong>Amazon Linux(AWS에 최적화된 OS로, AWS CLI와 기타 AWS 도구가 기본 설치)</strong>와 <strong>Ubuntu(Node.js, Docker, Kubernetes와 같은 컨테이너 기반 애플리케이션)</strong>를 사용한다. 필자는 인프라 구축하는 게 익숙하지 않아 <strong>Amazon Linux</strong>를 채택했다. <blockquote>
</blockquote>
</li>
<li><strong>인스턴스 유형</strong> - 가상 컴퓨터의 사양을 정할 수 있다. <strong>프리티어</strong>를 사용한다면 대다수가 <strong>t2.micro</strong>를 사용할 것이다 . 만약 <strong>CPU / 메모리</strong>가 더 필요하다면 <strong>scaling</strong>을 통해 인스턴스의 사양을 업그레이드할 수 있다.<blockquote>
</blockquote>
</li>
<li><strong>키 페어</strong> : 가상 컴퓨터를 접근 권한을 부여해주는 키이다. <strong>public key와 private key</strong>로 구성되어 있다. 추후 <strong>자동 배포</strong>를 할 때, <strong>public key</strong>를 통해 해당 인스턴스에 접근한다.<blockquote>
</blockquote>
</li>
<li><strong>네트워크 설정</strong> : 인스턴스의 접근 및 보안과 관련된 내용이다. 주로 인바운드 규칙(<strong>누군가가 ec2 instance에 접근할 때의 ip와 port 제한</strong>)을 설정하는데 기본 <strong>SSH</strong>을 port를 열어주고, 이후 필자는 <strong>로드밸런스를 통해 ec2를 접근</strong>할 예정이기 때문에 <strong>로드밸런스에 대한 ip와 port 3000</strong>을 열어줘야한다. 로드 밸런스를 사용하지 않고 바로 ec2에 접근할 예정이라면 <strong>https(443)과 http(80)</strong>을 열어줘야 한다.</li>
</ul>
<h3 id="elastic-ip">Elastic IP</h3>
<p><strong>AWS Instance</strong> 같은 경우 기본적으로 public ip같은 경우 <strong>동적</strong>으로 할당된다. 즉, <strong>인스턴스</strong>를 껐다 켰다를 하면 <strong>IP</strong>가 계속 바뀐다는 얘기다. 이를 해결하기 위해 <strong>AWS</strong>에서는 <strong>Elastic IP</strong>를 제공한다. 사실 탄력적 IP를 사용하는 건 어렵지 않다. 그냥 만들고 해당 instance와 연결하면 끝이다. 여기서 조심해야할 점은 <strong>만약 인스턴스를 내려서 Elastic IP를 사용하지 않는다면 비용이 청구된다.</strong> </p>
<h2 id="route53">Route53</h2>
<p><strong>Route53</strong>이란 <strong>AWS</strong>에서 제공하는 <strong>DNS</strong>이다. 즉 우리는 유저가 해당 도메인에 접근하여 서비스를 구축해야하기 때문에 <strong>Route53</strong>은 필수적이다. 도메인 구매 또한 <strong>route53</strong>을 통해 구매할 수 있지만 <del>많이 비싸다.</del> 그래서 필자는 <strong>가비아</strong>라는 도메인 구매 대행 서비스를 사용할 예정이다. <code>.store / .shop</code>으로 도메인을 구매하면 싸게 구매(년에 500원정도..?)할 수 있다. </p>
<p>사실 이 인프라를 구축하는 게 처음에는 <strong>너무나도 벽이다.</strong> 필자 또한 인프라 구축은 여전히 힘들지만, <del>그나마,,, 배포는 할 수 있는 정도</del> 한 3~4번 정도 벽에 부딪혀보니, 이제는 어떤식으로 해야할 지 감이 잡혔다... 다들 힘들겠지만 !!! 포기하지 않고 끝까지 하시면 !!! 다 할 수 있습니다.!! 화이팅</p>
<p>왜 갑자기 이 얘기를 하냐고요? route53도 생각보다 어려운 게 없다. 그냥. 가비아에서 도메인 구매하고 <strong>route53</strong>에 구매한 도메인 등록하고 <strong>route53</strong>에서 발급한 <strong>NS(네임 서버)</strong>를 가비아에 등록하면 끝이다. 네임서버를 등록하는 이유는 예를 들어 설명하겠다. 우리가 만약에 <strong>naver.com</strong>을 <strong>route53</strong>에 등록했다고 가정해보자. 만약 검증과정 없이 <strong>route53</strong>에서 naver를 무분별하게 등록하면 악의적으로 아무나 naver 사이트를 바꿀 수 있을 것이다. 이를 막기 위해 <strong>NS</strong>를 도입하여 도메인 소유자만이 naver와 연결할 수 있게 해주는 것이다. </p>
<p>다시 돌아오면 가비아에서 산 도메인을 가지고 <strong>route53</strong>에 등록하려면 <strong>route53</strong>에 발급해준 <strong>NS</strong>를 가비아에 등록해줘야한다. <strong>NS</strong>는 총 4개로 이루어져 있어서 4개 전체를 그대로 등록해주면 된다. 여기서 <code>.</code> 은 제외하고 입력해줘야한다. </p>
<h2 id="acmamazon-certificate-manager">ACM(Amazon Certificate Manager)</h2>
<p><strong>ACM (AWS Certificate Manager)</strong>는 SSL/TLS 인증서를 손쉽게 관리하고 배포할 수 있도록 도와주는 AWS 서비스다. 이를 통해 애플리케이션과 웹사이트의 HTTPS 연결을 안전하게 구축하고 데이터를 암호화할 수 있다. 아무리 보안을 신경안쓴다고 해서,, <strong>SSL</strong> 인증서를 발급 안하는 건 아니다.. 이건 그냥.. 기본적으로 발급을 해야한다. 우리 유저들의 개인정보가 손쉽게 탈취당하는 걸 보고싶다면? 발급 안해도 상관없다.. 최소한의 보안이라고 생각한다.  만약 발급을 하지 않으면 <strong>브라우저 내에서 안전하지 않는 페이지라고 뜰 것이다.. 이는 유저 이탈과 이어질 것이다..</strong></p>
<p>이것도 생각보다 쉽다. 그냥 ACM 들어가서 구매한 도메인 이름 등록하고 <strong>Route53 레코드</strong> 생성하면 된다. 사실 아직 이게 궁금하긴하다.. 굳이.. <strong>Route53</strong>에 레코드를 생성해야하냐이다.. 지금 보면 등록 안 해도 잘 굴러가는데..? 따지고 보면 <strong>route53</strong>은 <strong>DNS</strong>만 관리하는 서비스이다. <strong>DNS</strong>에 SSL 인증서가 필요할까? 일단 뭐 등록하라니깐 등록했습니다 저는 </p>
<h2 id="albapplication-load-balancer">ALB(Application Load Balancer)</h2>
<p><strong>AWS의 Elastic Load Balancing</strong> 서비스 중 하나로, 애플리케이션 계층(Layer 7)에서 트래픽을 관리하고 분산하는 역할을 수행한다. HTTP 및 HTTPS 트래픽을 대상으로 작동하며, 여러 EC2 인스턴스 또는 컨테이너에 트래픽을 균등하게 분배하는 데 사용된다. 주로 <code>트래픽 분산, SSL 인증서 적용, 리다이렉트, 헬스체크</code>에서 사용한다. 필자는 <strong>SSL</strong> 인증서를 도메인에 적용하기 위해 사용했다. </p>
<h3 id="cloudfront">CloudFront</h3>
<p><strong>CloudFront</strong>는 AWS의 콘텐츠 전송 네트워크 <strong>(Content Delivery Network - CDN)</strong> 서비스다. 주로 <code>콘텐츠 캐싱 및 빠른 전송, HTTPS 처리, 글로벌 콘텐츠 배포, 비용 절감</code>을 위해 사용한다. <strong>CloudFront</strong>는 정적 웹사이트 호스팅 (S3 + CloudFront), 동영상 스트리밍 (HLS/DASH),  정적/동적 콘텐츠의 글로벌 배포, API 응답의 캐싱에서 사용된다. 현재 필자는 아직 동영상 스트리밍 파트를 다루고 있지 않기 때문에 아직 <strong>CloudFront</strong>를 사용하지 않고 <strong>ALB</strong>만 사용해 배포를 했다. 그 이유는 우리는 <strong>NextJS 동적 페이지</strong>를 배포해야하기 때문이다! <strong>CloudFront</strong> 경우 정적 페이지를 배포하는 데 적합하다. </p>
<table>
<thead>
<tr>
<th>상황</th>
<th>ALB</th>
<th>CloudFront</th>
</tr>
</thead>
<tbody><tr>
<td>동적 콘텐츠 중심 (SSR, API)</td>
<td>✅ 적합</td>
<td>❌ 제한적</td>
</tr>
<tr>
<td>정적 콘텐츠 중심</td>
<td>❌ 부적합</td>
<td>✅ 적합</td>
</tr>
<tr>
<td>글로벌 사용자 대상 서비스</td>
<td>❌ 리전 내 동작</td>
<td>✅ 엣지 로케이션 활용</td>
</tr>
<tr>
<td>로드 밸런싱 (백엔드 부하 분산)</td>
<td>✅ 필수</td>
<td>❌ 로드 밸런싱 기능 없음</td>
</tr>
<tr>
<td>HTTPS 적용</td>
<td>✅ 가능</td>
<td>✅ 가능 (엣지에서 종료)</td>
</tr>
<tr>
<td>비용 최적화 (캐싱)</td>
<td>❌ 매번 오리진 서버로 요청</td>
<td>✅ 캐싱으로 비용 절감 가능</td>
</tr>
</tbody></table>
<blockquote>
<p><strong>Application Load Balancer 생성</strong></p>
</blockquote>
<ul>
<li><strong>네트워크 매핑</strong> : <strong>IP</strong> 주소 설정에 따라 선택한 서브넷의 대상으로 트래픽을 라우팅한다. 이 부분에서 주의해야할 점은 <strong>가용 영역</strong>이다. <strong>ec2 instance</strong> 별로 할당된 가용 영역이 존재한다. 이 가용영역과 일치해야 문제가 없을 것이다.<blockquote>
</blockquote>
</li>
<li><strong>보안 그룹</strong> : <strong>outbound</strong>라고 생각하면 편하다. 즉 로드밸런스에서 어떤 포트로 나갈 지에 대한 정의이다. 현재 <strong>ec2 instance</strong>에 접속하려면 결론적으로 <strong>3000</strong>포트로 접근해야하기 때문에 보안 그룹 부분에 3000포트를 열어줘야한다. <blockquote>
</blockquote>
</li>
<li><strong>리스너 및 라우팅</strong> : <strong>Inbound</strong>이다 <strong>route53</strong>에서 로드밸런스로 접근할 때 해당 포트를 열어줘야한다. <strong>Route53</strong> 같은 경우 443을 통해 로드밸런스에 접근하기 때문에 443 포트를 열어줘야한다. 인증서 또한 현재 <strong>ACM</strong>에서 발급 받은 인증서를 넣어주면 된다. </li>
</ul>
<p>이렇게 되면 최종 배포가 완료이다. 생각보다 너무 간략하게 쓴 거 같다. 하면서 참 <strong>Health Check</strong> 에러가 많이 발생햇는데.. 어렵다 어려워 .. 3번째 배포를 해보니 이젠 그나마.. 전보단 할 만해진 거 같다. 이상... </p>