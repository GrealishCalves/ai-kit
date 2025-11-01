# GraphQL Documentation

*Scraped from graphql.org/learn*

*Total pages: 22*


---

# Introduction to GraphQL

**Source:** https://graphql.org/learn/

## Table of Contents

- Describe your API with a type system
- Query exactly what you need
- Evolve your API without versioning
- Try it out!
- Next steps
- Introduction to GraphQL
- Describe your API with a type system
- Query exactly what you need
- Evolve your API without versioning
- Try it out!
- Next steps

## Content

Learn about GraphQL, how it works, and how to use it

GraphQL is a query language for your API, and a server-side runtime for executing queries using a type system you define for your data. The  GraphQL specification  was open-sourced in 2015 and has since been implemented in a variety of programming languages. GraphQL isn’t tied to any specific database or storage engine—it is backed by your existing code and data.

A GraphQL service is created by  defining types and their fields , and then writing a function for each field to  provide the required data . For example, a GraphQL service that tells you the name of a logged-in user might look like this:

Along with functions for each field on each type:

In the example above, the function that provides data for the  me  field on the  Query  type uses information about the authenticated user who made the request, while the  name  field on the  User  type is populated by using that user’s ID to fetch their full name from a database.

After a GraphQL service is running (typically at a URL on a web service), it can receive  GraphQL queries  to validate and execute from clients. The service first checks a query to ensure it only refers to the types and fields defined for the API and then runs the provided functions to produce a result.

For example, the query:

Could produce the following JSON result:

With even a simple query, we can see some of the key features that make GraphQL so powerful. The client can make queries to the API that mirror the structure of the data that they need and then receive just that data in the expected shape with a single request—and without having to worry about which underlying data sources provided it.

Client requirements change over time and GraphQL allows your API to evolve in response to those needs and without the overhead of managing different API versions. For example, if a new feature calls for more specific name values to be available, then the  User  type could be updated as follows:

Client tooling will encourage developers to use the new fields and remove usage of the deprecated  name  field. The field can be removed once it is determined it is no longer used; in the meantime GraphQL will continue to provide its data as expected.

The best way to learn GraphQL is to start writing queries. The query editors used throughout this guide are  interactive , so try adding an  id  and  appearsIn  fields to the  hero  object to see an updated result:

The examples in this guide are based on  a modified version of the SWAPI GraphQL schema . Because these queries are designed for illustrative purposes, they will not run on the full version of the SWAPI GraphQL API due to differences between the two schemas.  You can try the full version of the API here.

Now that you know some key GraphQL concepts, you’re ready to learn about all of the different aspects of its  type system .

For an in-depth learning experience with practical tutorials, see  the available training courses .

Learn about GraphQL, how it works, and how to use it

GraphQL is a query language for your API, and a server-side runtime for executing queries using a type system you define for your data. The  GraphQL specification  was open-sourced in 2015 and has since been implemented in a variety of programming languages. GraphQL isn’t tied to any specific database or storage engine—it is backed by your existing code and data.

A GraphQL service is created by  defining types and their fields , and then writing a function for each field to  provide the required data . For example, a GraphQL service that tells you the name of a logged-in user might look like this:

Along with functions for each field on each type:

In the example above, the function that provides data for the  me  field on the  Query  type uses information about the authenticated user who made the request, while the  name  field on the  User  type is populated by using that user’s ID to fetch their full name from a database.

After a GraphQL service is running (typically at a URL on a web service), it can receive  GraphQL queries  to validate and execute from clients. The service first checks a query to ensure it only refers to the types and fields defined for the API and then runs the provided functions to produce a result.

For example, the query:

Could produce the following JSON result:

With even a simple query, we can see some of the key features that make GraphQL so powerful. The client can make queries to the API that mirror the structure of the data that they need and then receive just that data in the expected shape with a single request—and without having to worry about which underlying data sources provided it.

Client requirements change over time and GraphQL allows your API to evolve in response to those needs and without the overhead of managing different API versions. For example, if a new feature calls for more specific name values to be available, then the  User  type could be updated as follows:

Client tooling will encourage developers to use the new fields and remove usage of the deprecated  name  field. The field can be removed once it is determined it is no longer used; in the meantime GraphQL will continue to provide its data as expected.

The best way to learn GraphQL is to start writing queries. The query editors used throughout this guide are  interactive , so try adding an  id  and  appearsIn  fields to the  hero  object to see an updated result:

The examples in this guide are based on  a modified version of the SWAPI GraphQL schema . Because these queries are designed for illustrative purposes, they will not run on the full version of the SWAPI GraphQL API due to differences between the two schemas.  You can try the full version of the API here.

Now that you know some key GraphQL concepts, you’re ready to learn about all of the different aspects of its  type system .

For an in-depth learning experience with practical tutorials, see  the available training courses .

## Code Examples

### Example 1

```
type Query {
  me: User
}
 
type User {
  name: String
}
```

### Example 2

```
type Query {
  me: User
}
 
type User {
  name: String
}
```

### Example 3

```
// Resolver for the `me` field on the `Query` type
function resolveQueryMe(_parent, _args, context, _info) {
  return context.request.auth.user;
}
 
// Resolver for the `name` field on the `User` type
function resolveUserName(user, _args, context, _info) {
  return context.db.getUserFullName(user.id);
}
```

### Example 4

```
// Resolver for the `me` field on the `Query` type
function resolveQueryMe(_parent, _args, context, _info) {
  return context.request.auth.user;
}
 
// Resolver for the `name` field on the `User` type
function resolveUserName(user, _args, context, _info) {
  return context.db.getUserFullName(user.id);
}
```

### Example 5

```
{
  me {
    name
  }
}
```

### Example 6

```
{
  me {
    name
  }
}
```

### Example 7

```
{
  "data": {
    "me": {
      "name": "Luke Skywalker"
    }
  }
}
```

### Example 8

```
{
  "data": {
    "me": {
      "name": "Luke Skywalker"
    }
  }
}
```

### Example 9

```
type User {
  fullName: String
  nickname: String
  name: String @deprecated(reason: "Use `fullName`.")
}
```

### Example 10

```
type User {
  fullName: String
  nickname: String
  name: String @deprecated(reason: "Use `fullName`.")
}
```

### Example 11

```
type Query {
  me: User
}
 
type User {
  name: String
}
```

### Example 12

```
type Query {
  me: User
}
 
type User {
  name: String
}
```

### Example 13

```
// Resolver for the `me` field on the `Query` type
function resolveQueryMe(_parent, _args, context, _info) {
  return context.request.auth.user;
}
 
// Resolver for the `name` field on the `User` type
function resolveUserName(user, _args, context, _info) {
  return context.db.getUserFullName(user.id);
}
```

### Example 14

```
// Resolver for the `me` field on the `Query` type
function resolveQueryMe(_parent, _args, context, _info) {
  return context.request.auth.user;
}
 
// Resolver for the `name` field on the `User` type
function resolveUserName(user, _args, context, _info) {
  return context.db.getUserFullName(user.id);
}
```

### Example 15

```
{
  me {
    name
  }
}
```

### Example 16

```
{
  me {
    name
  }
}
```

### Example 17

```
{
  "data": {
    "me": {
      "name": "Luke Skywalker"
    }
  }
}
```

### Example 18

```
{
  "data": {
    "me": {
      "name": "Luke Skywalker"
    }
  }
}
```

### Example 19

```
type User {
  fullName: String
  nickname: String
  name: String @deprecated(reason: "Use `fullName`.")
}
```

### Example 20

```
type User {
  fullName: String
  nickname: String
  name: String @deprecated(reason: "Use `fullName`.")
}
```


---

# GraphQL Best Practices

**Source:** https://graphql.org/learn/best-practices/

## Table of Contents

- GraphQL Best Practices

## Content

The GraphQL specification is intentionally silent on a handful of important issues facing APIs such as dealing with the network, authorization, and pagination. This doesn’t mean that there aren’t solutions for these issues when using GraphQL, just that they’re outside the description about what GraphQL  is  and instead just common practice.

The articles in this section should not be taken as gospel, and in some cases may rightfully be ignored in favor of some other approach. Some articles introduce some of the philosophy developed within Facebook around designing and deploying GraphQL services, while others are more tactical suggestions for solving common problems like serving over HTTP and performing authorization.

The GraphQL specification is intentionally silent on a handful of important issues facing APIs such as dealing with the network, authorization, and pagination. This doesn’t mean that there aren’t solutions for these issues when using GraphQL, just that they’re outside the description about what GraphQL  is  and instead just common practice.

The articles in this section should not be taken as gospel, and in some cases may rightfully be ignored in favor of some other approach. Some articles introduce some of the philosophy developed within Facebook around designing and deploying GraphQL services, while others are more tactical suggestions for solving common problems like serving over HTTP and performing authorization.


---

# Pagination

**Source:** https://graphql.org/learn/pagination/

## Table of Contents

- Plurals
- Slicing
- Pagination and edges
- End-of-list, counts, and connections
- Complete connection model
- Connection specification
- Recap
- Pagination
- Plurals
- Slicing
- Pagination and edges
- End-of-list, counts, and connections
- Complete connection model
- Connection specification
- Recap

## Content

Traverse lists of objects with a consistent field pagination model

A common use case in GraphQL is traversing the relationship between sets of objects. There are different ways that these relationships can be exposed in GraphQL, giving a varying set of capabilities to the client developer. On this page, we’ll explore how fields may be paginated using a cursor-based connection model.

The simplest way to expose a connection between objects is with a field that returns a plural  List type . For example, if we wanted to get a list of R2-D2’s friends, we could just ask for all of them:

Quickly, though, we realize that there are additional behaviors a client might want. A client might want to be able to specify how many friends they want to fetch—maybe they only want the first two. So we’d want to expose something like this:

But if we just fetched the first two, we might want to paginate through the list as well; once the client fetches the first two friends, they might want to send a second request to ask for the next two friends. How can we enable that behavior?

There are several ways we could do pagination:

The approach described in the first bullet is classic  offset-based pagination . However, this style of pagination can have performance and security downsides, especially for larger data sets. Additionally, if new records are added to the database after the user has made a request for a page of results, then offset calculations for subsequent pages may become ambiguous.

In general, we’ve found that  cursor-based pagination  is the most powerful of those designed. Especially if the cursors are opaque, either offset or ID-based pagination can be implemented using cursor-based pagination (by making the cursor the offset or the ID), and using cursors gives additional flexibility if the pagination model changes in the future. As a reminder that the cursors are opaque and their format should not be relied upon, we suggest base64 encoding them.

But that leads us to a problem—how do we get the cursor from the object? We wouldn’t want the cursor to live on the  User  type; it’s a property of the connection, not of the object. So we might want to introduce a new layer of indirection; our  friends  field should give us a list of edges, and an edge has both a cursor and the underlying node:

The concept of an edge also proves useful if there is information that is specific to the edge, rather than to one of the objects. For example, if we wanted to expose “friendship time” in the API, having it live on the edge is a natural place to put it.

Now we can paginate through the connection using cursors, but how do we know when we reach the end of the connection? We have to keep querying until we get an empty list back, but we’d like for the connection to tell us when we’ve reached the end so we don’t need that additional request. Similarly, what if we want additional information about the connection itself, for example, how many friends does R2-D2 have in total?

To solve both of these problems, our  friends  field can return a connection object. The connection object will be an Object type that has a field for the edges, as well as other information (like total count and information about whether a next page exists). So our final query might look more like this:

Note that we also might include  endCursor  and  startCursor  in this  PageInfo  object. This way, if we don’t need any of the additional information that the edge contains, we don’t need to query for the edges at all, since we got the cursors needed for pagination from  pageInfo . This leads to a potential usability improvement for connections; instead of just exposing the  edges  list, we could also expose a dedicated list of just the nodes, to avoid a layer of indirection.

Clearly, this is more complex than our original design of just having a plural! But by adopting this design, we’ve unlocked several capabilities for the client:

To see this in action, there’s an additional field in the example schema, called  friendsConnection , that exposes all of these concepts:

You can try it out in the example query. Try removing the  after  argument for the  friendsConnection  field to see how the pagination will be affected. Also, try replacing the  edges  field with the helper  friends  field on the connection, which lets you get directly to the list of friends without the additional edge layer of indirection, when appropriate for clients:

To ensure a consistent implementation of this pattern, the Relay project has a formal  specification  you can follow for building GraphQL APIs that use a cursor-based connection pattern - whether or not use you Relay.

To recap these recommendations for paginating fields in a GraphQL schema:

Traverse lists of objects with a consistent field pagination model

A common use case in GraphQL is traversing the relationship between sets of objects. There are different ways that these relationships can be exposed in GraphQL, giving a varying set of capabilities to the client developer. On this page, we’ll explore how fields may be paginated using a cursor-based connection model.

The simplest way to expose a connection between objects is with a field that returns a plural  List type . For example, if we wanted to get a list of R2-D2’s friends, we could just ask for all of them:

Quickly, though, we realize that there are additional behaviors a client might want. A client might want to be able to specify how many friends they want to fetch—maybe they only want the first two. So we’d want to expose something like this:

But if we just fetched the first two, we might want to paginate through the list as well; once the client fetches the first two friends, they might want to send a second request to ask for the next two friends. How can we enable that behavior?

There are several ways we could do pagination:

The approach described in the first bullet is classic  offset-based pagination . However, this style of pagination can have performance and security downsides, especially for larger data sets. Additionally, if new records are added to the database after the user has made a request for a page of results, then offset calculations for subsequent pages may become ambiguous.

In general, we’ve found that  cursor-based pagination  is the most powerful of those designed. Especially if the cursors are opaque, either offset or ID-based pagination can be implemented using cursor-based pagination (by making the cursor the offset or the ID), and using cursors gives additional flexibility if the pagination model changes in the future. As a reminder that the cursors are opaque and their format should not be relied upon, we suggest base64 encoding them.

But that leads us to a problem—how do we get the cursor from the object? We wouldn’t want the cursor to live on the  User  type; it’s a property of the connection, not of the object. So we might want to introduce a new layer of indirection; our  friends  field should give us a list of edges, and an edge has both a cursor and the underlying node:

The concept of an edge also proves useful if there is information that is specific to the edge, rather than to one of the objects. For example, if we wanted to expose “friendship time” in the API, having it live on the edge is a natural place to put it.

Now we can paginate through the connection using cursors, but how do we know when we reach the end of the connection? We have to keep querying until we get an empty list back, but we’d like for the connection to tell us when we’ve reached the end so we don’t need that additional request. Similarly, what if we want additional information about the connection itself, for example, how many friends does R2-D2 have in total?

To solve both of these problems, our  friends  field can return a connection object. The connection object will be an Object type that has a field for the edges, as well as other information (like total count and information about whether a next page exists). So our final query might look more like this:

Note that we also might include  endCursor  and  startCursor  in this  PageInfo  object. This way, if we don’t need any of the additional information that the edge contains, we don’t need to query for the edges at all, since we got the cursors needed for pagination from  pageInfo . This leads to a potential usability improvement for connections; instead of just exposing the  edges  list, we could also expose a dedicated list of just the nodes, to avoid a layer of indirection.

Clearly, this is more complex than our original design of just having a plural! But by adopting this design, we’ve unlocked several capabilities for the client:

To see this in action, there’s an additional field in the example schema, called  friendsConnection , that exposes all of these concepts:

You can try it out in the example query. Try removing the  after  argument for the  friendsConnection  field to see how the pagination will be affected. Also, try replacing the  edges  field with the helper  friends  field on the connection, which lets you get directly to the list of friends without the additional edge layer of indirection, when appropriate for clients:

To ensure a consistent implementation of this pattern, the Relay project has a formal  specification  you can follow for building GraphQL APIs that use a cursor-based connection pattern - whether or not use you Relay.

To recap these recommendations for paginating fields in a GraphQL schema:

## Code Examples

### Example 1

```
query {
  hero {
    name
    friends(first: 2) {
      name
    }
  }
}
```

### Example 2

```
query {
  hero {
    name
    friends(first: 2) {
      name
    }
  }
}
```

### Example 3

```
query {
  hero {
    name
    friends(first: 2) {
      edges {
        node {
          name
        }
        cursor
      }
    }
  }
}
```

### Example 4

```
query {
  hero {
    name
    friends(first: 2) {
      edges {
        node {
          name
        }
        cursor
      }
    }
  }
}
```

### Example 5

```
query {
  hero {
    name
    friends(first: 2) {
      totalCount
      edges {
        node {
          name
        }
        cursor
      }
      pageInfo {
        endCursor
        hasNextPage
      }
    }
  }
}
```

### Example 6

```
query {
  hero {
    name
    friends(first: 2) {
      totalCount
      edges {
        node {
          name
        }
        cursor
      }
      pageInfo {
        endCursor
        hasNextPage
      }
    }
  }
}
```

### Example 7

```
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  friendsConnection(first: Int, after: ID): FriendsConnection!
  appearsIn: [Episode]!
}
 
type FriendsConnection {
  totalCount: Int
  edges: [FriendsEdge]
  friends: [Character]
  pageInfo: PageInfo!
}
 
type FriendsEdge {
  cursor: ID!
  node: Character
}
 
type PageInfo {
  startCursor: ID
  endCursor: ID
  hasNextPage: Boolean!
}
```

### Example 8

```
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  friendsConnection(first: Int, after: ID): FriendsConnection!
  appearsIn: [Episode]!
}
 
type FriendsConnection {
  totalCount: Int
  edges: [FriendsEdge]
  friends: [Character]
  pageInfo: PageInfo!
}
 
type FriendsEdge {
  cursor: ID!
  node: Character
}
 
type PageInfo {
  startCursor: ID
  endCursor: ID
  hasNextPage: Boolean!
}
```

### Example 9

```
query {
  hero {
    name
    friends(first: 2) {
      name
    }
  }
}
```

### Example 10

```
query {
  hero {
    name
    friends(first: 2) {
      name
    }
  }
}
```

### Example 11

```
query {
  hero {
    name
    friends(first: 2) {
      edges {
        node {
          name
        }
        cursor
      }
    }
  }
}
```

### Example 12

```
query {
  hero {
    name
    friends(first: 2) {
      edges {
        node {
          name
        }
        cursor
      }
    }
  }
}
```

### Example 13

```
query {
  hero {
    name
    friends(first: 2) {
      totalCount
      edges {
        node {
          name
        }
        cursor
      }
      pageInfo {
        endCursor
        hasNextPage
      }
    }
  }
}
```

### Example 14

```
query {
  hero {
    name
    friends(first: 2) {
      totalCount
      edges {
        node {
          name
        }
        cursor
      }
      pageInfo {
        endCursor
        hasNextPage
      }
    }
  }
}
```

### Example 15

```
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  friendsConnection(first: Int, after: ID): FriendsConnection!
  appearsIn: [Episode]!
}
 
type FriendsConnection {
  totalCount: Int
  edges: [FriendsEdge]
  friends: [Character]
  pageInfo: PageInfo!
}
 
type FriendsEdge {
  cursor: ID!
  node: Character
}
 
type PageInfo {
  startCursor: ID
  endCursor: ID
  hasNextPage: Boolean!
}
```

### Example 16

```
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  friendsConnection(first: Int, after: ID): FriendsConnection!
  appearsIn: [Episode]!
}
 
type FriendsConnection {
  totalCount: Int
  edges: [FriendsEdge]
  friends: [Character]
  pageInfo: PageInfo!
}
 
type FriendsEdge {
  cursor: ID!
  node: Character
}
 
type PageInfo {
  startCursor: ID
  endCursor: ID
  hasNextPage: Boolean!
}
```


---

# Authorization

**Source:** https://graphql.org/learn/authorization/

## Table of Contents

- Type and field authorization
- Using type system directives
- Recap
- Authorization
- Type and field authorization
- Using type system directives
- Recap

## Content

Delegate authorization logic to the business logic layer

Most APIs will need to secure access to certain types of data depending on who requested it, and GraphQL is no different. GraphQL execution should begin after  authentication  middleware confirms the user’s identity and passes that information to the GraphQL layer. But after that, you still need to determine if the authenticated user is allowed to view the data provided by the specific fields that were included in the request. On this page, we’ll explore how a GraphQL schema can support authorization.

Authorization is a type of business logic that describes whether a given user/session/context has permission to perform an action or see a piece of data. For example:

“Only authors can see their drafts”

Enforcing this behavior should happen in the  business logic layer . Let’s consider the following  Post  type defined in a schema:

In this example, we can imagine that when a request initially reaches the server, authentication middleware will first check the user’s credentials and add information about their identity to the  context  object of the GraphQL request so that this data is available in every field resolver for the duration of its execution.

If a post’s body should only be visible to the user who authored it, then we will need to check that the authenticated user’s ID matches the post’s  authorId  value. It may be tempting to place authorization logic in the resolver for the post’s  body  field like so:

Notice that we define “author owns a post” by checking whether the post’s  authorId  field equals the current user’s  id . Can you spot the problem? We would need to duplicate this code for each entry point into the service. Then if the authorization logic is not kept perfectly in sync, users could see different data depending on which API they use. Yikes! We can avoid that by having a  single source of truth  for authorization, instead of putting it in the GraphQL layer.

Defining authorization logic inside the resolver is fine when learning GraphQL or prototyping. However, for a production codebase, delegate authorization logic to the business logic layer. Here’s an example of how authorization of the  Post  type’s fields could be implemented separately:

The resolver function for the post’s  body  field would then call a  postRepository  method instead of implementing the authorization logic directly:

In the example above, we see that the business logic layer requires the caller to provide a user object, which is available in the  context  object for the GraphQL request. We recommend passing a fully-hydrated user object instead of an opaque token or API key to your business logic layer. This way, we can handle the distinct concerns of  authentication  and authorization in different stages of the request processing pipeline.

In the example above, we saw how authorization logic can be delegated to the business logic layer through a function that is called in a field resolver. In general, it is recommended to perform all authorization logic in that layer, but if you decide to implement authorization in the GraphQL layer instead then one approach is to use  type system directives .

For example, a directive such as  @auth  could be defined in the schema with arguments that indicate what roles or permissions a user must have to access the data provided by the types and fields where the directive is applied:

It would be up to the GraphQL implementation to determine how an  @auth  directive affects execution when a client makes a request that includes the  body  field for  Post  type. However, the authorization logic should remain delegated to the business logic layer.

To recap these recommendations for authorization in GraphQL:

Delegate authorization logic to the business logic layer

Most APIs will need to secure access to certain types of data depending on who requested it, and GraphQL is no different. GraphQL execution should begin after  authentication  middleware confirms the user’s identity and passes that information to the GraphQL layer. But after that, you still need to determine if the authenticated user is allowed to view the data provided by the specific fields that were included in the request. On this page, we’ll explore how a GraphQL schema can support authorization.

Authorization is a type of business logic that describes whether a given user/session/context has permission to perform an action or see a piece of data. For example:

“Only authors can see their drafts”

Enforcing this behavior should happen in the  business logic layer . Let’s consider the following  Post  type defined in a schema:

In this example, we can imagine that when a request initially reaches the server, authentication middleware will first check the user’s credentials and add information about their identity to the  context  object of the GraphQL request so that this data is available in every field resolver for the duration of its execution.

If a post’s body should only be visible to the user who authored it, then we will need to check that the authenticated user’s ID matches the post’s  authorId  value. It may be tempting to place authorization logic in the resolver for the post’s  body  field like so:

Notice that we define “author owns a post” by checking whether the post’s  authorId  field equals the current user’s  id . Can you spot the problem? We would need to duplicate this code for each entry point into the service. Then if the authorization logic is not kept perfectly in sync, users could see different data depending on which API they use. Yikes! We can avoid that by having a  single source of truth  for authorization, instead of putting it in the GraphQL layer.

Defining authorization logic inside the resolver is fine when learning GraphQL or prototyping. However, for a production codebase, delegate authorization logic to the business logic layer. Here’s an example of how authorization of the  Post  type’s fields could be implemented separately:

The resolver function for the post’s  body  field would then call a  postRepository  method instead of implementing the authorization logic directly:

In the example above, we see that the business logic layer requires the caller to provide a user object, which is available in the  context  object for the GraphQL request. We recommend passing a fully-hydrated user object instead of an opaque token or API key to your business logic layer. This way, we can handle the distinct concerns of  authentication  and authorization in different stages of the request processing pipeline.

In the example above, we saw how authorization logic can be delegated to the business logic layer through a function that is called in a field resolver. In general, it is recommended to perform all authorization logic in that layer, but if you decide to implement authorization in the GraphQL layer instead then one approach is to use  type system directives .

For example, a directive such as  @auth  could be defined in the schema with arguments that indicate what roles or permissions a user must have to access the data provided by the types and fields where the directive is applied:

It would be up to the GraphQL implementation to determine how an  @auth  directive affects execution when a client makes a request that includes the  body  field for  Post  type. However, the authorization logic should remain delegated to the business logic layer.

To recap these recommendations for authorization in GraphQL:

## Code Examples

### Example 1

```
type Post {
  authorId: ID!
  body: String
}
```

### Example 2

```
type Post {
  authorId: ID!
  body: String
}
```

### Example 3

```
function Post_body(obj, args, context, info) {
  // Return the post body only if the user is the post's author
  if (context.user && context.user.id === obj.authorId) {
    return obj.body;
  }
  return null;
}
```

### Example 4

```
function Post_body(obj, args, context, info) {
  // Return the post body only if the user is the post's author
  if (context.user && context.user.id === obj.authorId) {
    return obj.body;
  }
  return null;
}
```

### Example 5

```
// Authorization logic lives inside `postRepository`
export const postRepository = {
  getBody({ user, post }) {
    const isAuthor = user?.id === post.authorId;
    return isAuthor ? post.body : null;
  },
};
```

### Example 6

```
// Authorization logic lives inside `postRepository`
export const postRepository = {
  getBody({ user, post }) {
    const isAuthor = user?.id === post.authorId;
    return isAuthor ? post.body : null;
  },
};
```

### Example 7

```
import { postRepository } from "postRepository";
 
function resolvePostBody(obj, args, context, info) {
  // Return the post body only if the user is the post's author
  return postRepository.getBody({
    user: context.user,
    post: obj,
  });
}
```

### Example 8

```
import { postRepository } from "postRepository";
 
function resolvePostBody(obj, args, context, info) {
  // Return the post body only if the user is the post's author
  return postRepository.getBody({
    user: context.user,
    post: obj,
  });
}
```

### Example 9

```
directive @auth(rule: Rule) on FIELD_DEFINITION
 
enum Rule {
  IS_AUTHOR
}
 
type Post {
  authorId: ID!
  body: String @auth(rule: IS_AUTHOR)
}
```

### Example 10

```
directive @auth(rule: Rule) on FIELD_DEFINITION
 
enum Rule {
  IS_AUTHOR
}
 
type Post {
  authorId: ID!
  body: String @auth(rule: IS_AUTHOR)
}
```

### Example 11

```
type Post {
  authorId: ID!
  body: String
}
```

### Example 12

```
type Post {
  authorId: ID!
  body: String
}
```

### Example 13

```
function Post_body(obj, args, context, info) {
  // Return the post body only if the user is the post's author
  if (context.user && context.user.id === obj.authorId) {
    return obj.body;
  }
  return null;
}
```

### Example 14

```
function Post_body(obj, args, context, info) {
  // Return the post body only if the user is the post's author
  if (context.user && context.user.id === obj.authorId) {
    return obj.body;
  }
  return null;
}
```

### Example 15

```
// Authorization logic lives inside `postRepository`
export const postRepository = {
  getBody({ user, post }) {
    const isAuthor = user?.id === post.authorId;
    return isAuthor ? post.body : null;
  },
};
```

### Example 16

```
// Authorization logic lives inside `postRepository`
export const postRepository = {
  getBody({ user, post }) {
    const isAuthor = user?.id === post.authorId;
    return isAuthor ? post.body : null;
  },
};
```

### Example 17

```
import { postRepository } from "postRepository";
 
function resolvePostBody(obj, args, context, info) {
  // Return the post body only if the user is the post's author
  return postRepository.getBody({
    user: context.user,
    post: obj,
  });
}
```

### Example 18

```
import { postRepository } from "postRepository";
 
function resolvePostBody(obj, args, context, info) {
  // Return the post body only if the user is the post's author
  return postRepository.getBody({
    user: context.user,
    post: obj,
  });
}
```

### Example 19

```
directive @auth(rule: Rule) on FIELD_DEFINITION
 
enum Rule {
  IS_AUTHOR
}
 
type Post {
  authorId: ID!
  body: String @auth(rule: IS_AUTHOR)
}
```

### Example 20

