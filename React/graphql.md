## graphql
Graphql is a query language for APIs and a runtime for fulfilling those queries with your existing data.
- gives client the power to ask for exactly what they need and nothing more.
- makes it easier to evolve APIs over time, and enables powerful developer tools

### Send a graphql query to your API and get exactly what you need
- always return predictable results
- Apps using GraphQL are fast and stable because they control the data they get, not the server 

```
{
    hero {
        name
        height
    }
}
```
### Get many resources in a single request
- Access not just the properties of one resource but also smoothly follow references between them
- Typical REST APIs require loading from multiple URLs
- GraphQL APIs get all the data your app needs in a single request
- Apps using GraphQL can be quick even on slow mobile network connections.

### Describe what's possible with the type system
 GraphQL APIs are organized in terms of types and fields, not endpoints.
 - uses types to ensure app only ask for whats possible and provide clear and helpful errors

 ```
 type Query {
     hero: Character
 }
 type Character {
     name: String
 }
 ```

 ### Powerful developer tool





 A GraphQL service is created by defining types and fields on those types, then providing functions for each field on each type. For example, a GraphQL service that tells us who the logged in user is (me) as well as that user's name might look something like this:

    type Query {
    me: User
    }
    
    type User {
    id: ID
    name: String
    }

Along with functions for each field on each type:

    function Query_me(request) {
    return request.auth.user;
    }
    
    function User_name(user) {
    return user.getName();
    }

Once a GraphQL service is running (typically at a URL on a web service), it can receive GraphQL queries to validate and execute. A received query is first checked to ensure it only refers to the types and fields defined, then runs the provided functions to produce a result.


## Queries and mutations
 how to query a graphql server

 At its simplest, GraphQL is about asking for specific fields on objects
 
|```{
  hero {
    name
  }
}```| ```{
  "data": {
    "hero": {
      "name": "R2-D2"
    }
  }
}``` |

you always get back what you expect, and the server knows exactly what fields the client is asking for

### Arguments

    {
    human(id: "1000") {
        name
        height
    }
    }
    // output
    {
    "data": {
        "human": {
        "name": "Luke Skywalker",
        "height": 1.72
        }
    }
    }

In a system like REST, you can only pass a single set of arguments - the query parameters and URL segments in your request. But in GraphQL, every field and nested object can get its own set of arguments, making GraphQL a complete replacement for making multiple API fetches. You can even pass arguments into scalar fields, to implement data transformations once on the server, instead of on every client separately.

    {
    human(id: "1000") {
        name
        height(unit: FOOT)
    }
    }
    // output
    {
    "data": {
        "human": {
        "name": "Luke Skywalker",
        "height": 5.6430448
        }
    }
    }


### Fragments

Let's say we had a relatively complicated page in our app, which lets us look at two heroes side by side, along with their friends. You can imagine that such a query could quickly get complicated, because we would need to repeat the fields at least once - one for each side of the comparison.

That's why GraphQL includes reusable units called fragments. Fragments let you construct sets of fields, and then include them in queries where you need to. Here's an example of how you could solve the above situation using fragments:

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
    // output

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
  

The concept of fragments is frequently used to split complicated application data requirements into smaller chunks, especially when you need to combine lots of UI components with different fragments into one initial data fetch.

### Using variables inside fragments
It is possible for fragments to access variables declared in the query or mutation. See variables.

    query HeroComparison($first: Int = 3) {
      leftComparison: hero(episode: EMPIRE) {
        ...comparisonFields
      }
      rightComparison: hero(episode: JEDI) {
        ...comparisonFields
      }
    }

    fragment comparisonFields on Character {
      name
      friendsConnection(first: $first) {
        totalCount
        edges {
          node {
            name
          }
        }
      }
    }

### Operation name

