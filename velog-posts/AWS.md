<h2 id="docker-란">Docker 란?</h2>
<p>먼저 <strong>Docker</strong>란? 애플리케이션을 <strong>컨테이너(Container)</strong>라는 가상 환경에서 실행할 수 있도록 해주는 오픈 소스 플랫폼이다. <strong>container</strong>는 애플리케이션과 그 실행에 필요한 모든 라이브러리, 종속성, 환경설정 등을 하나의 패키지로 묶어, 일관된 환경에서 실행되도록 보장해준다. 이를 통해 개발 환경과 운영 환경의 차이에서 오는 문제를 해결하고, 배포를 쉽고 빠르게 할 수 있게 한다. </p>
<h3 id="dockerfile">Dockerfile</h3>
<p>애플리케이션과 필요한 환경을 포함하는 <strong>Docker Image</strong>를 빌드하는데 사용된다. 운영체제, 애플리케이션, 라이브러리, 실행 명령어 등을 정의한다. </p>
<pre><code class="language-dockerfile"># 베이스 이미지 설정
FROM node:18-alpine

# 작업 디렉토리 설정
WORKDIR /app

# 의존성 파일만 복사
COPY package*.json ./

# 의존성 설치
RUN npm install

# 나머지 소스 코드 복사
COPY . .

# 애플리케이션 빌드
RUN npm run build

# 애플리케이션 실행 명령어
CMD [&quot;node&quot;, &quot;dist/src/main.js&quot;]
</code></pre>
<h3 id="dockerfile에서-사용하는-주요-명령어">Dockerfile에서 사용하는 주요 명령어</h3>
<table>
<thead>
<tr>
<th><strong>명령어</strong></th>
<th><strong>설명</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>FROM</strong></td>
<td>기본으로 사용할 베이스 이미지를 설정. (예: <code>node:18-alpine</code>, <code>ubuntu:22.04</code>)</td>
</tr>
<tr>
<td><strong>WORKDIR</strong></td>
<td>컨테이너 내부에서 작업 디렉토리를 설정. (예: <code>/app</code>)</td>
</tr>
<tr>
<td><strong>COPY</strong></td>
<td>호스트 시스템의 파일을 컨테이너로 복사.</td>
</tr>
<tr>
<td><strong>RUN</strong></td>
<td>컨테이너 빌드 중 실행할 명령어. (예: <code>RUN npm install</code>)</td>
</tr>
<tr>
<td><strong>CMD</strong></td>
<td>컨테이너 실행 시 기본으로 실행할 명령어를 설정.</td>
</tr>
</tbody></table>
<h3 id="docker-composeyml">docker-compose.yml</h3>
<p><strong>Docker-compose</strong>는 여러 컨테이너를 정의하고실행하기 위한 <strong>YML</strong> 파일이다. 각 <strong>Container</strong>를 정의하고 <code>Network, Volume, envirnoment</code>등을 관리한다. </p>
<pre><code class="language-yaml"># Docker Compose version
version: '3.8'
# 서로 연결된 Container 정의 / app, postgres, mongo, redis
services:
    app:
        build:
            # 현재 directory에서 Dockerfile 찾고 이를 기반으로 이미지 빌드
            context: .
            # dockerfile이 Dockerfile라고 명시
            dockerfile: Dockerfile
        ports:
            # 로컬 포트 8080이 컨테이너 포트 8080으로 연결
            - '8080:8080'
        env_file:
            # 환경변수는 .env.development 파일
            - .env.development
        depends_on:
            # compose는 3개의 DB를 먼저 실행하지만, 실제 준비 상태는 확인되지 않으므로
            # app 내에서 자체적으로 연결을 재시도해야한다.
            - postgres
            - mongo
            - redis
        networks:
            # container가 my_network라는 네트워크를 사용하도록 지정
            - my_network

    postgres:
        image: postgres:14
        # docker hub에서 postgres:14를 다운로드하여 사용
        environment:
            # .env.development에 존재하는 환경변수 사용
            POSTGRES_USER: ${POSTGRE_DB_USERNAME}
            POSTGRES_PASSWORD: ${POSTGRE_DB_PASSWORD}
            POSTGRES_DB: ${POSTGRE_DB_DATABASE}
        ports:
            # 로컬 포트 5432이 컨테이너 포트 5432으로 연결
            - '5432:5432'
        volumes:
            # postgres-data라는 이름의 volume을 사용하여 postgreSQL 데이터를
            # 해당 directory에 영구저장
            # container가 문제가 발생하더라도 데이터는 그대로 유지 가능
            - postgres-data:/var/lib/postgresql/data
        networks:
            # my_network를 통해 app과 연결
            - my_network

    mongo:
        image: mongo:5.0
        # docker hub에서 mongo:5를 다운로드하여 사용
        ports:
            # 로컬 포트 27017이 컨테이너 포트 27017으로 연결
            - '27017:27017'
        volumes:
            # mongo-data라는 이름의 볼륨을 사용하여 데이터를 /data/db 경로에 영구 저장
            - mongo-data:/data/db
        networks:
            # my_network를 통해 app과 연결
            - my_network

    redis:
        image: redis:6-alpine
        # docker hub에서 redis:6-alpine를 다운로드하여 사용
        ports:
            # 로컬 포트 6379이 컨테이너 내부 Redis 기본 포트 6379과 연결
            - '6379:6379'
        networks:
            # my_network를 통해 app과 연결
            - my_network