```
directive @auth(rule: Rule) on FIELD_DEFINITION
 
enum Rule {
  IS_AUTHOR
}
 
type Post {
  authorId: ID!
  body: String @auth(rule: IS_AUTHOR)
}
```


---

# Introspection

**Source:** https://graphql.org/learn/introspection/

## Table of Contents

- Type name introspection
- Schema introspection
- Introspection in production
- Next steps
- Introspection
- Type name introspection
- Schema introspection
- Introspection in production
- Next steps

## Content

Learn how to query information about a GraphQL schema

It’s often useful to ask a GraphQL schema for information about what features it supports. GraphQL allows us to do so using the  introspection system .

Introspection queries are special kinds of queries that allow you to learn about a GraphQL API’s schema, and they also help power GraphQL development tools. On this page, we’ll learn how to run different queries to learn more about an underlying schema’s types, fields, and descriptions.

We have already seen an example of introspection on the  Schemas and Types page . When querying a field that returned Union type, we included the  __typename  meta-field directly in a selection set to get the string value of the names of the different types returned by a search query. Let’s look at this example again:

We didn’t add the  __typename  field to our GraphQL API explicitly—the GraphQL specification says that it must be provided to clients by a GraphQL implementation. This field can be queried for any field with an Object, Interface, or Union type as the underlying output type.

Introspection can do more than provide type names in a query. If you designed the type system for a GraphQL API, then you’ll likely already know what types are available. But if you didn’t design it, you can ask GraphQL by querying the  __schema  field, which is always available on the  query  root operation type.

Let’s do so now and ask what types are available in the Star Wars schema:

Wow, that’s a lot of types! What are they? Let’s group them:

Now, let’s try to figure out a good place to start exploring what queries are available. When we designed our type system, we specified what type all queries would start at; let’s ask the introspection system about that:

The result matches what we said in the  type system section —that the  Query  type is where we will start. Note that the naming here was just by convention; we could have named our  Query  type anything else, and it still would have been returned here had we specified it was the starting type for queries. Naming it  Query , though, is a useful convention.

It is often useful to examine one specific type. Let’s take a look at the  Droid  type:

But what if we want to know more about Droid? For example, is it an Interface or Object type?

kind  returns a  __TypeKind  Enum type, one of whose values is  OBJECT . If we asked about  Character  instead we’d find that it is an Interface type:

It’s useful for an Object type to know what fields are available, so let’s ask the introspection system about  Droid :

Those are the fields that we defined on  Droid !

id  looks a bit weird there, it has no name for the type. That’s because it’s a  wrapper type  of kind  NON_NULL . If we queried for  ofType  on that field’s type, we would find the  ID  type there, telling us this is a non-null ID.

Similarly, both  friends  and  appearsIn  have no name, since they are the  LIST  wrapper type. We can query for  ofType  on those types, which will tell us what types are inside the list:

Let’s end with a feature of the introspection system particularly useful for tooling; let’s ask the system for documentation:

As demonstrated above, we can access the documentation about the type system using introspection and create documentation browsers or rich IDE experiences.

This has just scratched the surface of the introspection system; we can query for Enum type values, what Interface types another type implements, and more. We can even introspect on the introspection system itself.

To see an example of a specification-compliant GraphQL query introspection system implemented in code, you can view  src/type/introspection.ts  in the reference implementation.

Introspection is a useful feature of GraphQL, especially for client developers and tooling. However, for APIs intended only for your own applications, it’s typically not needed in production—required operations are usually baked into these applications at build time, making runtime introspection unnecessary.

Disabling introspection in production is common in order to reduce the API’s attack surface. This is often part of a broader API security strategy, which may also include authentication and authorization, operation safe-listing (or a range of alternative protections, such as depth-limiting, breadth-limiting, alias limits, cycle rejection, cost analysis, etc.), execution timeouts, and more.

To recap what we’ve learned about introspection:

Now that you’ve explored the GraphQL type system, how to query data from an API, and what the lifecycle of a request looks like, head over to the  Best Practices  section to learn more about running GraphQL in production.

Learn how to query information about a GraphQL schema

It’s often useful to ask a GraphQL schema for information about what features it supports. GraphQL allows us to do so using the  introspection system .

Introspection queries are special kinds of queries that allow you to learn about a GraphQL API’s schema, and they also help power GraphQL development tools. On this page, we’ll learn how to run different queries to learn more about an underlying schema’s types, fields, and descriptions.

We have already seen an example of introspection on the  Schemas and Types page . When querying a field that returned Union type, we included the  __typename  meta-field directly in a selection set to get the string value of the names of the different types returned by a search query. Let’s look at this example again:

We didn’t add the  __typename  field to our GraphQL API explicitly—the GraphQL specification says that it must be provided to clients by a GraphQL implementation. This field can be queried for any field with an Object, Interface, or Union type as the underlying output type.

Introspection can do more than provide type names in a query. If you designed the type system for a GraphQL API, then you’ll likely already know what types are available. But if you didn’t design it, you can ask GraphQL by querying the  __schema  field, which is always available on the  query  root operation type.

Let’s do so now and ask what types are available in the Star Wars schema:

Wow, that’s a lot of types! What are they? Let’s group them:

Now, let’s try to figure out a good place to start exploring what queries are available. When we designed our type system, we specified what type all queries would start at; let’s ask the introspection system about that:

The result matches what we said in the  type system section —that the  Query  type is where we will start. Note that the naming here was just by convention; we could have named our  Query  type anything else, and it still would have been returned here had we specified it was the starting type for queries. Naming it  Query , though, is a useful convention.

It is often useful to examine one specific type. Let’s take a look at the  Droid  type:

But what if we want to know more about Droid? For example, is it an Interface or Object type?

kind  returns a  __TypeKind  Enum type, one of whose values is  OBJECT . If we asked about  Character  instead we’d find that it is an Interface type:

It’s useful for an Object type to know what fields are available, so let’s ask the introspection system about  Droid :

Those are the fields that we defined on  Droid !

id  looks a bit weird there, it has no name for the type. That’s because it’s a  wrapper type  of kind  NON_NULL . If we queried for  ofType  on that field’s type, we would find the  ID  type there, telling us this is a non-null ID.

Similarly, both  friends  and  appearsIn  have no name, since they are the  LIST  wrapper type. We can query for  ofType  on those types, which will tell us what types are inside the list:

Let’s end with a feature of the introspection system particularly useful for tooling; let’s ask the system for documentation:

As demonstrated above, we can access the documentation about the type system using introspection and create documentation browsers or rich IDE experiences.

This has just scratched the surface of the introspection system; we can query for Enum type values, what Interface types another type implements, and more. We can even introspect on the introspection system itself.

To see an example of a specification-compliant GraphQL query introspection system implemented in code, you can view  src/type/introspection.ts  in the reference implementation.

Introspection is a useful feature of GraphQL, especially for client developers and tooling. However, for APIs intended only for your own applications, it’s typically not needed in production—required operations are usually baked into these applications at build time, making runtime introspection unnecessary.

Disabling introspection in production is common in order to reduce the API’s attack surface. This is often part of a broader API security strategy, which may also include authentication and authorization, operation safe-listing (or a range of alternative protections, such as depth-limiting, breadth-limiting, alias limits, cycle rejection, cost analysis, etc.), execution timeouts, and more.

To recap what we’ve learned about introspection:

Now that you’ve explored the GraphQL type system, how to query data from an API, and what the lifecycle of a request looks like, head over to the  Best Practices  section to learn more about running GraphQL in production.


---

# Handling File Uploads in GraphQL

**Source:** https://graphql.org/learn/file-uploads/

## Table of Contents

- Why uploads are challenging
- Risks to be aware of
  - Memory exhaustion from repeated variables
  - Stream leaks on failed operations
  - Cross-Site Request Forgery (CSRF)
  - Oversized or excess payloads
  - Untrusted file metadata
- Recommendation: Use signed URLs
- If you still choose to support uploads
- Handling File Uploads in GraphQL
- Why uploads are challenging
- Risks to be aware of
  - Memory exhaustion from repeated variables
  - Stream leaks on failed operations
  - Cross-Site Request Forgery (CSRF)
  - Oversized or excess payloads
  - Untrusted file metadata
- Recommendation: Use signed URLs
- If you still choose to support uploads

## Content

GraphQL was not designed with file uploads in mind. While it’s technically possible to implement them, doing so requires
extending the transport layer and introduces several risks, both in security and reliability.

This guide explains why file uploads via GraphQL are problematic and presents safer alternatives.

The  GraphQL specification  is transport-agnostic and serialization-agnostic (though HTTP and JSON are the most prevalent combination seen in the community).
GraphQL was designed to work with relatively small requests from clients, and was not designed with handling binary data in mind.

File uploads, by contrast, typically handle binary data such as images and PDFs — something many encodings, including JSON, cannot handle directly.
One option is to encode within our encoding (e.g. use a base64-encoded string within our JSON), but this is inefficient and is not suitable for larger binary files as it does not support streamed processing easily.
Instead,  multipart/form-data  is a common choice for transferring binary data; but it is not without its own set of complexities.

Supporting uploads over GraphQL usually involves adopting community conventions, the most prevalent of which is the
 GraphQL multipart request specification .
This specification has been successfully implemented in many languages and frameworks, but users
implementing it must pay very close attention to ensure that they do not introduce
security or reliability concerns.

GraphQL operations allow the same variable to be referenced multiple times. If a file upload variable is reused, the underlying
stream may be read multiple times or prematurely drained. This can result in incorrect behavior or memory exhaustion.

A safe practice is to use trusted documents or a validation rule to ensure each upload variable is referenced exactly once.

GraphQL executes in phases: validation, then execution. If validation fails or an authorization check prematurely terminates execution, uploaded
file streams may never be consumed. If your server buffers or retains these streams, it can cause memory leaks.

To avoid this, ensure that all streams are terminated when the request finishes, whether or not they were consumed in resolvers.
An alternative to consider is writing incoming files to temporary storage immediately, and passing references (like filenames) into
resolvers. Ensure this storage is cleaned up after request completion, regardless of success or failure.

multipart/form-data  is classified as a “simple” request in the CORS spec and does not trigger a preflight check. Without
explicit CSRF protection, your GraphQL server may unknowingly accept uploads from malicious origins.

Attackers may submit very large uploads or include extraneous files under unused variable names. Servers that accept and
buffer these can be overwhelmed.

Enforce request size caps and reject any files not explicitly referenced in the map field of the multipart payload.

Information such as file names, MIME types, and contents should never be trusted. To mitigate risk:

The most secure and scalable approach is to avoid uploading files through GraphQL entirely. Instead:

You should ensure that these file uploads are only retained for a short period such that an attacker completing only steps 1 and 2 will not exhaust your storage.
When processing the file upload (step 3), the file should be moved to more permanent storage as appropriate.

This separates responsibilities cleanly, protects your server from binary data handling, and aligns with best practices for
modern web architecture.

If your application truly requires file uploads through GraphQL, proceed with caution. At a minimum, you should:

GraphQL was not designed with file uploads in mind. While it’s technically possible to implement them, doing so requires
extending the transport layer and introduces several risks, both in security and reliability.

This guide explains why file uploads via GraphQL are problematic and presents safer alternatives.

The  GraphQL specification  is transport-agnostic and serialization-agnostic (though HTTP and JSON are the most prevalent combination seen in the community).
GraphQL was designed to work with relatively small requests from clients, and was not designed with handling binary data in mind.

File uploads, by contrast, typically handle binary data such as images and PDFs — something many encodings, including JSON, cannot handle directly.
One option is to encode within our encoding (e.g. use a base64-encoded string within our JSON), but this is inefficient and is not suitable for larger binary files as it does not support streamed processing easily.
Instead,  multipart/form-data  is a common choice for transferring binary data; but it is not without its own set of complexities.

Supporting uploads over GraphQL usually involves adopting community conventions, the most prevalent of which is the
 GraphQL multipart request specification .
This specification has been successfully implemented in many languages and frameworks, but users
implementing it must pay very close attention to ensure that they do not introduce
security or reliability concerns.

GraphQL operations allow the same variable to be referenced multiple times. If a file upload variable is reused, the underlying
stream may be read multiple times or prematurely drained. This can result in incorrect behavior or memory exhaustion.

A safe practice is to use trusted documents or a validation rule to ensure each upload variable is referenced exactly once.

GraphQL executes in phases: validation, then execution. If validation fails or an authorization check prematurely terminates execution, uploaded
file streams may never be consumed. If your server buffers or retains these streams, it can cause memory leaks.

To avoid this, ensure that all streams are terminated when the request finishes, whether or not they were consumed in resolvers.
An alternative to consider is writing incoming files to temporary storage immediately, and passing references (like filenames) into
resolvers. Ensure this storage is cleaned up after request completion, regardless of success or failure.

multipart/form-data  is classified as a “simple” request in the CORS spec and does not trigger a preflight check. Without
explicit CSRF protection, your GraphQL server may unknowingly accept uploads from malicious origins.

Attackers may submit very large uploads or include extraneous files under unused variable names. Servers that accept and
buffer these can be overwhelmed.

Enforce request size caps and reject any files not explicitly referenced in the map field of the multipart payload.

Information such as file names, MIME types, and contents should never be trusted. To mitigate risk:

The most secure and scalable approach is to avoid uploading files through GraphQL entirely. Instead:

You should ensure that these file uploads are only retained for a short period such that an attacker completing only steps 1 and 2 will not exhaust your storage.
When processing the file upload (step 3), the file should be moved to more permanent storage as appropriate.

This separates responsibilities cleanly, protects your server from binary data handling, and aligns with best practices for
modern web architecture.

If your application truly requires file uploads through GraphQL, proceed with caution. At a minimum, you should:


---

# Serving over HTTP

**Source:** https://graphql.org/learn/serving-over-http/

## Table of Contents

- API endpoint
- Where auth happens
- Request format
  - Headers
  - Methods
    - POST
    - GET
    - Choosing an HTTP method
- Response format
  - Headers
  - Body
  - Status Codes
- Server implementations
- Recap
- Serving over HTTP
- API endpoint
- Where auth happens
- Request format
  - Headers
  - Methods
    - POST
    - GET
    - Choosing an HTTP method
- Response format
  - Headers
  - Body
  - Status Codes
- Server implementations
- Recap

## Content

Respond to GraphQL requests using an HTTP server

The GraphQL specification doesn’t require particular client-server protocols when sending API requests and responses, but HTTP is the most common choice because of its ubiquity. On this page, we’ll review some key guidelines to follow when setting up a GraphQL server to operate over HTTP.

Note that the guidelines that follow only apply to stateless query and mutation operations. Visit the  Subscriptions page  for more information on transport protocols that commonly support these requests.

The recommendations on this page align with the detailed  GraphQL-over-HTTP specification  currently in development. Though not yet finalized, this draft specification acts as a single source of truth for GraphQL client and library maintainers, detailing how to expose and consume a GraphQL API using an HTTP transport. Unlike the language specification, adherence is not mandatory, but most implementations are moving towards these standards to maximize interoperability.

HTTP is commonly associated with REST, which uses “resources” as its core concept. In contrast, GraphQL’s conceptual model is an  entity graph . As a result, entities in GraphQL are not identified by URLs. Instead, a GraphQL server operates on a single URL/endpoint, usually  /graphql , and all GraphQL requests for a given service should be directed to this endpoint.

Most modern web frameworks use a pipeline model where requests are passed through a stack of  middleware , also known as  filters  or  plugins . As the request flows through the pipeline, it can be inspected, transformed, modified, or terminated with a response. GraphQL should be placed after all authentication middleware so that you have access to the same session and user information you would in your other HTTP endpoint handlers.

After authentication, the server should not make any  authorization  decisions about the request until GraphQL execution begins. Specifically, field-level authorization should be enforced by the business logic called from resolvers during  GraphQL’s ExecuteRequest() , allowing for a partial response to be generated if authorization-related errors are raised.

GraphQL clients and servers should support JSON for serialization and may support other formats. Clients should also indicate what media types they support in responses using the  Accept  HTTP header. Specifically, the client should include the  application/graphql-response+json  in the  Accept  header.

If no encoding information is included with this header, then it will be assumed to be  utf-8 . However, the server may respond with an error if a client doesn’t provide the  Accept  header with a request.

The  application/graphql-response+json  is described in the draft GraphQL over HTTP specification. To ensure compatibility, if a client sends a request to a legacy GraphQL server before 1st January 2025, the  Accept  header should also include the  application/json  media type as follows:  application/graphql-response+json, application/json

Your GraphQL HTTP server must handle the HTTP  POST  method for query and mutation operations, and may also accept the  GET  method for query operations.

A standard GraphQL POST request should set  application/json  for its  Content-type  header, and include a JSON-encoded body of the following form:

The  query  parameter is required and will contain the source text of a GraphQL document. Note that the term  query  here is a misnomer: the document may contain any valid GraphQL operations (and associated fragments).

The  operationName ,  variables , and  extensions  parameters are optional fields.  operationName  is only required if multiple operations are present in the  query  document.

Note that if the  Content-type  header is missing in the client’s request, then the server should respond with a  4xx  status code. As with the  Accept  header,  utf-8  encoding is assumed for a request body with an  application/json  media type when this information is not explicitly provided.

When receiving an HTTP GET request, the GraphQL document should be provided in the  query  parameter of the query string. For example, if we wanted to execute the following GraphQL query:

This request could be sent via an HTTP GET like so:

Query variables can be sent as a JSON-encoded string in an additional query parameter called  variables . If the query contains several named operations, an  operationName  query parameter can be used to control which one should be executed.

When choosing an HTTP method for a GraphQL request, there are a few points to consider. First, support for HTTP methods other than  POST  will be at the discretion of the GraphQL server, so a client will be limited to the supported verbs.

Additionally, the  GET  HTTP method may only be used for  query  operations, so if a client requests execution of a  mutation  operation then it must use the  POST  method instead.

When servers do support the  GET  method for query operations, a client may be encouraged to leverage this option to facilitate HTTP caching or edge caching in a content delivery network (CDN). However, because GraphQL documents strings may be quite long for complex operations, the query parameters may exceed the limits that browsers and CDNs impose on URL lengths.

In these cases, a common approach is for the server to store identified GraphQL documents using a technique such as  persisted documents ,  automatic persisted queries , or  trusted documents . This allows the client to send a short document identifier instead of the full query text.

The response type should be  application/graphql-response+json  but can be  application/json  to support legacy clients. The server will indicate the media type in the  Content-type  header of the response. It should also indicate the encoding, otherwise  utf-8  will be assumed.

Regardless of the HTTP method used to send the query and variables, the response should be returned in the body of the request in JSON format. As mentioned in the GraphQL specification, a query might result in some data and some errors, and those should be returned in a JSON object of the form:

If no errors were returned, the  errors  field must not be present in the response. If errors occurred before execution could start, the  errors  field must include these errors and the  data  field must not be present in the response. The  extensions  field is optional and information provided here will be at the discretion of the GraphQL implementation.

You can read more about GraphQL spec-compliant response formats on the  Response page .

If a response contains a  data  key and its value is not  null , then the server should respond with a  2xx  status code for either the  application/graphql-response+json  or  application/json  media types. This is the case even if the response includes errors, since HTTP does not yet have a status code representing “partial success.”

For validation errors that prevent the execution of a GraphQL operation, the server will typically send a  400  status code, although some legacy servers may return a  2xx  status code when the  application/json  media type is used.

For legacy servers that respond with the  application/json  media type, the use of  4xx  and  5xx  status codes for valid GraphQL requests that fail to execute is discouraged but may be used depending on the implementation. Because of potential ambiguities regarding the source of the server-side error, a  2xx  code should be used with this media type instead.

However, for responses with the  application/graphql-response+json  media type, the server will reply with a  4xx  or  5xx  status code if a valid GraphQL request fails to execute.

If you use Node.js, we recommend looking at the  list of JS server implementations .

You can view a  list of servers written in many other languages here .

To recap these recommendations for serving GraphQL over HTTP:

Respond to GraphQL requests using an HTTP server

The GraphQL specification doesn’t require particular client-server protocols when sending API requests and responses, but HTTP is the most common choice because of its ubiquity. On this page, we’ll review some key guidelines to follow when setting up a GraphQL server to operate over HTTP.

Note that the guidelines that follow only apply to stateless query and mutation operations. Visit the  Subscriptions page  for more information on transport protocols that commonly support these requests.

The recommendations on this page align with the detailed  GraphQL-over-HTTP specification  currently in development. Though not yet finalized, this draft specification acts as a single source of truth for GraphQL client and library maintainers, detailing how to expose and consume a GraphQL API using an HTTP transport. Unlike the language specification, adherence is not mandatory, but most implementations are moving towards these standards to maximize interoperability.

HTTP is commonly associated with REST, which uses “resources” as its core concept. In contrast, GraphQL’s conceptual model is an  entity graph . As a result, entities in GraphQL are not identified by URLs. Instead, a GraphQL server operates on a single URL/endpoint, usually  /graphql , and all GraphQL requests for a given service should be directed to this endpoint.

Most modern web frameworks use a pipeline model where requests are passed through a stack of  middleware , also known as  filters  or  plugins . As the request flows through the pipeline, it can be inspected, transformed, modified, or terminated with a response. GraphQL should be placed after all authentication middleware so that you have access to the same session and user information you would in your other HTTP endpoint handlers.

After authentication, the server should not make any  authorization  decisions about the request until GraphQL execution begins. Specifically, field-level authorization should be enforced by the business logic called from resolvers during  GraphQL’s ExecuteRequest() , allowing for a partial response to be generated if authorization-related errors are raised.

GraphQL clients and servers should support JSON for serialization and may support other formats. Clients should also indicate what media types they support in responses using the  Accept  HTTP header. Specifically, the client should include the  application/graphql-response+json  in the  Accept  header.

If no encoding information is included with this header, then it will be assumed to be  utf-8 . However, the server may respond with an error if a client doesn’t provide the  Accept  header with a request.

The  application/graphql-response+json  is described in the draft GraphQL over HTTP specification. To ensure compatibility, if a client sends a request to a legacy GraphQL server before 1st January 2025, the  Accept  header should also include the  application/json  media type as follows:  application/graphql-response+json, application/json

Your GraphQL HTTP server must handle the HTTP  POST  method for query and mutation operations, and may also accept the  GET  method for query operations.

A standard GraphQL POST request should set  application/json  for its  Content-type  header, and include a JSON-encoded body of the following form:

The  query  parameter is required and will contain the source text of a GraphQL document. Note that the term  query  here is a misnomer: the document may contain any valid GraphQL operations (and associated fragments).

The  operationName ,  variables , and  extensions  parameters are optional fields.  operationName  is only required if multiple operations are present in the  query  document.

Note that if the  Content-type  header is missing in the client’s request, then the server should respond with a  4xx  status code. As with the  Accept  header,  utf-8  encoding is assumed for a request body with an  application/json  media type when this information is not explicitly provided.

When receiving an HTTP GET request, the GraphQL document should be provided in the  query  parameter of the query string. For example, if we wanted to execute the following GraphQL query:

This request could be sent via an HTTP GET like so:

Query variables can be sent as a JSON-encoded string in an additional query parameter called  variables . If the query contains several named operations, an  operationName  query parameter can be used to control which one should be executed.

When choosing an HTTP method for a GraphQL request, there are a few points to consider. First, support for HTTP methods other than  POST  will be at the discretion of the GraphQL server, so a client will be limited to the supported verbs.

Additionally, the  GET  HTTP method may only be used for  query  operations, so if a client requests execution of a  mutation  operation then it must use the  POST  method instead.

When servers do support the  GET  method for query operations, a client may be encouraged to leverage this option to facilitate HTTP caching or edge caching in a content delivery network (CDN). However, because GraphQL documents strings may be quite long for complex operations, the query parameters may exceed the limits that browsers and CDNs impose on URL lengths.

In these cases, a common approach is for the server to store identified GraphQL documents using a technique such as  persisted documents ,  automatic persisted queries , or  trusted documents . This allows the client to send a short document identifier instead of the full query text.

The response type should be  application/graphql-response+json  but can be  application/json  to support legacy clients. The server will indicate the media type in the  Content-type  header of the response. It should also indicate the encoding, otherwise  utf-8  will be assumed.

Regardless of the HTTP method used to send the query and variables, the response should be returned in the body of the request in JSON format. As mentioned in the GraphQL specification, a query might result in some data and some errors, and those should be returned in a JSON object of the form:

If no errors were returned, the  errors  field must not be present in the response. If errors occurred before execution could start, the  errors  field must include these errors and the  data  field must not be present in the response. The  extensions  field is optional and information provided here will be at the discretion of the GraphQL implementation.

You can read more about GraphQL spec-compliant response formats on the  Response page .

If a response contains a  data  key and its value is not  null , then the server should respond with a  2xx  status code for either the  application/graphql-response+json  or  application/json  media types. This is the case even if the response includes errors, since HTTP does not yet have a status code representing “partial success.”

For validation errors that prevent the execution of a GraphQL operation, the server will typically send a  400  status code, although some legacy servers may return a  2xx  status code when the  application/json  media type is used.

For legacy servers that respond with the  application/json  media type, the use of  4xx  and  5xx  status codes for valid GraphQL requests that fail to execute is discouraged but may be used depending on the implementation. Because of potential ambiguities regarding the source of the server-side error, a  2xx  code should be used with this media type instead.

However, for responses with the  application/graphql-response+json  media type, the server will reply with a  4xx  or  5xx  status code if a valid GraphQL request fails to execute.

If you use Node.js, we recommend looking at the  list of JS server implementations .

You can view a  list of servers written in many other languages here .

To recap these recommendations for serving GraphQL over HTTP:

## Code Examples

### Example 1

```
{
  "query": "...",
  "operationName": "...",
  "variables": { "myVariable": "someValue", ... },
  "extensions": { "myExtension": "someValue", ... }
}
```

### Example 2

```
{
  "query": "...",
  "operationName": "...",
  "variables": { "myVariable": "someValue", ... },
  "extensions": { "myExtension": "someValue", ... }
}
```

### Example 3

```
{
  me {
    name
  }
}
```

### Example 4

```
{
  me {
    name
  }
}
```

### Example 5

```
http://myapi/graphql?query={me{name}}
```

### Example 6

```
http://myapi/graphql?query={me{name}}
```

### Example 7

```
{
  "data": { ... },
  "errors": [ ... ],
  "extensions": { ... }
}
```

### Example 8

```
{
  "data": { ... },
  "errors": [ ... ],
  "extensions": { ... }
}
```

### Example 9

```
{
  "query": "...",
  "operationName": "...",
  "variables": { "myVariable": "someValue", ... },
  "extensions": { "myExtension": "someValue", ... }
}
```

### Example 10

```
{
  "query": "...",
  "operationName": "...",
  "variables": { "myVariable": "someValue", ... },
  "extensions": { "myExtension": "someValue", ... }
}
```

### Example 11

```
{
  me {
    name
  }
}
```

### Example 12

```
{
  me {
    name
  }
}
```

### Example 13

```
http://myapi/graphql?query={me{name}}
```

### Example 14

```
http://myapi/graphql?query={me{name}}
```

### Example 15

```
{
  "data": { ... },
  "errors": [ ... ],
  "extensions": { ... }
}
```

### Example 16

```
{
  "data": { ... },
  "errors": [ ... ],
  "extensions": { ... }
}
```


---

# Execution

**Source:** https://graphql.org/learn/execution/

## Table of Contents

- Field resolvers
- Root fields and resolvers
- Asynchronous resolvers
- Trivial resolvers
- Scalar coercion
- List resolvers
- Producing the result
- Next steps
- Execution
- Field resolvers
- Root fields and resolvers
- Asynchronous resolvers
- Trivial resolvers
- Scalar coercion
- List resolvers
- Producing the result
- Next steps

## Content

Learn how GraphQL provides data for requested fields

After a parsed document is  validated , a client’s request will be  executed  by the GraphQL server and the returned result will mirror the shape of the requested query. On this page, you’ll learn about the execution phase of GraphQL operations where data is read from or written to an existing source depending on what fields are requested by a client.

GraphQL cannot execute operations without a type system, so let’s use an example type system to illustrate executing a query. This is a part of the same type system used throughout the examples in this guide:

To describe what happens when a query is executed, let’s walk through an example:

You can think of each field in a GraphQL query as a function or method of the previous type which returns the next type. This is exactly how GraphQL works—each field on each type is backed by a  resolver function  that is written by the GraphQL server developer. When a field is executed, the corresponding resolver is called to produce the next value.

If a field produces a scalar value like a string or number, then the execution completes. However, if a field produces an object value then the query will contain another selection of fields that apply to that object. This continues until the leaf values are reached. GraphQL queries always end at Scalar or Enum types.

