
# Library Management System with GraphQL

This project is a simple library management system implemented using GraphQL with Apollo Server. It allows you to manage authors and books, including adding new books, filtering books by author and genre, and updating author details.

## Objectives

The main objectives were:

1. **8.1: Number of Books and Authors** - Implement queries to return the number of books and authors.
2. **8.2: All Books** - Implement a query to return the details of all books.
3. **8.3: All Authors** - Implement a query to return the details of all authors, including the number of books they've written.
4. **8.4: Books of an Author** - Modify the query to filter books by a specific author.
5. **8.5: Books by Genre** - Modify the query to filter books by genre, and optionally by both author and genre.
6. **8.6: Adding a Book** - Implement a mutation to add a new book. If the author does not exist, they should be created.
7. **8.7: Updating the Birth Year of an Author** - Implement a mutation to update the birth year of an existing author.

## How Each Objective Was Achieved

### 1. **8.1: Number of Books and Authors**

We implemented the following queries in the GraphQL schema:

- `bookCount`: Returns the total number of books by counting the length of the `books` array.
- `authorCount`: Returns the total number of authors by counting the length of the `authors` array.

```graphql
type Query {
  bookCount: Int
  authorCount: Int
}
```

### 2. **8.2: All Books**

We created a `Book` type and implemented the `allBooks` query, which returns the details of all books.

```graphql
type Book {
  title: String!
  published: Int!
  author: String!
  genres: [String!]!
}

type Query {
  allBooks: [Book!]!
}
```

### 3. **8.3: All Authors**

We created an `Author` type and extended the `allAuthors` query to return the details of all authors, including a computed `bookCount` field.

```graphql
type Author {
  name: String!
  born: Int
  bookCount: Int
}

type Query {
  allAuthors: [Author!]!
}
```

The `bookCount` field is computed by filtering the `books` array for each author and counting the number of books they've written.

### 4. **8.4: Books of an Author**

The `allBooks` query was modified to accept an optional `author` argument. If provided, the query filters the books to include only those written by the specified author.

```graphql
type Query {
  allBooks(author: String): [Book!]!
}
```

### 5. **8.5: Books by Genre**

The `allBooks` query was further modified to accept an optional `genre` argument. The query now filters books by genre, and if both `author` and `genre` are provided, it filters by both.

```graphql
type Query {
  allBooks(author: String, genre: String): [Book!]!
}
```

### 6. **8.6: Adding a Book**

We implemented the `addBook` mutation, which allows users to add a new book. If the author of the book does not exist in the system, they are automatically added.

```graphql
type Mutation {
  addBook(
    title: String!
    author: String!
    published: Int!
    genres: [String!]!
  ): Book
}
```

The mutation checks if the author exists, and if not, creates a new author before adding the book to the `books` array.

### 7. **8.7: Updating the Birth Year of an Author**

The `editAuthor` mutation was implemented to update the birth year of an existing author.

```graphql
type Mutation {
  editAuthor(
    name: String!
    setBornTo: Int!
  ): Author
}
```

If the author exists, their `born` field is updated. If the author does not exist, `null` is returned.