volumes:
    # postgres-data와 mongo-data는 Docker 볼륨으로, PostgreSQL과 MongoDB 데이터를 영구 저장한다
    postgres-data:
    mongo-data:

networks:
    # my_network는 각 컨테이너가 서로 통신할 수 있도록 설정된 네트워크입니다.
    my_network:</code></pre>
<h3 id="docker-container-생성">Docker Container 생성</h3>
<pre><code class="language-json">{
  &quot;script&quot; : {
      &quot;docker&quot;: &quot;docker-compose -f docker-compose.dev.yml --env-file .env.development down &amp;&amp; docker-compose -f docker-compose.dev.yml --env-file .env.development up --build&quot;
  } 
}
</code></pre>
<p>필자 같은 경우 계속 docker container에 local 코드들을 올릴 것 같아서 <strong>script</strong> 태그를 만들었다. 먼저 <strong>docker-compose</strong>를 이용하여 기존에 존재하는 컨테이너 전부 중지하고 이후 다시 현재 내 <strong>Dockerfile</strong>을 기반으로 다시 docker서비스를 실행하는 script를 만들었다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/7e4eb906-4aa5-4077-a091-7ccaa99089da/image.png" /></p>
<p>실행되면 다음과 같은 <strong>개발자스러운</strong> 게 보일텐데 전혀 당황하지 않아도 된다. 그저 <strong>Docker Compose</strong>가 잘 실행되었다는 것을 의미한다. 만약 중간에 에러가 발생하면 참말로 유감이다. 구글링해서 찾는 것을 추천한다. 사실 모든 로그에는 정답이 적혀있다. 로그를 보면서 에러를 잡는 것을 추천한다. </p>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/3a15ab43-92ec-41dd-a291-73f7c7e205ab/image.png" /></p>
<p>최종적으로 <strong>Nest application successfully started</strong>라는 말이 나오면 정상적으로 docker container들이 문제없이 생성되었음을 알 수 있다. </p>
<blockquote>
<p><strong>Docker Desktop</strong>에서 보는 <strong>Nest application successfully started</strong>
<img alt="" src="https://velog.velcdn.com/images/rjs8833/post/c815b367-a005-4457-9e9e-4dbf22b07417/image.png" /></p>
</blockquote>
<h1 id="aws">AWS</h1>
<h2 id="ec2elastic-compute-cloud">EC2(Elastic Compute Cloud)</h2>
<p><strong>AWS EC2(Amazon Elastic Compute Cloud)</strong>는 <strong>Amazon Web Services(AWS)</strong>에서 제공하는 클라우드 기반 가상 서버 서비스이다. 사용자는 물리적 서버 없이도 애플리케이션을 실행할 수 있는 확장 가능하고 유연한 컴퓨팅 자원을 사용할 수 있다. <strong>EC2</strong>는 컴퓨팅 자원을 자유롭게 확장 또는 축소가 가능하고, 다양한 <strong>인스턴스(t2 / c5 / r5)</strong>가 존재하며, 사용자가 원하는 운영체제를 사용할 수 있게 해주는 <strong>가상 컴퓨터</strong>이다. 주로 <code>웹 어플리케이션 배포, 데이터베이스 호스팅, 벡엔드 서버 운영, 개발 및 테스트 환경</code>에서 주로 사용한다. <strong>EC2</strong> 인스턴스의 생명주기로 <strong>Pending(인스턴스 생성 중) / Running(인스턴스 실행 중) / Stopped(인스턴스가 중지되었지만 스토리지가 유지되므로 요금 발생) / Terminated(인스턴스가 삭제되었으며 복구 불가)</strong>가 있다.</p>
<blockquote>
<p>** EC2 기본 구성 요소**</p>
</blockquote>
<ul>
<li><strong>AMI(Amazon Machine Image)</strong> : <strong>EC2</strong> 인스턴스를 생성하기 위한 운영체제 및 소프트웨어 템플릿</li>
<li><strong>인스턴스 유형</strong> : <strong>EC2</strong>의 하드웨어 성능(CPU, Memory, Network)을 결정 <strong>ex) t2.micro / m5.large</strong></li>
<li><strong>key Pair</strong> : <strong>SSH</strong>를 통해 <strong>EC2 Instance</strong>에 접속하기 위한 보안키이며 보안키는 <code>Public Key / Private Key</code> 로 나뉜다.</li>
<li><strong>스토리지</strong><ul>
<li><strong>EBS(Elastic Block Store)</strong> : 인스턴스와 연결된 영구 스토리지로 데이터가 삭제되지 않는다.</li>
<li><strong>인스턴스 스토어</strong> : 임시 스토리지로 고속 I/O 작업에 적합하다.</li>
</ul>
</li>
<li><strong>보안 그룹</strong> : <strong>EC2 인스턴스</strong>에 대한 네트워크 방화벽 규칙. <strong>22번 포트(SSH) 허용, 80번 포트(HTTP) 허용</strong></li>
<li><strong>Elastic IP</strong>: <strong>고정된 퍼블릭 IP 주소</strong>를 EC2 인스턴스에 할당하여 외부에서 접근 가능하다.</li>
</ul>
<h3 id="aws-ec2-instance-생성">AWS EC2 Instance 생성</h3>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/561928be-57d4-47de-9ab3-09e26a4195fa/image.png" /></p>
<p>만약 자기가 <strong>Ubuntu</strong>에 익숙하다면 우분투를 사용하는 것을 추천한다.. 필자는.. 리눅스.. 나부랭이.. 그리구 !! 설명에 보시다시피 <strong>Amazon Linux 2023는 5년의 장기 지원을 제공하는 최신 범용 Linux 기반 OS입니다. AWS에 최적화되어 있으며 클라우드 애플리케이션을 개발 및 실행할 수 있는 안전하고 안정적인 고성능 실행 환경을 제공하도록 설계되었습니다.</strong> 라고 적혀있다.!!! </p>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/d381d1a5-513d-4c9b-b1a2-818e70ed4921/image.png" />
인스턴스 같은 경우.. 난 ㄴ돈이 없다.. <strong>프리티어 감사합니다.</strong> <strong>t2.micro</strong>쓰는 게 제일 Best practice일 것이다. </p>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/3b458e11-2ffd-4b4e-94bb-98e598502c03/image.png" /></p>
<p>키 페어 부분은 만약 어떤 사람이 해당 <strong>instance</strong>를 접근할 때 사용하는 키로써</p>
<pre><code class="language-bash">ssh -i &quot;your-key.pem&quot; ec2-user@your-ec2-ip</code></pre>
<p>를 통해 터미널에서 접근할 수 있게 해준다.</p>
<h2 id="elastic-ip">Elastic IP</h2>
<p><strong>Elastic IP</strong>는 AWS에서 제공하는 <strong>고정 퍼블릭 IPv4 주소</strong>다. 일반적으로 클라우드 환경에서는 EC2 인스턴스를 중지하거나 다시 시작할 때 퍼블릭 IP가 변경될 수 있지만, <strong>Elastic IP</strong>를 사용하면 인스턴스가 중지되더라도 동일한 IP 주소를 유지할 수 있다. 인스턴스를 재실행하는 경우가 많기 때문에 편리성을 위해 <strong>Elastic IP</strong>를 사용하는 게 일반적이다. </p>
<p>사실 <strong>Elastic IP</strong> 설정하는 건 어렵지 않다. 그저 <strong>탄력적 IP 주소 할당</strong>하고 작업에서 해당 <strong>instance</strong>를 연결해주면 끝이다. 연결되었다면 <strong>EC2</strong> 인스턴스에 들어가 확인하며 <strong>Elastic IP</strong>가 연결된 걸 확인할 수 있다. 
<img alt="" src="https://velog.velcdn.com/images/rjs8833/post/de60a20f-c486-4951-be54-3664745598c7/image.jpg" /></p>
<h2 id="rdsrelational-database-service">RDS(Relational Database Service)</h2>
<p>RDS는 AWS의 <strong>Managed Service(관리형 서비스)</strong>로, 관계형 데이터베이스를 쉽게 생성, 운영, 확장할 수 있도록 지원한다. 데이터 베이스 설정, 관리 확정 및 유지보수를 AWS가 자동으로 처리하고, 인프라를 운영하는 대신 애플리케이션 개발에 집중할 수 있으며 여러 <strong>RDBMS</strong>를 지원한다.</p>
<blockquote>
<p>** RDS의 주요 기능**</p>
</blockquote>
<ul>
<li><strong>자동화</strong> : <strong>RDS</strong>는 자동으로 데이터베이스 백업을 수행하여 복구 가능성을 제공한다. <strong>Point-In-Time Recovery(시점 복구)</strong>를 지원한다. 또한 AWS가 데이터베이스 엔진 소프트웨어의 보안 패치와 업데이트를 자동으로 수행한다.<blockquote>
</blockquote>
</li>
<li><strong>고가용성 및 장애 복구</strong> : 데이터베이스를 <strong>다중 가용 영역(AZ - availity zone)</strong> 복제하여, 장애 발생 시 자동으로 대체 노드로 전환하며 애플리케이션의 가동 중단을 최소화한다. 또한 읽기 작업 부하를 분산시키기 위해 읽기 복제본(Read Replica)을 생성한다. 읽기 전용 쿼리를 처리하여 주 데이터베이스의 부담 감소한다.<blockquote>
</blockquote>
</li>
<li><strong>보안</strong> : RDS는 AWS VPC 내에서 실행되며, 네트워크 수준의 격리를 제공하며 <strong>AWS KMS(Key Management Service)</strong>를 사용하여 데이터를 암호화시킨다. 또한 데이터베이스의 저장 데이터와 백업도 암호화 가능하다. <strong>AWS IAM을</strong> 통해 애플리케이션과 사용자에 대한 인증 및 권한 관리를 제공한다.<blockquote>
</blockquote>
</li>
<li><strong>확장성</strong> : 필요에 따라 데이터베이스의 크기와 성능을 수직 확장(Scale Up) 가능하다. <strong>자동 스토리지 확장(Auto Scaling)</strong>을 통해 디스크 용량이 부족하지 않도록 조정한다.</li>
</ul>
<p><strong>RDS</strong>는 일반적으로 <strong>VPC</strong> 내에 동작하며 <code>DB Instance / Multi-AZ / Read Replica / VPC 내 네트워크</code> 로 이루어져 있다.</p>
<h2 id="vpcvirtual-private-cloud">VPC(Virtual Private Cloud)</h2>
<p><strong>VPC</strong>는 <strong>AWS Cloud</strong> 내에 사용자가 정의한 가상 네트워크를 생성하여 리소스를 배포할 수 잇게 해주는 서비스이다. AWS 리소스<strong>(예: EC2, RDS, Lambda 등)</strong>를 배치할 네트워크 환경을 직접 설계하고 제어할 수 있다. <strong>서브넷, 라우팅 테이블, 게이트웨이</strong>와 같은 네트워크 설정을 구성할 수 있으며, <strong>public network(인터넷)와 private network</strong> 혼합하여 사용할 수 있다.</p>
<blockquote>
<p><strong>VPC의 주요 구성요소</strong></p>
</blockquote>
<ul>
<li><strong>서브넷(Subnet)</strong> : <strong>VPC</strong> 내부의 논리적 구역이다. <strong>IP</strong> 범위를 분할하여 퍼블릿 서브넷과 프라이빗 서브넷을 생성한다. 퍼블릭 서브넷은 인터넷 게이트웨이를 통해 외부에서 접근 가능하게 해주며 프라이빗 서브넷은 내부 네트워크에서만 접근 가능하게 한다. <blockquote>
</blockquote>
</li>
<li><strong>라우팅 테이블(Route Table)</strong> : 트래픽의 방향을 제어하는 역할을 한다. 서브넷 내 리소스가 어디로 데이터를 보낼 지 정의한다.<blockquote>
</blockquote>
</li>
<li><strong>인터넷 게이트웨이(Internet Gateway, IGW)</strong> : <strong>VPC</strong>와 언터넷을 연결하는 게이트웨이로써 퍼블릿 서브넷의 리소스가 인터넷에 접근할 수 있도록 지원해준다.<blockquote>
</blockquote>
</li>
<li><strong>NAT Gateway / Instance</strong> : 프라이빗 서브넷에서 인터넷으로 나가는 트래픽을 허용하는 역할이다. 외부에서 프라이빗 서브넷의 리소스에 접근하지 못하도록 한다.<blockquote>
</blockquote>
</li>
<li><strong>보안 그룹(Security Group)</strong> : <strong>인스턴스 수준의 방화벽</strong> 역할을 하며 리소스에 대한 인바운드 및 아웃바운트 트래픽을 제어한다.<blockquote>
</blockquote>
</li>
<li><strong>Network ACL</strong> : 서브넷 수준의 방화벽 역할을 하며 서브넷 내 모든 리소스에 대한 인바운드 및 아웃바운드 트래픽을 제어한다. </li>
</ul>
<h4 id="기본적인-vpc-구성">기본적인 VPC 구성</h4>
<pre><code class="language-plaintext">[VPC: 10.0.0.0/16]
    ├── 퍼블릭 서브넷: 10.0.1.0/24
    │      ├── EC2 인스턴스 (웹 서버)
    │      ├── ALB (로드 밸런서)
    │      └── 인터넷 게이트웨이 (IGW)
    └── 프라이빗 서브넷: 10.0.2.0/24
           ├── RDS (데이터베이스)
           ├── Lambda
           └── NAT 게이트웨이</code></pre>