At the top level of every GraphQL server is an Object type that represents the possible entry points into the GraphQL API, it’s often called “the  query  root operation type” or the  Query  type. If the API supports mutations to write data and subscriptions to fetch real-time data as well, then it will have  Mutation  and  Subscription  types that expose fields to perform these kinds of operations too. You can learn more about these types on the  Schema and Types page .

In this example, our  Query  type provides a field called  human  which accepts the argument  id . The resolver function for this field likely accesses a database and then constructs and returns a  Human  type:

This example is written in JavaScript, however GraphQL servers can be built in  many different languages . In the reference implementation, a resolver function receives four arguments:

Note that while a query operation could technically write data to the underlying data system during its execution, mutation operations are conventionally used for requests that produce side effects during their execution. And because mutation operations produce side effects, GraphQL implementations can be expected to execute these fields serially.

Let’s take a closer look at what’s happening in this resolver function:

The  id  argument in the GraphQL query specifies the user whose data is requested, while  context  provides access to retrieve this data from a database. Since loading from a database is an asynchronous operation, this returns a  Promise . In JavaScript, Promises are used to work with asynchronous values, but the same concept exists in many languages, often called  Futures ,  Tasks , or  Deferred . When the database returns the data, we can construct and return a new  Human  object.

Notice that while the resolver function needs to be aware of Promises, the GraphQL query does not. It simply expects the  human  field to return something that can be further resolved to a scalar  name  value. During execution, GraphQL will wait for Promises, Futures, and Tasks to be completed before continuing and will do so with optimal concurrency.

Now that a  Human  object is available, GraphQL execution can continue with the fields requested for this type:

A GraphQL server is powered by a type system that is used to determine what to do next. Even before the  human  field returns anything, GraphQL knows the next step will be to resolve fields on the  Human  type since the type system tells it that the  human  field will return this output type.

Resolving the name in this case is straightforward. The name resolver function is called and the  obj  argument is the new  Human  object returned from the previous field. In this case, we expect this object to have a  name  property, which we can read and return directly.

Many GraphQL libraries let you omit resolvers this simple, assuming that if a resolver isn’t provided for a field, a property of the same name should be read and returned.

While the  name  field is being resolved, the  appearsIn  and  starships  fields can be resolved concurrently. The  appearsIn  field could also have a trivial resolver, but let’s take a closer look:

Notice that our type system claims  appearsIn  will return Enum types with known values, however, this function is returning numbers! Indeed if we look up at the result we’ll see that the appropriate values for the Enum type are being returned. What’s going on?

This is an example of  scalar coercion . The type system knows what to expect and will convert the values returned by a resolver function into something that upholds the API contract. In this case, there may be an Enum type defined on our server that uses numbers like  4 ,  5 , and  6  internally, but represents them as the expected values in the GraphQL type system.

We’ve already seen some of what happens when a field returns a list of things with the  appearsIn  field above. It returned a  List type  containing Enum type values, and since that’s what the type system expected, each item in the list was coerced to the appropriate value. What happens when the  starships  field is resolved?

The resolver for this field is not just returning a Promise, it’s returning a  list  of Promises. The  Human  object had a list of IDs of the  Starships  they piloted, but we need to load all of those IDs to get real Starship objects.

GraphQL will wait for all of these Promises concurrently before continuing, and when left with a list of objects, it will continue yet again to load the  name  field on each of these items concurrently.

As each field is resolved, the resulting value is placed into a key-value map with the field name (or alias) as the key and the resolved value as the value. This continues from the bottom leaf fields of the query back up to the original field on the root  Query  type. Collectively these produce a structure that mirrors the original query which can then be sent (typically as JSON) to the client that requested it.

Let’s take one last look at the original query to see how all these resolving functions produce a result:

As we can see, each field in the nested selection sets resolves to a scalar leaf value during execution of the query.

To recap what we’ve learned about execution:

Now that we understand how operations are executed, we can move to the last stage of the lifecycle of a GraphQL request where the  response  is delivered to a client.

Learn how GraphQL provides data for requested fields

After a parsed document is  validated , a client’s request will be  executed  by the GraphQL server and the returned result will mirror the shape of the requested query. On this page, you’ll learn about the execution phase of GraphQL operations where data is read from or written to an existing source depending on what fields are requested by a client.

GraphQL cannot execute operations without a type system, so let’s use an example type system to illustrate executing a query. This is a part of the same type system used throughout the examples in this guide:

To describe what happens when a query is executed, let’s walk through an example:

You can think of each field in a GraphQL query as a function or method of the previous type which returns the next type. This is exactly how GraphQL works—each field on each type is backed by a  resolver function  that is written by the GraphQL server developer. When a field is executed, the corresponding resolver is called to produce the next value.

If a field produces a scalar value like a string or number, then the execution completes. However, if a field produces an object value then the query will contain another selection of fields that apply to that object. This continues until the leaf values are reached. GraphQL queries always end at Scalar or Enum types.

At the top level of every GraphQL server is an Object type that represents the possible entry points into the GraphQL API, it’s often called “the  query  root operation type” or the  Query  type. If the API supports mutations to write data and subscriptions to fetch real-time data as well, then it will have  Mutation  and  Subscription  types that expose fields to perform these kinds of operations too. You can learn more about these types on the  Schema and Types page .

In this example, our  Query  type provides a field called  human  which accepts the argument  id . The resolver function for this field likely accesses a database and then constructs and returns a  Human  type:

This example is written in JavaScript, however GraphQL servers can be built in  many different languages . In the reference implementation, a resolver function receives four arguments:

Note that while a query operation could technically write data to the underlying data system during its execution, mutation operations are conventionally used for requests that produce side effects during their execution. And because mutation operations produce side effects, GraphQL implementations can be expected to execute these fields serially.

Let’s take a closer look at what’s happening in this resolver function:

The  id  argument in the GraphQL query specifies the user whose data is requested, while  context  provides access to retrieve this data from a database. Since loading from a database is an asynchronous operation, this returns a  Promise . In JavaScript, Promises are used to work with asynchronous values, but the same concept exists in many languages, often called  Futures ,  Tasks , or  Deferred . When the database returns the data, we can construct and return a new  Human  object.

Notice that while the resolver function needs to be aware of Promises, the GraphQL query does not. It simply expects the  human  field to return something that can be further resolved to a scalar  name  value. During execution, GraphQL will wait for Promises, Futures, and Tasks to be completed before continuing and will do so with optimal concurrency.

Now that a  Human  object is available, GraphQL execution can continue with the fields requested for this type:

A GraphQL server is powered by a type system that is used to determine what to do next. Even before the  human  field returns anything, GraphQL knows the next step will be to resolve fields on the  Human  type since the type system tells it that the  human  field will return this output type.

Resolving the name in this case is straightforward. The name resolver function is called and the  obj  argument is the new  Human  object returned from the previous field. In this case, we expect this object to have a  name  property, which we can read and return directly.

Many GraphQL libraries let you omit resolvers this simple, assuming that if a resolver isn’t provided for a field, a property of the same name should be read and returned.

While the  name  field is being resolved, the  appearsIn  and  starships  fields can be resolved concurrently. The  appearsIn  field could also have a trivial resolver, but let’s take a closer look:

Notice that our type system claims  appearsIn  will return Enum types with known values, however, this function is returning numbers! Indeed if we look up at the result we’ll see that the appropriate values for the Enum type are being returned. What’s going on?

This is an example of  scalar coercion . The type system knows what to expect and will convert the values returned by a resolver function into something that upholds the API contract. In this case, there may be an Enum type defined on our server that uses numbers like  4 ,  5 , and  6  internally, but represents them as the expected values in the GraphQL type system.

We’ve already seen some of what happens when a field returns a list of things with the  appearsIn  field above. It returned a  List type  containing Enum type values, and since that’s what the type system expected, each item in the list was coerced to the appropriate value. What happens when the  starships  field is resolved?

The resolver for this field is not just returning a Promise, it’s returning a  list  of Promises. The  Human  object had a list of IDs of the  Starships  they piloted, but we need to load all of those IDs to get real Starship objects.

GraphQL will wait for all of these Promises concurrently before continuing, and when left with a list of objects, it will continue yet again to load the  name  field on each of these items concurrently.

As each field is resolved, the resulting value is placed into a key-value map with the field name (or alias) as the key and the resolved value as the value. This continues from the bottom leaf fields of the query back up to the original field on the root  Query  type. Collectively these produce a structure that mirrors the original query which can then be sent (typically as JSON) to the client that requested it.

Let’s take one last look at the original query to see how all these resolving functions produce a result:

As we can see, each field in the nested selection sets resolves to a scalar leaf value during execution of the query.

To recap what we’ve learned about execution:

Now that we understand how operations are executed, we can move to the last stage of the lifecycle of a GraphQL request where the  response  is delivered to a client.

## Code Examples

### Example 1

```
type Query {
  human(id: ID!): Human
}
 
type Human {
  name: String
  appearsIn: [Episode]
  starships: [Starship]
}
 
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
 
type Starship {
  name: String
}
```

### Example 2

```
type Query {
  human(id: ID!): Human
}
 
type Human {
  name: String
  appearsIn: [Episode]
  starships: [Starship]
}
 
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
 
type Starship {
  name: String
}
```

### Example 3

```
function resolveHumanQuery(obj, args, context, info) {
  return context.db.loadHumanByID(args.id).then(userData => new Human(userData));
}
```

### Example 4

```
function resolveHumanQuery(obj, args, context, info) {
  return context.db.loadHumanByID(args.id).then(userData => new Human(userData));
}
```

### Example 5

```
function resolveHuman(obj, args, context, info) {
  return context.db.loadHumanByID(args.id).then(userData => new Human(userData));
}
```

### Example 6

```
function resolveHuman(obj, args, context, info) {
  return context.db.loadHumanByID(args.id).then(userData => new Human(userData));
}
```

### Example 7

```
function resolveHumanName(obj, args, context, info) {
  return obj.name;
}
```

### Example 8

```
function resolveHumanName(obj, args, context, info) {
  return obj.name;
}
```

### Example 9

```
const Human = {
  appearsIn(obj) {
    return obj.appearsIn; // e.g. [4, 5, 6]
  },
};
```

### Example 10

```
const Human = {
  appearsIn(obj) {
    return obj.appearsIn; // e.g. [4, 5, 6]
  },
};
```

### Example 11

```
function resolveHumanStarships(obj, args, context, info) {
  return Promise.all(
    obj.starshipIDs.map(id =>
      context.db.loadStarshipByID(id).then(shipData => new Starship(shipData))
    )
  );
}
```

### Example 12

```
function resolveHumanStarships(obj, args, context, info) {
  return Promise.all(
    obj.starshipIDs.map(id =>
      context.db.loadStarshipByID(id).then(shipData => new Starship(shipData))
    )
  );
}
```

### Example 13

```
type Query {
  human(id: ID!): Human
}
 
type Human {
  name: String
  appearsIn: [Episode]
  starships: [Starship]
}
 
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
 
type Starship {
  name: String
}
```

### Example 14

```
type Query {
  human(id: ID!): Human
}
 
type Human {
  name: String
  appearsIn: [Episode]
  starships: [Starship]
}
 
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
 
type Starship {
  name: String
}
```

### Example 15

```
function resolveHumanQuery(obj, args, context, info) {
  return context.db.loadHumanByID(args.id).then(userData => new Human(userData));
}
```

### Example 16

```
function resolveHumanQuery(obj, args, context, info) {
  return context.db.loadHumanByID(args.id).then(userData => new Human(userData));
}
```

### Example 17

```
function resolveHuman(obj, args, context, info) {
  return context.db.loadHumanByID(args.id).then(userData => new Human(userData));
}
```

### Example 18

```
function resolveHuman(obj, args, context, info) {
  return context.db.loadHumanByID(args.id).then(userData => new Human(userData));
}
```

### Example 19

```
function resolveHumanName(obj, args, context, info) {
  return obj.name;
}
```

### Example 20

```
function resolveHumanName(obj, args, context, info) {
  return obj.name;
}
```

### Example 21

```
const Human = {
  appearsIn(obj) {
    return obj.appearsIn; // e.g. [4, 5, 6]
  },
};
```

### Example 22

```
const Human = {
  appearsIn(obj) {
    return obj.appearsIn; // e.g. [4, 5, 6]
  },
};
```

### Example 23

```
function resolveHumanStarships(obj, args, context, info) {
  return Promise.all(
    obj.starshipIDs.map(id =>
      context.db.loadStarshipByID(id).then(shipData => new Starship(shipData))
    )
  );
}
```

### Example 24

```
function resolveHumanStarships(obj, args, context, info) {
  return Promise.all(
    obj.starshipIDs.map(id =>
      context.db.loadStarshipByID(id).then(shipData => new Starship(shipData))
    )
  );
}
```


---

# Mutations

**Source:** https://graphql.org/learn/mutations/

## Table of Contents

- Add new data
- Update existing data
- Purpose-built mutations
- Remove existing data
- Multiple fields in mutations
- Next steps
- Mutations
- Add new data
- Update existing data
- Purpose-built mutations
- Remove existing data
- Multiple fields in mutations
- Next steps

## Content

Learn how to modify data with a GraphQL server

Most discussions of GraphQL focus on data fetching, but any complete data platform needs a way to modify server-side data as well.

In REST, any request might cause some side-effects on the server, but by convention, it’s suggested that one doesn’t use  GET  requests to modify data. GraphQL is similar—technically any field resolver could be implemented to cause a data write—but the  GraphQL specification states  that “the resolution of fields other than top-level mutation fields must always be side effect-free and idempotent.” Thus, for any spec-compliant GraphQL schemas, only the top-level fields in mutation operations are allowed to cause side effects.

On this page, you’ll learn how to use mutation operations to write data using GraphQL, and do so in a way that supports client use cases.

All of the features of GraphQL operations that apply to queries also apply to mutations, so review the  Queries  page first before proceeding.

When creating new data with a REST API, you would send a  POST  request to a specific endpoint and include information about the entities to be created in the body of the request. GraphQL takes a different approach.

Let’s look at an example mutation that’s defined in our schema:

Like queries, mutation fields are added to one of the  root operation types  that provide an entry point to the API. In this case, we define the  createReview  field on the  Mutation  type.

Mutation fields can also accept arguments and you might notice that the  review  argument has an input type set to  ReviewInput . This is known as  Input Object type , which allows us to pass in a structured object containing information the mutation can use instead of individual scalar values only.

As with queries, if the mutation field returns an Object type then you must specify a selection set of its fields in the operation:

While the  createReview  field could be defined with any valid output type in the schema, it’s conventional to specify an output type that relates to whatever is modified during the mutation—in this case, the  Review  type. This can be useful for clients that need to fetch the new state of an object after an update.

Recall that GraphQL is meant to work with your existing code and data, so the actual creation of the review is up to you when clients send this operation to the GraphQL server. A hypothetical function that writes the new review to a database during a  createReview  mutation might look like this:

You can learn more about how GraphQL provides data for fields on the  Execution page .

Similarly, we use mutations to update existing data. To change a human’s name, we’ll define a new mutation field and set that field’s output type to the  Human  type so we can return the updated human’s information to client after the server successfully writes the data:

This operation will update Luke Skywalker’s name:

The previous example demonstrates an important distinction from REST. To update a human’s properties using a REST API, you would likely send any updated data to a generalized endpoint for that resource using a  PATCH  request. With GraphQL, instead of simply creating an  updateHuman  mutation, you can define more specific mutation fields such as  updateHumanName that are designed for the task at hand.

Purpose-built mutation fields can help make a schema more expressive by allowing the input types for field arguments to be Non-Null types (a generic  updateHuman  mutation would likely need to accept many nullable arguments to handle different update scenarios). Defining this requirement in the schema also eliminates the need for other runtime logic to determine that the appropriate values were submitted to perform the client’s desired write operation.

GraphQL also allows us to express relationships between data that would be more difficult to model semantically with a basic CRUD-style request. For example, a user may wish to save a personal rating for a film. While the rating belongs to the user and doesn’t modify anything related to a film itself, we can ergonomically associate it with a  Film  object as follows:

As a general rule, a GraphQL API should be designed to help clients get and modify data in a way that makes sense for them, so the fields defined in a schema should be informed by those use cases.

Just as we can send a  DELETE  request to delete a resource with a REST API, we can use mutations to delete some existing data as well by defining another field on the  Mutation  type:

Here’s an example of the new mutation field:

As with mutations that create and update data, the GraphQL specification doesn’t indicate what should be returned from a successful mutation operation that deletes data, but we do have to specify some type as an output type for the field in the schema. Commonly, the deleted entity’s ID or a payload object containing data about the entity will be used to indicate that the operation was successful.

A mutation can contain multiple fields, just like a query. There’s one important distinction between queries and mutations, other than the name:

While query fields are executed in parallel, mutation fields run in series.

Let’s look at an example:

Serial execution  of these top-level fields means that if we send two  deleteStarship  mutations in one request, the first is guaranteed to finish before the second begins, ensuring that we don’t end up in a race condition with ourselves.

Note that serial execution of top-level  Mutation  fields differs from the notion of a database transaction. Some mutation fields may resolve successfully while others return errors, and there’s no way for GraphQL to revert the successful portions of the operation when this happens. So in the previous example, if the first starship is removed successfully but the  secondShip  field raises an error, there is no built-in way for GraphQL to revert the execution of the  firstShip  field afterward.

To recap what we’ve learned about mutations:

Now that we know how to use a GraphQL server to read and write data, we’re ready to learn how to fetch data in real time using  subscriptions . You may also wish to learn more about how GraphQL queries and mutations can be  served over HTTP .

Learn how to modify data with a GraphQL server

Most discussions of GraphQL focus on data fetching, but any complete data platform needs a way to modify server-side data as well.

In REST, any request might cause some side-effects on the server, but by convention, it’s suggested that one doesn’t use  GET  requests to modify data. GraphQL is similar—technically any field resolver could be implemented to cause a data write—but the  GraphQL specification states  that “the resolution of fields other than top-level mutation fields must always be side effect-free and idempotent.” Thus, for any spec-compliant GraphQL schemas, only the top-level fields in mutation operations are allowed to cause side effects.

On this page, you’ll learn how to use mutation operations to write data using GraphQL, and do so in a way that supports client use cases.

All of the features of GraphQL operations that apply to queries also apply to mutations, so review the  Queries  page first before proceeding.

When creating new data with a REST API, you would send a  POST  request to a specific endpoint and include information about the entities to be created in the body of the request. GraphQL takes a different approach.

Let’s look at an example mutation that’s defined in our schema:

Like queries, mutation fields are added to one of the  root operation types  that provide an entry point to the API. In this case, we define the  createReview  field on the  Mutation  type.

Mutation fields can also accept arguments and you might notice that the  review  argument has an input type set to  ReviewInput . This is known as  Input Object type , which allows us to pass in a structured object containing information the mutation can use instead of individual scalar values only.

As with queries, if the mutation field returns an Object type then you must specify a selection set of its fields in the operation:

While the  createReview  field could be defined with any valid output type in the schema, it’s conventional to specify an output type that relates to whatever is modified during the mutation—in this case, the  Review  type. This can be useful for clients that need to fetch the new state of an object after an update.

Recall that GraphQL is meant to work with your existing code and data, so the actual creation of the review is up to you when clients send this operation to the GraphQL server. A hypothetical function that writes the new review to a database during a  createReview  mutation might look like this:

You can learn more about how GraphQL provides data for fields on the  Execution page .

Similarly, we use mutations to update existing data. To change a human’s name, we’ll define a new mutation field and set that field’s output type to the  Human  type so we can return the updated human’s information to client after the server successfully writes the data:

This operation will update Luke Skywalker’s name:

The previous example demonstrates an important distinction from REST. To update a human’s properties using a REST API, you would likely send any updated data to a generalized endpoint for that resource using a  PATCH  request. With GraphQL, instead of simply creating an  updateHuman  mutation, you can define more specific mutation fields such as  updateHumanName that are designed for the task at hand.

Purpose-built mutation fields can help make a schema more expressive by allowing the input types for field arguments to be Non-Null types (a generic  updateHuman  mutation would likely need to accept many nullable arguments to handle different update scenarios). Defining this requirement in the schema also eliminates the need for other runtime logic to determine that the appropriate values were submitted to perform the client’s desired write operation.

GraphQL also allows us to express relationships between data that would be more difficult to model semantically with a basic CRUD-style request. For example, a user may wish to save a personal rating for a film. While the rating belongs to the user and doesn’t modify anything related to a film itself, we can ergonomically associate it with a  Film  object as follows:

As a general rule, a GraphQL API should be designed to help clients get and modify data in a way that makes sense for them, so the fields defined in a schema should be informed by those use cases.

Just as we can send a  DELETE  request to delete a resource with a REST API, we can use mutations to delete some existing data as well by defining another field on the  Mutation  type:

Here’s an example of the new mutation field:

As with mutations that create and update data, the GraphQL specification doesn’t indicate what should be returned from a successful mutation operation that deletes data, but we do have to specify some type as an output type for the field in the schema. Commonly, the deleted entity’s ID or a payload object containing data about the entity will be used to indicate that the operation was successful.

A mutation can contain multiple fields, just like a query. There’s one important distinction between queries and mutations, other than the name:

While query fields are executed in parallel, mutation fields run in series.

Let’s look at an example:

Serial execution  of these top-level fields means that if we send two  deleteStarship  mutations in one request, the first is guaranteed to finish before the second begins, ensuring that we don’t end up in a race condition with ourselves.

Note that serial execution of top-level  Mutation  fields differs from the notion of a database transaction. Some mutation fields may resolve successfully while others return errors, and there’s no way for GraphQL to revert the successful portions of the operation when this happens. So in the previous example, if the first starship is removed successfully but the  secondShip  field raises an error, there is no built-in way for GraphQL to revert the execution of the  firstShip  field afterward.

To recap what we’ve learned about mutations:

Now that we know how to use a GraphQL server to read and write data, we’re ready to learn how to fetch data in real time using  subscriptions . You may also wish to learn more about how GraphQL queries and mutations can be  served over HTTP .

## Code Examples

### Example 1

```
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
 
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 2

```
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
 
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 3

```
const Mutation = {
  createReview(_obj, args, context, _info) {
    return context.db
      .createNewReview(args.episode, args.review)
      .then(reviewData => new Review(reviewData));
  },
};
```

### Example 4

```
const Mutation = {
  createReview(_obj, args, context, _info) {
    return context.db
      .createNewReview(args.episode, args.review)
      .then(reviewData => new Review(reviewData));
  },
};
```

### Example 5

```
type Mutation {
  updateHumanName(id: ID!, name: String!): Human
}
```

### Example 6

```
type Mutation {
  updateHumanName(id: ID!, name: String!): Human
}
```

### Example 7

```
type Mutation {
  deleteStarship(id: ID!): ID!
}
```

### Example 8

```
type Mutation {
  deleteStarship(id: ID!): ID!
}
```

### Example 9

```
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
 
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 10

```
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
 
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 11

```
const Mutation = {
  createReview(_obj, args, context, _info) {
    return context.db
      .createNewReview(args.episode, args.review)
      .then(reviewData => new Review(reviewData));
  },
};
```

### Example 12

```
const Mutation = {
  createReview(_obj, args, context, _info) {
    return context.db
      .createNewReview(args.episode, args.review)
      .then(reviewData => new Review(reviewData));
  },
};
```

### Example 13

```
type Mutation {
  updateHumanName(id: ID!, name: String!): Human
}
```

### Example 14

```
type Mutation {
  updateHumanName(id: ID!, name: String!): Human
}
```

### Example 15

```
type Mutation {
  deleteStarship(id: ID!): ID!
}
```

### Example 16

```
type Mutation {
  deleteStarship(id: ID!): ID!
}
```


---

# GraphQL federation

**Source:** https://graphql.org/learn/federation/

## Table of Contents

- What is federation?
- What is federated GraphQL?
- How federation works in GraphQL
  - Schema composition
  - Gateway
- Benefits of GraphQL federation
  - Domain-driven development
  - Service integrity protection
  - Scalability and performance
  - Single, unified API
- Is GraphQL federation right for you?
- Getting started with GraphQL federation
- GraphQL federation
- What is federation?
- What is federated GraphQL?
- How federation works in GraphQL
  - Schema composition
  - Gateway
- Benefits of GraphQL federation
  - Domain-driven development
  - Service integrity protection
  - Scalability and performance
  - Single, unified API
- Is GraphQL federation right for you?
- Getting started with GraphQL federation

## Content

An alternative design approach to the classical monolith, often described as microservices, emphasizes breaking down complex systems into smaller, independently managed components. In some ways, GraphQL federation is like microservices for GraphQL - an architectural pattern that has found particular resonance in the GraphQL ecosystem.

GraphQL federation gained widespread adoption after
 Apollo GraphQL introduced Apollo Federation in 2019 .
Their implementation has become a reference point for the GraphQL community, helping establish
federation as a standard architectural pattern for building a distributed graph in the GraphQL
ecosystem.

With more companies and developers seeing the benefits of building a distributed GraphQL schema with
federation, the GraphQL ecosystem is now moving towards standardization of federation patterns. The
GraphQL Foundation’s
 Composite Schema Working Group , which includes
engineers from various organizations across the industry including
 Apollo GraphQL ,  ChilliCream ,
 Graphile ,  Hasura ,
 Netflix  and  The Guild , is actively working on
creating
 an official specification for GraphQL Federation .
This effort aims to standardize how GraphQL services can be composed and executed across distributed
systems, while ensuring room for innovation and different implementations.

Architecturally, federation is an approach to organizing and managing distributed systems. At its
core, federation allows autonomous components to work together while maintaining their independence.
Think of it like a federal government system: individual states maintain their sovereignty while
cooperating under a central authority for shared concerns.

In software architecture, federation enables organizations to:

Think of the “Login with Google” or “Login with Facebook” buttons you see on websites. This is
federation in action: you can use your Google or Facebook account to log into many different
websites, even though each company manages their own login system separately.

GraphQL federation applies those principles to GraphQL APIs. It enables organizations to build a
unified GraphQL schema from multiple independent services (most often called subgraphs), each
responsible for its portion of the application’s data graph.

Consider an e-commerce platform: You might have separate teams managing products, user accounts, and
order processing. With GraphQL federation, each team can:

The magic happens through a federated gateway that acts as the central coordinator, composing these
separate schemas into a unified schema that clients can query.

The federation process involves several key components:

Let’s break down schema composition in GraphQL federation with more detail and examples. Schema
composition is the process where multiple subgraph schemas are combined into one unified schema.
It’s more complex than simply merging schemas together, though, because it needs to handle
relationships, detect incompatibilities, and ensure types are properly connected across services and
subgraphs.

Based on the examples we provided before, here’s the unified schema GraphQL clients will see and can
query:

This unified schema combines types and fields from all three subgraphs (Users, Orders, and
Products), allowing clients to seamlessly query across these domains.

The federation gateway is the entry point to your distributed data graph. It presents a unified
GraphQL endpoint to clients and handles the complexity of routing queries to the appropriate
subgraphs and assembling the results, and often provides caching and performance optimizations.

Client

Gateway

Users Service

Orders Service

Products Service

Take the following query as an example:

The gateway will route parts of the query to the appropriate subgraphs, collect the results, and
assemble them into a single response that the client can consume.

Teams can work independently on their services while contributing to a cohesive API. This autonomy
accelerates development and reduces coordination overhead.

The schema composition step verifies integration between services by ensuring that changes in
individual subgraphs do not conflict with other subgraphs.

Subgraphs and services can be scaled independently based on their specific requirements. The product
catalog might need different scaling characteristics than the order processing system.

Thanks to GraphQL, clients get a single endpoint with unified schema spanning multiple subgraphs.
The complexity of distributed systems is hidden. The gateway ensures every query reaches its
destination and returns with the right data.

GraphQL federation aligns naturally with Domain Driven Design (DDD) principles by allowing teams to
maintain clear boundaries around their domains, while maintaining explicit integration points
through the GraphQL schema. It is particularly valuable for organizations where multiple teams need
to work independently on different parts of the GraphQL API, with the flexibility to use different
technologies and programming languages.

However, implementing federation requires substantial infrastructure support, including a dedicated
team to manage the gateway, schema registry, to help connect subgraphs to the federated API and
guide teams on best practices.

Before adopting federation, it’s crucial to consider whether your organization truly needs this
level of complexity. You can start with a monolithic setup and transition to federation as your needs
evolve, rather than implementing it prematurely.

Meta (formerly Facebook),  where GraphQL was created , has continued to
use a monolithic GraphQL API since 2012. However, companies like
 Netflix ,
 Expedia Group ,
 Volvo ,
and  Booking  have adopted federation to
better align with their organizational structures and microservices architecture.

