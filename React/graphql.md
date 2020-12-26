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

