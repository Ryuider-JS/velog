<h2 id="ormobject-relational-mapping">ORM(Object Relational Mapping)</h2>
<p><strong>ORM</strong>이란 <strong>객체(Object)-관계(Relation) 매핑(Mapping)</strong>의 약자로, <strong>객체 지향 프로그래밍(OOP - Object Oriented Programming)</strong>과 관계형 데이터베이스(<strong>RDB - Relational DataBase</strong>) 간의 데이터를 쉽게 변환하고 상호작용할 수 있도록 해주는 기술이다. 즉, 데이터베이스의 테이블을 객체(Object)로 표현하여, SQL 쿼리문을 작성하지 않고도 데이터베이스를 조작할 수 있도록 한다. <strong>ORM</strong>은 SQL 쿼리문을 작성하지 않고 객체를 조작하므로 개발 속도가 빠르며 데이터베이스 조작이 객체 지향적으로 이루어져 코드가 직관적이다.</p>
<p><strong>ORM</strong>은 SQL 쿼리문 대신 객체를 조작하므로 <strong>개발 속도가 빠르며</strong>, 데이터베이스 조작이 <strong>객체 지향적</strong>이여서 직관적이다. 하지만 복잡한 <strong>SQL 쿼리</strong>면 성능에 문제가 발생할 수 있으며 <strong>SQL 쿼리</strong>를 작성하는 것보다는 성능 손실이 발생할 수 있다.</p>
<blockquote>
<p><strong>ORM 동작 방식</strong></p>
</blockquote>
<p><strong>클래스와 테이블 매핑</strong> - ORM은 하나의 <strong>Class(Entity)</strong>가 데이터베이스의 한 <strong>Table</strong>에 대응되도록 매핑한다.
<strong>객체와 레코드 매핑</strong>  - 클래스의 인스턴스는 한 테이블의 한 행에 대응된다.
<strong>속성과 열(Column) 매핑</strong> - 클래스의 속성은 데이터베이스의 <strong>열(Column)</strong>에 대응된다. </p>
<h2 id="typeorm">TypeORM</h2>
<p><strong>TypeORM</strong>은 <code>TypeScript와 JavaScript(ES6 이상)</code> 환경에서 사용할 수 있는 <strong>ORM(Object-Relational Mapping) 라이브러리</strong>다. 관계형 데이터베이스와 객체 지향 프로그래밍의 데이터를 매핑하여 SQL 쿼리 없이 객체를 조작하는 방식으로 데이터베이스를 다룰 수 있게 해준다. <strong>TypeORM</strong>은 Node.js에서 동작하며, 다양한 데이터베이스<strong>(<code>MySQL / PostgreSQL / MariaDB / SQLite / Oracle / MongoDB / ETC</code>)</strong>를 지원하는 강력한 ORM 라이브러리다. 자체적으로 <strong>Migration</strong> 기능을 지원하며 점진적인 데이터베이스 구조와 버저닝을 모두 지원한다. 또한 <strong>Active Record와 Data Mapper</strong>를 지원하며, <strong>Eager &amp; Lazy</strong> 로딩을 지원하기 때문에 어떤 방식으로 데이터를 불러올 지 완전한 컨트롤이 가능하다. </p>
<blockquote>
<p><strong>DataSource</strong> - 사용할 데이터베이스 지정 및 정보 제공 역할</p>
</blockquote>
<pre><code class="language-typescript">import { DataSource } from &quot;typeorm&quot;
&gt;
const AppDataSource = new DataSource({
    type: &quot;mysql&quot;,
    host: &quot;localhost&quot;,
    port: 3306,
    username: &quot;test&quot;,
    password: &quot;test&quot;,
    database: &quot;test&quot;,
      entities: [
        // 여기에 Entity를 입력 
    ]
})</code></pre>
<h3 id="entity-생성하기">Entity 생성하기</h3>
<ul>
<li><strong>@Entity</strong> 데코레이터를 사용하면 클래스를 <strong>데이터베이스 테이블</strong>로 관리할 수 있다. </li>
<li><strong>@Column</strong> 데코레이터를 사용하면 <strong>데이터베이스 테이블의 칼럼</strong>을 생성할 수 있다.</li>
<li><strong>@PrimaryGeneratedColumn</strong> 데코레이터를 사용하면 <strong>자동 생성되는 ID 칼럼(AUTO_INCREMENT &amp; Primary Key)</strong>을 생성할 수 있다.</li>
</ul>
<pre><code class="language-typescript">@Entity()
export class Uesr {
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  firstName: string

