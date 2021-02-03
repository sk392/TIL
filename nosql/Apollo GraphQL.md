## Apollo GraphQL

GraphQL중에 Apollo GQL에 대해서 알아보자.


### Supported types

GraphQL을 정의할 때 지원하는 타입은 아래와 같습니다.

* Scalar types
* Object types
* the ```Query``` type
* the ```Mutation``` type
* Input types
* enum types

#### Scalar types

스칼라 타입은 primitive type이랑 비슷하고 구체적인 데이터를 활용하며 대부분의 primitive type을 커버하지만 필요한 경우 [custom scalar types](https://www.apollographql.com/docs/apollo-server/schema/custom-scalars/)을 사용할 수 있다.

* `Int`: A signed 32‐bit integer
* `Float`: A signed double-precision floating-point value
* `String`: A UTF‐8 character sequence
* `Boolean`: true or false
* `ID` (serialized as a `String`): fetch하거나 cache할 때 사용하는 유니크 키로서 string으로 serialized할 수 있다. 

#### Object types

오브젝트 타입은 여러 필드들을 가지고 있을 수 있습니다. scalar type이나 다른 object type도 가능!

```
type Book {
  title: String
  author: Author
}

type Author {
  name: String
  books: [Book]
}
```

#### the `query` type

쿼리는 클라이언트가 데이터 그래프에 요청하는 모든 쿼리의 최상위 진입점을 의미한다. object와 비슷하지만 query로 시작하고, 주로 읽기를 위해 씁니다.

```
//query의 예제 스키마
type Query {
  books: [Book]
  authors: [Author]
}
```

#### the `mutation` type

mutation Type은 Query와 비슷하지만 Query와 달리 쓰기 작업을 위한 기능이고 쿼리와 달리 동시에 여러 요청이 들어간 경우 나열된 순서대로 순차적으로 적용된다.

```
//mutation 예제 스키마
type Mutation {
  addBook(title: String, author: String): Book
}
```

```
//요청
mutation CreateBook {
  addBook(title: "Fox in Socks", author: "Dr. Seuss") {
    title
    author {
      name
    }
  }
}

//응답
{
  "data": {
    "addBook": {
      "title": "Fox in Socks",
      "author": {
        "name": "Dr. Seuss"
      }
    }
  }
}
```

#### input type

Query 및 Mutation으로 객체를 전달할 수 있는 특수 유형으로 여러 파라미터를 의미 있는 객체로 묶어서 전달할 수 있게 지원해주는 기능이며 지원 도구에 자동으로 노출되는 주석도 달수 있다. 

```
//origin
type Mutation {
  createPost(title: String, body: String, mediaUrls: [String]): Post
}

```

```
//change with input type
type Mutation {
  createPost(post: PostAndMediaInput): Post
}

input PostAndMediaInput {
  title: String
  body: String
  mediaUrls: [String]
}
```


#### enum types

enum은 기존에 다른 언어와 비슷하게 규정된 옵션 내에서만 적용할 때 사용된다. 또 serialized할 때마다 스트링으로 변환되기 때문에 scalar타입을 사용할 수 있는 필드 어느 곳에서든 사용할 수 있습니다.

```
enum AllowedColor {
  RED
  GREEN
  BLUE
}
//스키마
type Query {
  favoriteColor: AllowedColor # enum return value
  avatar(borderColor: AllowedColor): String # enum argument
}

//쿼리
query GetAvatar {
  avatar(borderColor: RED)
}

```


### Schema growing
대부분 버전 업데이트를 통한 기능 추가 변경은 안전하고 이전 버전을 호환하지만 다음과 같은 동작의 경우 클라이언트를 중단시킬 수 있습니다.

* type or field 제거
* type or field 네이밍 변경
* nullable field 추가
* field arguments 제거



> #### 출처


- [Apollo GraphQL Schema basics](https://www.apollographql.com/docs/apollo-server/schema/schema)