<h2 id="route53">Route53</h2>
<p>아마존에서 제공하는 <strong>DNS(Domain Name System)</strong> 서비스다. 구입한 도메인을 AWS의 각 서비스들에 연결할 수 있도록 해주며, 도메인 구입 및 관리도 지원한다. 단, 도메인 구입은 국내 호스팅 업체보다는 가격이 높으며 일부 도메인(.kr, .shop 등)은 지원하지 않는다. 도메인을 개별적으로 <strong>EC2나 S3</strong>에 연결할 때 반드시 Route 53을 이용할 필요가 없다. Route 53은 DNS 레코드를 관리하여 도메인 이름을 IP 주소와 매핑한다. 또한 <strong>Health Check</strong>를 통해 서비스 상태를 모니터링 하고비정상 상태인 엔드포인트로의 트래픽을 차단한다. 주로 <strong>EC2의 인스턴스, S3 버킷, CloudFront, Load Balancer</strong>와 연결하여 웹사이트 호스팅을 한다.</p>
<blockquote>
<p><strong>Route53 주요기능</strong></p>
</blockquote>
<ol>
<li><strong>도메인 이름 등록</strong>
Route 53에서 직접 도메인을 등록할 수 있으며, 등록된 도메인의 <strong>네임서버(NS)와 DNS 설정</strong>을 자동으로 관리한다.<blockquote>
</blockquote>
</li>
<li><strong>DNS 관리</strong>
Route 53은 DNS 관리의 기본인 다양한 레코드 타입을 지원한다.</li>
</ol>
<ul>
<li><strong>A 레코드</strong>: 도메인 이름을 IPv4 주소로 매핑.</li>
<li><strong>AAAA 레코드</strong> : 도메인 이름을 IPv6 주소로 매핑.</li>
<li><strong>CNAME 레코드</strong> : 도메인 이름을 다른 도메인 이름으로 매핑.</li>
<li><strong>MX 레코드</strong> : 이메일 서버를 지정.</li>
<li><strong>TXT 레코드</strong> : 인증 정보 또는 설명 추가.</li>
</ul>
<ol start="3">
<li><strong>트래픽 라우팅</strong>
Route 53은 트래픽을 분산하고 최적화된 엔드포인트로 라우팅하기 위한 다양한 라우팅 정책을 제공한다.</li>
</ol>
<ul>
<li><strong>단순 라우팅</strong> : 도메인을 단일 IP 주소나 엔드포인트로 매핑.</li>
<li><strong>가중치 기반 라우팅</strong> : 여러 엔드포인트 간 트래픽 비율을 설정.</li>
<li><strong>지연 시간 기반 라우팅</strong> : 사용자가 가장 적은 지연 시간으로 접속할 수 있는 엔드포인트로 라우팅.</li>
<li><strong>지리적 위치 라우팅</strong> : 사용자의 위치에 따라 특정 리전의 리소스로 라우팅.</li>
<li><strong>장애 복구 라우팅</strong> : 헬스 체크와 결합하여 비정상적인 엔드포인트로의 트래픽 차단.</li>
</ul>
<ol start="4">
<li><strong>헬스 체크 및 모니터링</strong>
Route 53은 헬스 체크를 통해 각 엔드포인트의 상태를 확인한다. 헬스 체크에 실패한 엔드포인트로의 트래픽은 차단되며, 대체 엔드포인트로 라우팅된다.<blockquote>
</blockquote>
</li>
<li><strong>클라우드 통합</strong>
Route 53은 AWS의 다른 서비스(EC2, S3, ELB 등)와 원활하게 통합된다. 예: Elastic Load Balancer(ALB 또는 NLB)의 DNS 이름을 Route 53의 A 레코드로 설정.</li>
</ol>
<p><strong>route53</strong>에서 도메인을 구매하는 것도 한 가지 방법이겠지만, 사실.. 너무 비싸다.. <strong>가비아</strong>에 비해.. 가비아는 1000원이면 사는 것들을.. </p>
<h2 id="albamazon-load-balancer">ALB(Amazon Load Balancer)</h2>
<p>AWS의 <strong>Application Load Balancer(ALB)</strong>는 HTTP(S) 및 webSocket 트래픽의 <strong>Load Balancer</strong>을 제공하는 서비스다. <strong>HTTP 기반 애플리케이션</strong>을 운영하는 데 최적화된 로드 밸런서로, 트래픽을 효율적으로 분산시키고 고가용성을 보장한다. 주로 <strong>애플리케이션 계층(Layer 7)</strong>에 동작한다. 트래픽을 <strong>여러 서브넷(하나의 네트워크를 필요한 크기만큼 분리)</strong>으로 분산 처리하여 EC2가 트래픽을 안정적으로 처리할 수 있도록 하는 부하 분산 서비스다. 로드밸런서와 오토스케일링이 설정되지 않은 상태에서 인스턴스의 처리 한계를 넘을 경우 서비스 제공이 지연되거나 중단될 수 있기 때문에 대규모 서비스 구축 시 1차 대비책으로 로드밸런서를, 2차 대비책으로 오토스케일링을 설정하게 된다.</p>
<blockquote>
<p><strong>Amazon Load Balancer</strong> 동작 방식</p>
</blockquote>
<ol>
<li><strong>클라이언트 트래픽 수신</strong>
ALB는 클라이언트로부터 HTTP 또는 HTTPS 요청을 수신한다.<blockquote>
</blockquote>
</li>
<li><strong>리스너(Listeners) 구성</strong>
리스너는 ALB가 트래픽을 처리하는 데 사용하는 규칙 집합이다. 리스너는 <strong>포트(예: 80, 443)와 프로토콜(HTTP/HTTPS)</strong>을 기반으로 설정된다.<blockquote>
</blockquote>
</li>
<li><strong>라우팅 규칙 평가</strong>
ALB는 설정된 규칙에 따라 트래픽을 적절한 대상 그룹(Target Group)으로 라우팅한다. 각 규칙은 조건(URL 경로, 호스트 헤더 등)과 작업(Target Group으로의 전달 등)으로 구성된다.<blockquote>
</blockquote>
</li>
<li><strong>타겟 그룹으로 트래픽 전달</strong>
타겟 그룹은 트래픽을 처리할 EC2 인스턴스, 컨테이너, 또는 Lambda 함수를 포함한다. <strong>헬스 체크(Health Check)</strong>를 기반으로 트래픽을 정상적인 대상에만 전달한다.<blockquote>
</blockquote>
</li>
<li><strong>응답 반환</strong>
대상에서 생성된 응답이 다시 ALB를 통해 클라이언트에 반환된다.</li>
</ol>
<p><strong>Amazon Load Balancer</strong> 구성 시 가장 많이 에러 뜨는 건 <strong>Health Check</strong>이다 먼저 Health Check란 <strong>ALB가</strong> 백엔드 타겟 그룹<strong>(EC2, ECS, Lambda 등)</strong>의 상태를 주기적으로 점검하여 트래픽이 정상적으로 처리될 수 있는지 확인하는 메커니즘이다. 타겟이 문제가 없으면 <strong>Healthy</strong> / 비정상이면 <strong>Unhealthy</strong> / 초기상태는 <strong>Initial</strong>이다. <strong>Unhealthy</strong>면 <strong>load balancer</strong>는 자동으로 해당 타켓에 요청을 보내지 않는다. </p>
<h2 id="acmamazon-certification-manager">ACM(Amazon Certification Manager)</h2>
<p>HTTPS 연결을 위한 <strong>SSL 인증서</strong>를 발급하고 관리하도록 해 주는 서비스다. 퍼블릭 인증서는 무료로 발급이 가능하며, 무분별한 인증서 발급을 방지하기 위해 인증서 발급 후 도메인 소유권을 검증해야 인증서가 활성화된다.</p>
<blockquote>
<p><strong>ACM의 주요 특징</strong></p>
</blockquote>
<ul>
<li><strong>자동 갱신</strong> : ACM에서 발급된 SSL 인증서는 만료 전에 자동으로 갱신되므로, 수동 갱신에 대한 번거로움을 없앤다. 인증서 갱신이 누락되어 서비스가 중단될 위험을 방지한다.<blockquote>
</blockquote>
</li>
<li><strong>AWS 서비스와 통합</strong> : ACM 인증서는 AWS의 다양한 서비스(예: ELB, CloudFront, API Gateway)와 손쉽게 통합된다. AWS 리소스와의 통합을 통해 HTTPS 트래픽을 간단히 설정할 수 있다.<blockquote>
</blockquote>
</li>
<li><strong>무료 퍼블릭 인증서</strong> : ACM을 통해 발급된 퍼블릭 인증서는 비용 없이 제공된다. 단, 이러한 인증서는 AWS 리소스 내에서만 사용할 수 있다.<blockquote>
</blockquote>
</li>
<li><strong>도메인 소유권 검증</strong> : 인증서를 활성화하기 위해 DNS 레코드 또는 이메일 기반으로 도메인 소유권을 검증한다. 검증이 완료되면 인증서가 활성화되어 HTTPS 통신이 가능하다.</li>
</ul>