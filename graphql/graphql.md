# GraphQL

GraphQL is a query language and runtime for APIs that enables clients to request precisely the data they need and nothing more. 
Unlike traditional REST APIs, where endpoints return fixed data structures, GraphQL gives clients the flexibility to define the shape and structure of the data they receive.
GraphQL's ability to deliver tailored responses and its flexibility in data fetching make it a powerful tool for modern application development. 
Its schema-driven approach ensures strong typing and clear API contracts, making it a great choice for developers and organizations alike.

## Key Components of GraphQL
### Schema

The schema defines the structure of the API, including types, queries, mutations, and subscriptions.

Example:

```java
type Query {
  user(id: ID!): User
}

type User {
  id: ID!
  name: String
  email: String
}
```

### Queries

Queries are used to fetch data from the server.

Example:

```java
{
  user(id: "1") {
    name
    email
  }
}

```

### Mutations

Mutations are used to modify data on the server.

Example:

```java
mutation {
  createUser(name: "John", email: "john@example.com") {
    id
    name
    email
  }
}

```

### Resolvers

Resolvers are functions that handle the logic for fetching or modifying data for a specific field in the schema.

Example:

```java
const resolvers = {
  Query: {
    user: (parent, args, context) => {
      return context.db.getUserById(args.id);
    },
  },
};
```

### Runtime

The GraphQL server processes client requests using the schema and resolvers to fetch and return the requested data.


## How GraphQL Works

1. Client Sends a Query or Mutation

The client defines the exact data it needs and sends the query or mutation to the GraphQL server.

Example Query:
```java
{
  user(id: "1") {
    name
    email
  }
}
```

2. GraphQL Server Parses the Query

The server validates the query against the schema to ensure it is well-formed and references valid types and fields.

3. Resolvers Fetch the Data

Each field in the query is mapped to a resolver, which contains the logic to fetch the data (e.g., database calls, API calls).

Example Resolver:
```java
const resolvers = {
  Query: {
    user: (parent, args, context) => {
      return context.db.getUserById(args.id);
    },
  },
};
```

4. Data is Aggregated and Returned

The server aggregates the results from the resolvers and structures the response in the same shape as the query.

Example Response:
```json
{
  "data": {
    "user": {
      "name": "John",
      "email": "john@example.com"
    }
  }
}
```

## Advantages of GraphQL

Flexible Data Fetching: Clients can request only the data they need.

Strong Typing: The schema provides a clear contract between the client and server.

Single Endpoint: All requests are sent to a single endpoint (e.g., /graphql).

Efficient Data Loading: Reduces over-fetching and under-fetching of data.

Easier Versioning: Deprecate fields without breaking existing clients.


## Use Cases for GraphQL

1. Mobile Applications - Limited bandwidth and specific data needs make GraphQL ideal for fetching minimal data.

2. Microservices - GraphQL can aggregate data from multiple services into a single query.

3. Complex Data Structures - Nested relationships and hierarchical data are easier to handle with GraphQL.

## Common GraphQL Tools

Apollo Server: A popular GraphQL server implementation.

GraphQL.js: Official JavaScript implementation of GraphQL.

Relay: A GraphQL client developed by Facebook.

GraphiQL: An in-browser IDE for exploring GraphQL APIs.