we have been using a shorthand syntax where we omit both the query keyword and the query name, but in production apps it's useful to use these to make our code less ambiguous.
Here’s an example that includes the keyword query as operation type and HeroNameAndFriends as operation name :

    query HeroNameAndFriends {
      hero {
        name
        friends {
          name
        }
      }
    }
    // output
    {
      "data": {
        "hero": {
          "name": "R2-D2",
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

The operation type is either **query, mutation, or subscription** and describes what type of operation you're intending to do. The operation type is required unless you're using the query shorthand syntax, in which case you can't supply a name or variable definitions for your operation.

The operation name is a meaningful and explicit name for your operation. It is only required in multi-operation documents, but its use is encouraged because it is very helpful for debugging and server-side logging. When something goes wrong (you see errors either in your network logs, or in the logs of your GraphQL server) it is easier to identify a query in your codebase by name instead of trying to decipher the contents. Think of this just like a function name in your favorite programming language. For example, in JavaScript we can easily work only with anonymous functions, but when we give a function a name, it's easier to track it down, debug our code, and log when it's called. In the same way, GraphQL query and mutation names, along with fragment names, can be a useful debugging tool on the server side to identify different GraphQL requests.

### Variables
So far, we have been writing all of our arguments inside the query string. But in most applications, the arguments to fields will be dynamic: For example, there might be a dropdown that lets you select which Star Wars episode you are interested in, or a search field, or a set of filters.

It wouldn't be a good idea to pass these dynamic arguments directly in the query string, because then our client-side code would need to dynamically manipulate the query string at runtime, and serialize it into a GraphQL-specific format. Instead, GraphQL has a first-class way to factor dynamic values out of the query, and pass them as a separate dictionary. These values are called variables.

When we start working with variables, we need to do three things:

- Replace the static value in the query with $variableName
- Declare $variableName as one of the variables accepted by the query
- Pass variableName: value in the separate, transport-specific (usually JSON) variables dictionary

        query HeroNameAndFriends($episode: Episode) {
          hero(episode: $episode) {
            name
            friends {
              name
            }
          }
        }

Variable

    {
      "episode": "JEDI"
    }

Now, in our client code, we can simply pass a different variable rather than needing to construct an entirely new query. This is also in general a good practice for denoting which arguments in our query are expected to be dynamic - we should never be doing string interpolation to construct queries from user-supplied values.

### Default variables
Default values can also be assigned to the variables in the query by adding the default value after the type declaration.

    query HeroNameAndFriends($episode: Episode = JEDI) {
      hero(episode: $episode) {
        name
        friends {
          name
        }
      }
    }

### Directives

We discussed above how variables enable us to avoid doing manual string interpolation to construct dynamic queries. Passing variables in arguments solves a pretty big class of these problems, but we might also need a way to dynamically change the structure and shape of our queries using variables. For example, we can imagine a UI component that has a summarized and detailed view, where one includes more fields than the other.

Let's construct a query for such a component:

    query Hero($episode: Episode, $withFriends: Boolean!) {
      hero(episode: $episode) {
        name
        friends @include(if: $withFriends) {
          name
        }
      }
    }

variables

    {
      "episode": "JEDI",
      "withFriends": false
    }

// output

    {
      "data": {
        "hero": {
          "name": "R2-D2"
        }
      }
    }

A directive can be attached to a field or fragment inclusion, and can affect execution of the query in any way the server desires. The core GraphQL specification includes exactly two directives, which must be supported by any spec-compliant GraphQL server implementation:

- `@include(if: Boolean)` Only include this field in the result if the argument is true.
- `@skip(if: Boolean)` Skip this field if the argument is true.

### Mutations

Most discussions of GraphQL focus on data fetching, but any complete data platform needs a way to modify server-side data as well.
In REST, any request might end up causing some side-effects on the server, but by convention it's suggested that one doesn't use `GET` requests to modify data. GraphQL is similar - technically any query could be implemented to cause a data write. However, it's useful to establish a convention that any operations that cause writes should be sent explicitly via a mutation.

Just like in queries, if the mutation field returns an object type, you can ask for nested fields. This can be useful for fetching the new state of an object after an update. Let's look at a simple example mutation:

    mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
      createReview(episode: $ep, review: $review) {
        stars
        commentary
      }
    }
variables

    {
      "ep": "JEDI",
      "review": {
        "stars": 5,
        "commentary": "This is a great movie!"
      }
    }
// output

    {
      "data": {
        "createReview": {
          "stars": 5,
          "commentary": "This is a great movie!"
        }
      }
    }

There's one important distinction between `queries and mutations`, other than the name:

**While query fields are executed in parallel, mutation fields run in series, one after the other.**
This means that if we send two incrementCredits mutations in one request, the first is guaranteed to finish before the second begins, ensuring that we don't end up with a race condition with ourselves.

### Meta fields
Given that there are some situations where you don't know what type you'll get back from the GraphQL service, you need some way to determine how to handle that data on the client. GraphQL allows you to request __typename, a meta field, at any point in a query to get the name of the object type at that point.

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

// output

    {
      "data": {
        "search": [
          {
            "__typename": "Human",
            "name": "Han Solo"
          },
          {
            "__typename": "Human",
            "name": "Leia Organa"
          },
          {
            "__typename": "Starship",
            "name": "TIE Advanced x1"
          }
        ]
      }
    }

In the above query, search returns a union type that can be one of three options. It would be impossible to tell apart the different types from the client without the __typename field.

GraphQL services provide a few meta fields, the rest of which are used to expose the Introspection system.

## Schemas and Types

Every GraphQL service defines a set of types which completely describe the set of possible data you can query on that service. Then, when queries come in, they are validated and executed against that schema.

GraphQL services can be written in any language. Since we can't rely on a specific programming language syntax, like JavaScript, to talk about GraphQL schemas, we'll define our own simple language. We'll use the "GraphQL schema language" - it's similar to the query language, and allows us to talk about GraphQL schemas in a language-agnostic way.

### Object types and fields

The most basic components of a GraphQL schema are object types, which just represent a kind of object you can fetch from your service, and what fields it has. In the GraphQL schema language, we might represent it like this:

    type Character {
      name: String!
      appearsIn: [Episode!]!
    }

- `Character` is a GraphQL Object Type, meaning it's a type with some fields. Most of the types in your schema will be object types.
- `name and appearsIn` are fields on the Character type. That means that name and appearsIn are the only fields that can appear in any part of a GraphQL query that operates on the Character type.
- `String` is one of the built-in scalar types - these are types that resolve to a single scalar object, and can't have sub-selections in the query. We'll go over scalar types more later.
- `String!` means that the field is non-nullable, meaning that the GraphQL service promises to always give you a value when you query this field. In the type language, we'll represent those with an exclamation mark.
- `[Episode!]!` represents an array of Episode objects. Since it is also non-nullable, you can always expect an array (with zero or more items) when you query the appearsIn field. And since Episode! is also non-nullable, you can always expect every item of the array to be an Episode object.

### Arguments

Every field on a GraphQL object type can have zero or more arguments, for example the length field below:

    type Starship {
      id: ID!
      name: String!
      length(unit: LengthUnit = METER): Float
    }

Arguments can be either required or optional. When an argument is optional, we can define a default value - if the unit argument is not passed, it will be set to METER by default.

### The Query and Mutation types

    type Query {
      hero(episode: Episode): Character
      droid(id: ID!): Droid
    }

Mutations work in a similar way - you define fields on the Mutation type, and those are available as the root mutation fields you can call in your query.

It's important to remember that other than the special status of being the "entry point" into the schema, the Query and Mutation types are the same as any other GraphQL object type, and their fields work exactly the same way.

GraphQL comes with a set of default scalar types out of the box:

- `Int` : A signed 32‐bit integer.
- `Float`: A signed double-precision floating-point value.
- `String`: A UTF‐8 character sequence.
- `Boolean`: true or false.
- `ID`: The ID scalar type represents a unique identifier, often used to refetch an object or as the key for a cache. The ID type is serialized in the same way as a String; however, defining it as an ID signifies that it is not intended to be human‐readable.

In most GraphQL service implementations, there is also a way to specify custom scalar types. For example, we could define a Date type:

```scalar Date```
Then it's up to our implementation to define how that type should be serialized, deserialized, and validated. For example, you could specify that the Date type should always be serialized into an integer timestamp, and your client should know to expect that format for any date fields

### Enumeration types
Also called Enums, enumeration types are a special kind of scalar that is restricted to a particular set of allowed values. This allows you to:

Validate that any arguments of this type are one of the allowed values
Communicate through the type system that a field will always be one of a finite set of values

    enum Episode {
      NEWHOPE
      EMPIRE
      JEDI
    }

Note that GraphQL service implementations in various languages will have their own language-specific way to deal with enums. In languages that support enums as a first-class citizen, the implementation might take advantage of that; in a language like JavaScript with no enum support, these values might be internally mapped to a set of integers. However, these details don't leak out to the client, which can operate entirely in terms of the string names of the enum values.

if `non-null` value ends up getting a null value that will actually trigger a GraphQL `execution error`, letting the client know that something has gone wrong.
The Non-Null type modifier can also be used when defining arguments for a field, which will cause the GraphQL server to return a `validation error` if a null value is passed as that argument, whether in the GraphQL string or in the variables.

    myField: [String!]
This means that the list itself can be null, but it can't have any null members. For example, in JSON:

    myField: null // valid
    myField: [] // valid
    myField: ['a', 'b'] // valid
    myField: ['a', null, 'b'] // error

### Interfaces
    interface Character {
      id: ID!
      name: String!
      friends: [Character]
      appearsIn: [Episode]!
    }

This means that any type that implements Character needs to have these exact fields, with these arguments and return types.

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

      hero(episode: $ep) {
        name
        ... on Droid {
          primaryFunction
        }
      }
    }

### Union types
Union types are very similar to interfaces, but they don't get to specify any common fields between the types.
```
union SearchResult = Human | Droid | Starship
```
Wherever we return a SearchResult type in our schema, we might get a Human, a Droid, or a Starship. Note that members of a union type need to be concrete object types; you can't create a union type out of interfaces or other unions.