  @Column()
  lastName: string

  @Column()
  isActive: boolean</code></pre>
<blockquote>
<p><strong>Column Options</strong></p>
</blockquote>
<ul>
<li><strong>type(ColumnType)</strong> - <code>varchar / text / int / bool</code> 기본적으로 typescript를 따르며, <strong>number</strong>인 경우 <code>float</code>가 default로 들어간다.</li>
<li><strong>name(string)</strong> - 데이터베이스에 저장될 <strong>column</strong> 이름. 기본값으로 <strong>property name</strong>을 사용한다. 주로, <strong>property</strong>가 <strong>camel case(userName)</strong>로 작성된 경우, 데이터베이스에선 <strong>snake case(user_name)</strong>으로 변경하기 위해 사용된다.</li>
<li><strong>nullable(boolean)</strong> - <strong>null</strong> 값이 가능한 지 여부 기본값으로는 <strong>required</strong>지만 <strong>optional</strong>이 필요한 경우 <strong>true</strong>로 변경해줘야한다.</li>
<li><strong>update(boolean)</strong> - 업데이트 가능여부 기본값이 <strong>ture</strong>지만 값이 변경되면 안되는 값인 경우 <strong>false</strong>로 설정해야한다.</li>
<li><strong>select(boolean)</strong> - 쿼리 실행 시 <strong>property</strong>를 가져올 지 결정. <strong>false</strong>일 경우 가져오지 않는다. (주로 <strong>password</strong>를 가져오지 않을 때 Column option에 <strong>select: false</strong> 속성을 넣는다.)</li>
<li><strong>default(string)</strong> - 칼럼 기본값 주로 좋아요 초기값을 넣을 때 사용한다. </li>
<li><strong>unique(boolean)</strong> - <strong>unique constraint</strong> 적용 여부 기본 <strong>false</strong> 같은 column에 동일한 값이 들어가면 안될 때 사용한다.</li>
<li><strong>comment(string)</strong> - 칼럼 코멘트 모든 데이터베이스에서 지원되지 않는다.</li>
<li><strong>enum(string[])</strong> - 칼럼에 입력 가능한 값을 <strong>enum</strong>으로 나열</li>
<li><strong>array(boolean)</strong> - 칼럼 array type으로 생성<blockquote>
</blockquote>
</li>
<li><em>Unique Column*</em></li>
<li><strong>@CreateDateColumn</strong>은 자동으로 Row 생성 날짜시간을 저장한다.</li>
<li><strong>@UpdateDateColumn</strong>은 자동으로 Row 최근 업데이트 날짜시간을 저장한다.</li>
<li><strong>@DeleteDateColumn</strong>은 자동으로 Row의 Soft Delete 날짜 시간을 저장한다. </li>
<li><strong>@VersionColumn</strong>은 자동으로 Row가 업데이트될 때마다 1씩 증가한다.</li>
</ul>
<h2 id="postgresql와-nestjs-연동하기">PostgreSQL와 NestJS 연동하기</h2>
<pre><code class="language-typescript">// app.module.ts

import { ConfigModule } from '@nestjs/config';
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { postgreSQLConfig } from './configs/postgreSQLConfig';
import { zodConfigSchema } from './configs/zodConfig.schema';

@Module({
    imports: [
        ConfigModule.forRoot({
            isGlobal: true,
            validate: (config) =&gt; zodConfigSchema.parse(config),
        }),

        TypeOrmModule.forRootAsync({
            name: 'PostgreSQL',
            useClass: postgreSQLConfig,
        }),
    ],
})
export class AppModule {}

// postgreSQLConfig.ts

import { TypeOrmModuleOptions, TypeOrmOptionsFactory } from '@nestjs/typeorm';

import { ConfigService } from '@nestjs/config';
import { Injectable } from '@nestjs/common';

@Injectable()
export class postgreSQLConfig implements TypeOrmOptionsFactory {
    constructor(private readonly configService: ConfigService) {}

