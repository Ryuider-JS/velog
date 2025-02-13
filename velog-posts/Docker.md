<h2 id="docker란">Docker란?</h2>
<p><strong>Docker</strong>는 소프트웨어 개발과 배포를 위한 컨테이너 기반의 오픈소스 플랫폼이다. <strong>Docker</strong>를 사용하면 애플리케이션과 그 실행에 필요한 모든 의존성을 하나의 패키지로 묶어 일관된 환경에서 실행할 수 있다. 이를 통해 환경 간 불일치 문제를 해결하고, 애플리케이션의 배포, 확장, 관리를 크게 단순화할 수 있다.</p>
<h3 id="docker-핵심-개념">Docker 핵심 개념</h3>
<h4 id="container">Container</h4>
<p><strong>container</strong>는 애플리케이션과 해당 애플리케이션이 실행되는 데 필요한 모든 것을 포함한 <strong>경량화된 독립 실행 환경</strong>이다. 동일한 컨테이너 이미지를 사용하면 <strong>어떤 환경에서도 동일한 동작을 보장</strong>할 수 있다. 컨테이너는 <strong>가상 머신(Virtual Mechine)</strong>과 달리 <strong>Host OS</strong>의 커널을 공유하므로 <strong>빠르고 가볍다.</strong></p>
<h4 id="image">Image</h4>
<p><strong>container</strong>를 생성하기 위한 불변의 템플릿으로써, 소스코드, 라이브러리, 실행 파일, 환경 설정 등을 포함되어 있다. 애플리케이션의 배포 단위로 사용되며 컨테이너는 이미지를 실행한 상태다. 쉽게 생각하자면 <strong>Container</strong>는 <strong>class의 instance</strong>이고, <strong>Image</strong>는 <strong>class</strong>라고 생각하면 편하다.</p>
<h4 id="dockerfile">DockerFile</h4>
<p>이미지를 생성하기 위한 스크립트 파일으로써, 이미지 빌드에 필요한 명령어들을 정의하며 애플리케이션 환경을 코드화하여 재현성을 높인다.</p>
<pre><code class="language-yml"># 베이스 이미지
FROM node:18-alpine

# 작업 디렉토리 설정
WORKDIR /app

# 패키지 설치
COPY package*.json ./
RUN npm install

# 애플리케이션 소스 복사
COPY . .

# NestJS 빌드
RUN npm run build

