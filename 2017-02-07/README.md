# GraphQL Schema Language Cheat Sheet
The definitive guide to express your GraphQL schema succinctly

_Last updated: 2017-01-28_

_Prepared by: Hafiz Ismail / @sogko_

## What is GraphQL [Schema Language](http://graphql.org/learn/schema/#type-language)?

It is a shorthand notation to succinctly express the basic shape of your GraphQL schema and its type system.

---

## What does it look like?

Below is an example of a typical GraphQL schema expressed in shorthand.

```graphql
# define E#ntity interface
interface Entity {
  id: ID!
  name: String
}

## define custom Url Scalar
scalar Url

## User type implements Entity interface
type User implements Entity {
  id: ID!
  name: String
  age: Int
  balance: Float
  is_active: Boolean
  friends: [User]!
  homepage: Url
}

## root Query type
type Query {
  me: User
  friends(limit: Int = 10): [User]!
}

## custom complex input type
input ListUsersInput {
  limit: Int
  since_id: ID
}

## root mutation type
type Mutation {
  users(params: ListUsersInput): [User]!
}

## GraphQL root schema type
schema {
  query: Root
  mutation: Mutation
  subscription: ...
}
```

## Schema

| keyword        | definition                              |
|----------------|-----------------------------------------|
| `schema`       | GraphQL schema definition               |
| `query`        | A read-only fetch operation             |
| `mutation`     | A write followed by a fetch operation   |
| `subscription` | A subscription operation (experimental) |

## [Built-in Scalar Types](http://graphql.org/learn/schema/#scalar-types)

| keyword   | definition                                                                                                                                                                                                                                                 |
|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Int`     | A signed 32‐bit integer                                                                                                                                                                                                                                    |
| `Float`   | A signed double-precision floating-point value                                                                                                                                                                                                             |
| `String`  | A UTF‐8 character sequence                                                                                                                                                                                                                                 |
| `Boolean` | `true` or `false`                                                                                                                                                                                                                                          |
| `ID`      | The ID scalar type represents a unique identifier, often used to refetch an object or as the key for a cache. The ID type is serialized in the same way as a String; however, defining it as an ID signifies that it is not intended to be human‐readable. |

## Type Definitions

| keyword     | definition                                                                                              |
|-------------|---------------------------------------------------------------------------------------------------------|
| `scalar`    | Represent the leaves of the query (values of fields)                                                    |
| `type`      | Simple [object](http://graphql.org/learn/schema/#object-types-and-fields) with fields                                                                               |
| `interface` | Abstract type that includes a certain set of fields that a type must include to implement the interface |
| `union`     | Similar to interfaces, but without specifing common fields between types                                |
| `enum`      | A special kind of scalar that is restricted to a particular set of allowed values                       |
| `input`     | Represent non-scalars for arguments                                                                     |

## [Type Modifiers](http://graphql.org/learn/schema/#lists-and-non-null)

| keyword      | definition                                |
|--------------|-------------------------------------------|
| `String`     | Nullable string                           |
| `String!`    | Non-nullable string                       |
| `[String]`   | List of nullable strings                  |
| `[String]!`  | Non-nullable list of nullable strings     |
| `[String!]!` | Non-nullable list of non-nullable strings |

## [Input Arguments](http://graphql.org/learn/schema/#arguments)

### Basic input

```graphql
type Query {
  users(limit: Int): [User]!
}
```

### Input with default value

```graphql
type Query {
  users(limit: Int = 10): [User]
}
```

### Input with multiple args

```graphql
type Query {
  users(limit: Int, sort: String): [User]
}
```

### Input with multiple args and default values

```graphql
type Query {
  users(limit: Int = 10, sort: String): [User]
}

## or

type Query {
  users(limit: Int, sort: String = "asc" ): [User]
}

## or

type Query {
  users(limit: Int = 10, sort: String = "asc" ): [User]
}
```

## [Input Types](http://graphql.org/learn/schema/#input-types)

```graphql
input ListUsersInput {
  limit: Int
  since_id: ID
}

type Mutation {
  users(params: ListUsersInput): [Users]!
}
```

## Custom Scalar

```graphql
scalar Url

type User {
  name: String
  homepage: Url
}
```

## [Interfaces](http://graphql.org/learn/schema/#interfaces)

Object implementing one or more interfaces
```graphql
interface Foo {
  is_foo: Boolean
}

interface Goo {
  is_goo: Boolean
}

type Bar implements Foo {
  is_foo: Boolean
  is_bar: Boolean
}

type Bar implements Foo, Goo {
  is_foo: Boolean
  is_goo: Boolean
  is_bar: Boolean
}
```

## [Unions](http://graphql.org/learn/schema/#union-types)

Union of one or more objects

```graphql
type Foo {
  name: String
}

type Bar {
  is_bar: String
}

union SingleUnion = Foo
union MultipleUnion = Foo | Bar

type Root {
  single: SingleUnion
  multiple: MultipleUnion
}
```

## [Enums](http://graphql.org/learn/schema/#enumeration-types)

```graphql
enum USER_STATE {
  NOT_FOUND
  ACTIVE
  INACTIVE
  SUSPENDED
}

type Root {
  stateForUser(userID: ID!): USER_STATE!
  users(state: USER_STATE, limit: Int = 10): [User]
}
```
