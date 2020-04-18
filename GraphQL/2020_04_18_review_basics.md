# GraphQL review
type, interface 이런 개념들이 좀 헷갈리는 것 같아서, 한 번 정리 하고자 공식 doc 한 번 봐보는걸로.
<a href ='https://graphql.org/learn/schema/'>official doc</a>

# type system
the GraphQL query language is basically about selecting fields on objects

Every GraphQL service defines a set of types which completely describe the set of possible data you can query on that service. Then, when queries come in, they are validated and executed against that schema.

# type language
모든 언어에서 쓰일 수 있도록 하기 위해서, GraphQL 고유의 간단한 언어를 정하고, 사용한다!
=> GraphQL schema language
query language 와 형태가 비슷하며, 이를 통해 앞서 언급한 특정 언어에 의존하지 않는 특성을 얻을 수 있다!

# object types and fields
그 이름이 말하는 그대로, 서비스에서 어떤 데이터 오브젝트를 활용할 수 있는지 정의해놓은 것이 object type 임! 그리고 그 object 안에 어떤 데이터가 존재하는지 명시한 것들이 field 인거고.

    type Character {
    name: String!
    appearsIn: [Episode!]!
    }

위에 쓰인 코드를 분석해보면, 기본은 확실히 알 수 있다!

[1] Object type
Character is a GraphQL Object Type, meaning it's a type with some fields. Most of the types in your schema will be object types. name and appearsIn are fields on the Character type. That means that name and appearsIn are the only fields that can appear in any part of a GraphQL query that operates on the Character type.
[2] Scalar type (여기선 String)
String is one of the built-in scalar types - these are types that resolve to a single scalar object, and can't have sub-selections in the query. We'll go over scalar types more later.
[3] Non-nullable 을 나타내는 !
String! means that the field is non-nullable, meaning that the GraphQL service promises to always give you a value when you query this field. In the type language, we'll represent those with an exclamation mark.
[4] array of objects
[Episode!]! represents an array of Episode objects. Since it is also non-nullable, you can always expect an array (with zero or more items) when you query the appearsIn field. <b>And since Episode! is also non-nullable, you can always expect every item of the array to be an Episode object.</b>

# arguments

    type Starship {
    id: ID!
    name: String!
    length(unit: LengthUnit = METER): Float
    }

all arguments in GraphQL are passed by name specifically.
즉, argument passing 할 때 이름 맞춰주라는거지.
(일반적인 언어에서 순서대로 param 받는거랑 다르다.)

# The Query and Mutation types 

Most types in your schema will just be normal object types, but there are two types that are special within a schema:

    schema {
    query: Query
    mutation: Mutation
    }

Every GraphQL service has a query type and may or may not have a mutation type. These types are the same as a regular object type, but they are special because they define the <b>entry point</b> of every GraphQL query. 
=> 오호... 모든 쿼리의 엔트리 포인트를 정의한다고 표현하는구나.

즉,

    query {
    hero {
        name
    }
    droid(id: "2000") {
        name
    }
    }
(1) client side 에서 이런 쿼리가 사용되고 있다면

GraphQL 서비스(server side겠지 주로??) 에서는

    type Query {
    hero(episode: Episode): Character
    droid(id: ID!): Droid
    }
(2) hero 와 droid field 를 갖는 Query 라는 type 이 존재하고 있어야 한다는 의미임!!

# Enumeration types
enumeration types are a special kind of scalar that is restricted to a particular set of allowed values. 
[1] Validate that any arguments of this type are one of the allowed values
[2] Communicate through the type system that a field will always be one of a finite set of values

### 예시

    enum Episode {
    NEWHOPE
    EMPIRE
    JEDI
    }

이렇게 되어있다고 해보자.
Episode 라는 type 을 schema 에 사용할 때마다, NEWHOPE, EMPIRE, JEDI type 중에 하나이어야 한다는걸 의미함!

# Interfaces
An Interface is an abstract type that includes a certain set of fields that a type must include to implement the interface.

### 예시

    interface Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
    }
Character type 을 상속하는(implement) 어떤 type 이든 interface 에 지정된 field 들을 필수적으로 가져야 한다는 의미이다! argument 와 return type 도 interface 에서 지정한 것과 동일해야 함!

즉, 이 개념을 적용시켜서 예시를 다시 들자면 아래와 같다.

    type Human implements Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
    starships: [Starship]
    totalCredits: Int
    }

    type Droid implements Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
    primaryFunction: String
    }

그리고 여기서 더 연장하자면

    query HeroForEpisode($ep: Episode!) {
    hero(episode: $ep) {
        name
        primaryFunction
    }
    }
이건 오류가 뜬다.
왜냐! Query type 내에 지정된 hero field 는 Episode 를 argument로 받고, Character type 을 return 하도록 되어있다. 즉, 위에서 선언한 Human 혹은 Droid 객체를 뱉어낸다는 의미이지. 근데,primaryFunction field 는 Character type 에 공통적으로 들어간 field 가 아니라, Droid type 에만 들어있는 field 이다. 그래서 오류가 난다!

그럼, 이걸 고치려면 어떻게 해야할까??? Droid type 인 경우에만 primartFunction field 의 값을 얻고 싶다면?

    query HeroForEpisode($ep: Episode!) {
    hero(episode: $ep) {
        name
        ... on Droid {
        primaryFunction
        }
    }
    }
이렇게 하면 된다!
'... on Droid {primaryFunction}' 이렇게 하는거지.

# Union types
Union types are very similar to interfaces, but they don't get to specify any common fields between the types.

    union SearchResult = Human | Droid | Starship

Wherever we return a SearchResult type in our schema, we might get a Human, a Droid, or a Starship. Note that members of a union type need to be concrete object types; you can't create a union type out of interfaces or other unions.

그래서, SearchResult union type 을 return 하는 field 를 query 한다면, conditional fragment 를 사용해야 한다.

    {
    search(text: "an") {
        __typename
        ... on Human {
        name
        height
        }
        ... on Droid {
        name
        primaryFunction
        }
        ... on Starship {
        name
        length
        }
    }
    }

### __typename field
The __typename field resolves to a String which lets you differentiate different data types from each other on the client.

그리고, 위의 코드를 조금 개선해보자면, Human 과 Droid 가 Character 라는 같은 interface 를 상속하다는 점을 이용하면 된다.

    {
    search(text: "an") {
        __typename
        ... on Character {
        name
        }
        ... on Human {
        height
        }
        ... on Droid {
        primaryFunction
        }
        ... on Starship {
        name
        length
        }
    }
    }

이렇게, Character interface 를 상속하는 object type 인 경우에는 무조건 name 은 공통적으로 뱉어내라고 하는거지!

# Input types
간단한 scalar object 말고 complex object 를 전달하기(pass) 위해 사용된다.
Mutation에 유용하게 사용될 수 있는데, 예를 들어, 하나의 객체를 통째로 넘겨서 생성시키고 싶을 때.

In the GraphQL schema language, input types look exactly the same as regular object types, but with the keyword input instead of type.
Input type 이 일반적인 다른 object 와 똑같이 생겨쓴ㄴ데, 선언할 때 type keyword 대신 Input keyword 를 사용한다.

### 예시
    input ReviewInput {
    stars: Int!
    commentary: String
    }
### 활용 예시
    mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
    createReview(episode: $ep, review: $review) {
        stars
        commentary
    }
    }
이렇게 ReviewInput 이라는 input type 이 통째로 pass 된다.