# 애플리케이션 실행
CMD [&quot;node&quot;, &quot;dist/src/main.js&quot;]</code></pre>
<blockquote>
<p><strong>Docker 주요 특징</strong></p>
</blockquote>
<ul>
<li><strong>이식성(Protability)</strong> : docker container는 어떤 환경에서도 동일하게 작동한다.</li>
<li><strong>경량성(Lightweight)</strong> : container는 Virtual Mechine보다 리소스를 적게 사용하며 부팅 시간이 빠르다.</li>
<li><strong>확장성(scalability)</strong> : Docker는 애플리케이션을 빠르게 확장하고 배포할 수 있다. <strong>Kubernetes</strong>와 같은 오케스트레이션 도구와 결합하면 확장 관리가 더욱 쉬워진다.</li>
<li><strong>격리성(isolation)</strong> : 각 container는 호스트와 격리된 환경에서 실행된다. 이를 통해 의존성 충돌을 방지하고 보안성을 강화한다.</li>
</ul>
<h3 id="docker를-사용하는-이유">Docker를 사용하는 이유</h3>
<p><strong>Docker</strong>를 사용하는 이유는 주로 <code>개발, 배포, 확장 과정에서의 효율성, 일관성, 확장성</code> 때문이다. <strong>Docker</strong>는 컨테이너 기술을 활용하여 소프트웨어 개발과 배포를 단순화하고, 환경 간의 차이로 인한 문제를 해결한다.</p>
<p>다양한 OS에서 수많은 라이브러리를 설치하게 되면 각 OS별로 setting 환경이 달라진다. 예를 들면 window에서는 되지만 mac os나 linux에서는 되지 않는 경우이다. docker가 나오기 전 이 문제를 해결하기 위해 나온 개념이 <strong>Virtual Machine</strong>이다. 이렇게 되면 같은 os에서 개발을 진행하기 때문에 모든 게 동일하게 동작한다. </p>
<p>하지만 <strong>Virtual Machine</strong> 문제가 있는데, 쉽게 생각하면 컴퓨터 내에 컴퓨터를 설치하게 되면 그만큼 cpu을 더 많이 사용하기 때문에 컴퓨터 성능에 영향을 미친다. 이를 해결하기 위해 <strong>Docker</strong>라는 게 나왔다. </p>
<p><strong>docker</strong>와 <strong>virtual machine</strong>의 차이는 커널이다. <strong>virtual machine</strong> 같은 경우 <strong>hypervisor</strong>를 통해 <strong>guest OS</strong>를 실행하기 때문에 <strong>docker</strong> 보다 무겁고 자원을 더 많이 소비한다. 반면 <strong>docker</strong>는 애플리케이션과 해당 애플리케이션이 필요로 하는 라이브러리 및 종속성을 포함하지만 <strong>Host의 OS</strong>을 공유하기 때문에 훨씬 더 가볍다. </p>
<h3 id="docker-compose">Docker-Compose</h3>
<p><strong>Docker-Compose</strong>란 여러 개의 Docker 컨테이너를 정의하고 실행할 수 있는 도구로, 복잡한 애플리케이션의 환경을 쉽게 구성하고 관리할 수 있도록 도와준다. <code>컨테이너 간의 네트워크 연결, 볼륨 공유, 종속성</code> 등을 설정하는 데 유용하며, 특히 <strong>마이크로서비스 아키텍처</strong>와 같은 다중 컨테이너 환경에서 자주 사용된다.</p>
<h3 id="docker-compose-주요-개념">Docker-Compose 주요 개념</h3>
<h4 id="docker-composeyml">docker-compose.yml</h4>
<p><strong>Docker Compose</strong>는 애플리케이션 환경을 정의하는 <strong>구성 파일(docker-compose.yml)</strong>을 사용한다. 이 파일 내에는 <strong>Service / Network / Volume</strong> 등을 정의할 수 있다. </p>
<pre><code class="language-yml">version: '3.8'
services:
    app:
        build:
            context: .
            dockerfile: Dockerfile
        ports:
            - '8080:8080'
        env_file:
            - .env.production
        depends_on:
            - postgres
            - mongo
            - redis
            - elasticsearch
            - logstash
            - kibana
        networks:
            - my_network

    postgres:
        image: postgres:14
        environment:
            POSTGRES_USER: ${POSTGRE_DB_USERNAME}
            POSTGRES_PASSWORD: ${POSTGRE_DB_PASSWORD}
            POSTGRES_DB: ${POSTGRE_DB_DATABASE}
        networks:
            - my_network

    mongo:
        image: mongo:5.0
        networks:
            - my_network

    redis:
        image: redis:6-alpine
        networks:
            - my_network

    elasticsearch:
        image: docker.elastic.co/elasticsearch/elasticsearch:7.17.4
        environment:
            - discovery.type=single-node
        networks:
            - my_network

    logstash:
        image: docker.elastic.co/logstash/logstash:7.17.4
        volumes:
            - ./logstash.conf:/usr/share/logstash/pipeline/logstash.conf
        depends_on:
            - elasticsearch
        networks:
            - my_network

    kibana:
        image: docker.elastic.co/kibana/kibana:7.17.4
        environment:
            - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
        depends_on:
            - elasticsearch
        networks:
            - my_network

volumes:
    postgres-data:
    mongo-data:
    es-data:

networks:
    my_network:
</code></pre>
<h4 id="service">Service</h4>
<p><strong>Service</strong>는 특정 역할을 수행하는 컨테이너를 의미한다. 주로 웹서버인 <strong>nginx</strong>, 데이터베이스인 <strong>mysql, mongodb</strong>, 캐시 서버인 <strong>redis</strong> 등을 각 서비스로 정의하여 고유한 컨테이너를 정의하는 데 사용된다. </p>
<h4 id="volume">Volume</h4>
<p><strong>container와 host</strong>간의 데이터 공유을 위해 사용된다. container를 재실행해도 데이터베이스의 데이터가 사라지지 않고 유지되도록 저장하는 역할을 한다.</p>
<h4 id="network">Network</h4>
<p>container간의 통신을 정의하는데, 기본적으로 <strong>docker-compose</strong>는 container 간의 네트워크를 자동 생성한다.</p>
<blockquote>
<p><strong>Docker Compose 기능</strong></p>
</blockquote>
<ul>
<li><strong>다중 컨테이너 환경 관리</strong> : 여러 서비스를 포함한 애플리케이션을 단일 명령으로 실행할 수 있다. 서비스 간의 의존성을 정의하여 실행 순서를 제어한다..</li>
<li><strong>환경 구성 자동화</strong> : <code>docker-compose.yml 파일</code>에 환경 설정을 정의하면, 동일한 환경을 쉽게 재현 가능하다.</li>
<li><strong>로컬 개발 환경 구축</strong> : <code>데이터베이스, 캐시, 웹 서버</code> 등 다양한 서비스를 한 번에 실행하여 로컬에서 테스트 가능하다.</li>
<li><strong>명령어 단순화</strong> : <strong>Docker Compose</strong>를 사용하면 여러 컨테이너를 단일 명령으로 실행, 중지, 관리 가능하다.</li>
<li><em>예: docker-compose up, docker-compose down.*</em></li>
</ul>
<h3 id="docker를-이용하여-next-docker에-올려보기">docker를 이용하여 next docker에 올려보기</h3>
<h4 id="dockerfile-1">Dockerfile</h4>
<pre><code class="language-docker"># 1. 운영체제 및 프로그램 설치
# FROM ubuntu:22.04

# 2. ubuntu에서 node(npm)와 yarn 설치
# RUN sudo apt install nodejs
# RUN sudo npm install -g yarn

# 1 &amp; 2. os와 program 설치 통합된 게 존재
FROM node:18

# 3. git clone해서 docker에 source code 다운로드
# RUN git clone

# git clone이 아닌 copy 명령어를 통해 생성
# RUN git clone ~~
COPY . /test/
WORKDIR /test/

# 4. docker에 실행하기
RUN yarn install
RUN yarn build
# docker build를 통해 이미지가 생성된다.
# docker images를 통해 이미지를 체크한다.
# docker run을 통해 cmd가 실행된다.
CMD yarn start</code></pre>
<h4 id="docker-composeyaml">docker-compose.yaml</h4>
<pre><code class="language-yml"># @format

version: '3.7'
services:
    test:
        build:
            context: .
            dockerfile: Dockerfile
        ports:
            - '3000:3000'</code></pre>
