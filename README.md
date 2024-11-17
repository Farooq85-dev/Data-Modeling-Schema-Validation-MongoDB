# Data-Modeling-Schema-Validation-MongoDB

### What is Data Modeling in MongoDb?

#### Data modeling in MongoDB is the process of defining the structure, storage, and organization of data in a database. Unlike traditional relational databases, MongoDB is schema-less, which means it can store documents of varying structures in the same collection. However, it is essential to design a schema that fits your application needs for consistency, efficiency, and performance.

### Key Concepts:

- Documents: Basic units of data in MongoDB stored in BSON format (similar to JSON).
- Collections: Groupings of documents (akin to tables in relational databases).
- Embedded Data: Storing related data within a single document.
- Referenced Data: Storing related data in separate documents with references (similar to foreign keys).
- Denormalization vs. Normalization: Trade-offs between embedding (denormalization) and referencing (normalization).

### Schema Design Strategies:

1. Embedding:-

For example:-

```
{
  _id: ObjectId("64a12345c789ef0123456789"),
  title: "Understanding MongoDB Data Modeling",
  content: "Data modeling is crucial...",
  comments: [
    { user: "John", comment: "Great article!", date: new Date() },
    { user: "Jane", comment: "Very helpful, thanks!", date: new Date() }
  ]
}
```

2. Referencing:-

```
// User document
{
  _id: ObjectId("64a12345c789ef0123450000"),
  username: "JohnDoe",
  email: "john.doe@example.com"
}

// Blog post document
{
  _id: ObjectId("64a12345c789ef0123456789"),
  title: "Understanding MongoDB Data Modeling",
  content: "Data modeling is crucial...",
  authorId: ObjectId("64a12345c789ef0123450000")
}
```

### Schema Validation in MongoDB

#### Schema validation ensures that documents inserted or updated in a collection adhere to a specific structure. This helps maintain data consistency, even in a flexible schema database like MongoDB. MongoDB uses JSON Schema to enforce rules. This can be set during collection creation or updated.

Example Schema Validation:

```
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["username", "email"],
      properties: {
        username: {
          bsonType: "string",
          description: "must be a string and is required"
        },
        email: {
          bsonType: "string",
          pattern: "^.+@.+\\..+$",
          description: "must be a valid email and is required"
        },
        age: {
          bsonType: "int",
          minimum: 18,
          description: "must be an integer and at least 18"
        }
      }
    }
  }
});
```
