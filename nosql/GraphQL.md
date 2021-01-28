## GraphQL

GraphQL은 페이스북에서 만든 쿼리언어, sql은 데이터베이스 시스템에 저장된 데이터를 효율적으로 가져오는게 목적, gql은 웹 클라이언트가 데이터를 서버로부터 효율적으로 가져오는 것이 목적이다.  기존에 REST API 방식에서는 프론트앤드 프로그래머는 백엔드 프로그래머가 작성한 API 스펙에 많이 의존하지만 gql을 사용하면 클라이언트 프로그래머가 작성, 관리한다. 다만 여전히 데이터 schema에 대한 협업 의존성은 존재.

크게 query mutation subscription 있는데

```
//SQL 사용 예시
SELECT plot_id, species_id, sex, weight, ROUND(weight / 1000.0, 2) FROM surveys;
```

```
//GQL 사용 예시
{
  hero {
    name
    friends {
      name
    }
  }
}
```

### REST API와 비교.

 REST API는 URL, METHOD등을 조합하기 때문에 다양한 Endpoint가 존재하는데 반면, gql은 단 하나의 Endpoint가 존한다. 
 
 ![](http://tech.kakao.com/files/graphql-mobile-api.png)


### GraphQL의 구조

#### 쿼리/뮤테이션(query/mutation)

쿼리와 뮤테이션, 응답구조는 거의 일치하며 쿼리는 데이터를 읽는데(R) 사용되고, 뮤테이션은 데이터를 변조하는데(CUD) 사용된다는 개념적인 규약이다.

![](http://tech.kakao.com/files/graphql-example.png)

#### 스키마/타입(schema/type)

gql의 스키마 작성은 C,C++의 Header파일 작성과 비슷하다.

##### 오브젝트 타입과 필드

```
type Character {
  name: String!
  appearsIn: [Episode!]!
}

```
* 오브젝트 타입 : Character
* 필드 : name, appearsIn
* 스칼라 타입 : String, ID, Int 등
* 느낌표(!) : 필수 값을 의미(non-nullable)
* 대괄호([, ]) : 배열을 의미(array)

#### 리졸버(resolver)

데이터베이스 사용 시에 데이터를 가져오기위해 sql을 작성하고 해당 sql로 데이터를 가져오는 구체적인 과정이 구현되어 있지만, gql에서는 데이터를 가져오는 구체적인 과정을 구현해야하고, 구체적인 과정은 resolver가 관리하고 이를 직접 구현해야 한다.

gql 쿼리에서는 각각 필드마다 함수가 하나씩 존재하고, 이 함수가 다음 타입을 반환합니다. 이러한 각각의 함수를 리졸버라고 한다. 만약 필드가 스칼라 값(숫자나 primitive타입)인 경우엔 실행이 종료되고 연쇄적인 리졸버 호출이 발생하지 않는다.


#### 인트로스펙션(instrospection)

기존 API 명세서를 주고 받는 절차를 대체하는 기능으로 gql의 서버 자체에서 현재 서버에 정의된 스키마의 실시간 공류를 위한 기능이다. 대부분의 서버용 gql라이브러리에는 쿼리용 IDE를 제공하며. 
아래는 apollo server라는 서버용 gql라이브러리에 포함 되어 있는 웹 IDE화면이다. 

![](http://tech.kakao.com/files/graphql-apollo-ide.png)

#### 오퍼레이션 네임 쿼리

오퍼레이션 네임 쿼리는 쿼리용 함수로 데이터베이스 내에 프로시져(procedure) 개념과 유사하다고 생각하면 된다. 덕분에 REST API대신에 한번에 원하는 모든 데이터를 가져올 수 있다.
오퍼레이션 네임 쿼리는 query로 시작하며 쿼리에 변수를 넣을 수 있도록 작성된다. eact apollo client의 경우에는 variables 라는 파라메터에 원하는 값을 넣어주면된다.

```
{//쿼리
  human(id: "1000") {
    name
    height
  }
}

//오퍼레이션 네임 쿼리
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}

```

```
//한번에 많은 쿼리 가져오기
query getStudentInfomation($studentId: ID){
  personalInfo(studentId: $studentId) {
    name
    address1
    address2
    major
  }
  classInfo(year: 2018, studentId: $studentId) {
    classCode
    className
    teacher {
      name
      major
    }
    classRoom {
      id
      maintainer {
        name
      }
    }
  }
  SATInfo(schoolCode: 0412, studentId: $studentId) {
    totalScore
    dueDate
  }
}

```

### Query & Mutations

#### Variables

기본적으로 변수를 작업할 때 3가지로 수행합니다.

1. 쿼리의 상수를 $variableName으로 전환.
2. $variableName 쿼리에서 허용하는 변수 중 하나로 선언(서버에서 받아줘야 가능)
3. variableName : value에 맞는 특정 정보

변수 정의는 선택적이거나 필수일 수 있습니다. 타입옆에 !가 없으면 선택사항이고, 있는 경우는 필수 사항으로 반드시 파라미터를 넘겨줘야 한다.

기본적으로 변수를 넣을 땐  아래와 같이 선언할 수 있다.

```
query HeroNameAndFriends($episode: Episode = JEDI) {
...
}
```

#### Directives

변수 말고도 쿼리의 구조나 모양을 동적으로 수정하고 싶을 수 있습니다. 더 많은 필드를 추가하거나 제외할 때 사용할 수 있는 방법이다.

```
query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}

```


@include(if: Boolean(변수))는 변수가 true인 경우에 해당 쿼리를 추가한다.
@skip(if: Boolean(변수))는 변수가 true인 경우에 해당 쿼리를 제외한다. 

#### Mutations

정보를 받아오는 것말고 Write할 때 사용하며, 쿼리의 형태는 매우 비슷하다. 

```
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}

```

다만 Mutations를 사용할 때 Query와 다른점이 Query는 병렬로 실행되고 Mutation은 순차적으로 실행됩니다. 2개의 필드를 같이 보냈다면 두번째 필드가 만들어지기전에 첫번째 필드는 완전히 끝나있다는 점이다.


#### inline Fragments

GraphQL에서도 인터페이스 및 공용체 유형을 정의하는 기능이 포함되어 있고, 이렇게 인터페이스나 공용체 유형을 반환하는 필드의 경우 inline Fragments를 사용하요 구체적인 유형의 데이터에 엑세스 할 수 있습니다.

```
query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    ... on Droid {
      primaryFunction
    }
    ... on Human {
      height
    }
  }
}
```

위와 같이 hero 필드는 Character를 반환하는데 이 캐릭터는 에피소드에 따라 Human이거나 Droid일 수 있고, 반환 되는 타입에 따라 inline Fragments를 사용하면 특정 타입의 데이터에 엑세스할 수 있습니다.


#### Fragments

같은 쿼리에 특정 데이터들을 모아서 볼 때, 각각 필드를 호출하게 되면 리졸버의 비효율이 발생하기 쉽다. 때문에 gql에 fragments라는 재사용 가능한 단위가 포함되어 있는데 다음과 같이 사용할 수 있다.

```
//query
{
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  appearsIn
  friends {
    name
  }
}

```

```
//result 

{
  "data": {
    "leftComparison": {
      "name": "Luke Skywalker",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ],
      "friends": [
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        },
        {
          "name": "C-3PO"
        },
        {
          "name": "R2-D2"
        }
      ]
    },
    "rightComparison": {
      "name": "R2-D2",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ],
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```


#### Meta fields

클라이언트에서 데이터를 받을 때 데이터의 형태를 구정시켜야하는 경우가 생길 수 있다. 이럴 때 meta fields를 사용해서 typename에 따라 name을 가져올 수 있다.

```
{
  search(text: "an") {
    __typename
    ... on Human {
      name
    }
    ... on Droid {
      name
    }
    ... on Starship {
      name
    }
  }
}
```


### 기타 특징

* gql은 특정 데이터베이스나 플렛폼은 물론, 네트워크 방식에도 종속적이지 않다. L7 HTTP POST와 웹소켓 프로토콜을 활용하지만 필요에 따라서 L4의 TCP/UDP나 L2방식의 이더넷 프레임을 활용할 수 있다.


> #### 출처

* [GraphQL 개념잡기](https://tech.kakao.com/2019/08/01/graphql-basic/)
* [GraphQL learn](https://graphql.org/learn/)
* [GraphQL Queries and Mutations](https://graphql.org/learn/queries/)
* [GraphQL spec](https://github.com/graphql/graphql-spec)