<p><code>docker-compose build</code>를 하게 되면, <strong>Dockerfile</strong>에 따라 <strong>Docker image</strong>를 빌드한다. 이후 docker image가 생성되었는 지 확인하려면 <code>docker images</code>를 확인한다.</p>
<p><code>docker-compose up</code>을 하게 되면 docker image를 통해 container를 생성 및 실행하게 된다. 즉 <strong>Dockerfile</strong>에 존재하는 CMD가 실행된다는 소리다. </p>
<p>이후 명령어를 통해 docker container가 생성 및 실행되었는 지 확인할 수 있는데, <code>docker ps</code>를 통해 container가 제대로 실행되었는 지 확인할 수 있다. 이후 log를 확인하고 싶으면 <code>docker-compose logs</code>를 통해 로그를 확인할 수 있다. <code>docker-compose up -d</code>를 하게 되면 계속 실행된다. </p>
<p>현재 container를 중단하고 싶다면 <code>docker-compose stop</code>을 통해 중지한다. 이는 컨테이너는 삭제되지 않고 보존된다. <code>docker-compose start</code>를 하게 되면 재실행할 수 있다.   하지만 <code>docker-compse down</code> 은 container 자체를 중단하고 삭제한다. 다시 container를 생성하려면 <code>docker-compose up</code>하면 된다. </p>
<h2 id="docker와-docker-compose-명령어-정리">Docker와 Docker Compose 명령어 정리</h2>
<h3 id="docker-명령어">Docker 명령어</h3>
<table>
<thead>
<tr>
<th>명령어</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>docker build -t &lt;name&gt;</code></td>
<td>현재 디렉토리의 Dockerfile을 기반으로 Docker 이미지를 빌드합니다.</td>
</tr>
<tr>
<td><code>docker images</code></td>
<td>로컬에 저장된 Docker 이미지 리스트를 확인합니다.</td>
</tr>
<tr>
<td><code>docker run &lt;image&gt;</code></td>
<td>이미지를 기반으로 컨테이너를 생성하고 실행합니다.</td>
</tr>
<tr>
<td><code>docker ps</code></td>
<td>실행 중인 Docker 컨테이너 목록을 확인합니다.</td>
</tr>
<tr>
<td><code>docker ps -a</code></td>
<td>중지된 컨테이너를 포함한 모든 컨테이너를 확인합니다.</td>
</tr>
<tr>
<td><code>docker logs &lt;container&gt;</code></td>
<td>특정 컨테이너의 로그를 확인합니다.</td>
</tr>
<tr>
<td><code>docker stop &lt;container&gt;</code></td>
<td>실행 중인 컨테이너를 중단합니다.</td>
</tr>
<tr>
<td><code>docker start &lt;container&gt;</code></td>
<td>중단된 컨테이너를 재실행합니다.</td>
</tr>
<tr>
<td><code>docker rm &lt;container&gt;</code></td>
<td>중지된 컨테이너를 삭제합니다.</td>
</tr>
<tr>
<td><code>docker rmi &lt;image&gt;</code></td>
<td>Docker 이미지를 삭제합니다.</td>
</tr>
<tr>
<td><code>docker exec -it &lt;container&gt; &lt;command&gt;</code></td>
<td>실행 중인 컨테이너 안에서 명령어를 실행합니다. (예: bash 실행).</td>
</tr>
</tbody></table>
<hr />
<h3 id="docker-compose-명령어">Docker Compose 명령어</h3>
<table>
<thead>
<tr>
<th>명령어</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>docker-compose build</code></td>
<td><code>docker-compose.yml</code> 파일에 정의된 서비스의 이미지를 빌드합니다.</td>
</tr>
<tr>
<td><code>docker-compose up</code></td>
<td>Docker 이미지를 기반으로 컨테이너를 생성 및 실행합니다.</td>
</tr>
<tr>
<td><code>docker-compose up -d</code></td>
<td>백그라운드 모드로 컨테이너를 실행합니다. (detached 모드).</td>
</tr>
<tr>
<td><code>docker-compose down</code></td>
<td>모든 컨테이너를 중단하고 삭제합니다.</td>
</tr>
<tr>
<td><code>docker-compose stop</code></td>
<td>실행 중인 컨테이너를 중단합니다. (컨테이너는 삭제되지 않음).</td>
</tr>
<tr>
<td><code>docker-compose start</code></td>
<td>중단된 컨테이너를 재실행합니다.</td>
</tr>
<tr>
<td><code>docker-compose ps</code></td>
<td>현재 <code>docker-compose</code>로 실행 중인 컨테이너 목록을 확인합니다.</td>
</tr>
<tr>
<td><code>docker-compose logs</code></td>
<td><code>docker-compose</code>로 실행된 컨테이너의 로그를 확인합니다.</td>
</tr>
<tr>
<td><code>docker-compose logs -f</code></td>
<td>로그를 실시간 스트리밍으로 확인합니다.</td>
</tr>
<tr>
<td><code>docker-compose exec &lt;service&gt; &lt;command&gt;</code></td>
<td>실행 중인 서비스에서 명령어를 실행합니다. (예: bash 접속).</td>
</tr>
<tr>
<td><code>docker-compose restart</code></td>
<td>실행 중인 모든 컨테이너를 재시작합니다.</td>
</tr>
<tr>
<td><code>docker-compose config</code></td>
<td><code>docker-compose.yml</code> 파일의 유효성을 검증합니다.</td>
</tr>
</tbody></table>
<h3 id="자주-쓰이는-docker-및-docker-compose-명령어">자주 쓰이는 Docker 및 Docker Compose 명령어</h3>
<pre><code class="language-bash"># 이미지 빌드
docker-compose build