As you see, some of the world’s largest industry leaders have successfully federated their GraphQL
APIs, proving that it works reliably for production applications at an extraordinary scale.

If you’re considering adopting GraphQL federation, here are some steps to get started:

When migrating from a monolithic to federated GraphQL API, the simplest starting point is to treat
your existing schema as your first subgraph. From there, you can follow the steps above to gradually
decompose your schema into smaller pieces.

An alternative design approach to the classical monolith, often described as microservices, emphasizes breaking down complex systems into smaller, independently managed components. In some ways, GraphQL federation is like microservices for GraphQL - an architectural pattern that has found particular resonance in the GraphQL ecosystem.

GraphQL federation gained widespread adoption after
 Apollo GraphQL introduced Apollo Federation in 2019 .
Their implementation has become a reference point for the GraphQL community, helping establish
federation as a standard architectural pattern for building a distributed graph in the GraphQL
ecosystem.

With more companies and developers seeing the benefits of building a distributed GraphQL schema with
federation, the GraphQL ecosystem is now moving towards standardization of federation patterns. The
GraphQL Foundation’s
 Composite Schema Working Group , which includes
engineers from various organizations across the industry including
 Apollo GraphQL ,  ChilliCream ,
 Graphile ,  Hasura ,
 Netflix  and  The Guild , is actively working on
creating
 an official specification for GraphQL Federation .
This effort aims to standardize how GraphQL services can be composed and executed across distributed
systems, while ensuring room for innovation and different implementations.

Architecturally, federation is an approach to organizing and managing distributed systems. At its
core, federation allows autonomous components to work together while maintaining their independence.
Think of it like a federal government system: individual states maintain their sovereignty while
cooperating under a central authority for shared concerns.

In software architecture, federation enables organizations to:

Think of the “Login with Google” or “Login with Facebook” buttons you see on websites. This is
federation in action: you can use your Google or Facebook account to log into many different
websites, even though each company manages their own login system separately.

GraphQL federation applies those principles to GraphQL APIs. It enables organizations to build a
unified GraphQL schema from multiple independent services (most often called subgraphs), each
responsible for its portion of the application’s data graph.

Consider an e-commerce platform: You might have separate teams managing products, user accounts, and
order processing. With GraphQL federation, each team can:

The magic happens through a federated gateway that acts as the central coordinator, composing these
separate schemas into a unified schema that clients can query.

The federation process involves several key components:

Let’s break down schema composition in GraphQL federation with more detail and examples. Schema
composition is the process where multiple subgraph schemas are combined into one unified schema.
It’s more complex than simply merging schemas together, though, because it needs to handle
relationships, detect incompatibilities, and ensure types are properly connected across services and
subgraphs.

Based on the examples we provided before, here’s the unified schema GraphQL clients will see and can
query:

This unified schema combines types and fields from all three subgraphs (Users, Orders, and
Products), allowing clients to seamlessly query across these domains.

The federation gateway is the entry point to your distributed data graph. It presents a unified
GraphQL endpoint to clients and handles the complexity of routing queries to the appropriate
subgraphs and assembling the results, and often provides caching and performance optimizations.

Client

Gateway

Users Service

Orders Service

Products Service

Take the following query as an example:

The gateway will route parts of the query to the appropriate subgraphs, collect the results, and
assemble them into a single response that the client can consume.

Teams can work independently on their services while contributing to a cohesive API. This autonomy
accelerates development and reduces coordination overhead.

The schema composition step verifies integration between services by ensuring that changes in
individual subgraphs do not conflict with other subgraphs.

Subgraphs and services can be scaled independently based on their specific requirements. The product
catalog might need different scaling characteristics than the order processing system.

Thanks to GraphQL, clients get a single endpoint with unified schema spanning multiple subgraphs.
The complexity of distributed systems is hidden. The gateway ensures every query reaches its
destination and returns with the right data.

GraphQL federation aligns naturally with Domain Driven Design (DDD) principles by allowing teams to
maintain clear boundaries around their domains, while maintaining explicit integration points
through the GraphQL schema. It is particularly valuable for organizations where multiple teams need
to work independently on different parts of the GraphQL API, with the flexibility to use different
technologies and programming languages.

However, implementing federation requires substantial infrastructure support, including a dedicated
team to manage the gateway, schema registry, to help connect subgraphs to the federated API and
guide teams on best practices.

Before adopting federation, it’s crucial to consider whether your organization truly needs this
level of complexity. You can start with a monolithic setup and transition to federation as your needs
evolve, rather than implementing it prematurely.

Meta (formerly Facebook),  where GraphQL was created , has continued to
use a monolithic GraphQL API since 2012. However, companies like
 Netflix ,
 Expedia Group ,
 Volvo ,
and  Booking  have adopted federation to
better align with their organizational structures and microservices architecture.

As you see, some of the world’s largest industry leaders have successfully federated their GraphQL
APIs, proving that it works reliably for production applications at an extraordinary scale.

If you’re considering adopting GraphQL federation, here are some steps to get started:

When migrating from a monolithic to federated GraphQL API, the simplest starting point is to treat
your existing schema as your first subgraph. From there, you can follow the steps above to gradually
decompose your schema into smaller pieces.

## Code Examples

### Example 1

```
type Product @key(fields: "id") {
  id: ID!
  title: String!
  price: Float!
  inStock: Boolean!
}
```

### Example 2

```
type Product @key(fields: "id") {
  id: ID!
  title: String!
  price: Float!
  inStock: Boolean!
}
```

### Example 3

```
type Order @key(fields: "id") {
  id: ID!
  products: [Product!]!
  total: Float!
}
 
type Product {
  id: ID!
}
```

### Example 4

```
type Order @key(fields: "id") {
  id: ID!
  products: [Product!]!
  total: Float!
}
 
type Product {
  id: ID!
}
```

### Example 5

```
type Query {
  user(id: ID!): User
}
 
type User {
  id: ID!
  name: String!
  email: String
  orders: [Order!]!
}
 
type Order {
  id: ID!
}
```

### Example 6

```
type Query {
  user(id: ID!): User
}
 
type User {
  id: ID!
  name: String!
  email: String
  orders: [Order!]!
}
 
type Order {
  id: ID!
}
```

### Example 7

```
type Query {
  user(id: ID!): User
}
 
type User {
  id: ID!
  name: String!
  email: String
  orders: [Order!]!
}
 
type Order {
  id: ID!
  products: [Product!]!
  total: Float!
}
 
type Product {
  id: ID!
  title: String!
  price: Float!
  inStock: Boolean!
}
```

### Example 8

```
type Query {
  user(id: ID!): User
}
 
type User {
  id: ID!
  name: String!
  email: String
  orders: [Order!]!
}
 
type Order {
  id: ID!
  products: [Product!]!
  total: Float!
}
 
type Product {
  id: ID!
  title: String!
  price: Float!
  inStock: Boolean!
}
```

### Example 9

```
query {
  user(id: "123") {
    # Resolved by Users subgraph
    name
    orders {
      # Resolved by Orders subgraph
      id
      products {
        # Resolved by Products subgraph
        title
        price
      }
    }
  }
}
```

### Example 10

```
query {
  user(id: "123") {
    # Resolved by Users subgraph
    name
    orders {
      # Resolved by Orders subgraph
      id
      products {
        # Resolved by Products subgraph
        title
        price
      }
    }
  }
}
```

### Example 11

```
type Product @key(fields: "id") {
  id: ID!
  title: String!
  price: Float!
  inStock: Boolean!
}
```

### Example 12

```
type Product @key(fields: "id") {
  id: ID!
  title: String!
  price: Float!
  inStock: Boolean!
}
```

### Example 13

```
type Order @key(fields: "id") {
  id: ID!
  products: [Product!]!
  total: Float!
}
 
type Product {
  id: ID!
}
```

### Example 14

```
type Order @key(fields: "id") {
  id: ID!
  products: [Product!]!
  total: Float!
}
 
type Product {
  id: ID!
}
```

### Example 15

```
type Query {
  user(id: ID!): User
}
 
type User {
  id: ID!
  name: String!
  email: String
  orders: [Order!]!
}
 
type Order {
  id: ID!
}
```

### Example 16

```
type Query {
  user(id: ID!): User
}
 
type User {
  id: ID!
  name: String!
  email: String
  orders: [Order!]!
}
 
type Order {
  id: ID!
}
```

### Example 17

```
type Query {
  user(id: ID!): User
}
 
type User {
  id: ID!
  name: String!
  email: String
  orders: [Order!]!
}
 
type Order {
  id: ID!
  products: [Product!]!
  total: Float!
}
 
type Product {
  id: ID!
  title: String!
  price: Float!
  inStock: Boolean!
}
```

### Example 18

```
type Query {
  user(id: ID!): User
}
 
type User {
  id: ID!
  name: String!
  email: String
  orders: [Order!]!
}
 
type Order {
  id: ID!
  products: [Product!]!
  total: Float!
}
 
type Product {
  id: ID!
  title: String!
  price: Float!
  inStock: Boolean!
}
```

### Example 19

```
query {
  user(id: "123") {
    # Resolved by Users subgraph
    name
    orders {
      # Resolved by Orders subgraph
      id
      products {
        # Resolved by Products subgraph
        title
        price
      }
    }
  }
}
```

### Example 20

```
query {
  user(id: "123") {
    # Resolved by Users subgraph
    name
    orders {
      # Resolved by Orders subgraph
      id
      products {
        # Resolved by Products subgraph
        title
        price
      }
    }
  }
}
```


---

# Common HTTP Errors and How to Debug Them

**Source:** https://graphql.org/learn/debug-errors/

## Table of Contents

- 400 Bad Request
  - What it means
  - Common causes
  - How to debug
- 405 Method Not Allowed
  - What it means
  - Common causes
  - How to debug
- 500 Internal Server Error
  - What it means
  - Common causes
  - How to debug
- GraphQL errors with
  - What it means
  - Common causes
  - How to debug
- Implementation-specific status codes
- Understanding GraphQL response formats
  - What to know
- Common HTTP Errors and How to Debug Them
- 400 Bad Request
  - What it means
  - Common causes
  - How to debug
- 405 Method Not Allowed
  - What it means
  - Common causes
  - How to debug
- 500 Internal Server Error
  - What it means
  - Common causes
  - How to debug
- GraphQL errors with
  - What it means
  - Common causes
  - How to debug
- Implementation-specific status codes
- Understanding GraphQL response formats
  - What to know

## Content

When building or consuming a GraphQL API over HTTP, it’s common to run into
errors, especially during development. Understanding how to recognize and resolve these issues
can save you time and frustration.

This guide outlines common HTTP and GraphQL errors, what they mean, and how to debug them
effectively. It follows the recommendations of the
 GraphQL over HTTP spec (draft) ,
which standardizes how GraphQL works over HTTP, including request formats, status codes,
and media types. Keep in mind that implementations may vary, so treat this as a general guide rather
than a definitive reference.

The server couldn’t parse your request. Either the GraphQL query string is malformed,
or the JSON body isn’t valid. This is the primary error status recommended by the GraphQL over HTTP specification.

You’re using an unsupported HTTP method. Most GraphQL servers require  POST  for mutations
and may allow  GET  for queries.

Something went wrong on the server.

The HTTP layer succeeded, but the GraphQL operation produced errors.

Older servers and clients (those not using
 Content-Type: application/graphql-response+json ) may also use 200 OK in the case of
complete request failure (no  data ). Common causes include:

Check the  errors  array in the response body. If the response includes a  data  property, then
your query document is likely valid and you most likely hit a runtime exception - perhaps due to
invalid input, access denial, or a bug in server logic.

If there’s no  data  field, the request likely failed during validation. For example:

Use introspection or an IDE to verify your query matches the schema.

Some GraphQL server implementations may use additional HTTP status codes that are not explicitly recommended
by the specification. These vary by implementation.

These error codes are not recommended by the specification for most cases. Different GraphQL servers handle
validation errors differently. When in doubt, use error codes supported by the specification.

Traditionally, GraphQL servers have returned responses using the  application/json  media type.
However, the  GraphQL over HTTP spec  recommends
that clients request (and servers respond with) a more specific type:  application/graphql-response+json .

This newer media type identifies the payload as a GraphQL response and helps clients
distinguish it from other types of JSON, making the response safe to process even if
it uses a non-2xx status code. A trusted proxy, gateway, or other intermediary
might describe an error using JSON, but would never do so using  application/graphql-response+json 
unless it was a valid GraphQL response.

When building or consuming a GraphQL API over HTTP, it’s common to run into
errors, especially during development. Understanding how to recognize and resolve these issues
can save you time and frustration.

This guide outlines common HTTP and GraphQL errors, what they mean, and how to debug them
effectively. It follows the recommendations of the
 GraphQL over HTTP spec (draft) ,
which standardizes how GraphQL works over HTTP, including request formats, status codes,
and media types. Keep in mind that implementations may vary, so treat this as a general guide rather
than a definitive reference.

The server couldn’t parse your request. Either the GraphQL query string is malformed,
or the JSON body isn’t valid. This is the primary error status recommended by the GraphQL over HTTP specification.

You’re using an unsupported HTTP method. Most GraphQL servers require  POST  for mutations
and may allow  GET  for queries.

Something went wrong on the server.

The HTTP layer succeeded, but the GraphQL operation produced errors.

Older servers and clients (those not using
 Content-Type: application/graphql-response+json ) may also use 200 OK in the case of
complete request failure (no  data ). Common causes include:

Check the  errors  array in the response body. If the response includes a  data  property, then
your query document is likely valid and you most likely hit a runtime exception - perhaps due to
invalid input, access denial, or a bug in server logic.

If there’s no  data  field, the request likely failed during validation. For example:

Use introspection or an IDE to verify your query matches the schema.

Some GraphQL server implementations may use additional HTTP status codes that are not explicitly recommended
by the specification. These vary by implementation.

These error codes are not recommended by the specification for most cases. Different GraphQL servers handle
validation errors differently. When in doubt, use error codes supported by the specification.

Traditionally, GraphQL servers have returned responses using the  application/json  media type.
However, the  GraphQL over HTTP spec  recommends
that clients request (and servers respond with) a more specific type:  application/graphql-response+json .

This newer media type identifies the payload as a GraphQL response and helps clients
distinguish it from other types of JSON, making the response safe to process even if
it uses a non-2xx status code. A trusted proxy, gateway, or other intermediary
might describe an error using JSON, but would never do so using  application/graphql-response+json 
unless it was a valid GraphQL response.

## Code Examples

### Example 1

```
{
  "errors": [
    {
      "message": "Cannot query field \"foo\" on type \"Query\".",
      "locations": [{ "line": 1, "column": 3 }]
    }
  ]
}
```

### Example 2

```
{
  "errors": [
    {
      "message": "Cannot query field \"foo\" on type \"Query\".",
      "locations": [{ "line": 1, "column": 3 }]
    }
  ]
}
```

### Example 3

```
{
  "errors": [
    {
      "message": "Cannot query field \"foo\" on type \"Query\".",
      "locations": [{ "line": 1, "column": 3 }]
    }
  ]
}
```

### Example 4

```
{
  "errors": [
    {
      "message": "Cannot query field \"foo\" on type \"Query\".",
      "locations": [{ "line": 1, "column": 3 }]
    }
  ]
}
```


---

# Security

**Source:** https://graphql.org/learn/security/

## Table of Contents

- Transport layer security
- Demand control
  - Trusted documents
  - Paginated fields
  - Depth limiting
  - Breadth and batch limiting
  - Rate limiting
  - Query complexity analysis
- Schema considerations
  - Validating and sanitizing argument values
  - Introspection
  - Error messages
- Authentication and authorization
- Recap
- Security
- Transport layer security
- Demand control
  - Trusted documents
  - Paginated fields
  - Depth limiting
  - Breadth and batch limiting
  - Rate limiting
  - Query complexity analysis
- Schema considerations
  - Validating and sanitizing argument values
  - Introspection
  - Error messages
- Authentication and authorization
- Recap

## Content

Protect GraphQL APIs from malicious operations

As with any type of API, you will need to consider what security measures should be used to protect a GraphQL implementation’s server resources and underlying data sources during request execution.

Many generalizable API security best practices apply to GraphQL, especially in relation to a given implementation’s selected transport protocol. However, there are GraphQL-specific security considerations that will make the API layer more resilient to potential attacks too.

On this page, we’ll survey potential attack vectors for GraphQL—many of which are denial of service attacks—along with how a layered security posture can help protect a GraphQL API from malicious operations.

The GraphQL specification does not require a specific transport protocol for requests, but  HTTP is a popular choice  for stateless query and mutation operations. For longer-lived subscription operations, WebSockets or server-sent events are often used. Whatever the chosen protocol, security considerations in the transport layer can provide an important first line of defense for a GraphQL API.

The same security measures that would be used for any type of API served with a given transport protocol should typically be used for GraphQL as well. For example, when using HTTP for queries and mutations, you should use HTTPS to encrypt data, set appropriate timeout durations for requests, and, if using HTTP caching, ensure sensitive data is cached privately (or not at all).

For GraphQL APIs that only serve first-party clients, you can write operations as needed in development environments and then lock those operations so that only they may be sent by front-end apps in production. Specifically, using  trusted documents  will allow you to create an allowlist of operations that can be executed against a schema.

As a build step during development clients may submit their GraphQL documents to the server’s allowlist, where each document is stored with a unique ID—usually its hash. At runtime, the client can send a document ID instead of the full GraphQL document, and the server will only execute requests for known document IDs.

Trusted documents are simply  persisted documents  that are deemed to not be malicious, usually because they were authored by your developers and approved through code review. Efforts are underway to  standardize  persisted documents  as part of the GraphQL-over-HTTP specification —an increasing number of GraphQL clients and servers already support them (sometimes under their legacy misnomer:  persisted queries ).

Keep in mind that trusted documents can’t be used for public APIs because the operations sent by third-party clients won’t known in advance. However, the other recommendations that follow can help defend a public GraphQL API from malicious operations.

A first step toward implementing demand control for a GraphQL API is limiting the amount of data that can be queried from a single field. When a field outputs a List type and the resulting list could potentially return a lot of data, it should be paginated to limit the maximum number of items that may be returned in a single request.

For example, compare the unbounded output of the  friends  field to the limited data returned by the  friendsConnection  field in this operation:

Read more about the connection-based model for paginating fields on the  Pagination page .

One of GraphQL’s strengths is that clients can write expressive operations that reflect the relationships between the data exposed in the API. However, some queries may become cyclical when the fields in the selection set are deeply nested:

Even when the N+1 problem has been remediated through batched requests to underlying data sources, overly nested fields may still place excessive load on server resources and impact API performance.

For this reason, it’s a good idea to limit the maximum depth of fields that a single operation can have. Many GraphQL implementations expose configuration options that allow you to specify a maximum depth for a GraphQL document and return an error to the client if a request exceeds this limit before execution begins.

Since nesting list fields can result in exponential increases to the amount of data returned, it’s recommended to apply a separate smaller limit to how deeply lists can be nested.

For cases where a client may have a legitimate use case for a deeply nested query and it’s impractical to set a low blanket limit on all queries, you may need to rely on techniques such as  rate limiting  or  query complexity analysis .

In addition to limiting operation depth, there should also be guardrails in place to limit the number of top-level fields and field aliases included in a single operation.

Consider what would happen during the execution of the following query operation:

Even though the overall depth of this query is shallow, the underlying data source for the API will still have to handle a large number of requests to resolve data for the aliased  hero  field.

Similarly, a client may send a GraphQL document with many batched operations in a request:

Depending on the client implementation, query batching can be a useful strategy for limiting the number of round trips to a server to fetch all of the required data to render a user interface. However, there should be an upper limit on the total number of queries allowed in a single batch.

As with depth limiting, a GraphQL implementation may have configuration options to restrict operation breadth, field alias usage, and batching.

Depth, breadth, and batch limiting help prevent broad categories of malicious operations such as cyclic queries and batching attacks, but they don’t provide a way to declare that a particular field is computationally expensive to resolve. So for more advanced demand control requirements, you may wish to implement rate limiting.

Rate limiting may take place in different layers of an application, for example, in the network layer or the business logic layer. Because GraphQL allows clients to specify exactly what data they need in their queries, a server may not be able to know in advance if a request includes fields that will place a higher load on its resources during execution. As a result, applying useful rate limits for a GraphQL API typically requires a different approach than simply keeping track of the total number of incoming requests over a time period in the network layer; therefore applying rate limits within the business logic layer is generally recommended.

By applying weights to the types and fields in a schema, you can estimate the cost of incoming requests using the technique known as  query complexity analysis . If the combination of fields included in a request exceeds a maximum allowable cost per request, you may choose to reject the request outright. The estimated cost can also be factored into rate limiting: if the request proceeds, the total cost of the request can be deducted from the overall query budget allocated to a client for a specific time period.

While the GraphQL specification doesn’t provide any guidelines on implementing query complexity analysis or rate limits for an API, there is  a community-maintained draft specification  for implementing custom type system directives that support these calculations.

GraphQL is strongly typed, and the  validation stage  of a request will check a parsed document against the type system to determine its validity. However, for certain cases you may find that the built-in Scalar types aren’t enough to make sure that the argument value a client provides is appropriate to execute a field resolver safely.

Consider the following  ReviewInput  type that is used with the  createReview  mutation:

Depending on what kind of client application will submit and display the review data, it may be necessary to sanitize the string provided for the  commentary  field in case a user submits unsafe HTML in it.

The sanitization step should take place in the business logic layer that is called during execution of the  createReview  resolver function. To surface this to frontend developers, it could also be expressed as a part of the schema using a custom  Scalar type .

Introspection  is a powerful feature of GraphQL that allows you to query a GraphQL API for information about its schema. However, GraphQL APIs that only serve first-party clients may forbid introspection queries in non-development environments.

Note that disabling introspection can be used as a form of “security through obscurity,” but will not be sufficient to entirely obscure a GraphQL API by itself. It may be used as a part of a broader security strategy to limit the discoverability of API information from potential attackers. For more comprehensive protection of sensitive schema details and user data, trusted documents and authorization should be used.

On the  Response page , we saw that a GraphQL implementation may provide developers with useful information about validation errors in a request, along with suggestions for how these errors may be addressed. For example:

These hints can be helpful when debugging client-side errors, but they may provide more information about a schema in a production environment than we would like to reveal. Hiding detailed error information in GraphQL responses outside of development environments is important because, even with introspection disabled, an attacker could ultimately infer the shape of an entire schema by running numerous operations with incorrect field names.

In addition to request errors, details about errors that are raised during field execution should be masked as they may reveal sensitive information about the server or underlying data sources.

Auth-related considerations for GraphQL APIs are discussed in-depth on the  Authorization page .

To recap these recommendations for securing a GraphQL API:

Protect GraphQL APIs from malicious operations

As with any type of API, you will need to consider what security measures should be used to protect a GraphQL implementation’s server resources and underlying data sources during request execution.

Many generalizable API security best practices apply to GraphQL, especially in relation to a given implementation’s selected transport protocol. However, there are GraphQL-specific security considerations that will make the API layer more resilient to potential attacks too.

On this page, we’ll survey potential attack vectors for GraphQL—many of which are denial of service attacks—along with how a layered security posture can help protect a GraphQL API from malicious operations.

The GraphQL specification does not require a specific transport protocol for requests, but  HTTP is a popular choice  for stateless query and mutation operations. For longer-lived subscription operations, WebSockets or server-sent events are often used. Whatever the chosen protocol, security considerations in the transport layer can provide an important first line of defense for a GraphQL API.

The same security measures that would be used for any type of API served with a given transport protocol should typically be used for GraphQL as well. For example, when using HTTP for queries and mutations, you should use HTTPS to encrypt data, set appropriate timeout durations for requests, and, if using HTTP caching, ensure sensitive data is cached privately (or not at all).

For GraphQL APIs that only serve first-party clients, you can write operations as needed in development environments and then lock those operations so that only they may be sent by front-end apps in production. Specifically, using  trusted documents  will allow you to create an allowlist of operations that can be executed against a schema.

As a build step during development clients may submit their GraphQL documents to the server’s allowlist, where each document is stored with a unique ID—usually its hash. At runtime, the client can send a document ID instead of the full GraphQL document, and the server will only execute requests for known document IDs.

Trusted documents are simply  persisted documents  that are deemed to not be malicious, usually because they were authored by your developers and approved through code review. Efforts are underway to  standardize  persisted documents  as part of the GraphQL-over-HTTP specification —an increasing number of GraphQL clients and servers already support them (sometimes under their legacy misnomer:  persisted queries ).

Keep in mind that trusted documents can’t be used for public APIs because the operations sent by third-party clients won’t known in advance. However, the other recommendations that follow can help defend a public GraphQL API from malicious operations.

A first step toward implementing demand control for a GraphQL API is limiting the amount of data that can be queried from a single field. When a field outputs a List type and the resulting list could potentially return a lot of data, it should be paginated to limit the maximum number of items that may be returned in a single request.

For example, compare the unbounded output of the  friends  field to the limited data returned by the  friendsConnection  field in this operation:

Read more about the connection-based model for paginating fields on the  Pagination page .

One of GraphQL’s strengths is that clients can write expressive operations that reflect the relationships between the data exposed in the API. However, some queries may become cyclical when the fields in the selection set are deeply nested:

Even when the N+1 problem has been remediated through batched requests to underlying data sources, overly nested fields may still place excessive load on server resources and impact API performance.

For this reason, it’s a good idea to limit the maximum depth of fields that a single operation can have. Many GraphQL implementations expose configuration options that allow you to specify a maximum depth for a GraphQL document and return an error to the client if a request exceeds this limit before execution begins.

Since nesting list fields can result in exponential increases to the amount of data returned, it’s recommended to apply a separate smaller limit to how deeply lists can be nested.

For cases where a client may have a legitimate use case for a deeply nested query and it’s impractical to set a low blanket limit on all queries, you may need to rely on techniques such as  rate limiting  or  query complexity analysis .

In addition to limiting operation depth, there should also be guardrails in place to limit the number of top-level fields and field aliases included in a single operation.

Consider what would happen during the execution of the following query operation:

Even though the overall depth of this query is shallow, the underlying data source for the API will still have to handle a large number of requests to resolve data for the aliased  hero  field.

Similarly, a client may send a GraphQL document with many batched operations in a request:

Depending on the client implementation, query batching can be a useful strategy for limiting the number of round trips to a server to fetch all of the required data to render a user interface. However, there should be an upper limit on the total number of queries allowed in a single batch.

As with depth limiting, a GraphQL implementation may have configuration options to restrict operation breadth, field alias usage, and batching.

Depth, breadth, and batch limiting help prevent broad categories of malicious operations such as cyclic queries and batching attacks, but they don’t provide a way to declare that a particular field is computationally expensive to resolve. So for more advanced demand control requirements, you may wish to implement rate limiting.

Rate limiting may take place in different layers of an application, for example, in the network layer or the business logic layer. Because GraphQL allows clients to specify exactly what data they need in their queries, a server may not be able to know in advance if a request includes fields that will place a higher load on its resources during execution. As a result, applying useful rate limits for a GraphQL API typically requires a different approach than simply keeping track of the total number of incoming requests over a time period in the network layer; therefore applying rate limits within the business logic layer is generally recommended.

By applying weights to the types and fields in a schema, you can estimate the cost of incoming requests using the technique known as  query complexity analysis . If the combination of fields included in a request exceeds a maximum allowable cost per request, you may choose to reject the request outright. The estimated cost can also be factored into rate limiting: if the request proceeds, the total cost of the request can be deducted from the overall query budget allocated to a client for a specific time period.

While the GraphQL specification doesn’t provide any guidelines on implementing query complexity analysis or rate limits for an API, there is  a community-maintained draft specification  for implementing custom type system directives that support these calculations.

GraphQL is strongly typed, and the  validation stage  of a request will check a parsed document against the type system to determine its validity. However, for certain cases you may find that the built-in Scalar types aren’t enough to make sure that the argument value a client provides is appropriate to execute a field resolver safely.

Consider the following  ReviewInput  type that is used with the  createReview  mutation:

Depending on what kind of client application will submit and display the review data, it may be necessary to sanitize the string provided for the  commentary  field in case a user submits unsafe HTML in it.

The sanitization step should take place in the business logic layer that is called during execution of the  createReview  resolver function. To surface this to frontend developers, it could also be expressed as a part of the schema using a custom  Scalar type .

Introspection  is a powerful feature of GraphQL that allows you to query a GraphQL API for information about its schema. However, GraphQL APIs that only serve first-party clients may forbid introspection queries in non-development environments.