    createTypeOrmOptions(): TypeOrmModuleOptions {
        return {
            type: 'postgres',
            host: this.configService.get&lt;string&gt;('POSTGRE_DB_HOST'),
            port: this.configService.get&lt;number&gt;('POSTGRE_DB_PORT'),
            username: this.configService.get&lt;string&gt;('POSTGRE_DB_USERNAME'),
            password: this.configService.get&lt;string&gt;('POSTGRE_DB_PASSWORD'),
            database: this.configService.get&lt;string&gt;('POSTGRE_DB_DATABASE'),
            synchronize: true,
            autoLoadEntities: true,
            logging: false,
        };
    }
}

// zodConfig.schema.ts

import { z } from 'zod';

export const zodConfigSchema = z.object({
    ENV: z.enum(['test', 'dev', 'prod']),
    POSTGRE_DB_TYPE: z.literal('postgres'),
    POSTGRE_DB_HOST: z.string(),
    POSTGRE_DB_PORT: z.preprocess((val) =&gt; Number(val), z.number()),
    POSTGRE_DB_USERNAME: z.string(),
    POSTGRE_DB_PASSWORD: z.string(),
    POSTGRE_DB_DATABASE: z.string(),
});
</code></pre>
<p>필자는 코드를 <strong>마구마구 분리하는 것</strong>을 좋아한다. 하나하나 쌓이다보면 결국 몇 백줄이 되는 코드를 마주할 수 있을 것이다. 개발자라면 알 것이다. <strong>파일을 분리하는 게 얼마나 가독성이 올라가고 유지보수성이 증가하는지.</strong> 필자 또한 <strong>TypeORM</strong>을 통해 <strong>postgreSQL</strong>을 연결하는 코드가 10몇줄이 되니깐 마구마구 분리하고 싶어졌다. 다행히도 예전에 TypeORM Config로 분리했던 기억이 있어서 그걸 바탕으로 이번에도 분리해 보았다. </p>
<p>먼저 <strong>app.module.ts</strong>를 보자면 별거 없다 <strong>ConfigModule</strong>에서의 validate와 forRootAsync <strong>forRootAsync가 궁금하다면</strong> <a href="https://velog.io/@rjs8833/postgreSQL%EB%9E%91-%EC%B9%9C%ED%95%9C-%EC%B2%99-%ED%95%98%EA%B8%B0">Postgresql 연결</a> 참고하시는 걸 추천한다. 짧게 요약해서 설명하자면 <strong>NestJS에서 .env 파일</strong>의 환경 변수는 ConfigModule을 통해 로드된다. 이 과정은 비동기로 동작하며, 환경 변수를 읽고 파싱하는 작업이 완료된 후에야 접근할 수 있다. <strong>TypeOrmModule.forRoot</strong>는 동기적으로 동작하기 때문에, 환경 변수 사용이 어렵다. 하지만 <strong>forRootAsync를 사용하면</strong>, 설정 값을 동기적이든 비동기적이든 동적으로 생성할 수 있다. </p>
<p>그리구 추후 <strong>MongoDB</strong>를 연결할 예정인데, <strong>postgreSQL</strong>과 구분하기 위해서 <strong>TypeORM Option</strong>으로 <strong>name: &quot;PostgreSQL&quot;</strong>을 넣었다.</p>
<blockquote>
<p><strong>Joi와 Zod</strong> 비교</p>
</blockquote>
<table>
<thead>
<tr>
<th><strong>특징</strong></th>
<th><strong>Joi</strong></th>
<th><strong>Zod</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>타입스크립트 통합</strong></td>
<td>- TypeScript를 직접 지원하지 않음.</td>
<td>- <strong>TypeScript와 통합이 우수</strong>: 타입 추론 가능.</td>
</tr>
<tr>
<td><strong>사용 방식</strong></td>
<td>- NestJS <code>ConfigModule</code>과 기본적으로 통합됨.</td>
<td>- <code>validate</code> 옵션을 통해 추가적으로 연동 필요.</td>
</tr>
<tr>
<td><strong>코드 간결성</strong></td>
<td>- 더 많은 설정 코드가 필요할 수 있음.</td>
<td>- 상대적으로 간결한 코드 작성 가능.</td>
</tr>
<tr>
<td><strong>유효성 검증 기능</strong></td>
<td>- 다양한 데이터 타입과 복잡한 검증 로직 지원.</td>
<td>- 검증 기능이 강력하지만, Joi에 비해 일부 제약이 있음.</td>
</tr>
<tr>
<td><strong>커스터마이징</strong></td>
<td>- 에러 메시지와 검증 옵션이 매우 유연하게 커스터마이징 가능.</td>
<td>- 에러 메시지 커스터마이징은 가능하나 복잡한 경우 제한적.</td>
</tr>
<tr>
<td><strong>성능</strong></td>
<td>- 검증 로직이 풍부하여 약간 무거움.</td>
<td>- 가벼운 라이브러리로 더 빠름.</td>
</tr>
<tr>
<td><strong>배우기 쉬움</strong></td>
<td>- 직관적이나 옵션이 많아 초보자에겐 복잡할 수 있음.</td>
<td>- 직관적이고 배우기 쉬움.</td>
</tr>
<tr>
<td><strong>생태계 지원</strong></td>
<td>- 오랜 기간 사용된 라이브러리로, 문서와 플러그인 풍부.</td>
<td>- 상대적으로 신생 라이브러리, 발전 중.</td>
</tr>
</tbody></table>
<p>사실 <strong>Joi</strong>를 쓰려고 했다. <strong>NestJS</strong>와 기본 통합되어 있어 별도의 작업 없이 바로 <strong>validationSchema 옵션</strong>에 연결 가능하기 때문이다. 하지만 다음 <strong>NPM Trend</strong>를 봐보자. 
<img alt="" src="https://velog.velcdn.com/images/rjs8833/post/0b39d3a9-2ade-4c9f-a070-9712695e9883/image.png" /></p>
<p>올해만 봐도 <strong>Zod</strong>가 급증하는 것을 볼 수 있다. 이는 <strong>TypeScript와의 긴밀한 통합 및 사용성의 간결함</strong> 때문이라 생각한다. <strong>Joi</strong>가 편할 진 몰라도 최대한 <strong>Zod</strong>의 익숙해지자 <strong>Zod</strong>를 사용하기로 결심했다. </p>
<h2 id="typeorm-repository-method">TypeORM Repository Method</h2>
<h3 id="create">create</h3>
<p>객체를 생성하는 역할을 한다. <strong>save</strong>와 다르게 데이터베이스에 데이터를 생성하지 않고 <strong>객체</strong> 생성만 한다. </p>
<pre><code class="language-typescript">const user = repository.create()
const user = repository.create({
  id: 1,
  firstName: &quot;Ryu&quot;,
  lastName: &quot;Jiseung&quot;,
})</code></pre>
<h3 id="save">save</h3>
<p>저장할 <strong>Entity</strong>를 입력해주면 저장할 수 있다. create와 다르게 실제 데이터베이스에 저장한다. 만약 <strong>Row가 존재한다면 (PK 기준) 업데이트 한다.</strong> 또한 여러 객체를 한 번에 저장도 가능하다.</p>
<pre><code class="language-typescript">await repository.save(user)
await repository.save([
  category1,
  category2,
  category3
])</code></pre>
<h3 id="upsert">upsert</h3>
<p><strong>update와 insert</strong>를 합친 게 upsert이다. 데이터 생성 시도를 한 후 만약 이미 존재하는 데이터라면 업데이트를 진행한다. <strong>save</strong>와 다르게 upsert는 하나의 <strong>transaction</strong>에서 작업이 실행된다.</p>
<pre><code class="language-typescript">await repository.upsert(
  [
    { externalId: &quot;abc123&quot;, firstName: &quot;Jiseung&quot; },
    { externalId: &quot;bac321&quot;, firstName: &quot;Ryu&quot; }
  ], 
  [&quot;externalId&quot;],
)</code></pre>
<blockquote>
<p><strong>save vs upsert</strong>
일반적인 <strong>CRUD</strong>는 <strong>save</strong> 사용
Primary Key 외에 Unique Key를 기반으로 데이터를 판단하고 처리해야 하는 경우, <strong>upsert</strong> 사용</p>
</blockquote>
<table>
<thead>
<tr>
<th><strong>특징</strong></th>
<th><strong>save</strong></th>
<th><strong>upsert</strong></th>
</tr>
</thead>
<tbody><tr>
<td><strong>중복 판단 기준</strong></td>
<td>Primary Key에 의해 자동 판단.</td>
<td>Primary Key 또는 Unique Key를 명시적으로 설정해야 함.</td>
</tr>
<tr>
<td><strong>사용 시점</strong></td>
<td>단순 CRUD 작업(삽입 또는 업데이트) 시 사용.</td>
<td>중복 키 충돌 처리가 필요하거나, 특정 키를 기준으로 업데이트 또는 삽입 작업을 동시에 처리할 때 사용.</td>
</tr>
<tr>
<td><strong>필수 옵션</strong></td>
<td>없음 (Primary Key가 기본 기준).</td>
<td><code>conflictPaths</code> 옵션을 통해 중복 키를 명시적으로 정의해야 함.</td>
</tr>
<tr>
<td><strong>배치 처리</strong></td>
<td>단일 또는 다중 엔티티 삽입/업데이트 가능.</td>
<td>여러 엔티티를 대상으로 효율적으로 충돌 처리하며 삽입/업데이트 가능.</td>
</tr>
<tr>
<td><strong>데이터베이스 지원</strong></td>
<td>모든 주요 데이터베이스에서 동작.</td>
<td>PostgreSQL, MySQL 등의 Upsert 기능을 지원하는 DB에서만 동작.</td>
</tr>
</tbody></table>
<h3 id="delete">delete</h3>
<p>특정 <strong>row</strong>를 삭제할 때 사용되며 대체적으로 <strong>PK</strong>를 사용해서 삭제한다. 원한다면 <strong>findOPtionsWhere</strong> 조건으로 여러 값을 삭제할 수도 있다.</p>
<pre><code class="language-typescript">await repository.delete(1)
await repository.delete([1, 2, 3])
await repository.delete({ firstName: &quot;Ryu&quot; })</code></pre>
<h3 id="softdelete--restore">softDelete / restore</h3>
<p><strong>softeDelete</strong>는 비영구적으로 삭제하는 기능으로 <strong>restore</strong>를 통해서 <strong>Row</strong>를 복구할 수 있다.</p>
<pre><code class="language-typescript">// 삭제
await repository.softDelete(1)
// 복구
await repository.restore(1)</code></pre>
<h3 id="update">update</h3>
<p>첫번째 파라미터로 검색 조건을 입력해주고, 두번째 파라미터로 변경 필드를 입력해준다. </p>
<pre><code class="language-typescript">await repository.update(
  { age: 18 },
  { category: &quot;ADULT&quot; }
)

await repository.update(
  1,
  { firstName: &quot;Ryu&quot; }
)</code></pre>
<h3 id="find--findone--findandcount">find / findOne / findAndCount</h3>
<ul>
<li><strong>find</strong> : 해당되는 <strong>Row</strong>를 모두 반환한다.</li>
<li><strong>findOne</strong> : 해당되는 첫번쨰 <strong>Row</strong>를 반환한다. 없다면 null</li>
<li><strong>findAndCount</strong> : 해당되는 <strong>Row</strong>와 전체 갯수를 반환한다.<pre><code class="language-typescript">const rows = await repository.find({
where: {
  firstName: &quot;Ryu&quot;
}
})
</code></pre>
</li>
</ul>
<p>const row = await repository.findOne({
  where: {
    firstName: &quot;Ryu&quot;
  }
})</p>
<p>const [rows, count] = await repository.findAndCount({
  where: {
    firstName: &quot;Ryu&quot;
  }
})</p>
<pre><code>### exist
특정 조건의 **Row**가 존재하는 지 **boolean**으로 확인할 수 있다.
```typescript
await.repository.exists({
  where: {
    firstName: &quot;Ryu&quot;
  }
})
```

### preload
데이터베이스에 저장된 값을 **PK** 기준으로 불러오고 입력된 객체의 값으로 프로퍼티를 덮어쓴다. 이 과정에서 실제 데이터베이스에 **save**되지 않는다. 
```typescript
const partialUser = {
  id: 1,
  firstName: &quot;Ryu&quot;,
  profile: {
    id: 1,
  },
}
const user = await.repository.preload(partialUser)</code></pre><h3 id="findoptions">findOptions</h3>
<p>모든 <strong>find</strong>는 <strong>findOptions</strong>를 <strong>argument</strong>로 갖는데 어떤 값들을 불러올 지 <strong>필터링</strong>하는 역할을 한다. </p>
<pre><code class="language-typescript">export interface FindOneOptions&lt;Entity = any&gt; {
  select?: FindOptionsSelect&lt;Entity&gt;
  where?: FindOptionsWhere&lt;Entity&gt;[]
  relations?: FindOptionsRelations&lt;Entity&gt;
  order?: FindOptionsOrder&lt;Entity&gt;
  cache?: boolean | number
}

export interface FindManyOptions&lt;Entity = any&gt; extends FindOneOptions&lt;Entity&gt; {
  skip?: number
  take?: number
}</code></pre>
<blockquote>
<p><strong>select</strong> : 불러올 <strong>Column</strong>을 지정할 수 있다.
<strong>where</strong> : 필터링할 조건을 설정할 수 있다.
<strong>relations</strong> : 불러올 관계 테이블을 지정할 수 있다.
<strong>order</strong> : 정렬을 지정할 수 있다. <strong>(ASC / DESC)</strong>
<strong>cache</strong> : 캐싱 기간을 지정할 수 있다. 
<strong>skip</strong> : 정렬 후 스킵할 데이터 갯수를 정할 수 있다.
<strong>take</strong> : 처음 몇 개의 데이터를 불러올 지 정할 수 있다. </p>
</blockquote>
<h3 id="advanced-findoptions">Advanced findOptions</h3>
<h4 id="equal-operator">Equal Operator</h4>
<p>같은 값을 찾을 때 사용한다.</p>
<pre><code class="language-typescript">const user = await userRepository.find({
  where: { age: Equal(25) }
})</code></pre>
<h4 id="not-operator">Not Operator</h4>
<p>아닌 값을 찾을 때 사용한다.</p>
<pre><code class="language-typescript">const user = await userRepository.find({
  where: { age: Not(25) }
})</code></pre>
<h4 id="lessthan--lestthanorequal-operator">LessThan &amp; LestThanOrEqual Operator</h4>
<p>적은 값 &amp; 적거나 같은 값을 찾을 때 사용한다</p>
<pre><code class="language-typescript">const user = await userRepository.find({
  where: { age: LessThan(25) }
})

const user = await userRepository.find({
  where: { age: LessThanOrEqual(25) }
})</code></pre>
<h4 id="between">Between</h4>
<p>사이 값을 찾을 때 사용한다.</p>
<pre><code class="language-typescript">const user = await userRepository.find({
  where: { age: Between(20, 30) }
})</code></pre>
<h4 id="like--ilike">Like &amp; ILike</h4>
<p>스트링에 매칭되는 값을 찾을 때 사용한다. <strong>Like</strong>는 대소문자 구분할 때, <strong>ILike</strong>는 대소문자 구분하지 않을 때 사용한다.</p>
<pre><code class="language-typescript">const user = await userRepository.find({
  where: { age: Like(&quot;Co%&quot;) }
})

const user = await userRepository.find({
  where: { age: ILike(&quot;co%&quot;) }
})</code></pre>
<h4 id="in">In</h4>
<p>리스트에 매칭되는 값을 찾는다.</p>
<pre><code class="language-typescript">const user = await userRepository.find({
  where: { age: In([20, 25, 30]) }
})</code></pre>
<h4 id="isnull">IsNull</h4>
<p><strong>Null</strong>인 값을 필터링할 때 사용</p>
<pre><code class="language-typescript">const user = await userRepository.find({
  where: { profile: IsNull() }
})</code></pre>
<blockquote>
<p><strong>통계</strong></p>
</blockquote>
<ul>
<li><strong>count</strong> : 해당되는 갯수를 반환한다.</li>
<li><strong>sum</strong> : 해당되는 칼럼 값을 모두 더한다.</li>
<li><strong>average</strong> : 해당되는 칼럼 값의 평균을 구한다.</li>
<li><strong>minimum</strong> : 해당되는 칼럼 값의 최소치를 반환한다.</li>
<li><strong>maximum</strong> : 해당되는 칼럼 값의 최대치를 반환한다. </li>
</ul>
<h3 id="entity-embedding">Entity Embedding</h3>
<pre><code class="language-typescript">import { Column } from &quot;typeorm&quot;

export class Name {
    @Column()
    first: string

    @Column()
    last: string
}

import { Entity, PrimaryGeneratedColumn, Column } from &quot;typeorm&quot;
import { Name } from &quot;./Name&quot;

@Entity()
export class User {
    @PrimaryGeneratedColumn()
    id: string

    @Column(() =&gt; Name)
    name: Name

    @Column()
    isActive: boolean
}

import { Entity, PrimaryGeneratedColumn, Column } from &quot;typeorm&quot;
import { Name } from &quot;./Name&quot;

@Entity()
export class Employee {
    @PrimaryGeneratedColumn()
    id: string

    @Column(() =&gt; Name)
    name: Name

    @Column()
    salary: number
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/d9f34fed-3b7a-49c4-a89b-86ffecae1e31/image.png" /></p>
<h3 id="entity-inheritance">Entity Inheritance</h3>
<pre><code class="language-typescript">// 추상 클래스
// 인스턴스 생성 불가
// 상속만 가능
export abstract class Content {
    @PrimaryGeneratedColumn()
    id: number

    @Column()
    title: string

    @Column()
    description: string
}

@Entity()
export class Photo extends Content {
    @Column()
    size: string
}

@Entity()
export class Question extends Content {
    @Column()
    viewCount: number
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/ff7ad1c0-a917-4598-a8c2-03ce608bad8d/image.png" /></p>
<h3 id="single-table-inheritance">Single Table Inheritance</h3>
<pre><code class="language-typescript">@Entity()
@TableInheritance({ column: { type: &quot;varchar&quot;, name: &quot;type&quot; } })
export class Content {
    @PrimaryGeneratedColumn()
    id: number

    @Column()
    title: string

    @Column()
    description: string
}

@ChildEntity()
export class Photo extends Content {
    @Column()
    size: string
}

@ChildEntity()
export class Question extends Content {
    @Column()
    viewCount: number
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/27f99ba9-829a-4c4b-b7a3-c2a44ed4c692/image.png" /></p>
<h2 id="relationships">Relationships</h2>
<blockquote>
<p><strong>TypeORM이 제공하는 네가지 Relationship</strong>
<strong>@OneToOne</strong>(일대일) - A 테이블의 Row 하나와 B 테이블의 Row 하나가 연결되는 관계
<strong>@ManyToOne</strong>(다대일) - A 테이블의 Row 여러개와 B 테이블의 Row 하나가 연결되는 관계
<strong>@OneToMany</strong>(일대다) - A 테이블의 Row 하나와 B 테이블의 Row 여러개가 연결되는 관계
<strong>@ManyToMany</strong>(다대다) - A 테이블의 Row 여러개와 B 테이블의 Row 여러개가 연결되는 관계</p>
</blockquote>
<h3 id="relationship-decorator-적용하기">Relationship Decorator 적용하기</h3>
<h4 id="manytoone--onetomany-relationship">ManyToOne / OneToMany Relationship</h4>
<p><strong>첫번째 파라미터</strong>에는 타입을 반환하는 함수를 입력한다. (Class
Transformer Type과 같은 개념). <strong>두번째 파라미터</strong>에는 첫번째 파라미터에 입력한 클래스의 칼럼중 하나를 입력한다. 이 칼럼은 서로 관련지을 프로퍼티여야한다.</p>
<p>예를들어 <strong>ManyToOne 관계</strong>이니 <strong>photo 테이블</strong>에 user_id 칼럼이 생성되며 <strong>user 테이블</strong>과 관계가 형성된다.
특정 photo와 관련있는 user는 photo.user로 불러올 수 있고, user와 관련있는 photo들은 user.photos로 불러 올 수 있다.</p>
<pre><code class="language-typescript">@Entity()
export class Photo {
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  url: string

  @ManyToOne(
    () =&gt; User,
    (user) =&gt; user.photos
  )
  user: User</code></pre>
<pre><code class="language-typescript">@Entity()
export class User{
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  name: string

  @OneToMany(
    () =&gt; Photo,
    (photo) =&gt; photo.user
  )
  photo: Photo[]</code></pre>
<blockquote>
</blockquote>
<p><strong>photo 테이블에는 user_id 칼럼</strong> 자동으로 생긴다. 네이밍 패턴은 <code>{상대 테이블 이름}_id</code>. user_id는 user 테이블의 id 칼럼을 <strong>Foreign Key</strong>로 레퍼런스한다.</p>
<blockquote>
</blockquote>
<p>user 테이블은 추가로 칼럼이 생성되지 않는다. 원래 <strong>ManyToOne 또는 OneToMany 관계</strong>는 Foreign Key 레퍼런스를 들고있는 테이블이 Many 입장이다.
<img alt="" src="https://velog.velcdn.com/images/rjs8833/post/4d6a6d1b-d4a8-4148-a3a2-5fcd38149099/image.png" /></p>
<h4 id="onetoone-relationship">OneToOne Relationship</h4>
<p><strong>OneToOne Relationship</strong>도 마찬가지로 <strong>Decorator</strong> 원하는 프
로퍼티에 정의해주면 된다. <strong>ManyToOne</strong>은 상대의 레퍼런스를 갖는 테이블이 명확하다. <strong>OneToOne</strong>은 두 테이블 누가 레퍼런스를 들고 있어도 상관이 없기 때문에 어떤 테이블이 레퍼런스를 들고 있을지 명시해줘야 한다. <strong>JoinColumn Decorator</strong>을 사용해서 어떤 프로퍼티가 레퍼런스를 들고 있을지 정해 줄 수 있다. <strong>JoinColumn</strong>은 꼭 한쪽에만 적용해야한다. 둘 모두 적용하는건 불가능하고 의미도 없다.</p>
<pre><code class="language-typescript">@Entity()
export class Profile{
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  gender: string

  @Column()
  photo: string

  @OneToOne(
    () =&gt; User, 
    (user) =&gt; user.profile
  )
  profile: Prifile</code></pre>
<pre><code class="language-typescript">@Entity()
export class User{
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  name: string

  @OneToOne(() =&gt; Profile)
  @JoinColumn()
  profile: Profile</code></pre>
<h4 id="manytomany-relationship">ManyToMany Relationship</h4>
<p><strong>ManyToMany Relationship</strong>도 <strong>OneToOne Relationship</strong>과 마찬가지로 <strong>JoinTable Decorator</strong> 한쪽에 적용 해줘야한다. 중간 테이블이 생성될 때 <strong>JoinTable이 적용된 테이블 이름</strong>이 먼저 위치하게된다. <strong>JoinTable</strong>은 주로 두 테이블 간의 메인이 되는 <strong>table</strong>에 <strong>Decorator</strong>를 달아주는 편이다. </p>
<pre><code class="language-typescript">@Entity()
export class Category{
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  name: string

  @ManyToMany(() =&gt; Question)
  questions: Question[];</code></pre>
<pre><code class="language-typescript">@Entity()
export class Question{
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  title: string

  @Column()
  text: string

  @ManyToMany(() =&gt; Category)
  @JoinTable()
  categories: Category[];</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/rjs8833/post/a2c2f4de-f602-491d-bee1-5f02f9416317/image.png" /></p>
<blockquote>
<p><strong>Relationship Decorator에서의 cascade</strong>
cascade는 <strong>연관된 엔티티(자식 엔티티)</strong>가 부모 엔티티의 작업과 함께 처리되도록 설정하는 옵션이며, <code>@OneToOne, @OneToMany, @ManyToOne, @ManyToMany</code> 관계에서 사용할 수 있다.</p>
</blockquote>
<p><strong>cascade: true</strong>는 삽입, 업데이트, 삭제, 복구 모든 작업에 대해 자동으로 처리되도록 설정한다.</p>
<pre><code class="language-typescript">@Entity()
class Author {
  @PrimaryGeneratedColumn()
  id: number;
&gt;
  @Column()
  name: string;
&gt;
  @OneToMany(() =&gt; Book, (book) =&gt; book.author, { cascade: true })
  books: Book[];
}
&gt;
@Entity()
class Book {
  @PrimaryGeneratedColumn()
  id: number;
&gt;
  @Column()
  title: string;
&gt;
  @ManyToOne(() =&gt; Author, (author) =&gt; author.books)
  author: Author;
}</code></pre>
<blockquote>
<p><strong>Decorator DataBase Schema</strong>에서의 자주 쓰이는 Option</p>
</blockquote>
<ul>
<li><strong>nullable</strong> : 해당 필드가 NULL 값을 허용할지 여부를 결정한다.<pre><code class="language-typescript">@Column({ type: 'string', nullable: true })
description?: string;</code></pre>
</li>
<li><strong>unique</strong> : 해당 필드가 데이터베이스에서 유일한 값을 가져야 하는지 설정한다. <pre><code class="language-typescript">@Column({ type: 'string', unique: true })
email: string;</code></pre>
</li>
<li><strong>cascade</strong> : 연관된 엔터티에서 특정 작업(삽입, 업데이트, 삭제 등)이 실행될 때, 이 엔터티도 자동으로 영향을 받도록 설정한다.<pre><code class="language-typescript">@OneToMany(() =&gt; Post, (post) =&gt; post.author, { cascade: true })
posts: Post[];</code></pre>
</li>
<li><strong>default</strong> : 필드의 기본 값을 설정할 때 사용한다.<pre><code class="language-typescript">@Column({ type: 'string', default: 'active' })
status: string;</code></pre>
</li>
<li><strong>length</strong> : 해당 문자열 필드의 최대 길이를 설정할 때 사용한다.<pre><code class="language-typescript">@Column({ type: 'string', length: 50 })
name: string;</code></pre>
</li>
</ul>