# 컨테이너 생성 및 실행
docker-compose up

# background 실행
docker-compose up -d

# 실행 중인 컨테이너 확인
docker ps
docker-compose ps

# 로그 확인
docker-compose logs # 과거 로그 전부
docker-compose logs -f # 로그 실시간

# 컨테이너 중단 
# 중지만 되고 컨테이너는 보존
docker-compose stop
# 중지와 삭제 동시
docker-compose down

# 중단된 컨테이너 재실행
docker-compose start

# docker 이미지 목록
docker images

# 사용하지 않는 Docker image 삭제
docker image prune -f</code></pre>
<h3 id="port-forwarding">Port Forwarding</h3>
<p><strong>Port Forwarding</strong>이란 네트워크의 특정 포트로 들어오는 데이터를 특정 내부 네트워크 장치나 포트로 전달하도록 설정하는 네트워크 기능이다. 이를 통해 <code>외부 네트워크(예: 인터넷)</code>에서 <code>내부 네트워크(예: 로컬 네트워크)</code>로의 연결을 가능하게 한다. 보통 <strong>NAT(Network Address Translation)</strong>를 사용하는 라우터에서 설정된다.</p>
<pre><code class="language-yml">version: '3.7'
services:
    test:
        build:
            context: .
            dockerfile: Dockerfile
        # 브라우저에서 들어온 port:docker에 실행한 port
        ports:
            - '3000:3000'</code></pre>
<p>다음은 <strong>docker-compose.yml</strong>이다. ports 속성을 통해 <strong>호스트 머신(외부 네트워크)</strong>의 포트와 Docker 컨테이너 내부의 포트를 연결(port mapping)한다.</p>
<h3 id="dockerfile-최적화하기">Dockerfile 최적화하기</h3>
<p><strong>dockerfile</strong>
<strong>yarn install</strong> 할 필요 없는 경우에도 계속 install을 하고 있음
이를 해결하기 위해 <strong>package.json이나 yarn.lock</strong>이 변경되지 않으면 그대로 install 실행하지 않기</p>
<pre><code class="language-yml"># 1. 운영체제 및 프로그램 설치
# FROM ubuntu:22.04

# 2. ubuntu에서 node(npm)와 yarn 설치
# RUN sudo apt install nodejs
# RUN sudo npm install -g yarn

# 1 &amp; 2. os와 program 설치 통합된 게 존재
FROM node:18

# 3. git clone해서 docker에 source code 다운로드
# RUN git clone

# git clone이 아닌 copy 명령어를 통해 생성
# RUN git clone ~~
# COPY . /test/
# WORKDIR /test/

# 이렇게 하면 문제가 뭐냐면
# docker는 최적화를 하기 위해 한 줄 씩 캐시되어 있음
# 만약 코드가 변경이 되면 즉 test가 변경이 되면 그 하위에 존재하는
# 모든 캐시는 삭제됨
# test는 코드가 계속 변경되기 때문에 
# test 하위는 재 실행된다는 얘기임
# 즉 install을 상위로 올려놓고 copy와 build를 하위로 내려 최적화를 시키자
# package.json / yarn.lock 파일이 변경 안되면 install 하지 않기!
# install 시간 줄일 수 있다.
COPY ./package.json /test/
COPY ./yarn.lock /test/
WORKDIR /test/
RUN yarn install

# 4. docker에 실행하기
COPY . /test/
RUN yarn build
# docker build를 통해 이미지가 생성된다.
# docker images를 통해 이미지를 체크한다.
# docker run을 통해 cmd가 실행된다.
CMD yarn start</code></pre>