Note that disabling introspection can be used as a form of “security through obscurity,” but will not be sufficient to entirely obscure a GraphQL API by itself. It may be used as a part of a broader security strategy to limit the discoverability of API information from potential attackers. For more comprehensive protection of sensitive schema details and user data, trusted documents and authorization should be used.

On the  Response page , we saw that a GraphQL implementation may provide developers with useful information about validation errors in a request, along with suggestions for how these errors may be addressed. For example:

These hints can be helpful when debugging client-side errors, but they may provide more information about a schema in a production environment than we would like to reveal. Hiding detailed error information in GraphQL responses outside of development environments is important because, even with introspection disabled, an attacker could ultimately infer the shape of an entire schema by running numerous operations with incorrect field names.

In addition to request errors, details about errors that are raised during field execution should be masked as they may reveal sensitive information about the server or underlying data sources.

Auth-related considerations for GraphQL APIs are discussed in-depth on the  Authorization page .

To recap these recommendations for securing a GraphQL API:

## Code Examples

### Example 1

```
query {
  hero {
    name
    friends {
      name
      friends {
        name
        friends {
          name
          friends {
            name
          }
        }
      }
    }
  }
}
```

### Example 2

```
query {
  hero {
    name
    friends {
      name
      friends {
        name
        friends {
          name
          friends {
            name
          }
        }
      }
    }
  }
}
```

### Example 3

```
query {
  viewer {
    friends1: friends(limit: 1) { name }
    friends2: friends(limit: 2) { name }
    friends3: friends(limit: 3) { name }
    # ...
    friends100: friends(limit: 100) { name }
  }
}
```

### Example 4

```
query {
  viewer {
    friends1: friends(limit: 1) { name }
    friends2: friends(limit: 2) { name }
    friends3: friends(limit: 3) { name }
    # ...
    friends100: friends(limit: 100) { name }
  }
}
```

### Example 5

```
query NewHopeHero {
  hero(episode: NEWHOPE) {
    name
  }
}
query EmpireHero {
  hero(episode: EMPIRE) {
    name
  }
}
### ...
query JediHero {
  hero(episode: JEDI) {
    name
  }
}
```

### Example 6

```
query NewHopeHero {
  hero(episode: NEWHOPE) {
    name
  }
}
query EmpireHero {
  hero(episode: EMPIRE) {
    name
  }
}
### ...
query JediHero {
  hero(episode: JEDI) {
    name
  }
}
```

### Example 7

```
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 8

```
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 9

```
query {
  hero {
    name
    friends {
      name
      friends {
        name
        friends {
          name
          friends {
            name
          }
        }
      }
    }
  }
}
```

### Example 10

```
query {
  hero {
    name
    friends {
      name
      friends {
        name
        friends {
          name
          friends {
            name
          }
        }
      }
    }
  }
}
```

### Example 11

```
query {
  viewer {
    friends1: friends(limit: 1) { name }
    friends2: friends(limit: 2) { name }
    friends3: friends(limit: 3) { name }
    # ...
    friends100: friends(limit: 100) { name }
  }
}
```

### Example 12

```
query {
  viewer {
    friends1: friends(limit: 1) { name }
    friends2: friends(limit: 2) { name }
    friends3: friends(limit: 3) { name }
    # ...
    friends100: friends(limit: 100) { name }
  }
}
```

### Example 13

```
query NewHopeHero {
  hero(episode: NEWHOPE) {
    name
  }
}
query EmpireHero {
  hero(episode: EMPIRE) {
    name
  }
}
### ...
query JediHero {
  hero(episode: JEDI) {
    name
  }
}
```

### Example 14

```
query NewHopeHero {
  hero(episode: NEWHOPE) {
    name
  }
}
query EmpireHero {
  hero(episode: EMPIRE) {
    name
  }
}
### ...
query JediHero {
  hero(episode: JEDI) {
    name
  }
}
```

### Example 15

```
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 16

```
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```


---

# Schema Design

**Source:** https://graphql.org/learn/schema-design/

## Table of Contents

- Versioning
- Nullability
- Schema Design
- Versioning
- Nullability

## Content

Design and evolve a type system over time without versions

While there’s nothing that prevents a GraphQL service from being versioned just like any other API, GraphQL takes a strong opinion on avoiding versioning by providing the tools for the continuous evolution of a GraphQL schema.

Why do most APIs version? When there’s limited control over the data that’s returned from an API endpoint,  any change  can be considered a breaking change, and breaking changes require a new version. If adding new features to an API requires a new version, then a tradeoff emerges between releasing often and having many incremental versions versus the understandability and maintainability of the API.

In contrast, GraphQL only returns the data that’s explicitly requested, so new capabilities can be added via new types or new fields on existing types without creating a breaking change. This has led to a common practice of always avoiding breaking changes and serving a versionless API.

Most type systems which recognise “null” provide both the common type and the  nullable  version of that type, whereby default types do not include “null” unless explicitly declared. However, in a GraphQL type system, every field is  nullable  by default. This is because there are many things that can go awry in a networked service backed by databases and other services. A database could go down, an asynchronous action could fail, an exception could be thrown. Beyond simply system failures, authorization can often be granular, where individual fields within a request can have different authorization rules.

By defaulting every field to  nullable , any of these reasons may result in just that field returned “null” rather than having a complete failure for the request. Instead, GraphQL provides  non-null  variants of types which make a guarantee to clients that if requested, the field will never return “null”. Instead, if an error occurs, the previous parent field will be “null” instead.

When designing a GraphQL schema, it’s important to keep in mind all the problems that could go wrong and if “null” is an appropriate value for a failed field. Typically it is, but occasionally, it’s not. In those cases, use non-null types to make that guarantee.

Design and evolve a type system over time without versions

While there’s nothing that prevents a GraphQL service from being versioned just like any other API, GraphQL takes a strong opinion on avoiding versioning by providing the tools for the continuous evolution of a GraphQL schema.

Why do most APIs version? When there’s limited control over the data that’s returned from an API endpoint,  any change  can be considered a breaking change, and breaking changes require a new version. If adding new features to an API requires a new version, then a tradeoff emerges between releasing often and having many incremental versions versus the understandability and maintainability of the API.

In contrast, GraphQL only returns the data that’s explicitly requested, so new capabilities can be added via new types or new fields on existing types without creating a breaking change. This has led to a common practice of always avoiding breaking changes and serving a versionless API.

Most type systems which recognise “null” provide both the common type and the  nullable  version of that type, whereby default types do not include “null” unless explicitly declared. However, in a GraphQL type system, every field is  nullable  by default. This is because there are many things that can go awry in a networked service backed by databases and other services. A database could go down, an asynchronous action could fail, an exception could be thrown. Beyond simply system failures, authorization can often be granular, where individual fields within a request can have different authorization rules.

By defaulting every field to  nullable , any of these reasons may result in just that field returned “null” rather than having a complete failure for the request. Instead, GraphQL provides  non-null  variants of types which make a guarantee to clients that if requested, the field will never return “null”. Instead, if an error occurs, the previous parent field will be “null” instead.

When designing a GraphQL schema, it’s important to keep in mind all the problems that could go wrong and if “null” is an appropriate value for a failed field. Typically it is, but occasionally, it’s not. In those cases, use non-null types to make that guarantee.


---

# Subscriptions

**Source:** https://graphql.org/learn/subscriptions/

## Table of Contents

- Subscribing to updates
- Using subscriptions at scale
- Next steps
- Subscriptions
- Subscribing to updates
- Using subscriptions at scale
- Next steps

## Content

Learn how to get real-time updates from a GraphQL server

In addition to reading and writing data using stateless query and mutation operations, the GraphQL specification also describes how to receive real-time updates via long-lived requests. On this page, we’ll explore how clients can subscribe to details of events on a GraphQL server using subscription operations.

Many of the features of GraphQL operations that apply to queries also apply to subscriptions, so review the  Queries  page first before proceeding.

Subscription fields are defined exactly as  Query  and  Mutation  fields are, but using the  subscription  root operation type instead:

Similarly, a subscription is initiated using the  subscription  keyword as the operation type:

GraphQL subscriptions are typically backed by a separate pub/sub system so that messages about updated data can be published as needed during runtime and then consumed by the resolver functions for the subscription fields in the API.

For the previous example, we could imagine publishing a  REVIEW_CREATED  message to a pub/sub system when a client submits a new review with the  createReview  mutation, and then listening for this event in the resolver function for the  reviewCreated  subscription field so that the server may send this data to subscribed clients.

Note that subscriptions are different from the concept of  live queries , a loosely defined feature that some GraphQL implementations may offer.  Read more about the ongoing discussion on a formal specification for live queries here.

As with query and mutation operations, GraphQL doesn’t specify what transport protocol to use, so it’s up to the server to decide. In practice, you will often see them implemented with WebSockets or server-sent events. Clients that want to send subscription operations will also need to support the chosen protocol. There are community-maintained specifications for implementing GraphQL subscriptions with  WebSockets  and  server-sent events .

One important distinction to make with subscription operations is that a document may contain a number of different subscription operations with different root fields, but each operation must have exactly one root field.

For example, this operation would be invalid:

But this document is valid:

As with query and mutation operations, the document above contains multiple operations so each operation must be named, and the name of the operation to execute must be specified in the request.

Subscription operations are a powerful feature of GraphQL, but they are more complicated to implement than queries or mutations because you must maintain the GraphQL document, variables, and other context over the lifetime of the subscription. These requirements can be challenging when scaling GraphQL servers horizontally because each subscribed client must be bound to a specific instance of the server.

Similarly, client libraries will typically need more advanced features to handle these kinds of operations, such as logic to resubscribe to operations if the connection is disrupted or mechanisms to handle potential race conditions between initial queries and updated subscription data.

In practice, a GraphQL API that supports subscriptions will call for a more complicated architecture than one that only exposes query and mutation fields. How this architecture is designed will depend on the specific GraphQL implementation, what pub/sub system supports the subscriptions, and the chosen transport protocol, as well as other requirements related to availability, scalability, and security of the real-time data.

Subscription operations are well suited for data that changes often and incrementally, and for clients that need to receive those incremental updates as close to real-time as possible to deliver the expected user experience. For data with less frequent updates, periodic polling, mobile push notifications, or re-fetching queries based on user interaction may be the better solutions for keeping a client UI up to date.

To recap what we’ve learned about subscriptions:

Now that we’ve covered all three operation types in GraphQL, let’s explore how client requests are  validated  by a GraphQL server.

Learn how to get real-time updates from a GraphQL server

In addition to reading and writing data using stateless query and mutation operations, the GraphQL specification also describes how to receive real-time updates via long-lived requests. On this page, we’ll explore how clients can subscribe to details of events on a GraphQL server using subscription operations.

Many of the features of GraphQL operations that apply to queries also apply to subscriptions, so review the  Queries  page first before proceeding.

Subscription fields are defined exactly as  Query  and  Mutation  fields are, but using the  subscription  root operation type instead:

Similarly, a subscription is initiated using the  subscription  keyword as the operation type:

GraphQL subscriptions are typically backed by a separate pub/sub system so that messages about updated data can be published as needed during runtime and then consumed by the resolver functions for the subscription fields in the API.

For the previous example, we could imagine publishing a  REVIEW_CREATED  message to a pub/sub system when a client submits a new review with the  createReview  mutation, and then listening for this event in the resolver function for the  reviewCreated  subscription field so that the server may send this data to subscribed clients.

Note that subscriptions are different from the concept of  live queries , a loosely defined feature that some GraphQL implementations may offer.  Read more about the ongoing discussion on a formal specification for live queries here.

As with query and mutation operations, GraphQL doesn’t specify what transport protocol to use, so it’s up to the server to decide. In practice, you will often see them implemented with WebSockets or server-sent events. Clients that want to send subscription operations will also need to support the chosen protocol. There are community-maintained specifications for implementing GraphQL subscriptions with  WebSockets  and  server-sent events .

One important distinction to make with subscription operations is that a document may contain a number of different subscription operations with different root fields, but each operation must have exactly one root field.

For example, this operation would be invalid:

But this document is valid:

As with query and mutation operations, the document above contains multiple operations so each operation must be named, and the name of the operation to execute must be specified in the request.

Subscription operations are a powerful feature of GraphQL, but they are more complicated to implement than queries or mutations because you must maintain the GraphQL document, variables, and other context over the lifetime of the subscription. These requirements can be challenging when scaling GraphQL servers horizontally because each subscribed client must be bound to a specific instance of the server.

Similarly, client libraries will typically need more advanced features to handle these kinds of operations, such as logic to resubscribe to operations if the connection is disrupted or mechanisms to handle potential race conditions between initial queries and updated subscription data.

In practice, a GraphQL API that supports subscriptions will call for a more complicated architecture than one that only exposes query and mutation fields. How this architecture is designed will depend on the specific GraphQL implementation, what pub/sub system supports the subscriptions, and the chosen transport protocol, as well as other requirements related to availability, scalability, and security of the real-time data.

Subscription operations are well suited for data that changes often and incrementally, and for clients that need to receive those incremental updates as close to real-time as possible to deliver the expected user experience. For data with less frequent updates, periodic polling, mobile push notifications, or re-fetching queries based on user interaction may be the better solutions for keeping a client UI up to date.

To recap what we’ve learned about subscriptions:

Now that we’ve covered all three operation types in GraphQL, let’s explore how client requests are  validated  by a GraphQL server.

## Code Examples

### Example 1

```
type Subscription {
  reviewCreated: Review
}
```

### Example 2

```
type Subscription {
  reviewCreated: Review
}
```

### Example 3

```
subscription NewReviewCreated {
  reviewCreated {
    rating
    commentary
  }
}
```

### Example 4

```
subscription NewReviewCreated {
  reviewCreated {
    rating
    commentary
  }
}
```

### Example 5

```
subscription {
  reviewCreated {
    rating
    commentary
  }
  humanFriendsUpdated {
    name
    friends {
      name
    }
  }
}
```

### Example 6

```
subscription {
  reviewCreated {
    rating
    commentary
  }
  humanFriendsUpdated {
    name
    friends {
      name
    }
  }
}
```

### Example 7

```
subscription NewReviewCreated {
  reviewCreated {
    rating
    commentary
  }
}
subscription FriendListUpdated($id: ID!) {
  humanFriendsUpdated(id: $id) {
    name
    friends {
      name
    }
  }
}
```

### Example 8

```
subscription NewReviewCreated {
  reviewCreated {
    rating
    commentary
  }
}
subscription FriendListUpdated($id: ID!) {
  humanFriendsUpdated(id: $id) {
    name
    friends {
      name
    }
  }
}
```

### Example 9

```
type Subscription {
  reviewCreated: Review
}
```

### Example 10

```
type Subscription {
  reviewCreated: Review
}
```

### Example 11

```
subscription NewReviewCreated {
  reviewCreated {
    rating
    commentary
  }
}
```

### Example 12

```
subscription NewReviewCreated {
  reviewCreated {
    rating
    commentary
  }
}
```

### Example 13

```
subscription {
  reviewCreated {
    rating
    commentary
  }
  humanFriendsUpdated {
    name
    friends {
      name
    }
  }
}
```

### Example 14

```
subscription {
  reviewCreated {
    rating
    commentary
  }
  humanFriendsUpdated {
    name
    friends {
      name
    }
  }
}
```

### Example 15

```
subscription NewReviewCreated {
  reviewCreated {
    rating
    commentary
  }
}
subscription FriendListUpdated($id: ID!) {
  humanFriendsUpdated(id: $id) {
    name
    friends {
      name
    }
  }
}
```

### Example 16

```
subscription NewReviewCreated {
  reviewCreated {
    rating
    commentary
  }
}
subscription FriendListUpdated($id: ID!) {
  humanFriendsUpdated(id: $id) {
    name
    friends {
      name
    }
  }
}
```


---

# Global Object Identification

**Source:** https://graphql.org/learn/global-object-identification/

## Table of Contents

- Specification
- Reserved Types
- Node Interface
  - Introspection
- Node root field
  - Introspection
- Field stability
- Plural identifying root fields
  - Fields
- Global Object Identification
- Specification
- Reserved Types
- Node Interface
  - Introspection
- Node root field
  - Introspection
- Field stability
- Plural identifying root fields
  - Fields

## Content

Consistent object access enables simple caching and object lookups

To provide options for GraphQL clients to elegantly handle caching and data
refetching, GraphQL servers need to expose object identifiers in a standardized
way.

For this to work, a client will need to query via a standard mechanism to
request an object by ID. Then, in the response, the schema will need to provide a
standard way of providing these IDs.

Because little is known about the object other than its ID, we call these
objects “nodes.” Here is an example query for a node:

The Node interface looks like:

With a User conforming via:

Everything below describes with more formal requirements a specification around object
identification in order to conform to ensure consistency across server implementations. These
specifications are based on how a server can be compliant with the  Relay  API client, but
can be useful for any client.

A GraphQL server compatible with this spec must reserve certain types and type names
to support the consistent object identification model. In particular, this spec creates
guidelines for the following types:

The server must provide an interface called  Node . That interface
must include exactly one field, called  id  that returns a non-null  ID .

This  id  should be a globally unique identifier for this object, and given
just this  id , the server should be able to refetch the object.

A server that correctly implements the above interface will accept the following
introspection query, and return the provided response:

yields

The server must provide a root field called  node  that returns the  Node 
interface. This root field must take exactly one argument, a non-null ID
named  id .

If a query returns an object that implements  Node , then this root field
should refetch the identical object when value returned by the server in the
 Node ’s  id  field is passed as the  id  parameter to the  node  root field.

The server must make a best effort to fetch this data, but it may not always
be possible; for example, the server may return a  User  with a valid  id ,
but when the request is made to refetch that user with the  node  root field,
the user’s database may be unavailable, or the user may have deleted their
account. In this case, the result of querying this field should be  null .

A server that correctly implements the above requirement will accept the
following introspection query, and return a response that contains the
provided response.

yields

If two objects appear in a query, both implementing  Node  with identical
IDs, then the two objects must be equal.

For the purposes of this definition, object equality is defined as follows:

For example:

might return:

Because  fourNode.id  and  fiveNode.userWithIdOneLess.id  are the same, we are
guaranteed by the conditions above that  fourNode.name  must be the same as
 fiveNode.userWithIdOneLess.name , and indeed it is.

Imagine a root field named  username , that takes a user’s username and
returns the corresponding user:

might return:

Clearly, we can link up the object in the response, the user with ID 4,
with the request, identifying the object with username “zuck”. Now imagine a
root field named  usernames , that takes a list of usernames and returns a
list of objects:

might return:

For clients to be able to link the usernames to the responses, it needs to
know that the array in the response will be the same size as the array
passed as an argument, and that the order in the response will match the
order in the argument. We call these  plural identifying root fields , and
their requirements are described below.

A server compliant with this spec may expose root fields that accept a list of input
arguments, and returns a list of responses. For spec-compliant clients to use these fields,
these fields must be  plural identifying root fields , and obey the following
requirements.

NOTE Spec-compliant servers may expose root fields that are not  plural
identifying root fields ; the spec-compliant client will just be unable to use those
fields as root fields in its queries.

Plural identifying root fields  must have a single argument. The type of that
argument must be a non-null list of non-nulls. In our  usernames  example, the
field would take a single argument named  usernames , whose type (using our type
system shorthand) would be  [String!]! .

The return type of a  plural identifying root field  must be a list, or a
non-null wrapper around a list. The list must wrap the  Node  interface, an
object that implements the  Node  interface, or a non-null wrapper around
those types.

Whenever the  plural identifying root field  is used, the length of the
list in the response must be the same as the length of the list in the
arguments. Each item in the response must correspond to its item in the input;
more formally, if passing the root field an input list  Lin  resulted in output
value  Lout , then for an arbitrary permutation  P , passing the root field
 P(Lin)  must result in output value  P(Lout) .

Because of this, servers are advised to not have the response type
wrap a non-null wrapper, because if it is unable to fetch the object for
a given entry in the input, it still must provide a value in the output
for that input entry;  null  is a useful value for doing so.

Consistent object access enables simple caching and object lookups

To provide options for GraphQL clients to elegantly handle caching and data
refetching, GraphQL servers need to expose object identifiers in a standardized
way.

For this to work, a client will need to query via a standard mechanism to
request an object by ID. Then, in the response, the schema will need to provide a
standard way of providing these IDs.

Because little is known about the object other than its ID, we call these
objects “nodes.” Here is an example query for a node:

The Node interface looks like:

With a User conforming via:

Everything below describes with more formal requirements a specification around object
identification in order to conform to ensure consistency across server implementations. These
specifications are based on how a server can be compliant with the  Relay  API client, but
can be useful for any client.

A GraphQL server compatible with this spec must reserve certain types and type names
to support the consistent object identification model. In particular, this spec creates
guidelines for the following types:

The server must provide an interface called  Node . That interface
must include exactly one field, called  id  that returns a non-null  ID .

This  id  should be a globally unique identifier for this object, and given
just this  id , the server should be able to refetch the object.

A server that correctly implements the above interface will accept the following
introspection query, and return the provided response:

yields

The server must provide a root field called  node  that returns the  Node 
interface. This root field must take exactly one argument, a non-null ID
named  id .

If a query returns an object that implements  Node , then this root field
should refetch the identical object when value returned by the server in the
 Node ’s  id  field is passed as the  id  parameter to the  node  root field.

The server must make a best effort to fetch this data, but it may not always
be possible; for example, the server may return a  User  with a valid  id ,
but when the request is made to refetch that user with the  node  root field,
the user’s database may be unavailable, or the user may have deleted their
account. In this case, the result of querying this field should be  null .

A server that correctly implements the above requirement will accept the
following introspection query, and return a response that contains the
provided response.

yields

If two objects appear in a query, both implementing  Node  with identical
IDs, then the two objects must be equal.

For the purposes of this definition, object equality is defined as follows:

For example:

might return:

Because  fourNode.id  and  fiveNode.userWithIdOneLess.id  are the same, we are
guaranteed by the conditions above that  fourNode.name  must be the same as
 fiveNode.userWithIdOneLess.name , and indeed it is.

Imagine a root field named  username , that takes a user’s username and
returns the corresponding user:

might return:

Clearly, we can link up the object in the response, the user with ID 4,
with the request, identifying the object with username “zuck”. Now imagine a
root field named  usernames , that takes a list of usernames and returns a
list of objects:

might return:

For clients to be able to link the usernames to the responses, it needs to
know that the array in the response will be the same size as the array
passed as an argument, and that the order in the response will match the
order in the argument. We call these  plural identifying root fields , and
their requirements are described below.

A server compliant with this spec may expose root fields that accept a list of input
arguments, and returns a list of responses. For spec-compliant clients to use these fields,
these fields must be  plural identifying root fields , and obey the following
requirements.

NOTE Spec-compliant servers may expose root fields that are not  plural
identifying root fields ; the spec-compliant client will just be unable to use those
fields as root fields in its queries.

Plural identifying root fields  must have a single argument. The type of that
argument must be a non-null list of non-nulls. In our  usernames  example, the
field would take a single argument named  usernames , whose type (using our type
system shorthand) would be  [String!]! .

The return type of a  plural identifying root field  must be a list, or a
non-null wrapper around a list. The list must wrap the  Node  interface, an
object that implements the  Node  interface, or a non-null wrapper around
those types.

Whenever the  plural identifying root field  is used, the length of the
list in the response must be the same as the length of the list in the
arguments. Each item in the response must correspond to its item in the input;
more formally, if passing the root field an input list  Lin  resulted in output
value  Lout , then for an arbitrary permutation  P , passing the root field
 P(Lin)  must result in output value  P(Lout) .

Because of this, servers are advised to not have the response type
wrap a non-null wrapper, because if it is unable to fetch the object for
a given entry in the input, it still must provide a value in the output
for that input entry;  null  is a useful value for doing so.

## Code Examples

### Example 1

```
{
  node(id: "4") {
    id
    ... on User {
      name
    }
  }
}
```

### Example 2

```
{
  node(id: "4") {
    id
    ... on User {
      name
    }
  }
}
```

### Example 3

```
# An object with a Globally Unique ID
interface Node {
  # The ID of the object.
  id: ID!
}
```

### Example 4

```
# An object with a Globally Unique ID
interface Node {
  # The ID of the object.
  id: ID!
}
```

### Example 5

```
type User implements Node {
  id: ID!
  # Full name
  name: String!
}
```

### Example 6

```
type User implements Node {
  id: ID!
  # Full name
  name: String!
}
```

### Example 7

```
{
  __type(name: "Node") {
    name
    kind
    fields {
      name
      type {
        kind
        ofType {
          name
          kind
        }
      }
    }
  }
}
```

### Example 8

```
{
  __type(name: "Node") {
    name
    kind
    fields {
      name
      type {
        kind
        ofType {
          name
          kind
        }
      }
    }
  }
}
```

### Example 9

```
{
  "__type": {
    "name": "Node",
    "kind": "INTERFACE",
    "fields": [
      {
        "name": "id",
        "type": {
          "kind": "NON_NULL",
          "ofType": {
            "name": "ID",
            "kind": "SCALAR"
          }
        }
      }
    ]
  }
}
```

### Example 10

```
{
  "__type": {
    "name": "Node",
    "kind": "INTERFACE",
    "fields": [
      {
        "name": "id",
        "type": {
          "kind": "NON_NULL",
          "ofType": {
            "name": "ID",
            "kind": "SCALAR"
          }
        }
      }
    ]
  }
}
```

### Example 11

```
{
  __schema {
    queryType {
      fields {
        name
        type {
          name
          kind
        }
        args {
          name
          type {
            kind
            ofType {
              name
              kind
            }
          }
        }
      }
    }
  }
}
```

### Example 12

```
{
  __schema {
    queryType {
      fields {
        name
        type {
          name
          kind
        }
        args {
          name
          type {
            kind
            ofType {
              name
              kind
            }
          }
        }
      }
    }
  }
}
```

### Example 13

```
{
  "__schema": {
    "queryType": {
      "fields": [
        // This array may have other entries
        {
          "name": "node",
          "type": {
            "name": "Node",
            "kind": "INTERFACE"
          },
          "args": [
            {
              "name": "id",
              "type": {
                "kind": "NON_NULL",
                "ofType": {
                  "name": "ID",
                  "kind": "SCALAR"
                }
              }
            }
          ]
        }
      ]
    }
  }
}
```

### Example 14

```
{
  "__schema": {
    "queryType": {
      "fields": [
        // This array may have other entries
        {
          "name": "node",
          "type": {
            "name": "Node",
            "kind": "INTERFACE"
          },
          "args": [
            {
              "name": "id",
              "type": {
                "kind": "NON_NULL",
                "ofType": {
                  "name": "ID",
                  "kind": "SCALAR"
                }
              }
            }
          ]
        }
      ]
    }
  }
}
```

### Example 15

```
{
  fourNode: node(id: "4") {
    id
    ... on User {
      name
      userWithIdOneGreater {
        id
        name
      }
    }
  }
  fiveNode: node(id: "5") {
    id
    ... on User {
      name
      userWithIdOneLess {
        id
        name
      }
    }
  }
}
```

### Example 16

```
{
  fourNode: node(id: "4") {
    id
    ... on User {
      name
      userWithIdOneGreater {
        id
        name
      }
    }
  }
  fiveNode: node(id: "5") {
    id
    ... on User {
      name
      userWithIdOneLess {
        id
        name
      }
    }
  }
}
```

### Example 17

```
{
  "fourNode": {
    "id": "4",
    "name": "Mark Zuckerberg",
    "userWithIdOneGreater": {
      "id": "5",
      "name": "Chris Hughes"
    }
  },
  "fiveNode": {
    "id": "5",
    "name": "Chris Hughes",
    "userWithIdOneLess": {
      "id": "4",
      "name": "Mark Zuckerberg"
    }
  }
}
```

### Example 18

```
{
  "fourNode": {
    "id": "4",
    "name": "Mark Zuckerberg",
    "userWithIdOneGreater": {
      "id": "5",
      "name": "Chris Hughes"
    }
  },
  "fiveNode": {
    "id": "5",
    "name": "Chris Hughes",
    "userWithIdOneLess": {
      "id": "4",
      "name": "Mark Zuckerberg"
    }
  }
}
```

### Example 19

```
{
  username(username: "zuck") {
    id
  }
}
```

### Example 20

```
{
  username(username: "zuck") {
    id
  }
}
```

### Example 21

```
{
  "username": {
    "id": "4"
  }
}
```

### Example 22

```
{
  "username": {
    "id": "4"
  }
}
```

### Example 23

```
{
  usernames(usernames: ["zuck", "moskov"]) {
    id
  }
}
```

### Example 24

```
{
  usernames(usernames: ["zuck", "moskov"]) {
    id
  }
}
```

### Example 25

```
{
  "usernames": [
    {
      "id": "4"
    },
    {
      "id": "6"
    }
  ]
}
```

### Example 26

```
{
  "usernames": [
    {
      "id": "4"
    },
    {
      "id": "6"
    }
  ]
}
```

### Example 27

```
{
  node(id: "4") {
    id
    ... on User {
      name
    }
  }
}
```

### Example 28

```
{
  node(id: "4") {
    id
    ... on User {
      name
    }
  }
}
```

### Example 29

```
# An object with a Globally Unique ID
interface Node {
  # The ID of the object.
  id: ID!
}
```

### Example 30

```
# An object with a Globally Unique ID
interface Node {
  # The ID of the object.
  id: ID!
}
```

### Example 31

```
type User implements Node {
  id: ID!
  # Full name
  name: String!
}
```

### Example 32

```
type User implements Node {
  id: ID!
  # Full name
  name: String!
}
```

### Example 33

```
{
  __type(name: "Node") {
    name
    kind
    fields {
      name
      type {
        kind
        ofType {
          name
          kind
        }
      }
    }
  }
}
```

### Example 34

```
{
  __type(name: "Node") {
    name
    kind
    fields {
      name
      type {
        kind
        ofType {
          name
          kind
        }
      }
    }
  }
}
```

### Example 35

```
{
  "__type": {
    "name": "Node",
    "kind": "INTERFACE",
    "fields": [
      {
        "name": "id",
        "type": {
          "kind": "NON_NULL",
          "ofType": {
            "name": "ID",
            "kind": "SCALAR"
          }
        }
      }
    ]
  }
}
```

### Example 36

```
{
  "__type": {
    "name": "Node",
    "kind": "INTERFACE",
    "fields": [
      {
        "name": "id",
        "type": {
          "kind": "NON_NULL",
          "ofType": {
            "name": "ID",
            "kind": "SCALAR"
          }
        }
      }
    ]
  }
}
```

### Example 37

```
{
  __schema {
    queryType {
      fields {
        name
        type {
          name
          kind
        }
        args {
          name
          type {
            kind
            ofType {
              name
              kind
            }
          }
        }
      }
    }
  }
}
```

### Example 38

```
{
  __schema {
    queryType {
      fields {
        name
        type {
          name
          kind
        }
        args {
          name
          type {
            kind
            ofType {
              name
              kind
            }
          }
        }
      }
    }
  }
}
```

### Example 39

```
{
  "__schema": {
    "queryType": {
      "fields": [
        // This array may have other entries
        {
          "name": "node",
          "type": {
            "name": "Node",
            "kind": "INTERFACE"
          },
          "args": [
            {
              "name": "id",
              "type": {
                "kind": "NON_NULL",
                "ofType": {
                  "name": "ID",
                  "kind": "SCALAR"
                }
              }
            }
          ]
        }
      ]
    }
  }
}
```

### Example 40

```
{
  "__schema": {
    "queryType": {
      "fields": [
        // This array may have other entries
        {
          "name": "node",
          "type": {
            "name": "Node",
            "kind": "INTERFACE"
          },
          "args": [
            {
              "name": "id",
              "type": {
                "kind": "NON_NULL",
                "ofType": {
                  "name": "ID",
                  "kind": "SCALAR"
                }
              }
            }
          ]
        }
      ]
    }
  }
}
```

### Example 41

```
{
  fourNode: node(id: "4") {
    id
    ... on User {
      name
      userWithIdOneGreater {
        id
        name
      }
    }
  }
  fiveNode: node(id: "5") {
    id
    ... on User {
      name
      userWithIdOneLess {
        id
        name
      }
    }
  }
}
```

### Example 42

```
{
  fourNode: node(id: "4") {
    id
    ... on User {
      name
      userWithIdOneGreater {
        id
        name
      }
    }
  }
  fiveNode: node(id: "5") {
    id
    ... on User {
      name
      userWithIdOneLess {
        id
        name
      }
    }
  }
}
```

### Example 43

```
{
  "fourNode": {
    "id": "4",
    "name": "Mark Zuckerberg",
    "userWithIdOneGreater": {
      "id": "5",
      "name": "Chris Hughes"
    }
  },
  "fiveNode": {
    "id": "5",
    "name": "Chris Hughes",
    "userWithIdOneLess": {
      "id": "4",
      "name": "Mark Zuckerberg"
    }
  }
}
```

### Example 44

```
{
  "fourNode": {
    "id": "4",
    "name": "Mark Zuckerberg",
    "userWithIdOneGreater": {
      "id": "5",
      "name": "Chris Hughes"
    }
  },
  "fiveNode": {
    "id": "5",
    "name": "Chris Hughes",
    "userWithIdOneLess": {
      "id": "4",
      "name": "Mark Zuckerberg"
    }
  }
}
```

### Example 45

```
{
  username(username: "zuck") {
    id
  }
}
```

### Example 46

```
{
  username(username: "zuck") {
    id
  }
}
```

### Example 47

```
{
  "username": {
    "id": "4"
  }
}
```

### Example 48

```
{
  "username": {
    "id": "4"
  }
}
```

### Example 49

```
{
  usernames(usernames: ["zuck", "moskov"]) {
    id
  }
}
```

### Example 50

```
{
  usernames(usernames: ["zuck", "moskov"]) {
    id
  }
}
```

### Example 51

```
{
  "usernames": [
    {
      "id": "4"
    },
    {
      "id": "6"
    }
  ]
}
```

### Example 52

```
{
  "usernames": [
    {
      "id": "4"
    },
    {
      "id": "6"
    }
  ]
}
```


---

# Queries

**Source:** https://graphql.org/learn/queries/

## Table of Contents

- Fields
- Arguments
- Operation type and name
- Aliases
- Variables
  - Variable definitions
  - Default variables
- Fragments
  - Using variables inside fragments
  - Inline Fragments
  - Meta fields
- Directives
- Next steps
- Queries
- Fields
- Arguments
- Operation type and name
- Aliases
- Variables
  - Variable definitions
  - Default variables
- Fragments
  - Using variables inside fragments
  - Inline Fragments
  - Meta fields
- Directives
- Next steps

## Content

Learn how to fetch data from a GraphQL server

GraphQL supports three main operation types—queries, mutations, and subscriptions. We have already seen several examples of basic queries in this guide, and on this page, you’ll learn in detail how to use the various features of query operations to read data from a server.

At its simplest, GraphQL is about asking for specific  fields  on objects. Let’s start by looking at the  hero  field that’s defined on the  Query  type in the schema:

We can see what result we get when we query it:

When creating a GraphQL  document  we always start with a  root operation type  (the  Query  Object type for this example) because it serves as an entry point to the API. From there we must specify the  selection set  of fields we are interested in, all the way down to their leaf values which will be Scalar or Enum types. The field  name  returns a  String  type, in this case the name of the main hero of Star Wars,  "R2-D2" .

The GraphQL specification indicates that a request’s result will be returned on a top-level  data  key in the response. If the request raised any errors, there will be information about what went wrong on a top-level  errors  key. From there, you can see that the result has the same shape as the query. This is essential to GraphQL, because you always get back what you expect, and the server knows exactly what fields the client is asking for.

In the previous example, we just asked for the name of our hero which returned a  String , but fields can also return Object types (and lists thereof). In that case, you can make a  sub-selection  of fields for that Object type:

GraphQL queries can traverse related objects and their fields, letting clients fetch lots of related data in one request, instead of making several roundtrips as one would need in a classic REST architecture.

Note that in this example, the  friends  field returns an array of items. GraphQL queries look the same for single items or lists of items; however, we know which one to expect based on what is indicated in the schema.

If the only thing we could do was traverse objects and their fields, GraphQL would already be a very useful language for data fetching. But when you add the ability to pass  arguments  to fields, things get much more interesting:

The client must then provide the required  id  value with the query:

In a system like REST, you can only pass a single set of arguments—the query parameters and URL segments in your request. But in GraphQL, every field and nested object can get its own set of arguments, making GraphQL a complete replacement for making multiple API fetches.

You can even pass arguments into fields that output Scalar types; one use case for this would be to implement data transformations once on the server, instead of on every client separately:

Arguments can be of many different types. In the above example, we have used an Enum type, which represents one of a finite set of options (in this case, units of length, either  METER  or  FOOT ). GraphQL comes with a  default set of types , but a GraphQL server can also declare custom types, as long as they can be serialized into your transport format.

Read more about the GraphQL type system here.

In the examples above we have been using a shorthand syntax where we omit the  query  keyword before the operation’s selection set. In addition to specifying the  operation type  explicitly, we can also add a unique  operation name , which is useful in production apps because it makes debugging and tracing easier.

Here’s an example that includes the  query  keyword as the operation type and  HeroNameAndFriends  as the operation name:

The operation type is either  query ,  mutation , or  subscription  and describes what type of operation you intend to do. This keyword is required unless you’re using the shorthand syntax for queries (it is always required for mutations and subscriptions). Additionally, if you wish to provide a name for your operation, then you must specify the operation type as well.

The operation name is an explicit name that you assign to your operation; you should pick a meaningful name. It is required when sending multiple operations in one document, but even if you’re only sending one operation it’s encouraged because operation names are helpful for debugging and server-side logging. When something goes wrong (you see errors either in your network logs or in the logs of your GraphQL server) it is easier to identify a query in your codebase by name instead of trying to decipher the contents.

Think of this just like a function name in your favorite programming language. For example, in JavaScript, we can easily work only with anonymous functions, but when we give a function a name, it’s easier to track it down, debug our code, and log when it’s called. In the same way, GraphQL query and mutation names, along with fragment names, can be a useful debugging tool on the server side to identify different GraphQL requests.

If you have a sharp eye, you may have noticed that, since the result object fields match the name of the fields in the query but don’t include arguments, you can’t directly query for the same field with different arguments. That’s why you need  aliases —they let you rename the result of a field to anything you want.

In the above example, the two  hero  fields would have conflicted, but since we can alias them to different names, we can get both results in one request.

So far, we have been writing all of our arguments inside the query string. But in most applications, the arguments to fields will be dynamic. For example, there might be a dropdown that lets you select which Star Wars episode you are interested in, or a search field, or a set of filters.

It wouldn’t be a good idea to pass these dynamic arguments directly in the query string, because then our client-side code would need to dynamically manipulate the query string at runtime, and serialize it into a GraphQL-specific format. Instead, GraphQL has a first-class way to factor dynamic values out of the query and pass them as a separate dictionary. These values are called  variables .

When we start working with variables, we need to do three things:

Here’s what it looks like all together:

You must specify an operation type and name in a GraphQL document to use variables.

Now, in our client code, we can simply pass a different variable rather than needing to construct an entirely new query. In general, this is also a good practice for denoting which arguments in our query are expected to be dynamic—we should never be doing string interpolation to construct queries from user-supplied values.

The variable definitions are the part that looks like  ($episode: Episode)  in the query above. It works just like the argument definitions for a function in a typed language. It lists all of the variables, prefixed by  $ , followed by their type, in this case,  Episode .

All declared variables must be either Scalar, Enum, or Input Object types. So if you want to pass a complex object into a field, you need to know what  input type  matches it on the server.

Variable definitions can be optional or required. In the case above, since there isn’t an  !  next to the  Episode  type, it’s optional. But if the field you are passing the variable into requires a non-null argument, then the variable has to be required as well.

To learn more about the syntax for these variable definitions, it’s useful to learn schema definition language (SDL), which is explained in detail on  the Schemas and Types page .

Default values can also be assigned to the variables in the query by adding the default value after the type declaration:

When default values are provided for all variables, you can call the query without passing any variables. If any variables are passed as part of the variables dictionary, they will override the defaults.

Let’s say we have a relatively complicated page in our app, which lets us look at two heroes side by side, along with their friends. You can imagine that such a query could quickly get complicated because we would need to repeat the fields at least once—one for each side of the comparison.

That’s why GraphQL includes reusable units called  fragments . Fragments let you construct sets of fields, and then include them in queries where needed. Here’s an example of how you could solve the above situation using fragments:

You can see how the above query would be pretty repetitive if we weren’t able to use fragments. The concept of fragments is frequently used to split complicated application data requirements into smaller chunks, especially when you need to combine many UI components with different fragments into one initial data fetch.

It is possible for fragments to access variables declared in the operation as well:

Like many other type systems, GraphQL schemas include the ability to define Interface and Union types. You can learn more about them on the  Schemas and Types page.

If you are querying a field that returns an Interface or a Union type, you will need to use  inline fragments  to access data on the underlying concrete type. It’s easiest to see with an example:

In this query, the  hero  field returns the type  Character , which might be either a  Human  or a  Droid  depending on the  episode  argument. In the direct selection, you can only ask for fields on the  Character  interface, such as  name .

To ask for a field on the concrete type, you need to use an inline fragment with a type condition. Because the first fragment is labeled as  ... on Droid , the  primaryFunction  field will only be executed if the  Character  returned from  hero  is of the  Droid  type. Similarly for the  height  field for the  Human  type.

Named fragments can also be used in the same way, since a named fragment always has a type attached.

As we have seen with  Union types , there are some situations where you don’t know what type you’ll get back from the GraphQL service so you need some way to determine how to handle that data on the client.

GraphQL allows you to request  __typename , a meta field, at any point in a query to get the name of the Object type at that point:

In the above query,  search  returns a Union type that can be one of three options. Without the  __typename  field, it would be impossible for a client to tell the different types apart.

All field names beginning with two underscores ( __ ) are reserved by GraphQL. In addition to  __typename , GraphQL services provide the  __schema  and  __type  meta-fields which expose the  introspection  system.

We discussed above how variables enable us to avoid doing manual string interpolation to construct dynamic queries. Passing variables in arguments solves a large class of these problems, but we might also need a way to dynamically change the structure and shape of our queries using variables. For example, we can imagine a UI component that has a summarized and detailed view, where one includes more fields than the other.

Let’s construct a query for such a component:

Try editing the variables above to instead pass  true  for  withFriends , and see how the result changes.

We needed to use a feature in GraphQL called a  directive . Specifically, an  executable directive  can be attached to a field or fragment inclusion by a client, and can affect execution of the query in any way the server desires. The core GraphQL specification includes exactly two directives, which must be supported by any spec-compliant GraphQL server implementation:

Directives can be useful to get out of situations where you otherwise would need to do string manipulation to add and remove fields in your query. Server implementations may also add experimental features by defining completely new directives.

Looking for information on how to define directives that can be used to annotate the types, fields, or arguments in your GraphQL schema? See the  Schemas and Types page  for more information on defining and using type system directives.

To recap what we’ve learned about queries:

Now that we understand the ins and outs of how to read data from a GraphQL server with query operations, it’s time to learn how to change data and trigger side effects using  mutations .

Learn how to fetch data from a GraphQL server

GraphQL supports three main operation types—queries, mutations, and subscriptions. We have already seen several examples of basic queries in this guide, and on this page, you’ll learn in detail how to use the various features of query operations to read data from a server.

At its simplest, GraphQL is about asking for specific  fields  on objects. Let’s start by looking at the  hero  field that’s defined on the  Query  type in the schema:

We can see what result we get when we query it:

When creating a GraphQL  document  we always start with a  root operation type  (the  Query  Object type for this example) because it serves as an entry point to the API. From there we must specify the  selection set  of fields we are interested in, all the way down to their leaf values which will be Scalar or Enum types. The field  name  returns a  String  type, in this case the name of the main hero of Star Wars,  "R2-D2" .

The GraphQL specification indicates that a request’s result will be returned on a top-level  data  key in the response. If the request raised any errors, there will be information about what went wrong on a top-level  errors  key. From there, you can see that the result has the same shape as the query. This is essential to GraphQL, because you always get back what you expect, and the server knows exactly what fields the client is asking for.

In the previous example, we just asked for the name of our hero which returned a  String , but fields can also return Object types (and lists thereof). In that case, you can make a  sub-selection  of fields for that Object type:

GraphQL queries can traverse related objects and their fields, letting clients fetch lots of related data in one request, instead of making several roundtrips as one would need in a classic REST architecture.

Note that in this example, the  friends  field returns an array of items. GraphQL queries look the same for single items or lists of items; however, we know which one to expect based on what is indicated in the schema.

If the only thing we could do was traverse objects and their fields, GraphQL would already be a very useful language for data fetching. But when you add the ability to pass  arguments  to fields, things get much more interesting:

The client must then provide the required  id  value with the query:

In a system like REST, you can only pass a single set of arguments—the query parameters and URL segments in your request. But in GraphQL, every field and nested object can get its own set of arguments, making GraphQL a complete replacement for making multiple API fetches.

You can even pass arguments into fields that output Scalar types; one use case for this would be to implement data transformations once on the server, instead of on every client separately:

Arguments can be of many different types. In the above example, we have used an Enum type, which represents one of a finite set of options (in this case, units of length, either  METER  or  FOOT ). GraphQL comes with a  default set of types , but a GraphQL server can also declare custom types, as long as they can be serialized into your transport format.

Read more about the GraphQL type system here.

In the examples above we have been using a shorthand syntax where we omit the  query  keyword before the operation’s selection set. In addition to specifying the  operation type  explicitly, we can also add a unique  operation name , which is useful in production apps because it makes debugging and tracing easier.

Here’s an example that includes the  query  keyword as the operation type and  HeroNameAndFriends  as the operation name:

The operation type is either  query ,  mutation , or  subscription  and describes what type of operation you intend to do. This keyword is required unless you’re using the shorthand syntax for queries (it is always required for mutations and subscriptions). Additionally, if you wish to provide a name for your operation, then you must specify the operation type as well.

The operation name is an explicit name that you assign to your operation; you should pick a meaningful name. It is required when sending multiple operations in one document, but even if you’re only sending one operation it’s encouraged because operation names are helpful for debugging and server-side logging. When something goes wrong (you see errors either in your network logs or in the logs of your GraphQL server) it is easier to identify a query in your codebase by name instead of trying to decipher the contents.

Think of this just like a function name in your favorite programming language. For example, in JavaScript, we can easily work only with anonymous functions, but when we give a function a name, it’s easier to track it down, debug our code, and log when it’s called. In the same way, GraphQL query and mutation names, along with fragment names, can be a useful debugging tool on the server side to identify different GraphQL requests.

If you have a sharp eye, you may have noticed that, since the result object fields match the name of the fields in the query but don’t include arguments, you can’t directly query for the same field with different arguments. That’s why you need  aliases —they let you rename the result of a field to anything you want.

In the above example, the two  hero  fields would have conflicted, but since we can alias them to different names, we can get both results in one request.

So far, we have been writing all of our arguments inside the query string. But in most applications, the arguments to fields will be dynamic. For example, there might be a dropdown that lets you select which Star Wars episode you are interested in, or a search field, or a set of filters.

It wouldn’t be a good idea to pass these dynamic arguments directly in the query string, because then our client-side code would need to dynamically manipulate the query string at runtime, and serialize it into a GraphQL-specific format. Instead, GraphQL has a first-class way to factor dynamic values out of the query and pass them as a separate dictionary. These values are called  variables .

When we start working with variables, we need to do three things:

Here’s what it looks like all together:

You must specify an operation type and name in a GraphQL document to use variables.

Now, in our client code, we can simply pass a different variable rather than needing to construct an entirely new query. In general, this is also a good practice for denoting which arguments in our query are expected to be dynamic—we should never be doing string interpolation to construct queries from user-supplied values.

The variable definitions are the part that looks like  ($episode: Episode)  in the query above. It works just like the argument definitions for a function in a typed language. It lists all of the variables, prefixed by  $ , followed by their type, in this case,  Episode .

All declared variables must be either Scalar, Enum, or Input Object types. So if you want to pass a complex object into a field, you need to know what  input type  matches it on the server.

Variable definitions can be optional or required. In the case above, since there isn’t an  !  next to the  Episode  type, it’s optional. But if the field you are passing the variable into requires a non-null argument, then the variable has to be required as well.

To learn more about the syntax for these variable definitions, it’s useful to learn schema definition language (SDL), which is explained in detail on  the Schemas and Types page .

Default values can also be assigned to the variables in the query by adding the default value after the type declaration:

When default values are provided for all variables, you can call the query without passing any variables. If any variables are passed as part of the variables dictionary, they will override the defaults.

Let’s say we have a relatively complicated page in our app, which lets us look at two heroes side by side, along with their friends. You can imagine that such a query could quickly get complicated because we would need to repeat the fields at least once—one for each side of the comparison.

That’s why GraphQL includes reusable units called  fragments . Fragments let you construct sets of fields, and then include them in queries where needed. Here’s an example of how you could solve the above situation using fragments:

You can see how the above query would be pretty repetitive if we weren’t able to use fragments. The concept of fragments is frequently used to split complicated application data requirements into smaller chunks, especially when you need to combine many UI components with different fragments into one initial data fetch.

It is possible for fragments to access variables declared in the operation as well:

Like many other type systems, GraphQL schemas include the ability to define Interface and Union types. You can learn more about them on the  Schemas and Types page.

If you are querying a field that returns an Interface or a Union type, you will need to use  inline fragments  to access data on the underlying concrete type. It’s easiest to see with an example:

In this query, the  hero  field returns the type  Character , which might be either a  Human  or a  Droid  depending on the  episode  argument. In the direct selection, you can only ask for fields on the  Character  interface, such as  name .

To ask for a field on the concrete type, you need to use an inline fragment with a type condition. Because the first fragment is labeled as  ... on Droid , the  primaryFunction  field will only be executed if the  Character  returned from  hero  is of the  Droid  type. Similarly for the  height  field for the  Human  type.

Named fragments can also be used in the same way, since a named fragment always has a type attached.

As we have seen with  Union types , there are some situations where you don’t know what type you’ll get back from the GraphQL service so you need some way to determine how to handle that data on the client.

GraphQL allows you to request  __typename , a meta field, at any point in a query to get the name of the Object type at that point:

In the above query,  search  returns a Union type that can be one of three options. Without the  __typename  field, it would be impossible for a client to tell the different types apart.

All field names beginning with two underscores ( __ ) are reserved by GraphQL. In addition to  __typename , GraphQL services provide the  __schema  and  __type  meta-fields which expose the  introspection  system.

We discussed above how variables enable us to avoid doing manual string interpolation to construct dynamic queries. Passing variables in arguments solves a large class of these problems, but we might also need a way to dynamically change the structure and shape of our queries using variables. For example, we can imagine a UI component that has a summarized and detailed view, where one includes more fields than the other.

Let’s construct a query for such a component:

Try editing the variables above to instead pass  true  for  withFriends , and see how the result changes.

We needed to use a feature in GraphQL called a  directive . Specifically, an  executable directive  can be attached to a field or fragment inclusion by a client, and can affect execution of the query in any way the server desires. The core GraphQL specification includes exactly two directives, which must be supported by any spec-compliant GraphQL server implementation:

Directives can be useful to get out of situations where you otherwise would need to do string manipulation to add and remove fields in your query. Server implementations may also add experimental features by defining completely new directives.

Looking for information on how to define directives that can be used to annotate the types, fields, or arguments in your GraphQL schema? See the  Schemas and Types page  for more information on defining and using type system directives.

To recap what we’ve learned about queries:

Now that we understand the ins and outs of how to read data from a GraphQL server with query operations, it’s time to learn how to change data and trigger side effects using  mutations .

## Code Examples

### Example 1

```
type Query {
  hero: Character
}
```

### Example 2

```
type Query {
  hero: Character
}
```

### Example 3

```
type Query {
  human(id: ID!): Human
}
```

### Example 4

```
type Query {
  human(id: ID!): Human
}
```

### Example 5

```
query HeroNameAndFriends($episode: Episode = JEDI) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

