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