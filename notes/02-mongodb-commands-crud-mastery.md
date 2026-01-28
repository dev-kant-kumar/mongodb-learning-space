# MongoDB Commands & CRUD Mastery 💻

> **Purpose:** Master all MongoDB commands, CRUD operations, and edge cases through practical examples.

---

## Table of Contents
1. [MongoDB Shell Basics](#mongodb-shell-basics)
2. [Database Operations](#database-operations)
3. [Collection Operations](#collection-operations)
4. [CREATE Operations](#create-operations)
5. [READ Operations](#read-operations)
6. [UPDATE Operations](#update-operations)
7. [DELETE Operations](#delete-operations)
8. [Query Operators](#query-operators)
9. [Update Operators](#update-operators)
10. [Cursor Methods](#cursor-methods)
11. [Aggregation Basics](#aggregation-basics)
12. [Practical Examples](#practical-examples)

---

## MongoDB Shell Basics

### Starting MongoDB Shell

```bash
# Start mongo shell (legacy)
mongo

# Start mongosh (modern shell)
mongosh

# Connect to specific host and port
mongosh "mongodb://localhost:27017"

# Connect with authentication
mongosh "mongodb://username:password@localhost:27017"

# Connect to Atlas
mongosh "mongodb+srv://cluster0.xxxxx.mongodb.net/myDatabase" --username myUser
```

---

### Shell Help Commands

```javascript
// Get help
help

// Database help
db.help()

// Collection help
db.collectionName.help()

// Show all databases
show dbs

// Show all collections in current database
show collections

// Show current database
db

// Get MongoDB version
db.version()

// Get server status
db.serverStatus()
```

---

## Database Operations

### Create / Switch Database

```javascript
// Switch to database (creates if doesn't exist)
use myDatabase

// Check current database
db

// Note: Database is created only when you insert data
use testDB        // Database not created yet
db.users.insertOne({ name: "John" })  // Now database is created
```

---

### Show Databases

```javascript
// Show all databases
show dbs
// or
show databases

// Note: Empty databases don't appear in the list
```

---

### Drop Database

```javascript
// Switch to database first
use myDatabase

// Drop current database
db.dropDatabase()

// Result
{ "ok" : 1 }
```

---

### Database Stats

```javascript
// Get database statistics
db.stats()

// Output example
{
  "db" : "myDatabase",
  "collections" : 5,
  "views" : 0,
  "objects" : 1250,
  "avgObjSize" : 1024,
  "dataSize" : 1280000,
  "storageSize" : 2048000,
  "indexes" : 8,
  "indexSize" : 819200,
  "totalSize" : 2867200,
  "ok" : 1
}
```

---

## Collection Operations

### Create Collection

```javascript
// Implicit creation (happens on first insert)
db.users.insertOne({ name: "John" })

// Explicit creation
db.createCollection("products")

// Create with options
db.createCollection("orders", {
  capped: true,
  size: 5242880,  // 5MB
  max: 5000       // Maximum 5000 documents
})

// Create with validation
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "email"],
      properties: {
        name: {
          bsonType: "string",
          description: "must be a string and is required"
        },
        email: {
          bsonType: "string",
          pattern: "^.+@.+$",
          description: "must be a valid email"
        },
        age: {
          bsonType: "int",
          minimum: 0,
          maximum: 150,
          description: "must be an integer between 0 and 150"
        }
      }
    }
  }
})
```

---

### Show Collections

```javascript
// Show all collections
show collections
// or
show tables

// Get collection names programmatically
db.getCollectionNames()

// Output
[ "users", "products", "orders" ]
```

---

### Rename Collection

```javascript
// Rename collection
db.oldName.renameCollection("newName")

// Rename with drop target
db.oldName.renameCollection("newName", true)
```

---

### Drop Collection

```javascript
// Drop collection
db.users.drop()

// Result
true  // if successful
false // if collection doesn't exist
```

---

### Collection Stats

```javascript
// Get collection statistics
db.users.stats()

// Output example
{
  "ns" : "myDatabase.users",
  "size" : 204800,
  "count" : 1000,
  "avgObjSize" : 204,
  "storageSize" : 262144,
  "nindexes" : 2,
  "totalIndexSize" : 81920,
  "ok" : 1
}
```

---

## CREATE Operations

### insertOne()
Insert a single document.

```javascript
// Basic insert
db.users.insertOne({
  name: "John Doe",
  email: "john@example.com",
  age: 30
})

// Result
{
  "acknowledged" : true,
  "insertedId" : ObjectId("507f1f77bcf86cd799439011")
}
```

```javascript
// Insert with custom _id
db.users.insertOne({
  _id: 1,
  name: "Jane Smith",
  email: "jane@example.com"
})

// Insert with nested document
db.users.insertOne({
  name: "Bob Wilson",
  email: "bob@example.com",
  address: {
    street: "123 Main St",
    city: "New York",
    state: "NY",
    zipcode: "10001"
  }
})

// Insert with array
db.users.insertOne({
  name: "Alice Brown",
  email: "alice@example.com",
  hobbies: ["reading", "coding", "hiking"],
  scores: [85, 92, 78, 95]
})

// Insert with date
db.users.insertOne({
  name: "Charlie Davis",
  email: "charlie@example.com",
  createdAt: new Date(),
  birthDate: ISODate("1990-05-15T00:00:00Z")
})
```

---

### insertMany()
Insert multiple documents at once.

```javascript
// Basic insert many
db.users.insertMany([
  { name: "User 1", email: "user1@example.com", age: 25 },
  { name: "User 2", email: "user2@example.com", age: 30 },
  { name: "User 3", email: "user3@example.com", age: 35 }
])

// Result
{
  "acknowledged" : true,
  "insertedIds" : [
    ObjectId("507f1f77bcf86cd799439011"),
    ObjectId("507f1f77bcf86cd799439012"),
    ObjectId("507f1f77bcf86cd799439013")
  ]
}
```

```javascript
// Insert with ordered: false (continues on error)
db.users.insertMany([
  { _id: 1, name: "User 1" },
  { _id: 2, name: "User 2" },
  { _id: 1, name: "Duplicate" },  // Will fail
  { _id: 3, name: "User 3" }      // Will still insert
], { ordered: false })

// Insert many with different structures
db.products.insertMany([
  {
    name: "Laptop",
    category: "Electronics",
    price: 999.99,
    specs: {
      cpu: "Intel i7",
      ram: "16GB",
      storage: "512GB SSD"
    }
  },
  {
    name: "Book",
    category: "Literature",
    price: 19.99,
    author: "John Doe",
    pages: 350
  },
  {
    name: "Headphones",
    category: "Electronics",
    price: 149.99,
    wireless: true,
    colors: ["black", "white", "blue"]
  }
])
```

---

### insert() (Deprecated)
Legacy method, still works but use `insertOne()` or `insertMany()` instead.

```javascript
// Single document
db.users.insert({ name: "John", email: "john@example.com" })

// Multiple documents
db.users.insert([
  { name: "User 1" },
  { name: "User 2" }
])
```

---

### Bulk Operations

```javascript
// Initialize bulk operation
const bulk = db.users.initializeUnorderedBulkOp()

// Add operations
bulk.insert({ name: "User 1", age: 25 })
bulk.insert({ name: "User 2", age: 30 })
bulk.insert({ name: "User 3", age: 35 })

// Execute
bulk.execute()

// Ordered bulk (stops on first error)
const orderedBulk = db.users.initializeOrderedBulkOp()
```

---

## READ Operations

### find()
Find multiple documents.

```javascript
// Find all documents
db.users.find()

// Find with filter (equality)
db.users.find({ age: 30 })

// Find with multiple conditions (AND)
db.users.find({ age: 30, city: "New York" })

// Find with projection (select specific fields)
db.users.find(
  { age: 30 },
  { name: 1, email: 1, _id: 0 }
)
// Returns only name and email, excludes _id

// Pretty print
db.users.find().pretty()
```

---

### findOne()
Find single document (first match).

```javascript
// Find one document
db.users.findOne({ email: "john@example.com" })

// Find one with projection
db.users.findOne(
  { age: 30 },
  { name: 1, email: 1 }
)

// Find by _id
db.users.findOne({ _id: ObjectId("507f1f77bcf86cd799439011") })

// Returns null if not found
db.users.findOne({ name: "NonExistent" })
// Result: null
```

---

### Query Filters

```javascript
// Exact match
db.users.find({ age: 30 })

// Greater than
db.users.find({ age: { $gt: 25 } })

// Greater than or equal
db.users.find({ age: { $gte: 25 } })

// Less than
db.users.find({ age: { $lt: 40 } })

// Less than or equal
db.users.find({ age: { $lte: 40 } })

// Not equal
db.users.find({ status: { $ne: "inactive" } })

// In array
db.users.find({ age: { $in: [25, 30, 35] } })

// Not in array
db.users.find({ age: { $nin: [25, 30] } })

// Range query (AND)
db.users.find({ age: { $gte: 25, $lte: 35 } })
```

---

### Logical Operators

```javascript
// AND (implicit)
db.users.find({ age: 30, city: "New York" })

// AND (explicit)
db.users.find({
  $and: [
    { age: { $gte: 25 } },
    { age: { $lte: 35 } }
  ]
})

// OR
db.users.find({
  $or: [
    { age: { $lt: 25 } },
    { age: { $gt: 35 } }
  ]
})

// NOT
db.users.find({
  age: { $not: { $gt: 30 } }
})

// NOR (not any condition is true)
db.users.find({
  $nor: [
    { age: { $lt: 25 } },
    { status: "inactive" }
  ]
})

// Complex combinations
db.users.find({
  $and: [
    {
      $or: [
        { age: { $lt: 25 } },
        { age: { $gt: 35 } }
      ]
    },
    { status: "active" }
  ]
})
```

---

### Existence and Type Queries

```javascript
// Field exists
db.users.find({ email: { $exists: true } })

// Field doesn't exist
db.users.find({ phone: { $exists: false } })

// Type check
db.users.find({ age: { $type: "int" } })
db.users.find({ age: { $type: "number" } })
db.users.find({ name: { $type: "string" } })

// Multiple types
db.users.find({ age: { $type: ["int", "double"] } })
```

---

### Array Queries

```javascript
// Exact array match
db.users.find({ hobbies: ["reading", "coding"] })

// Contains element
db.users.find({ hobbies: "reading" })

// Contains all elements
db.users.find({ hobbies: { $all: ["reading", "coding"] } })

// Array size
db.users.find({ hobbies: { $size: 3 } })

// Element match (complex)
db.users.find({
  scores: {
    $elemMatch: { $gte: 80, $lt: 90 }
  }
})

// Array index access
db.users.find({ "scores.0": 85 })
```

---

### Nested Document Queries

```javascript
// Dot notation
db.users.find({ "address.city": "New York" })

// Multiple nested fields
db.users.find({
  "address.city": "New York",
  "address.state": "NY"
})

// Exact nested document match
db.users.find({
  address: {
    street: "123 Main St",
    city: "New York",
    state: "NY"
  }
})
// Note: Must match exactly, including field order
```

---

### Regular Expression Queries

```javascript
// Contains pattern
db.users.find({ name: /john/i })  // case-insensitive

// Starts with
db.users.find({ name: /^John/ })

// Ends with
db.users.find({ email: /\.com$/ })

// Using $regex operator
db.users.find({
  email: {
    $regex: "gmail",
    $options: "i"  // case-insensitive
  }
})

// Complex patterns
db.users.find({
  phone: { $regex: /^\d{3}-\d{3}-\d{4}$/ }
})
```

---

### Projection

```javascript
// Include specific fields
db.users.find(
  {},
  { name: 1, email: 1 }
)
// Returns: _id, name, email (id included by default)

// Exclude _id
db.users.find(
  {},
  { name: 1, email: 1, _id: 0 }
)

// Exclude specific fields
db.users.find(
  {},
  { password: 0, ssn: 0 }
)

// Array slicing
db.users.find(
  {},
  { name: 1, hobbies: { $slice: 2 } }  // First 2 elements
)

db.users.find(
  {},
  { name: 1, hobbies: { $slice: -2 } }  // Last 2 elements
)

db.users.find(
  {},
  { name: 1, hobbies: { $slice: [1, 3] } }  // Skip 1, limit 3
)

// Element match in projection
db.users.find(
  { "scores": { $gte: 90 } },
  { name: 1, scores: { $elemMatch: { $gte: 90 } } }
)
```

---

### Cursor Methods

```javascript
// Limit results
db.users.find().limit(5)

// Skip documents
db.users.find().skip(10)

// Sort (ascending)
db.users.find().sort({ age: 1 })

// Sort (descending)
db.users.find().sort({ age: -1 })

// Multiple sort fields
db.users.find().sort({ age: -1, name: 1 })

// Count documents
db.users.find({ age: { $gte: 25 } }).count()

// Chain methods
db.users.find({ status: "active" })
  .sort({ age: -1 })
  .skip(10)
  .limit(5)

// Pagination pattern
const page = 2
const pageSize = 10
db.users.find()
  .sort({ createdAt: -1 })
  .skip((page - 1) * pageSize)
  .limit(pageSize)
```

---

### Count Operations

```javascript
// Count all documents
db.users.countDocuments()

// Count with filter
db.users.countDocuments({ age: { $gte: 25 } })

// Estimated count (faster but approximate)
db.users.estimatedDocumentCount()

// Legacy count (deprecated)
db.users.count({ age: 30 })
```

---

### Distinct

```javascript
// Get distinct values
db.users.distinct("city")
// Returns: ["New York", "Los Angeles", "Chicago"]

// Distinct with filter
db.users.distinct("city", { age: { $gte: 25 } })

// Distinct on nested field
db.users.distinct("address.state")
```

---

## UPDATE Operations

### updateOne()
Update first matching document.

```javascript
// Basic update
db.users.updateOne(
  { name: "John Doe" },
  { $set: { age: 31 } }
)

// Result
{
  "acknowledged" : true,
  "matchedCount" : 1,
  "modifiedCount" : 1
}

// Update multiple fields
db.users.updateOne(
  { _id: 1 },
  {
    $set: {
      age: 31,
      city: "Boston",
      updatedAt: new Date()
    }
  }
)

// Update nested field
db.users.updateOne(
  { _id: 1 },
  { $set: { "address.city": "Boston" } }
)

// Upsert (insert if not exists)
db.users.updateOne(
  { email: "new@example.com" },
  { $set: { name: "New User", age: 25 } },
  { upsert: true }
)
```

---

### updateMany()
Update all matching documents.

```javascript
// Update multiple documents
db.users.updateMany(
  { city: "New York" },
  { $set: { timezone: "EST" } }
)

// Result
{
  "acknowledged" : true,
  "matchedCount" : 150,
  "modifiedCount" : 150
}

// Update all documents
db.users.updateMany(
  {},
  { $set: { verified: false } }
)

// Increment all ages
db.users.updateMany(
  {},
  { $inc: { age: 1 } }
)
```

---

### replaceOne()
Replace entire document (except _id).

```javascript
// Replace document
db.users.replaceOne(
  { _id: 1 },
  {
    name: "John Doe",
    email: "john.doe@example.com",
    age: 30,
    city: "New York"
  }
)

// Note: All old fields are removed except _id
// This is different from updateOne with $set
```

---

### findOneAndUpdate()
Update and return the document.

```javascript
// Return original document (before update)
db.users.findOneAndUpdate(
  { name: "John Doe" },
  { $set: { age: 31 } }
)

// Return updated document
db.users.findOneAndUpdate(
  { name: "John Doe" },
  { $set: { age: 31 } },
  { returnNewDocument: true }
)

// With projection
db.users.findOneAndUpdate(
  { name: "John Doe" },
  { $set: { age: 31 } },
  {
    returnNewDocument: true,
    projection: { name: 1, age: 1 }
  }
)

// With upsert
db.users.findOneAndUpdate(
  { email: "new@example.com" },
  { $set: { name: "New User" } },
  {
    upsert: true,
    returnNewDocument: true
  }
)
```

---

### findOneAndReplace()
Replace and return the document.

```javascript
// Replace and return
db.users.findOneAndReplace(
  { name: "John Doe" },
  { name: "John Smith", age: 30, email: "john@example.com" },
  { returnNewDocument: true }
)
```

---

## DELETE Operations

### deleteOne()
Delete first matching document.

```javascript
// Delete one document
db.users.deleteOne({ name: "John Doe" })

// Result
{
  "acknowledged" : true,
  "deletedCount" : 1
}

// Delete by _id
db.users.deleteOne({ _id: ObjectId("507f1f77bcf86cd799439011") })

// Delete with filter
db.users.deleteOne({
  age: { $lt: 18 },
  status: "inactive"
})
```

---

### deleteMany()
Delete all matching documents.

```javascript
// Delete multiple documents
db.users.deleteMany({ status: "inactive" })

// Result
{
  "acknowledged" : true,
  "deletedCount" : 47
}

// Delete all documents (dangerous!)
db.users.deleteMany({})

// Delete with complex filter
db.users.deleteMany({
  $and: [
    { createdAt: { $lt: ISODate("2023-01-01") } },
    { lastLogin: { $exists: false } }
  ]
})
```

---

### findOneAndDelete()
Delete and return the deleted document.

```javascript
// Delete and return document
db.users.findOneAndDelete({ name: "John Doe" })

// Returns the deleted document
{
  "_id" : ObjectId("..."),
  "name" : "John Doe",
  "email" : "john@example.com",
  "age" : 30
}

// With sort (delete oldest inactive user)
db.users.findOneAndDelete(
  { status: "inactive" },
  { sort: { lastLogin: 1 } }
)

// With projection
db.users.findOneAndDelete(
  { status: "inactive" },
  { projection: { name: 1, email: 1 } }
)
```

---

## Query Operators

### Comparison Operators

```javascript
// $eq - Equal
db.users.find({ age: { $eq: 30 } })
// Same as: db.users.find({ age: 30 })

// $ne - Not equal
db.users.find({ status: { $ne: "inactive" } })

// $gt - Greater than
db.users.find({ age: { $gt: 25 } })

// $gte - Greater than or equal
db.users.find({ age: { $gte: 25 } })

// $lt - Less than
db.users.find({ age: { $lt: 40 } })

// $lte - Less than or equal
db.users.find({ age: { $lte: 40 } })

// $in - In array
db.users.find({ status: { $in: ["active", "pending"] } })

// $nin - Not in array
db.users.find({ status: { $nin: ["inactive", "deleted"] } })
```

---

### Logical Operators

```javascript
// $and - All conditions must be true
db.users.find({
  $and: [
    { age: { $gte: 25 } },
    { city: "New York" }
  ]
})

// $or - At least one condition must be true
db.users.find({
  $or: [
    { age: { $lt: 25 } },
    { status: "premium" }
  ]
})

// $not - Inverts the effect of a query expression
db.users.find({
  age: { $not: { $gte: 30 } }
})

// $nor - All conditions must be false
db.users.find({
  $nor: [
    { status: "inactive" },
    { age: { $lt: 18 } }
  ]
})
```

---

### Element Operators

```javascript
// $exists - Field exists or not
db.users.find({ email: { $exists: true } })
db.users.find({ middleName: { $exists: false } })

// $type - Field type check
db.users.find({ age: { $type: "int" } })
db.users.find({ age: { $type: "number" } })

// Type codes
db.users.find({ name: { $type: 2 } })  // 2 = string
```

---

### Evaluation Operators

```javascript
// $regex - Regular expression
db.users.find({
  email: { $regex: /^john/i }
})

// $expr - Allows use of aggregation expressions
db.users.find({
  $expr: { $gt: ["$spent", "$budget"] }
})

// $mod - Modulo operation
db.users.find({
  age: { $mod: [5, 0] }  // age % 5 == 0
})

// $text - Text search (requires text index)
db.articles.find({
  $text: { $search: "mongodb tutorial" }
})

// $where - JavaScript expression (slow, avoid if possible)
db.users.find({
  $where: "this.age > 25 && this.city === 'New York'"
})
```

---

### Array Operators

```javascript
// $all - Contains all elements
db.users.find({
  hobbies: { $all: ["reading", "coding"] }
})

// $elemMatch - Array element matches all conditions
db.users.find({
  scores: { $elemMatch: { $gte: 80, $lt: 90 } }
})

// $size - Array has specific length
db.users.find({
  hobbies: { $size: 3 }
})
```

---

## Update Operators

### Field Update Operators

```javascript
// $set - Set field value
db.users.updateOne(
  { _id: 1 },
  { $set: { age: 31, city: "Boston" } }
)

// $unset - Remove field
db.users.updateOne(
  { _id: 1 },
  { $unset: { middleName: "" } }
)

// $rename - Rename field
db.users.updateMany(
  {},
  { $rename: { "name": "fullName" } }
)

// $inc - Increment value
db.users.updateOne(
  { _id: 1 },
  { $inc: { age: 1, loginCount: 1 } }
)

// Negative increment (decrement)
db.products.updateOne(
  { _id: 1 },
  { $inc: { stock: -5 } }
)

// $mul - Multiply value
db.products.updateOne(
  { _id: 1 },
  { $mul: { price: 1.1 } }  // Increase by 10%
)

// $min - Update if new value is less than current
db.users.updateOne(
  { _id: 1 },
  { $min: { lowestScore: 75 } }
)

// $max - Update if new value is greater than current
db.users.updateOne(
  { _id: 1 },
  { $max: { highestScore: 95 } }
)

// $currentDate - Set to current date
db.users.updateOne(
  { _id: 1 },
  {
    $currentDate: {
      lastModified: true,
      "metadata.lastUpdated": { $type: "timestamp" }
    }
  }
)
```

---

### Array Update Operators

```javascript
// $push - Add element to array
db.users.updateOne(
  { _id: 1 },
  { $push: { hobbies: "gaming" } }
)

// $push with $each (multiple elements)
db.users.updateOne(
  { _id: 1 },
  {
    $push: {
      hobbies: {
        $each: ["swimming", "cycling"],
        $sort: 1,           // Sort array
        $slice: 5           // Keep only 5 elements
      }
    }
  }
)

// $pull - Remove elements matching condition
db.users.updateOne(
  { _id: 1 },
  { $pull: { hobbies: "gaming" } }
)

// $pull with condition
db.users.updateOne(
  { _id: 1 },
  { $pull: { scores: { $lt: 60 } } }
)

// $pop - Remove first or last element
db.users.updateOne(
  { _id: 1 },
  { $pop: { hobbies: 1 } }   // Remove last
)

db.users.updateOne(
  { _id: 1 },
  { $pop: { hobbies: -1 } }  // Remove first
)

// $addToSet - Add element only if not exists
db.users.updateOne(
  { _id: 1 },
  { $addToSet: { hobbies: "reading" } }
)

// $addToSet with $each
db.users.updateOne(
  { _id: 1 },
  {
    $addToSet: {
      hobbies: { $each: ["swimming", "cycling"] }
    }
  }
)

// $ (positional operator) - Update matched array element
db.users.updateOne(
  { _id: 1, "scores.subject": "math" },
  { $set: { "scores.$.score": 95 } }
)

// $[] (all positional) - Update all array elements
db.users.updateOne(
  { _id: 1 },
  { $inc: { "scores.$[].score": 5 } }
)

// $[<identifier>] (filtered positional) - Update specific elements
db.users.updateOne(
  { _id: 1 },
  { $inc: { "scores.$[elem].score": 10 } },
  { arrayFilters: [{ "elem.score": { $gte: 80 } }] }
)
```

---

## Cursor Methods

### Iteration Methods

```javascript
// forEach
db.users.find().forEach(function(doc) {
  print("Name: " + doc.name)
})

// map
db.users.find().map(function(doc) {
  return doc.name
})

// toArray
const users = db.users.find().toArray()

// hasNext and next
const cursor = db.users.find()
while (cursor.hasNext()) {
  const doc = cursor.next()
  print(doc.name)
}
```

---

### Modification Methods

```javascript
// sort
db.users.find().sort({ age: -1, name: 1 })

// limit
db.users.find().limit(10)

// skip
db.users.find().skip(20)

// count
db.users.find({ age: { $gte: 25 } }).count()

// Chaining
db.users.find({ status: "active" })
  .sort({ createdAt: -1 })
  .skip(10)
  .limit(5)
  .forEach(doc => print(doc.name))
```

---

### Cursor Information

```javascript
// explain - Query execution plan
db.users.find({ age: 30 }).explain("executionStats")

// size - Number of documents
db.users.find().size()

// itcount - Iterate and count
db.users.find().itcount()
```

---

## Aggregation Basics

### Aggregation Pipeline

```javascript
// Basic aggregation
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$customerId", total: { $sum: "$amount" } } },
  { $sort: { total: -1 } },
  { $limit: 10 }
])
```

---

### Common Aggregation Stages

```javascript
// $match - Filter documents
db.orders.aggregate([
  { $match: { status: "completed" } }
])

// $project - Reshape documents
db.users.aggregate([
  {
    $project: {
      name: 1,
      email: 1,
      ageInMonths: { $multiply: ["$age", 12] }
    }
  }
])

// $group - Group by field
db.orders.aggregate([
  {
    $group: {
      _id: "$customerId",
      totalOrders: { $sum: 1 },
      totalAmount: { $sum: "$amount" },
      avgAmount: { $avg: "$amount" }
    }
  }
])

// $sort - Sort documents
db.orders.aggregate([
  { $sort: { amount: -1 } }
])

// $limit - Limit results
db.orders.aggregate([
  { $limit: 10 }
])

// $skip - Skip documents
db.orders.aggregate([
  { $skip: 20 }
])

// $unwind - Deconstruct arrays
db.orders.aggregate([
  { $unwind: "$items" }
])

// $lookup - Join collections
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customerInfo"
    }
  }
])

// $count - Count documents
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $count: "completedOrders" }
])

// $addFields - Add new fields
db.users.aggregate([
  {
    $addFields: {
      fullName: { $concat: ["$firstName", " ", "$lastName"] }
    }
  }
])
```

---

## Practical Examples

### Example 1: User Management System

```javascript
// Create users collection with sample data
db.users.insertMany([
  {
    username: "john_doe",
    email: "john@example.com",
    age: 30,
    status: "active",
    roles: ["user", "admin"],
    profile: {
      firstName: "John",
      lastName: "Doe",
      bio: "Software developer"
    },
    createdAt: new Date("2023-01-15"),
    loginCount: 45
  },
  {
    username: "jane_smith",
    email: "jane@example.com",
    age: 28,
    status: "active",
    roles: ["user"],
    profile: {
      firstName: "Jane",
      lastName: "Smith",
      bio: "Product manager"
    },
    createdAt: new Date("2023-02-20"),
    loginCount: 32
  },
  {
    username: "bob_wilson",
    email: "bob@example.com",
    age: 35,
    status: "inactive",
    roles: ["user"],
    profile: {
      firstName: "Bob",
      lastName: "Wilson",
      bio: "Designer"
    },
    createdAt: new Date("2022-12-10"),
    loginCount: 12
  }
])

// Find active users
db.users.find({ status: "active" })

// Find admin users
db.users.find({ roles: "admin" })

// Update user status
db.users.updateOne(
  { username: "bob_wilson" },
  { $set: { status: "active" } }
)

// Increment login count
db.users.updateOne(
  { username: "john_doe" },
  { $inc: { loginCount: 1 } }
)

// Add role to user
db.users.updateOne(
  { username: "jane_smith" },
  { $addToSet: { roles: "moderator" } }
)

// Delete inactive users who haven't logged in much
db.users.deleteMany({
  status: "inactive",
  loginCount: { $lt: 5 }
})
```

---

### Example 2: E-commerce Orders

```javascript
// Create orders collection
db.orders.insertMany([
  {
    orderId: "ORD001",
    customerId: ObjectId("507f1f77bcf86cd799439011"),
    items: [
      { productId: "P001", name: "Laptop", quantity: 1, price: 999.99 },
      { productId: "P002", name: "Mouse", quantity: 2, price: 25.00 }
    ],
    totalAmount: 1049.99,
    status: "completed",
    shippingAddress: {
      street: "123 Main St",
      city: "New York",
      state: "NY",
      zipcode: "10001"
    },
    orderDate: new Date("2024-01-15"),
    deliveryDate: new Date("2024-01-20")
  },
  {
    orderId: "ORD002",
    customerId: ObjectId("507f1f77bcf86cd799439012"),
    items: [
      { productId: "P003", name: "Keyboard", quantity: 1, price: 79.99 }
    ],
    totalAmount: 79.99,
    status: "pending",
    shippingAddress: {
      street: "456 Oak Ave",
      city: "Los Angeles",
      state: "CA",
      zipcode: "90001"
    },
    orderDate: new Date("2024-01-25")
  }
])

// Find completed orders
db.orders.find({ status: "completed" })

// Find orders above certain amount
db.orders.find({ totalAmount: { $gte: 100 } })

// Update order status
db.orders.updateOne(
  { orderId: "ORD002" },
  {
    $set: {
      status: "shipped",
      shippedDate: new Date()
    }
  }
)

// Add tracking number
db.orders.updateOne(
  { orderId: "ORD002" },
  { $set: { trackingNumber: "TRACK123456" } }
)

// Find orders by city
db.orders.find({ "shippingAddress.city": "New York" })

// Aggregation: Total sales by status
db.orders.aggregate([
  {
    $group: {
      _id: "$status",
      count: { $sum: 1 },
      totalRevenue: { $sum: "$totalAmount" }
    }
  }
])

// Aggregation: Unwind items and count products sold
db.orders.aggregate([
  { $unwind: "$items" },
  {
    $group: {
      _id: "$items.productId",
      productName: { $first: "$items.name" },
      totalQuantity: { $sum: "$items.quantity" },
      totalRevenue: { $sum: { $multiply: ["$items.quantity", "$items.price"] } }
    }
  },
  { $sort: { totalRevenue: -1 } }
])
```

---

### Example 3: Blog Posts and Comments

```javascript
// Create posts collection
db.posts.insertMany([
  {
    title: "Introduction to MongoDB",
    slug: "intro-to-mongodb",
    author: "John Doe",
    content: "MongoDB is a powerful NoSQL database...",
    tags: ["mongodb", "database", "nosql"],
    published: true,
    publishedDate: new Date("2024-01-10"),
    views: 1250,
    likes: 45,
    comments: [
      {
        commentId: 1,
        author: "Jane Smith",
        text: "Great article!",
        date: new Date("2024-01-11")
      },
      {
        commentId: 2,
        author: "Bob Wilson",
        text: "Very helpful, thanks!",
        date: new Date("2024-01-12")
      }
    ]
  },
  {
    title: "Advanced MongoDB Queries",
    slug: "advanced-mongodb-queries",
    author: "Jane Smith",
    content: "In this post, we'll explore advanced querying...",
    tags: ["mongodb", "queries", "advanced"],
    published: true,
    publishedDate: new Date("2024-01-20"),
    views: 890,
    likes: 32,
    comments: []
  }
])

// Find published posts
db.posts.find({ published: true })

// Find posts by tag
db.posts.find({ tags: "mongodb" })

// Find posts with multiple tags
db.posts.find({ tags: { $all: ["mongodb", "advanced"] } })

// Increment views
db.posts.updateOne(
  { slug: "intro-to-mongodb" },
  { $inc: { views: 1 } }
)

// Add comment
db.posts.updateOne(
  { slug: "intro-to-mongodb" },
  {
    $push: {
      comments: {
        commentId: 3,
        author: "Alice Brown",
        text: "Thanks for sharing!",
        date: new Date()
      }
    }
  }
)

// Remove comment
db.posts.updateOne(
  { slug: "intro-to-mongodb" },
  { $pull: { comments: { commentId: 2 } } }
)

// Find posts sorted by views
db.posts.find().sort({ views: -1 }).limit(10)

// Text search (requires text index)
db.posts.createIndex({ title: "text", content: "text" })
db.posts.find({ $text: { $search: "mongodb queries" } })

// Aggregation: Posts by author
db.posts.aggregate([
  {
    $group: {
      _id: "$author",
      postCount: { $sum: 1 },
      totalViews: { $sum: "$views" },
      totalLikes: { $sum: "$likes" }
    }
  },
  { $sort: { postCount: -1 } }
])

// Aggregation: Most popular tags
db.posts.aggregate([
  { $unwind: "$tags" },
  {
    $group: {
      _id: "$tags",
      count: { $sum: 1 }
    }
  },
  { $sort: { count: -1 } },
  { $limit: 5 }
])
```

---

## Edge Cases and Best Practices

### 1. Handling Null and Missing Fields

```javascript
// Insert documents with null/missing fields
db.test.insertMany([
  { name: "John", age: 30 },
  { name: "Jane", age: null },
  { name: "Bob" }  // age missing
])

// Find null age
db.test.find({ age: null })
// Returns: Jane and Bob

// Find only where age is explicitly null
db.test.find({ age: { $type: 10 } })  // 10 = null type
// Returns: Only Jane

// Find missing field
db.test.find({ age: { $exists: false } })
// Returns: Only Bob

// Find not null
db.test.find({ age: { $ne: null } })
// Returns: Only John
```

---

### 2. Array Edge Cases

```javascript
// Empty array vs missing field
db.test.insertMany([
  { name: "User1", tags: [] },
  { name: "User2", tags: ["a", "b"] },
  { name: "User3" }
])

// Find empty arrays
db.test.find({ tags: { $size: 0 } })

// Find missing or empty
db.test.find({
  $or: [
    { tags: { $exists: false } },
    { tags: { $size: 0 } }
  ]
})
```

---

### 3. Large Result Sets

```javascript
// Bad: Loads everything into memory
const allUsers = db.users.find().toArray()

// Good: Use cursor
db.users.find().forEach(user => {
  // Process one at a time
})

// Good: Use pagination
const page = 1
const pageSize = 100
db.users.find()
  .skip((page - 1) * pageSize)
  .limit(pageSize)
```

---

### 4. Update Safety

```javascript
// Always use $set to prevent document replacement
db.users.updateOne(
  { _id: 1 },
  { $set: { age: 31 } }  // Good
)

// This replaces the entire document!
db.users.updateOne(
  { _id: 1 },
  { age: 31 }  // Bad - removes all other fields!
)
```

---

### 5. Atomic Counters

```javascript
// Thread-safe increment
db.counters.updateOne(
  { _id: "orderId" },
  { $inc: { sequence: 1 } },
  { upsert: true }
)

// Get and increment in one operation
const result = db.counters.findOneAndUpdate(
  { _id: "orderId" },
  { $inc: { sequence: 1 } },
  { upsert: true, returnNewDocument: true }
)
const nextId = result.sequence
```

---

## Summary Checklist

### CREATE Mastery ✓
- [ ] insertOne() with all data types
- [ ] insertMany() with ordered/unordered options
- [ ] Custom _id handling
- [ ] Bulk operations
- [ ] Error handling for duplicate keys

### READ Mastery ✓
- [ ] find() with all query operators
- [ ] findOne() optimization
- [ ] Complex queries with $and, $or, $not
- [ ] Array queries ($all, $elemMatch, $size)
- [ ] Nested document queries
- [ ] Regular expressions
- [ ] Projection techniques
- [ ] Cursor methods (sort, limit, skip)
- [ ] Pagination patterns
- [ ] Count operations

### UPDATE Mastery ✓
- [ ] updateOne() vs updateMany()
- [ ] All update operators ($set, $inc, $push, $pull, etc.)
- [ ] Array update techniques
- [ ] Positional operators ($, $[], $[<identifier>])
- [ ] Upsert operations
- [ ] findOneAndUpdate() patterns
- [ ] Atomic operations

### DELETE Mastery ✓
- [ ] deleteOne() vs deleteMany()
- [ ] Safe deletion patterns
- [ ] findOneAndDelete()
- [ ] Soft deletes (status field approach)

---

## Next Steps

1. **Practice with real data** - Use the exercises in the companion practice file
2. **Master aggregation** - Study the aggregation framework deeply
3. **Learn indexing** - Optimize query performance
4. **Study transactions** - Multi-document ACID operations
5. **Build projects** - Apply knowledge in real applications

---

**Remember:** MongoDB is flexible, but that flexibility requires discipline. Always think about:
- Data access patterns
- Document size limits
- Query performance
- Atomic operations
- Error handling

**Practice makes perfect. Keep coding!** 🚀
