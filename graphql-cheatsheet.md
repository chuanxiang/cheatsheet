### Links

https://www.graphqlhub.com

https://graphql.org/learn/

[The Fullstack Tutorial for GraphQL](https://www.howtographql.com/)

## Queries/Mutations

### Field, Comment
```
{
  hero {
    name
    # Queries can have comments!
    friends {
      name
    }
  }
}
```

### Arguments
```
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
```

### Alias
```
{
  # query hero, but use with alias and parameter
  empireHero: hero(episode: EMPIRE) {
    name
  }
  jediHero: hero(episode: JEDI) {
    name
  }
}
```
```json
{
  "data": {
    "empireHero": {
      "name": "Luke Skywalker"
    },
    "jediHero": {
      "name": "R2-D2"
    }
  }
}
```

### Fragments
```
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
 
# inline fragment
```

### Operation name
Note: For logging in server
```
query HeroNameAndFriends {
  hero {
    name
    friends {
      name
    }
  }
}
```

### Variables
1. Replace the static value in the query with $variableName
2. Declare $variableName as one of the variables accepted by the query
3. Pass variableName: value in the separate, transport-specific (usually JSON) variables dictionary

```
# Episode is the type
# TYPE! means parameter is compulsory
# JEDI is default value

query HeroNameAndFriends($episode: Episode = JEDI) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

### Directives
* @include(if: Boolean) Only include this field in the result if the argument is true.
* @skip(if: Boolean) Skip this field if the argument is true.

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

### Mutations
```
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}
```
Variables
```json
{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}
```

### Inline Fragments
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

### Meta Fields
typename
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
result
```json
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
```

## Schemas/Types

### Object types
```
type Character {
  name: String!
  appearsIn: [Episode]!
  length(unit: LengthUnit = METER): Float
}
```

### Query and Mutation Types
```
schema {
  query: Query
  mutation: Mutation
}

type Query {
  hero(episode: Episode): Character
  droid(id: ID!): Droid
}
```

### Scalar types
* Int
* Float
* String
* Boolean - true or false
* ID - string
* Customer type - `scalar Date`

### Enum
```
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
```

### List/Not Null
```
type Character {
  name: String!
  appearsIn: [Episode]!
}

query DroidById($id: ID!) {
  droid(id: $id) {
    name
  }
}

# list can be null, but content can't
myField: [String!]
```

### Interfaces
```
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}

# any type that implements Character needs to have these exact fields,
# with these arguments and return types.
type Human implements Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
  starships: [Starship]
  totalCredits: Int
}
```

### Union
```
union SearchResult = Human | Droid | Starship
```

### Input types
```
input ReviewInput {
  stars: Int!
  commentary: String
}

mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}
```

Variables
```json
{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}
```

## Validation
Validate graphql at compile time instead of runtime


## Execution

### Root type / Query type
```javascript
Query: {
  human(obj, args, context, info) {
    return context.db.loadHumanByID(args.id).then(
      userData => new Human(userData)
    )
  }
}
```
Resoler arguments
* `obj` The previous object, which for a field on the root Query type is often not used.
* `args` The arguments provided to the field in the GraphQL query.
* `context` A value which is provided to every resolver and holds important contextual information like the currently logged in user, or access to a database.
* `info` A value which holds field-specific information relevant to the current query as well as the schema details, also refer type GraphQLResolveInfo for more details.

### Asynchronous resolvers
### Trivial resolvers
### Scalar coercion
### List resolvers

## Business Logic Layer
![Business Logic Layer](imgs/graphql/business_layer.png)

### Authentication
```javascript
// Bad practise
var postType = new GraphQLObjectType({
  name: ‘Post’,
  fields: {
    body: {
      type: GraphQLString,
      resolve: (post, args, context, { rootValue }) => {
        // return the post body only if the user is the post's author
        if (context.user && (context.user.id === post.authorId)) {
          return post.body;
        }
        return null;
      }
    }
  }
});

// Good practise
//Authorization logic lives inside postRepository
var postRepository = require('postRepository');
var postType = new GraphQLObjectType({
  name: ‘Post’,
  fields: {
    body: {
      type: GraphQLString,
      resolve: (post, args, context, { rootValue }) => {
        return postRepository.getBody(context.user, post);
      }
    }
  }
});
```