### Example 6

```
query HeroNameAndFriends($episode: Episode = JEDI) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

### Example 7

```
type Query {
  hero: Character
}
```

### Example 8

```
type Query {
  hero: Character
}
```

### Example 9

```
type Query {
  human(id: ID!): Human
}
```

### Example 10

```
type Query {
  human(id: ID!): Human
}
```

### Example 11

```
query HeroNameAndFriends($episode: Episode = JEDI) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

### Example 12

```
query HeroNameAndFriends($episode: Episode = JEDI) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```


---

# Validation

**Source:** https://graphql.org/learn/validation/

## Table of Contents

- Validation examples
  - Requesting non-existent fields
  - Selection sets and leaf fields
  - Missing fragments for fields that output abstract types
  - Cyclic fragment spreads
- Validation errors
- Next steps
- Validation
- Validation examples
  - Requesting non-existent fields
  - Selection sets and leaf fields
  - Missing fragments for fields that output abstract types
  - Cyclic fragment spreads
- Validation errors
- Next steps

## Content

Learn how GraphQL validates operations using a schema

On this page, we’ll explore an important phase in the lifecycle of a GraphQL request called  validation . A request must be syntactically correct to run, but it should also be valid when checked against the API’s schema.

In practice, when a GraphQL operation reaches the server, the document is first parsed and then validated using the type system. This allows servers and clients to effectively inform developers when an invalid query has been created, without relying on runtime checks. Once the operation is validated, it can be  executed  on the server and a response will be delivered to the client.

The GraphQL specification describes the detailed conditions that must be satisfied for a request to be considered valid. In the sections that follow, we’ll look at a few examples of common validation issues that may occur in GraphQL operations.

When we query for a field, the field must be defined on the relevant type. As  hero  returns a  Character  type, its selection set may only request the  Character  type’s fields;  Character  does not have a  favoriteSpaceship  field, so this query is invalid:

Whenever we query for a field and it returns something other than a Scalar or Enum type, we need to specify what data we want to get back from the field (a “selection set”). The  hero  query field returns a  Character , and we’ve already seen examples that request fields like  name  and  appearsIn  on it. If we omit those leaf field selections, then the query will not be valid:

Similarly, querying fields of a scalar or enum doesn’t make sense, therefore adding a selection set to a leaf field will make the query invalid:

Earlier, it was noted that a query can only ask for fields on the type in question. So when we query for  hero  which returns a  Character , we can only request fields that exist on the  Character  Interface type. What happens if we want to query for R2-D2’s primary function, though?

That query is invalid, because  primaryFunction  is not one of the shared fields defined by the  Character  Interface type. We want some way of indicating that we wish to fetch  primaryFunction  if the  Character  is a  Droid , and to ignore that field otherwise. We can use the  fragments  to do this. By setting up a fragment defined on  Droid  and including it in the selection set, we ensure that we only query for  primaryFunction  where it is defined:

This query is valid, but it’s a bit verbose; named fragments were valuable above when we used them multiple times, but we’re only using this one once. Instead of using a named fragment, we can use an  inline fragment ; this still allows us to indicate the type we are querying on, but without naming a separate fragment:

To start, let’s take a complex valid query. This is a nested query, but with the duplicated fields factored out into a fragment:

The following is an alternative to the above query, attempting to use recursion instead of the explicit three levels of nesting. This new query is invalid because a fragment cannot refer to itself (directly or indirectly) since the resulting cycle could create an unbounded result!

This has just scratched the surface of the validation system; there are a number of validation rules in place to ensure that a GraphQL operation is semantically meaningful. The specification goes into more detail about this topic in the  validation section , and the  validation directory in the reference implementation  contains code implementing a specification-compliant GraphQL validator.

As we have seen in the examples above, when a GraphQL server encounters a validation error in a request, it will return information about what happened in the  errors  key of the response. Specifically, when GraphQL detects a validation issue in a document, it raises a  request error  before execution begins, meaning that no partial data will be included in the response.

And because the GraphQL specification requires all implementations to validate incoming requests against the schema, developers won’t need to write specific runtime logic to handle these validation issues manually.

To recap what we’ve learned about validation:

Head over to the  Execution  page to learn how GraphQL provides data for each field in a request after the validation step successfully completes.

Learn how GraphQL validates operations using a schema

On this page, we’ll explore an important phase in the lifecycle of a GraphQL request called  validation . A request must be syntactically correct to run, but it should also be valid when checked against the API’s schema.

In practice, when a GraphQL operation reaches the server, the document is first parsed and then validated using the type system. This allows servers and clients to effectively inform developers when an invalid query has been created, without relying on runtime checks. Once the operation is validated, it can be  executed  on the server and a response will be delivered to the client.

The GraphQL specification describes the detailed conditions that must be satisfied for a request to be considered valid. In the sections that follow, we’ll look at a few examples of common validation issues that may occur in GraphQL operations.

When we query for a field, the field must be defined on the relevant type. As  hero  returns a  Character  type, its selection set may only request the  Character  type’s fields;  Character  does not have a  favoriteSpaceship  field, so this query is invalid:

Whenever we query for a field and it returns something other than a Scalar or Enum type, we need to specify what data we want to get back from the field (a “selection set”). The  hero  query field returns a  Character , and we’ve already seen examples that request fields like  name  and  appearsIn  on it. If we omit those leaf field selections, then the query will not be valid:

Similarly, querying fields of a scalar or enum doesn’t make sense, therefore adding a selection set to a leaf field will make the query invalid:

Earlier, it was noted that a query can only ask for fields on the type in question. So when we query for  hero  which returns a  Character , we can only request fields that exist on the  Character  Interface type. What happens if we want to query for R2-D2’s primary function, though?

That query is invalid, because  primaryFunction  is not one of the shared fields defined by the  Character  Interface type. We want some way of indicating that we wish to fetch  primaryFunction  if the  Character  is a  Droid , and to ignore that field otherwise. We can use the  fragments  to do this. By setting up a fragment defined on  Droid  and including it in the selection set, we ensure that we only query for  primaryFunction  where it is defined:

This query is valid, but it’s a bit verbose; named fragments were valuable above when we used them multiple times, but we’re only using this one once. Instead of using a named fragment, we can use an  inline fragment ; this still allows us to indicate the type we are querying on, but without naming a separate fragment:

To start, let’s take a complex valid query. This is a nested query, but with the duplicated fields factored out into a fragment:

The following is an alternative to the above query, attempting to use recursion instead of the explicit three levels of nesting. This new query is invalid because a fragment cannot refer to itself (directly or indirectly) since the resulting cycle could create an unbounded result!

This has just scratched the surface of the validation system; there are a number of validation rules in place to ensure that a GraphQL operation is semantically meaningful. The specification goes into more detail about this topic in the  validation section , and the  validation directory in the reference implementation  contains code implementing a specification-compliant GraphQL validator.

As we have seen in the examples above, when a GraphQL server encounters a validation error in a request, it will return information about what happened in the  errors  key of the response. Specifically, when GraphQL detects a validation issue in a document, it raises a  request error  before execution begins, meaning that no partial data will be included in the response.

And because the GraphQL specification requires all implementations to validate incoming requests against the schema, developers won’t need to write specific runtime logic to handle these validation issues manually.

To recap what we’ve learned about validation:

Head over to the  Execution  page to learn how GraphQL provides data for each field in a request after the validation step successfully completes.


---

# Performance

**Source:** https://graphql.org/learn/performance/

## Table of Contents

- Client-side caching
- GET
- The N+1 Problem
- Demand control
- JSON (with GZIP)
- Performance monitoring
- Recap
- Performance
- Client-side caching
- GET
- The N+1 Problem
- Demand control
- JSON (with GZIP)
- Performance monitoring
- Recap

## Content

Optimize the execution and delivery of GraphQL responses

At first glance, GraphQL requests may seem challenging to cache given that the API is served through a single endpoint and you may not know in advance what fields a client will include in an operation.

In practice, however, GraphQL is as cacheable as any API that enables parameterized requests, such as a REST API that allows clients to specify different query parameters for a particular endpoint. There are many ways to optimize GraphQL requests on the client and server sides, as well as in the transport layer, and different GraphQL server and client libraries will often have common caching features built directly into them.

On this page, we’ll explore several different tactics that can be leveraged in GraphQL clients and servers to optimize how data is fetched from the API.

There is a range of client-side caching strategies that GraphQL clients may implement to help not only with performance but also to ensure that you render a consistent and responsive interface to your users.  Read more about client-side caching .

GraphQL implementations that adhere to the  GraphQL over HTTP specification  will support the  POST  HTTP method by default, but may also support  GET  requests for query operations.

Using  GET  can improve query performance because requests made with this HTTP method are typically considered cacheable by default and can help facilitate HTTP caching or the use of a content delivery network (CDN) when caching-related headers are provided in the server response.

However, because browsers and CDNs impose size limits on URLs, it may not be possible to send a large document for complex operations in the query string of the URL. Using  persisted queries , either in the form of  trusted documents  or  automatic persisted queries , will allow the client to send a hash of the query instead, and the server can look up the full version of the document by looking up the hash in a server-side store before validating and executing the operation.

Sending hashed queries instead of their plaintext versions has the additional benefit of reducing the amount of data sent by the client in the network request.

GraphQL is designed in a way that allows you to write clean code on the server, where every field on every type has a focused single-purpose function for resolving that value. However, without additional consideration, a naive GraphQL service could be very “chatty” or repeatedly load data from your databases.

Consider the following query—to fetch a hero along with a list of their friends, we can imagine that as the field resolvers execute there will be one request to the underlying data source to get the character object, another to fetch their friends, and then three subsequent requests to load the lists of starships for each human friend:

This is known as the N+1 problem, where an initial request to an underlying data source (for a hero’s friends) leads to N subsequent requests to resolve the data for all of the requested fields (for the friends’ starship data).

This is commonly solved by a batching technique, where multiple requests for data from a backend are collected over a short period and then dispatched in a single request to an underlying database or microservice by using a tool like Facebook’s  DataLoader . Additionally, certain GraphQL implementations may have built-in capabilities that allow you to translate operation selection sets into optimized queries to underlying data sources.

Depending on how a GraphQL schema has been designed, it may be possible for clients to request highly complex operations that place excessive load on the underlying data sources during execution. These kinds of operations may be sent inadvertently by an known client, but they may also be sent by malicious actors.

Certain demand control mechanisms can help guard a GraphQL API against these operations, such as paginating list fields, limiting operation depth and breadth, and query complexity analysis.  You can read more about demand control on the Security page .

GraphQL services typically respond using JSON even though the GraphQL spec  does not require it . JSON may seem like an odd choice for an API layer promising better network performance, however, because it is mostly text it compresses exceptionally well with algorithms such as GZIP, deflate and brotli.

It’s encouraged that any production GraphQL services enable GZIP and encourage their clients to send the header:

JSON is also very familiar to client and API developers, and is easy to read and debug. In fact, the GraphQL syntax is partly inspired by JSON syntax.

Monitoring a GraphQL API over time can provide insight into how certain operations impact API performance and help you determine what adjustments to make to maintain its health. For example, you may find that certain fields take a long time to resolve due to under-optimized requests to a backing data source, or you may find that other fields routinely raise errors during execution.

Observability tooling can provide insight into where bottlenecks exist in the execution of certain GraphQL operations by allowing you to instrument the API service to collect metrics, traces, and logs during requests. For example,  OpenTelemetry  provides a suite of vendor-agnostic tools that can be used in many different languages to support instrumentation of GraphQL APIs.

To recap these recommendations for improving GraphQL API performance:

Optimize the execution and delivery of GraphQL responses

At first glance, GraphQL requests may seem challenging to cache given that the API is served through a single endpoint and you may not know in advance what fields a client will include in an operation.

In practice, however, GraphQL is as cacheable as any API that enables parameterized requests, such as a REST API that allows clients to specify different query parameters for a particular endpoint. There are many ways to optimize GraphQL requests on the client and server sides, as well as in the transport layer, and different GraphQL server and client libraries will often have common caching features built directly into them.

On this page, we’ll explore several different tactics that can be leveraged in GraphQL clients and servers to optimize how data is fetched from the API.

There is a range of client-side caching strategies that GraphQL clients may implement to help not only with performance but also to ensure that you render a consistent and responsive interface to your users.  Read more about client-side caching .

GraphQL implementations that adhere to the  GraphQL over HTTP specification  will support the  POST  HTTP method by default, but may also support  GET  requests for query operations.

Using  GET  can improve query performance because requests made with this HTTP method are typically considered cacheable by default and can help facilitate HTTP caching or the use of a content delivery network (CDN) when caching-related headers are provided in the server response.

However, because browsers and CDNs impose size limits on URLs, it may not be possible to send a large document for complex operations in the query string of the URL. Using  persisted queries , either in the form of  trusted documents  or  automatic persisted queries , will allow the client to send a hash of the query instead, and the server can look up the full version of the document by looking up the hash in a server-side store before validating and executing the operation.

Sending hashed queries instead of their plaintext versions has the additional benefit of reducing the amount of data sent by the client in the network request.

GraphQL is designed in a way that allows you to write clean code on the server, where every field on every type has a focused single-purpose function for resolving that value. However, without additional consideration, a naive GraphQL service could be very “chatty” or repeatedly load data from your databases.

Consider the following query—to fetch a hero along with a list of their friends, we can imagine that as the field resolvers execute there will be one request to the underlying data source to get the character object, another to fetch their friends, and then three subsequent requests to load the lists of starships for each human friend:

This is known as the N+1 problem, where an initial request to an underlying data source (for a hero’s friends) leads to N subsequent requests to resolve the data for all of the requested fields (for the friends’ starship data).

This is commonly solved by a batching technique, where multiple requests for data from a backend are collected over a short period and then dispatched in a single request to an underlying database or microservice by using a tool like Facebook’s  DataLoader . Additionally, certain GraphQL implementations may have built-in capabilities that allow you to translate operation selection sets into optimized queries to underlying data sources.

Depending on how a GraphQL schema has been designed, it may be possible for clients to request highly complex operations that place excessive load on the underlying data sources during execution. These kinds of operations may be sent inadvertently by an known client, but they may also be sent by malicious actors.

Certain demand control mechanisms can help guard a GraphQL API against these operations, such as paginating list fields, limiting operation depth and breadth, and query complexity analysis.  You can read more about demand control on the Security page .

GraphQL services typically respond using JSON even though the GraphQL spec  does not require it . JSON may seem like an odd choice for an API layer promising better network performance, however, because it is mostly text it compresses exceptionally well with algorithms such as GZIP, deflate and brotli.

It’s encouraged that any production GraphQL services enable GZIP and encourage their clients to send the header:

JSON is also very familiar to client and API developers, and is easy to read and debug. In fact, the GraphQL syntax is partly inspired by JSON syntax.

Monitoring a GraphQL API over time can provide insight into how certain operations impact API performance and help you determine what adjustments to make to maintain its health. For example, you may find that certain fields take a long time to resolve due to under-optimized requests to a backing data source, or you may find that other fields routinely raise errors during execution.

Observability tooling can provide insight into where bottlenecks exist in the execution of certain GraphQL operations by allowing you to instrument the API service to collect metrics, traces, and logs during requests. For example,  OpenTelemetry  provides a suite of vendor-agnostic tools that can be used in many different languages to support instrumentation of GraphQL APIs.

To recap these recommendations for improving GraphQL API performance:

## Code Examples

### Example 1

```
Accept-Encoding: gzip
```

### Example 2

```
Accept-Encoding: gzip
```

### Example 3

```
Accept-Encoding: gzip
```

### Example 4

```
Accept-Encoding: gzip
```


---

# Response

**Source:** https://graphql.org/learn/response/

## Table of Contents

- Data
- Errors
  - Request errors
  - Field errors
  - Network errors
- Extensions
- Next steps
- Response
- Data
- Errors
  - Request errors
  - Field errors
  - Network errors
- Extensions
- Next steps

## Content

Learn how GraphQL returns data to clients

After a GraphQL document has been  validated  and  executed , the server will return a  response  to the requesting client. One of GraphQL’s strengths is that the server response reflects the shape of the client’s original request, but a response may also contain helpful information if something unexpected happened before or during the execution of an operation. On this page, we’ll take a deeper exploration of this final phase in the lifecycle of a GraphQL request.

As we have seen in the examples throughout this guide, when a GraphQL request is executed the response is returned on a top-level  data  key. For example:

The inclusion of the  data  key isn’t an arbitrary decision made by the underlying GraphQL implementation—it’s described by the  GraphQL specification . Under this key, you will find the result of the execution of the requested operation, which may include partial data for the requested fields if errors were raised during the execution of some field resolvers.

One thing that the GraphQL specification doesn’t require is a specific serialization format for the response. That said, responses are typically formatted as JSON, as in the example above.

Additionally, the GraphQL specification doesn’t require the use of a particular transport protocol for requests either, although it is  common for HTTP to be used  for stateless query and mutations operations. Long-lived, stateful subscription operations are often supported by WebSockets or server-sent events instead.

There is  a draft GraphQL over HTTP specification  available with further guidelines for using HTTP with GraphQL clients and servers.

In addition to the  data  key, the GraphQL specification outlines how  errors  should be formatted in the response. Whether the GraphQL implementation provides partial data with the error information in the response will depend on the type of error that was raised. Let’s look at the different kinds of errors that can occur during the lifecycle of a GraphQL request.

Request errors typically occur because the client made a mistake. For example, there may be a  syntax error  in the document, such as a missing bracket or the use of an unknown root operation type keyword:

The  message  key inside the  errors  object provides some helpful information for the developer to understand what kind of error was raised, and the  locations  key, if present, indicates where the error occurred in the document.

Sometimes, a GraphQL request may be syntactically correct, but when the server parses and  validates  the document against the schema, it finds an issue with part of the operation and raises a  validation error .

For example, the client may request a field that does not exist on the  Starship  type:

Validation errors also occur when clients specify incorrect variable types for their corresponding field arguments:

In the previous examples, we can see that when a request error occurs the  data  key will not be included because the server returns an error response to the client before the field resolvers are executed.

Field errors are raised if something unexpected happens during execution. For example, a resolver may raise an error directly, or may return invalid data such as a null value for a field that has a Non-Null output type.

In these cases, GraphQL will attempt to continue executing the other fields and return a partial response, with the  data  key appearing alongside the  errors  key.

Let’s look at an example:

The mutation above attempts to delete two starships in a single operation, but no starship exists with an ID of  3010  so the server throws an error. In the response, we can see information about the error that occurred for the field that was aliased as  secondShip . Under the  data  key, we also see that the ID of the first ship was returned to indicate successful deletion, while the second ship produced a null result due to the error raised in its resolver function.

As with network calls to any type of API, network errors that are not specific to GraphQL may happen at any point during a request. These kinds of errors will block communication between the client and server before the request is complete, such as an SSL error or a connection timeout. Depending on the GraphQL server and client libraries that you choose, there may be features built into them that support special network error handling such as retries for failed operations.

The final top-level key allowed by the GraphQL specification in a response is the  extensions  key. This key is reserved for GraphQL implementations to provide additional information about the response and though it must be an object if present, there are no other restrictions on what it may contain.

For example, some GraphQL servers may include telemetry data or information about rate limit consumption under this key. Note that what data is available in  extensions  and whether this data is available in production or development environments will depend entirely on the specific GraphQL implementation.

To recap what we’ve learned about GraphQL response formats:

Now that you understand the different phases of a GraphQL request and how responses are provided to clients, head over to the  Introspection  page to learn about how a GraphQL server can query information about its own schema.

Learn how GraphQL returns data to clients

After a GraphQL document has been  validated  and  executed , the server will return a  response  to the requesting client. One of GraphQL’s strengths is that the server response reflects the shape of the client’s original request, but a response may also contain helpful information if something unexpected happened before or during the execution of an operation. On this page, we’ll take a deeper exploration of this final phase in the lifecycle of a GraphQL request.

As we have seen in the examples throughout this guide, when a GraphQL request is executed the response is returned on a top-level  data  key. For example:

The inclusion of the  data  key isn’t an arbitrary decision made by the underlying GraphQL implementation—it’s described by the  GraphQL specification . Under this key, you will find the result of the execution of the requested operation, which may include partial data for the requested fields if errors were raised during the execution of some field resolvers.

One thing that the GraphQL specification doesn’t require is a specific serialization format for the response. That said, responses are typically formatted as JSON, as in the example above.

Additionally, the GraphQL specification doesn’t require the use of a particular transport protocol for requests either, although it is  common for HTTP to be used  for stateless query and mutations operations. Long-lived, stateful subscription operations are often supported by WebSockets or server-sent events instead.

There is  a draft GraphQL over HTTP specification  available with further guidelines for using HTTP with GraphQL clients and servers.

In addition to the  data  key, the GraphQL specification outlines how  errors  should be formatted in the response. Whether the GraphQL implementation provides partial data with the error information in the response will depend on the type of error that was raised. Let’s look at the different kinds of errors that can occur during the lifecycle of a GraphQL request.

Request errors typically occur because the client made a mistake. For example, there may be a  syntax error  in the document, such as a missing bracket or the use of an unknown root operation type keyword:

The  message  key inside the  errors  object provides some helpful information for the developer to understand what kind of error was raised, and the  locations  key, if present, indicates where the error occurred in the document.

Sometimes, a GraphQL request may be syntactically correct, but when the server parses and  validates  the document against the schema, it finds an issue with part of the operation and raises a  validation error .

For example, the client may request a field that does not exist on the  Starship  type:

Validation errors also occur when clients specify incorrect variable types for their corresponding field arguments:

In the previous examples, we can see that when a request error occurs the  data  key will not be included because the server returns an error response to the client before the field resolvers are executed.

Field errors are raised if something unexpected happens during execution. For example, a resolver may raise an error directly, or may return invalid data such as a null value for a field that has a Non-Null output type.

In these cases, GraphQL will attempt to continue executing the other fields and return a partial response, with the  data  key appearing alongside the  errors  key.

Let’s look at an example:

The mutation above attempts to delete two starships in a single operation, but no starship exists with an ID of  3010  so the server throws an error. In the response, we can see information about the error that occurred for the field that was aliased as  secondShip . Under the  data  key, we also see that the ID of the first ship was returned to indicate successful deletion, while the second ship produced a null result due to the error raised in its resolver function.

As with network calls to any type of API, network errors that are not specific to GraphQL may happen at any point during a request. These kinds of errors will block communication between the client and server before the request is complete, such as an SSL error or a connection timeout. Depending on the GraphQL server and client libraries that you choose, there may be features built into them that support special network error handling such as retries for failed operations.

The final top-level key allowed by the GraphQL specification in a response is the  extensions  key. This key is reserved for GraphQL implementations to provide additional information about the response and though it must be an object if present, there are no other restrictions on what it may contain.

For example, some GraphQL servers may include telemetry data or information about rate limit consumption under this key. Note that what data is available in  extensions  and whether this data is available in production or development environments will depend entirely on the specific GraphQL implementation.

To recap what we’ve learned about GraphQL response formats:

Now that you understand the different phases of a GraphQL request and how responses are provided to clients, head over to the  Introspection  page to learn about how a GraphQL server can query information about its own schema.


---

# Thinking in Graphs

**Source:** https://graphql.org/learn/thinking-in-graphs/

## Table of Contents

- It’s Graphs All the Way Down
- Shared Language
- Business Logic Layer
  - Working with Legacy Data
- One Step at a time
- Footnotes
- Thinking in Graphs
- It’s Graphs All the Way Down
- Shared Language
- Business Logic Layer
  - Working with Legacy Data
- One Step at a time
- Footnotes

## Content

With GraphQL, you model your business domain as a graph

Graphs are powerful tools for modeling many real-world phenomena because they resemble our natural mental models and verbal descriptions of the underlying process. With GraphQL, you model your business domain as a graph by defining a schema; within your schema, you define different types of nodes and how they connect/relate to one another. On the client, this creates a pattern similar to Object-Oriented Programming: types that reference other types. On the server, since GraphQL only defines the interface, you have the freedom to use it with any backend (new or legacy!).

Naming things is a hard but important part of building intuitive APIs

Think of your GraphQL schema as an expressive shared language for your team and your users. To build a good schema, examine the everyday language you use to describe your business. For example, let’s try to describe an email app in plain English:

Naming things is a hard but important part of building intuitive APIs, so take time to carefully think about what makes sense for your problem domain and users. Your team should develop a shared understanding and consensus of these business domain rules because you will need to choose intuitive, durable names for nodes and relationships in the GraphQL schema. Try to imagine some of the queries you will want to execute:

Fetch the number of unread emails in my inbox for all my accounts

Fetch the “preview info” for the first 20 drafts in the main account

Your business logic layer should act as the single source of truth for enforcing business domain rules

Where should you define the actual business logic? Where should you perform validation and authorization checks? The answer: inside a dedicated business logic layer. Your business logic layer should act as the single source of truth for enforcing business domain rules.

In the diagram above, all entry points (REST, GraphQL, and RPC) into the system will be processed with the same validation, authorization, and error handling rules.

Prefer building a GraphQL schema that describes how clients use the data, rather than mirroring the legacy database schema.

Sometimes, you will find yourself working with legacy data sources that do not perfectly reflect how clients consume the data. In these cases, prefer building a GraphQL schema that describes how clients use the data, rather than mirroring the legacy database schema.

Build your GraphQL schema to express “how” rather than “what”. Then you can improve your implementation details without breaking the interface with older clients.

Get validation and feedback more frequently

Don’t try to model your entire business domain in one sitting. Rather, build only the part of the schema that you need for one scenario at a time. By gradually expanding the schema, you will get validation and feedback more frequently to steer you toward building the right solution.

“Turtles all the way down”  is an expression of the problem of infinite regress.  ↩

With GraphQL, you model your business domain as a graph

Graphs are powerful tools for modeling many real-world phenomena because they resemble our natural mental models and verbal descriptions of the underlying process. With GraphQL, you model your business domain as a graph by defining a schema; within your schema, you define different types of nodes and how they connect/relate to one another. On the client, this creates a pattern similar to Object-Oriented Programming: types that reference other types. On the server, since GraphQL only defines the interface, you have the freedom to use it with any backend (new or legacy!).

Naming things is a hard but important part of building intuitive APIs

Think of your GraphQL schema as an expressive shared language for your team and your users. To build a good schema, examine the everyday language you use to describe your business. For example, let’s try to describe an email app in plain English:

Naming things is a hard but important part of building intuitive APIs, so take time to carefully think about what makes sense for your problem domain and users. Your team should develop a shared understanding and consensus of these business domain rules because you will need to choose intuitive, durable names for nodes and relationships in the GraphQL schema. Try to imagine some of the queries you will want to execute:

Fetch the number of unread emails in my inbox for all my accounts

Fetch the “preview info” for the first 20 drafts in the main account

Your business logic layer should act as the single source of truth for enforcing business domain rules

Where should you define the actual business logic? Where should you perform validation and authorization checks? The answer: inside a dedicated business logic layer. Your business logic layer should act as the single source of truth for enforcing business domain rules.

In the diagram above, all entry points (REST, GraphQL, and RPC) into the system will be processed with the same validation, authorization, and error handling rules.

Prefer building a GraphQL schema that describes how clients use the data, rather than mirroring the legacy database schema.

Sometimes, you will find yourself working with legacy data sources that do not perfectly reflect how clients consume the data. In these cases, prefer building a GraphQL schema that describes how clients use the data, rather than mirroring the legacy database schema.

Build your GraphQL schema to express “how” rather than “what”. Then you can improve your implementation details without breaking the interface with older clients.

Get validation and feedback more frequently

Don’t try to model your entire business domain in one sitting. Rather, build only the part of the schema that you need for one scenario at a time. By gradually expanding the schema, you will get validation and feedback more frequently to steer you toward building the right solution.

“Turtles all the way down”  is an expression of the problem of infinite regress.  ↩

## Code Examples

### Example 1

```
{
  accounts {
    inbox {
      unreadEmailCount
    }
  }
}
```

### Example 2

```
{
  accounts {
    inbox {
      unreadEmailCount
    }
  }
}
```

### Example 3

```
{
  mainAccount {
    drafts(first: 20) {
      ...previewInfo
    }
  }
}
 
fragment previewInfo on Email {
  subject
  bodyPreviewSentence
}
```

### Example 4

```
{
  mainAccount {
    drafts(first: 20) {
      ...previewInfo
    }
  }
}
 
fragment previewInfo on Email {
  subject
  bodyPreviewSentence
}
```

### Example 5

```
{
  accounts {
    inbox {
      unreadEmailCount
    }
  }
}
```

### Example 6

```
{
  accounts {
    inbox {
      unreadEmailCount
    }
  }
}
```

### Example 7

```
{
  mainAccount {
    drafts(first: 20) {
      ...previewInfo
    }
  }
}
 
fragment previewInfo on Email {
  subject
  bodyPreviewSentence
}
```

### Example 8

```
{
  mainAccount {
    drafts(first: 20) {
      ...previewInfo
    }
  }
}
 
fragment previewInfo on Email {
  subject
  bodyPreviewSentence
}
```


---

# Schemas and Types

**Source:** https://graphql.org/learn/schema/

## Table of Contents

- Type system
- Type language
- Object types and fields
  - Arguments
  - The Query, Mutation, and Subscription types
- Scalar types
- Enum types
- Type modifiers
  - Non-Null
  - List
- Interface types
- Union types
- Input Object types
- Directives
- Documentation
  - Descriptions
  - Comments
- Next steps
- Schemas and Types
- Type system
- Type language
- Object types and fields
  - Arguments
  - The Query, Mutation, and Subscription types
- Scalar types
- Enum types
- Type modifiers
  - Non-Null
  - List
- Interface types
- Union types
- Input Object types
- Directives
- Documentation
  - Descriptions
  - Comments
- Next steps

## Content

Learn about the different elements of the GraphQL type system

The GraphQL  type system  describes what data can be queried from the API. The collection of those capabilities is referred to as the service’s  schema  and clients can use that schema to send queries to the API that return predictable results.

On this page, we’ll explore GraphQL’s  six kinds of named type definitions  as well as other features of the type system to learn how they may be used to describe your data and the relationships between them. Since GraphQL can be used with any backend framework or programming language, we’ll avoid implementation-specific details and talk only about the concepts.

If you’ve seen a GraphQL query before, you know that the GraphQL query language is basically about selecting fields on objects. So, for example, in the following query:

Because the shape of a GraphQL query closely matches the result, we can predict what the query will return without knowing that much about the server. But it’s useful to have an exact description of the data we can request. For example, what fields can we select? What kinds of objects might they return? What fields are available on those sub-objects?

That’s where the schema comes in. Every GraphQL service defines a set of types that completely describe the set of possible data we can query on that service. Then, when requests come in, they are validated and executed against that schema.

GraphQL services can be written in any language and there are many different approaches you can take when defining the types in a schema:

Since we can’t rely on a specific programming language to discuss GraphQL schemas in this guide, we’ll use SDL because it’s similar to the query language that we’ve seen so far and allows us to talk about GraphQL schemas in a language-agnostic way.

The most basic components of a GraphQL schema are  Object types , which just represent a kind of object you can fetch from your service, and what fields it has. In SDL, we represent it like this:

The language is readable, but let’s go over it so that we can have a shared vocabulary:

Now you know what a GraphQL Object type looks like and how to read the basics of SDL.

Every field on a GraphQL Object type can have zero or more  arguments , for example, the  length  field below:

All arguments are named. Unlike languages such as JavaScript and Python where functions take a list of ordered arguments, all arguments in GraphQL are passed by name specifically. In this case, the  length  field has one defined argument called  unit .

Arguments can be either required or optional. When an argument is optional, we can define a  default value . If the  unit  argument is not passed, then it will be set to  METER  by default.

Every GraphQL schema must support  query  operations. The  entry point  for this  root operation type  is a regular Object type called  Query  by default. So if you see a query that looks like this:

That means that the GraphQL service needs to have a  Query  type with a  droid  field:

Schemas may also support  mutation  and  subscription  operations by adding additional  Mutation  and  Subscription  types and then defining fields on the corresponding root operation types.

It’s important to remember that other than the special status of being entry points into the schema, the  Query ,  Mutation , and  Subscription  types are the same as any other GraphQL Object type, and their fields work exactly the same way.

You can name your root operation types differently too; if you choose to do so then you will need to inform GraphQL of the new names using the  schema  keyword:

A GraphQL Object type has a name and fields, but at some point, those fields must resolve to some concrete data. That’s where the  Scalar types  come in: they represent the leaf values of the query.

In the following query, the  name  and  appearsIn  fields will resolve to Scalar types:

We know this because those fields don’t have any sub-fields—they are the leaves of the query.

GraphQL comes with a set of  default Scalar types  out of the box:

In most GraphQL service implementations, there is also a way to specify custom Scalar types. For example, we could define a  Date  type:

Then it’s up to our implementation to define how that type should be serialized, deserialized, and validated. For example, you could specify that the  Date  type should always be serialized into an integer timestamp, and your client should know to expect that format for any date fields.

Enum types , also known as enumeration types, are a special kind of scalar that is restricted to a particular set of allowed values. This allows you to:

Here’s what an Enum type definition looks like in SDL:

This means that wherever we use the type  Episode  in our schema, we expect it to be exactly one of  NEWHOPE ,  EMPIRE , or  JEDI .

Types are assumed to be nullable and singular by default in GraphQL. However, when you use these named types in a schema (or in  query variable declarations ) you can apply additional  type modifiers  that will affect the meaning of those values.

As we saw with the Object type example above, GraphQL supports two type modifiers—the  List  and  Non-Null  types—and they can be used individually or in combination with each other.

Let’s look at an example:

Here, we’re using a  String  type and marking it as a Non-Null type by adding an exclamation mark ( ! ) after the type name. This means that our server always expects to return a non-null value for this field, and if the resolver produces a null value, then that will trigger a GraphQL execution error, letting the client know that something has gone wrong.

As we saw in an example above, the Non-Null type modifier can also be used when defining arguments for a field, which will cause the GraphQL server to return a validation error if a null value is passed as that argument:

Lists work in a similar way. We can use a type modifier to mark a type as a List type, which indicates that this field will return an array of that type. In SDL, this is denoted by wrapping the type in square brackets,  [  and  ] . It works the same for arguments, where the validation step will expect an array for that value. Here’s an example:

As we see above, the Non-Null and List modifiers can be combined. For example, you can have a List of Non-Null  String  types:

This means that the  list itself  can be null, but it can’t have any null members. For example, in JSON:

Now, let’s say we defined a Non-Null List of  String  types:

This means that the list itself cannot be null, but it can contain null values:

Lastly, you can also have a Non-Null List of Non-Null  String  types:

This means that the list cannot be null and it cannot contain null values:

You can arbitrarily nest any number of Non-Null and List modifiers, according to your needs.

Like many type systems, GraphQL supports  abstract types . The first of these types that we’ll explore is an  Interface type , which defines a certain set of fields that a concrete Object type or other Interface type must also include to implement it.

For example, you could have a  Character  Interface type that represents any character in the Star Wars trilogy:

This means that any type that  implements   Character  needs to have these exact fields as well as the same arguments and return types.

For example, here are some types that might implement  Character :

You can see that both of these types have all of the fields from the  Character  Interface type, but also bring in extra fields— totalCredits ,  starships  and  primaryFunction —that are specific to that particular type of character.

Interface types are useful when you want to return an object or set of objects, but those might be of several different types. For example, note that the following query produces an error:

The  hero  field returns the type  Character , which means it might be either a  Human  or a  Droid  depending on the  episode  argument. In the query above, you can only ask for fields that exist on the  Character  Interface type, which doesn’t include  primaryFunction .

To ask for a field on a specific Object type, you need to use an  inline fragment :

Interface types may also implement other Interface types:

Note that Interface types may not implement themselves or contain any cyclic references to each other.

GraphQL supports a second abstract type called a  Union type . Union types share similarities with Interface types, but they cannot define any shared fields among the constituent types.

A Union type is defined by indicating its member Object types:

Wherever we return a  SearchResult  type in our schema, we might get a  Human , a  Droid , or a  Starship . Note that members of a Union type need to be concrete Object types; you can’t define one using Interface types or other Union types as members.

In this case, if you query a field that returns the  SearchResult  Union type, you need to use an  inline fragment  to be able to query any fields that are defined on the member Object types:

The  __typename  field is a special  meta-field  that automatically exists on every Object type and resolves to the name of that type, providing a way to differentiate between data types on the client.

Also, in this case, since  Human  and  Droid  share a common interface ( Character ), you can query their common fields in one place and still get the same result:

Note that  name  is still specified on  Starship  because otherwise it wouldn’t show up in the results given that  Starship  is not a  Character !

Most of the examples we’ve covered on this page demonstrate how Object, Scalar, Enum, Interface, and Union types may be used as  output types  for the fields in a schema. But we have also seen that field arguments must specify their  input types .

So far, we’ve only talked about using scalar values (like Enums or String types) as an input type for a field argument. However, you can also pass complex objects as arguments using an  Input Object type , which is the last kind of named types in GraphQL that we’ll explore.

This is particularly valuable in the case of  mutations , where you might want to pass in a whole object to be created. In SDL, Input Object types look similar to regular Object types, but with the keyword  input  instead of  type :

Here is how you could use the Input Object type in a mutation:

The fields on an Input Object type can refer to other Input Object types, but you can’t mix input and output types in your schema. Input Object types also can’t have arguments on their fields.

In certain instances where field arguments are insufficient or certain common behaviors must be replicated in multiple locations,  directives  allow us to modify parts of a GraphQL schema or operation by using an  @  character followed by the directive’s name.

Type system directives  allow us to annotate the types, fields, and arguments in a schema so that they may be validated or executed differently.

Directives may also be defined for use in GraphQL operations as  executable directives .  Read more about executable directives on the Queries page.

The GraphQL specification defines several  built-in directives . For example, for implementations that support SDL, the  @deprecated  directive will be available to annotate deprecated parts of the schema:

While you won’t need to define the  @deprecated  directive in your schema explicitly if you’re using a GraphQL implementation that supports SDL, it’s underlying definition would look like this:

Note that, just like fields, directives can accept arguments and those arguments can have default values. The  @deprecated  directive has a nullable  reason  argument that accepts a  String  as an input type and falls back to  "No longer supported" . And with directives, we must specify where they may be used, such as on a  FIELD_DEFINITION  or  ENUM_VALUE  for the  @deprecated  directive.

In addition to GraphQL’s built-in directives, you may define your own  custom directives . As with custom Scalar types, it’s up to your chosen GraphQL implementation to determine how to handle custom directives during query execution.

GraphQL allows you to add documentation to the types, fields, and arguments in a schema. In fact, the GraphQL specification encourages you to do this in all cases unless the name of the type, field, or argument is self-descriptive. Schema descriptions are defined using Markdown syntax and can be multiline or single line.

In SDL, they would look like this:

In addition to making a GraphQL API schema more expressive, descriptions are helpful to client developers because they are available in  introspection queries  and visible in developer tools such as  GraphiQL .

Occasionally, you may need to add comments in a schema that do not describe types, fields, or arguments, and are not meant to be seen by clients. In those cases, you can add a single-line comment to SDL by preceding the text with a  #  character:

Comments may also be added to client queries:

To recap what we’ve learned about schemas and types:

Now that you understand the key features of the type system, you’re ready to learn more about how to  query data from a GraphQL API .

Learn about the different elements of the GraphQL type system

The GraphQL  type system  describes what data can be queried from the API. The collection of those capabilities is referred to as the service’s  schema  and clients can use that schema to send queries to the API that return predictable results.

On this page, we’ll explore GraphQL’s  six kinds of named type definitions  as well as other features of the type system to learn how they may be used to describe your data and the relationships between them. Since GraphQL can be used with any backend framework or programming language, we’ll avoid implementation-specific details and talk only about the concepts.

If you’ve seen a GraphQL query before, you know that the GraphQL query language is basically about selecting fields on objects. So, for example, in the following query:

Because the shape of a GraphQL query closely matches the result, we can predict what the query will return without knowing that much about the server. But it’s useful to have an exact description of the data we can request. For example, what fields can we select? What kinds of objects might they return? What fields are available on those sub-objects?

That’s where the schema comes in. Every GraphQL service defines a set of types that completely describe the set of possible data we can query on that service. Then, when requests come in, they are validated and executed against that schema.

GraphQL services can be written in any language and there are many different approaches you can take when defining the types in a schema:

Since we can’t rely on a specific programming language to discuss GraphQL schemas in this guide, we’ll use SDL because it’s similar to the query language that we’ve seen so far and allows us to talk about GraphQL schemas in a language-agnostic way.

The most basic components of a GraphQL schema are  Object types , which just represent a kind of object you can fetch from your service, and what fields it has. In SDL, we represent it like this:

The language is readable, but let’s go over it so that we can have a shared vocabulary:

Now you know what a GraphQL Object type looks like and how to read the basics of SDL.

Every field on a GraphQL Object type can have zero or more  arguments , for example, the  length  field below:

All arguments are named. Unlike languages such as JavaScript and Python where functions take a list of ordered arguments, all arguments in GraphQL are passed by name specifically. In this case, the  length  field has one defined argument called  unit .

Arguments can be either required or optional. When an argument is optional, we can define a  default value . If the  unit  argument is not passed, then it will be set to  METER  by default.

Every GraphQL schema must support  query  operations. The  entry point  for this  root operation type  is a regular Object type called  Query  by default. So if you see a query that looks like this:

That means that the GraphQL service needs to have a  Query  type with a  droid  field:

Schemas may also support  mutation  and  subscription  operations by adding additional  Mutation  and  Subscription  types and then defining fields on the corresponding root operation types.

It’s important to remember that other than the special status of being entry points into the schema, the  Query ,  Mutation , and  Subscription  types are the same as any other GraphQL Object type, and their fields work exactly the same way.

You can name your root operation types differently too; if you choose to do so then you will need to inform GraphQL of the new names using the  schema  keyword:

A GraphQL Object type has a name and fields, but at some point, those fields must resolve to some concrete data. That’s where the  Scalar types  come in: they represent the leaf values of the query.

In the following query, the  name  and  appearsIn  fields will resolve to Scalar types:

We know this because those fields don’t have any sub-fields—they are the leaves of the query.

GraphQL comes with a set of  default Scalar types  out of the box:

In most GraphQL service implementations, there is also a way to specify custom Scalar types. For example, we could define a  Date  type:

Then it’s up to our implementation to define how that type should be serialized, deserialized, and validated. For example, you could specify that the  Date  type should always be serialized into an integer timestamp, and your client should know to expect that format for any date fields.

Enum types , also known as enumeration types, are a special kind of scalar that is restricted to a particular set of allowed values. This allows you to:

Here’s what an Enum type definition looks like in SDL:

This means that wherever we use the type  Episode  in our schema, we expect it to be exactly one of  NEWHOPE ,  EMPIRE , or  JEDI .

Types are assumed to be nullable and singular by default in GraphQL. However, when you use these named types in a schema (or in  query variable declarations ) you can apply additional  type modifiers  that will affect the meaning of those values.

As we saw with the Object type example above, GraphQL supports two type modifiers—the  List  and  Non-Null  types—and they can be used individually or in combination with each other.

Let’s look at an example:

Here, we’re using a  String  type and marking it as a Non-Null type by adding an exclamation mark ( ! ) after the type name. This means that our server always expects to return a non-null value for this field, and if the resolver produces a null value, then that will trigger a GraphQL execution error, letting the client know that something has gone wrong.

As we saw in an example above, the Non-Null type modifier can also be used when defining arguments for a field, which will cause the GraphQL server to return a validation error if a null value is passed as that argument:

Lists work in a similar way. We can use a type modifier to mark a type as a List type, which indicates that this field will return an array of that type. In SDL, this is denoted by wrapping the type in square brackets,  [  and  ] . It works the same for arguments, where the validation step will expect an array for that value. Here’s an example:

As we see above, the Non-Null and List modifiers can be combined. For example, you can have a List of Non-Null  String  types:

This means that the  list itself  can be null, but it can’t have any null members. For example, in JSON:

Now, let’s say we defined a Non-Null List of  String  types:

This means that the list itself cannot be null, but it can contain null values:

Lastly, you can also have a Non-Null List of Non-Null  String  types:

This means that the list cannot be null and it cannot contain null values:

You can arbitrarily nest any number of Non-Null and List modifiers, according to your needs.

Like many type systems, GraphQL supports  abstract types . The first of these types that we’ll explore is an  Interface type , which defines a certain set of fields that a concrete Object type or other Interface type must also include to implement it.

For example, you could have a  Character  Interface type that represents any character in the Star Wars trilogy:

This means that any type that  implements   Character  needs to have these exact fields as well as the same arguments and return types.

For example, here are some types that might implement  Character :

You can see that both of these types have all of the fields from the  Character  Interface type, but also bring in extra fields— totalCredits ,  starships  and  primaryFunction —that are specific to that particular type of character.

Interface types are useful when you want to return an object or set of objects, but those might be of several different types. For example, note that the following query produces an error:

The  hero  field returns the type  Character , which means it might be either a  Human  or a  Droid  depending on the  episode  argument. In the query above, you can only ask for fields that exist on the  Character  Interface type, which doesn’t include  primaryFunction .

To ask for a field on a specific Object type, you need to use an  inline fragment :

Interface types may also implement other Interface types:

Note that Interface types may not implement themselves or contain any cyclic references to each other.

GraphQL supports a second abstract type called a  Union type . Union types share similarities with Interface types, but they cannot define any shared fields among the constituent types.

A Union type is defined by indicating its member Object types:

Wherever we return a  SearchResult  type in our schema, we might get a  Human , a  Droid , or a  Starship . Note that members of a Union type need to be concrete Object types; you can’t define one using Interface types or other Union types as members.

In this case, if you query a field that returns the  SearchResult  Union type, you need to use an  inline fragment  to be able to query any fields that are defined on the member Object types:

The  __typename  field is a special  meta-field  that automatically exists on every Object type and resolves to the name of that type, providing a way to differentiate between data types on the client.

Also, in this case, since  Human  and  Droid  share a common interface ( Character ), you can query their common fields in one place and still get the same result:

Note that  name  is still specified on  Starship  because otherwise it wouldn’t show up in the results given that  Starship  is not a  Character !

Most of the examples we’ve covered on this page demonstrate how Object, Scalar, Enum, Interface, and Union types may be used as  output types  for the fields in a schema. But we have also seen that field arguments must specify their  input types .

So far, we’ve only talked about using scalar values (like Enums or String types) as an input type for a field argument. However, you can also pass complex objects as arguments using an  Input Object type , which is the last kind of named types in GraphQL that we’ll explore.

This is particularly valuable in the case of  mutations , where you might want to pass in a whole object to be created. In SDL, Input Object types look similar to regular Object types, but with the keyword  input  instead of  type :

Here is how you could use the Input Object type in a mutation:

The fields on an Input Object type can refer to other Input Object types, but you can’t mix input and output types in your schema. Input Object types also can’t have arguments on their fields.

In certain instances where field arguments are insufficient or certain common behaviors must be replicated in multiple locations,  directives  allow us to modify parts of a GraphQL schema or operation by using an  @  character followed by the directive’s name.

Type system directives  allow us to annotate the types, fields, and arguments in a schema so that they may be validated or executed differently.

Directives may also be defined for use in GraphQL operations as  executable directives .  Read more about executable directives on the Queries page.

The GraphQL specification defines several  built-in directives . For example, for implementations that support SDL, the  @deprecated  directive will be available to annotate deprecated parts of the schema:

While you won’t need to define the  @deprecated  directive in your schema explicitly if you’re using a GraphQL implementation that supports SDL, it’s underlying definition would look like this:

Note that, just like fields, directives can accept arguments and those arguments can have default values. The  @deprecated  directive has a nullable  reason  argument that accepts a  String  as an input type and falls back to  "No longer supported" . And with directives, we must specify where they may be used, such as on a  FIELD_DEFINITION  or  ENUM_VALUE  for the  @deprecated  directive.

In addition to GraphQL’s built-in directives, you may define your own  custom directives . As with custom Scalar types, it’s up to your chosen GraphQL implementation to determine how to handle custom directives during query execution.

GraphQL allows you to add documentation to the types, fields, and arguments in a schema. In fact, the GraphQL specification encourages you to do this in all cases unless the name of the type, field, or argument is self-descriptive. Schema descriptions are defined using Markdown syntax and can be multiline or single line.

In SDL, they would look like this:

In addition to making a GraphQL API schema more expressive, descriptions are helpful to client developers because they are available in  introspection queries  and visible in developer tools such as  GraphiQL .

Occasionally, you may need to add comments in a schema that do not describe types, fields, or arguments, and are not meant to be seen by clients. In those cases, you can add a single-line comment to SDL by preceding the text with a  #  character:

Comments may also be added to client queries:

To recap what we’ve learned about schemas and types:

Now that you understand the key features of the type system, you’re ready to learn more about how to  query data from a GraphQL API .

## Code Examples

### Example 1

```
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

### Example 2

```
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

### Example 3

```
type Starship {
  id: ID!
  name: String!
  length(unit: LengthUnit = METER): Float
}
```

### Example 4

```
type Starship {
  id: ID!
  name: String!
  length(unit: LengthUnit = METER): Float
}
```

### Example 5

```
type Query {
  droid(id: ID!): Droid
}
```

### Example 6

```
type Query {
  droid(id: ID!): Droid
}
```

### Example 7

```
schema {
  query: MyQueryType
  mutation: MyMutationType
  subscription: MySubscriptionType
}
```

### Example 8

```
schema {
  query: MyQueryType
  mutation: MyMutationType
  subscription: MySubscriptionType
}
```

### Example 9

```
scalar Date
```

### Example 10

```
scalar Date
```

### Example 11

```
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
```

### Example 12

```
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
```

### Example 13

```
type Character {
  name: String!
}
```

### Example 14

```
type Character {
  name: String!
}
```

### Example 15

```
type Character {
  name: String!
  appearsIn: [Episode]!
}
```

### Example 16

```
type Character {
  name: String!
  appearsIn: [Episode]!
}
```

### Example 17

```
myField: [String!]
```

### Example 18

```
myField: [String!]
```

### Example 19

```
myField: null // valid
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // error
```

### Example 20

```
myField: null // valid
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // error
```

### Example 21

```
myField: [String]!
```

### Example 22

```
myField: [String]!
```

### Example 23

```
myField: null // error
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // valid
```

### Example 24

```
myField: null // error
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // valid
```

### Example 25

```
myField: [String!]!
```

### Example 26

```
myField: [String!]!
```

### Example 27

```
myField: null // error
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // error
```

### Example 28

```
myField: null // error
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // error
```

### Example 29

```
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}
```

### Example 30

```
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}
```

### Example 31

```
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
```

### Example 32

```
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
```

### Example 33

```
interface Node {
  id: ID!
}
 
interface Character implements Node {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}
```

### Example 34

```
interface Node {
  id: ID!
}
 
interface Character implements Node {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}
```

### Example 35

```
union SearchResult = Human | Droid | Starship
```

### Example 36

```
union SearchResult = Human | Droid | Starship
```

### Example 37

```
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 38

```
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 39

```
type User {
  fullName: String
  name: String @deprecated(reason: "Use `fullName`.")
}
```

### Example 40

```
type User {
  fullName: String
  name: String @deprecated(reason: "Use `fullName`.")
}
```

### Example 41

```
directive @deprecated(
  reason: String = "No longer supported"
) on FIELD_DEFINITION | ENUM_VALUE
```

### Example 42

```
directive @deprecated(
  reason: String = "No longer supported"
) on FIELD_DEFINITION | ENUM_VALUE
```

### Example 43

```
"""
A character from the Star Wars universe
"""
type Character {
  "The name of the character."
  name: String!
}
 
"""
The episodes in the Star Wars trilogy
"""
enum Episode {
  "Star Wars Episode IV: A New Hope, released in 1977." 
  NEWHOPE
  "Star Wars Episode V: The Empire Strikes Back, released in 1980."
  EMPIRE
  "Star Wars Episode VI: Return of the Jedi, released in 1983."
  JEDI
}
 
"""
The query type, represents all of the entry points into our object graph
"""
type Query {
  """
  Fetches the hero of a specified Star Wars film.
  """
  hero(
    "The name of the film that the hero appears in."
    episode: Episode
  ): Character
}
```

### Example 44

```
"""
A character from the Star Wars universe
"""
type Character {
  "The name of the character."
  name: String!
}
 
"""
The episodes in the Star Wars trilogy
"""
enum Episode {
  "Star Wars Episode IV: A New Hope, released in 1977." 
  NEWHOPE
  "Star Wars Episode V: The Empire Strikes Back, released in 1980."
  EMPIRE
  "Star Wars Episode VI: Return of the Jedi, released in 1983."
  JEDI
}
 
"""
The query type, represents all of the entry points into our object graph
"""
type Query {
  """
  Fetches the hero of a specified Star Wars film.
  """
  hero(
    "The name of the film that the hero appears in."
    episode: Episode
  ): Character
}
```

### Example 45

```
# This line is treated like whitespace and ignored by GraphQL
type Character {
  name: String!
}
```

### Example 46

```
# This line is treated like whitespace and ignored by GraphQL
type Character {
  name: String!
}
```

### Example 47

```
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

### Example 48

```
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

### Example 49

```
type Starship {
  id: ID!
  name: String!
  length(unit: LengthUnit = METER): Float
}
```

### Example 50

```
type Starship {
  id: ID!
  name: String!
  length(unit: LengthUnit = METER): Float
}
```

### Example 51

```
type Query {
  droid(id: ID!): Droid
}
```

### Example 52

```
type Query {
  droid(id: ID!): Droid
}
```

### Example 53

```
schema {
  query: MyQueryType
  mutation: MyMutationType
  subscription: MySubscriptionType
}
```

### Example 54

```
schema {
  query: MyQueryType
  mutation: MyMutationType
  subscription: MySubscriptionType
}
```

### Example 55

```
scalar Date
```

### Example 56

```
scalar Date
```

### Example 57

```
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
```

### Example 58

```
enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}
```

### Example 59

```
type Character {
  name: String!
}
```

### Example 60

```
type Character {
  name: String!
}
```

### Example 61

```
type Character {
  name: String!
  appearsIn: [Episode]!
}
```

### Example 62

```
type Character {
  name: String!
  appearsIn: [Episode]!
}
```

### Example 63

```
myField: [String!]
```

### Example 64

```
myField: [String!]
```

### Example 65

```
myField: null // valid
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // error
```

### Example 66

```
myField: null // valid
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // error
```

### Example 67

```
myField: [String]!
```

### Example 68

```
myField: [String]!
```

### Example 69

```
myField: null // error
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // valid
```

### Example 70

```
myField: null // error
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // valid
```

### Example 71

```
myField: [String!]!
```

### Example 72

```
myField: [String!]!
```

### Example 73

```
myField: null // error
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // error
```

### Example 74

```
myField: null // error
myField: [] // valid
myField: ["a", "b"] // valid
myField: ["a", null, "b"] // error
```

### Example 75

```
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}
```

### Example 76

```
interface Character {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}
```

### Example 77

```
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
```

### Example 78

```
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
```

### Example 79

```
interface Node {
  id: ID!
}
 
interface Character implements Node {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}
```

### Example 80

```
interface Node {
  id: ID!
}
 
interface Character implements Node {
  id: ID!
  name: String!
  friends: [Character]
  appearsIn: [Episode]!
}
```

### Example 81

```
union SearchResult = Human | Droid | Starship
```

### Example 82

```
union SearchResult = Human | Droid | Starship
```

### Example 83

```
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 84

```
input ReviewInput {
  stars: Int!
  commentary: String
}
 
type Mutation {
  createReview(episode: Episode, review: ReviewInput!): Review
}
```

### Example 85

```
type User {
  fullName: String
  name: String @deprecated(reason: "Use `fullName`.")
}
```

### Example 86

```
type User {
  fullName: String
  name: String @deprecated(reason: "Use `fullName`.")
}
```

### Example 87

```
directive @deprecated(
  reason: String = "No longer supported"
) on FIELD_DEFINITION | ENUM_VALUE
```

### Example 88

```
directive @deprecated(
  reason: String = "No longer supported"
) on FIELD_DEFINITION | ENUM_VALUE
```

### Example 89

```
"""
A character from the Star Wars universe
"""
type Character {
  "The name of the character."
  name: String!
}
 
"""
The episodes in the Star Wars trilogy
"""
enum Episode {
  "Star Wars Episode IV: A New Hope, released in 1977." 
  NEWHOPE
  "Star Wars Episode V: The Empire Strikes Back, released in 1980."
  EMPIRE
  "Star Wars Episode VI: Return of the Jedi, released in 1983."
  JEDI
}
 
"""
The query type, represents all of the entry points into our object graph
"""
type Query {
  """
  Fetches the hero of a specified Star Wars film.
  """
  hero(
    "The name of the film that the hero appears in."
    episode: Episode
  ): Character
}
```

### Example 90

```
"""
A character from the Star Wars universe
"""
type Character {
  "The name of the character."
  name: String!
}
 
"""
The episodes in the Star Wars trilogy
"""
enum Episode {
  "Star Wars Episode IV: A New Hope, released in 1977." 
  NEWHOPE
  "Star Wars Episode V: The Empire Strikes Back, released in 1980."
  EMPIRE
  "Star Wars Episode VI: Return of the Jedi, released in 1983."
  JEDI
}
 
"""
The query type, represents all of the entry points into our object graph
"""
type Query {
  """
  Fetches the hero of a specified Star Wars film.
  """
  hero(
    "The name of the film that the hero appears in."
    episode: Episode
  ): Character
}
```

### Example 91

```
# This line is treated like whitespace and ignored by GraphQL
type Character {
  name: String!
}
```

### Example 92

```
# This line is treated like whitespace and ignored by GraphQL
type Character {
  name: String!
}
```


---

# Caching

**Source:** https://graphql.org/learn/caching/

## Table of Contents

- Globally unique IDs
  - Standardize how objects are identified in a schema
  - Compatibility with existing APIs
  - Alternatives
- Recap
- Caching
- Globally unique IDs
  - Standardize how objects are identified in a schema
  - Compatibility with existing APIs
  - Alternatives
- Recap

## Content

Provide Object Identifiers so clients can build rich caches

In an endpoint-based API, clients can use  HTTP caching  to avoid refetching resources and to identify when two resources are the same. The URL in these APIs is a  globally unique identifier  that the client can leverage to build a cache.

In GraphQL, there’s no URL-like primitive that provides this globally unique identifier for a given object. Hence, it’s a best practice for the API to expose such an identifier for clients to use as a prerequisite for certain types of caching.

One possible pattern for this is reserving a field, like  id , to be a globally unique identifier. The example schema used throughout these docs uses this approach:

This is a powerful tool for client developers. In the same way that the URLs of a resource-based API provide a globally unique key, the  id  field in this system provides a globally unique key.

If the backend uses something like UUIDs for identifiers, then exposing this globally unique ID may be very straightforward! If the backend doesn’t have a globally unique ID for every object already, the GraphQL layer might have to construct one. Oftentimes, that’s as simple as appending the name of the type to the ID and using that as the identifier. The server might then make that ID opaque by base64-encoding it.

Optionally, this ID can then be used to work with the  node  pattern when using  global object identification .

One concern with using the  id  field for this purpose is how a client using the GraphQL API would work with existing APIs. For example, if our existing API accepted a type-specific ID, but our GraphQL API uses globally unique IDs, then using both at once can be tricky.

In these cases, the GraphQL API can expose the previous API’s IDs in a separate field. This gives us the best of both worlds:

While globally unique IDs have proven to be a powerful pattern in the past, they are not the only pattern that can be used, nor are they right for every situation. The critical functionality that the client needs is the ability to derive a globally unique identifier for their caching. While having the server derive that ID simplifies the client, the client can also derive the identifier. This could be as simple as combining the type of the object (queried with  __typename ) with some type-unique identifier.

Additionally, if replacing an existing API with a GraphQL API, then it may be confusing if all of the fields in GraphQL are the same  except   id , which changed to be globally unique. This would be another reason why one might choose not to use  id  as the globally unique field.

To recap these recommendations for using caching with GraphQL APIs:

Provide Object Identifiers so clients can build rich caches

In an endpoint-based API, clients can use  HTTP caching  to avoid refetching resources and to identify when two resources are the same. The URL in these APIs is a  globally unique identifier  that the client can leverage to build a cache.

In GraphQL, there’s no URL-like primitive that provides this globally unique identifier for a given object. Hence, it’s a best practice for the API to expose such an identifier for clients to use as a prerequisite for certain types of caching.

One possible pattern for this is reserving a field, like  id , to be a globally unique identifier. The example schema used throughout these docs uses this approach:

This is a powerful tool for client developers. In the same way that the URLs of a resource-based API provide a globally unique key, the  id  field in this system provides a globally unique key.

If the backend uses something like UUIDs for identifiers, then exposing this globally unique ID may be very straightforward! If the backend doesn’t have a globally unique ID for every object already, the GraphQL layer might have to construct one. Oftentimes, that’s as simple as appending the name of the type to the ID and using that as the identifier. The server might then make that ID opaque by base64-encoding it.

Optionally, this ID can then be used to work with the  node  pattern when using  global object identification .

One concern with using the  id  field for this purpose is how a client using the GraphQL API would work with existing APIs. For example, if our existing API accepted a type-specific ID, but our GraphQL API uses globally unique IDs, then using both at once can be tricky.

In these cases, the GraphQL API can expose the previous API’s IDs in a separate field. This gives us the best of both worlds:

While globally unique IDs have proven to be a powerful pattern in the past, they are not the only pattern that can be used, nor are they right for every situation. The critical functionality that the client needs is the ability to derive a globally unique identifier for their caching. While having the server derive that ID simplifies the client, the client can also derive the identifier. This could be as simple as combining the type of the object (queried with  __typename ) with some type-unique identifier.

Additionally, if replacing an existing API with a GraphQL API, then it may be confusing if all of the fields in GraphQL are the same  except   id , which changed to be globally unique. This would be another reason why one might choose not to use  id  as the globally unique field.

To recap these recommendations for using caching with GraphQL APIs:

