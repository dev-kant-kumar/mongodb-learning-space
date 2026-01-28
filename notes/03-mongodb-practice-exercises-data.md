# MongoDB Practice Exercises & Data 🎯

> **Purpose:** Progressive exercises with realistic data to achieve MongoDB CRUD mastery.

---

## Table of Contents
1. [Practice Data Setup](#practice-data-setup)
2. [Beginner Exercises](#beginner-exercises)
3. [Intermediate Exercises](#intermediate-exercises)
4. [Advanced Exercises](#advanced-exercises)
5. [Real-World Scenarios](#real-world-scenarios)
6. [Challenge Problems](#challenge-problems)
7. [Solutions](#solutions)

---

## Practice Data Setup

### Dataset 1: E-commerce System

```javascript
// Switch to practice database
use ecommerce_practice

// Users Collection
db.users.insertMany([
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0e1"),
    username: "john_doe",
    email: "john.doe@email.com",
    password: "hashed_password_123",
    firstName: "John",
    lastName: "Doe",
    age: 32,
    gender: "male",
    phone: "555-0101",
    address: {
      street: "123 Main St",
      city: "New York",
      state: "NY",
      zipcode: "10001",
      country: "USA"
    },
    role: "customer",
    isVerified: true,
    createdAt: ISODate("2023-06-15T10:30:00Z"),
    lastLogin: ISODate("2024-01-20T14:25:00Z"),
    preferences: {
      newsletter: true,
      notifications: true,
      theme: "dark"
    },
    wishlist: ["65a1b2c3d4e5f6a7b8c9d0f1", "65a1b2c3d4e5f6a7b8c9d0f2"],
    loyaltyPoints: 1250
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0e2"),
    username: "jane_smith",
    email: "jane.smith@email.com",
    password: "hashed_password_456",
    firstName: "Jane",
    lastName: "Smith",
    age: 28,
    gender: "female",
    phone: "555-0102",
    address: {
      street: "456 Oak Ave",
      city: "Los Angeles",
      state: "CA",
      zipcode: "90001",
      country: "USA"
    },
    role: "customer",
    isVerified: true,
    createdAt: ISODate("2023-08-20T09:15:00Z"),
    lastLogin: ISODate("2024-01-25T16:45:00Z"),
    preferences: {
      newsletter: false,
      notifications: true,
      theme: "light"
    },
    wishlist: ["65a1b2c3d4e5f6a7b8c9d0f3"],
    loyaltyPoints: 850
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0e3"),
    username: "bob_wilson",
    email: "bob.wilson@email.com",
    password: "hashed_password_789",
    firstName: "Bob",
    lastName: "Wilson",
    age: 45,
    gender: "male",
    phone: "555-0103",
    address: {
      street: "789 Pine Rd",
      city: "Chicago",
      state: "IL",
      zipcode: "60601",
      country: "USA"
    },
    role: "customer",
    isVerified: false,
    createdAt: ISODate("2023-12-05T11:20:00Z"),
    lastLogin: ISODate("2023-12-10T08:30:00Z"),
    preferences: {
      newsletter: true,
      notifications: false,
      theme: "light"
    },
    wishlist: [],
    loyaltyPoints: 0
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0e4"),
    username: "alice_brown",
    email: "alice.brown@email.com",
    password: "hashed_password_321",
    firstName: "Alice",
    lastName: "Brown",
    age: 35,
    gender: "female",
    phone: "555-0104",
    address: {
      street: "321 Elm St",
      city: "Seattle",
      state: "WA",
      zipcode: "98101",
      country: "USA"
    },
    role: "admin",
    isVerified: true,
    createdAt: ISODate("2023-05-10T13:45:00Z"),
    lastLogin: ISODate("2024-01-27T12:15:00Z"),
    preferences: {
      newsletter: true,
      notifications: true,
      theme: "dark"
    },
    wishlist: ["65a1b2c3d4e5f6a7b8c9d0f1", "65a1b2c3d4e5f6a7b8c9d0f4"],
    loyaltyPoints: 3200
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0e5"),
    username: "charlie_davis",
    email: "charlie.davis@email.com",
    password: "hashed_password_654",
    firstName: "Charlie",
    lastName: "Davis",
    age: 29,
    gender: "non-binary",
    phone: "555-0105",
    address: {
      street: "654 Maple Dr",
      city: "Austin",
      state: "TX",
      zipcode: "73301",
      country: "USA"
    },
    role: "customer",
    isVerified: true,
    createdAt: ISODate("2023-09-12T15:30:00Z"),
    lastLogin: ISODate("2024-01-26T10:20:00Z"),
    preferences: {
      newsletter: true,
      notifications: true,
      theme: "dark"
    },
    wishlist: ["65a1b2c3d4e5f6a7b8c9d0f2", "65a1b2c3d4e5f6a7b8c9d0f3", "65a1b2c3d4e5f6a7b8c9d0f5"],
    loyaltyPoints: 450
  }
])

// Products Collection
db.products.insertMany([
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0f1"),
    name: "Wireless Bluetooth Headphones",
    slug: "wireless-bluetooth-headphones",
    description: "Premium noise-canceling wireless headphones with 30-hour battery life",
    category: "Electronics",
    subcategory: "Audio",
    brand: "AudioTech",
    price: 149.99,
    originalPrice: 199.99,
    discount: 25,
    stock: 45,
    images: ["headphones-1.jpg", "headphones-2.jpg", "headphones-3.jpg"],
    specifications: {
      bluetooth: "5.0",
      batteryLife: "30 hours",
      weight: "250g",
      color: ["Black", "Silver", "Blue"]
    },
    ratings: {
      average: 4.5,
      count: 234
    },
    reviews: [
      {
        userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e1"),
        rating: 5,
        comment: "Excellent sound quality!",
        date: ISODate("2024-01-15T10:30:00Z")
      },
      {
        userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e2"),
        rating: 4,
        comment: "Good but a bit expensive",
        date: ISODate("2024-01-18T14:20:00Z")
      }
    ],
    tags: ["wireless", "bluetooth", "headphones", "audio", "noise-canceling"],
    featured: true,
    status: "active",
    createdAt: ISODate("2023-11-01T09:00:00Z"),
    updatedAt: ISODate("2024-01-20T11:30:00Z")
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0f2"),
    name: "Smart Fitness Watch",
    slug: "smart-fitness-watch",
    description: "Track your health and fitness with this advanced smartwatch",
    category: "Electronics",
    subcategory: "Wearables",
    brand: "FitTech",
    price: 249.99,
    originalPrice: 299.99,
    discount: 17,
    stock: 28,
    images: ["watch-1.jpg", "watch-2.jpg"],
    specifications: {
      display: "1.4 inch AMOLED",
      batteryLife: "7 days",
      waterproof: "5ATM",
      sensors: ["Heart Rate", "SpO2", "GPS"]
    },
    ratings: {
      average: 4.7,
      count: 189
    },
    reviews: [],
    tags: ["smartwatch", "fitness", "health", "wearable"],
    featured: true,
    status: "active",
    createdAt: ISODate("2023-11-15T10:00:00Z"),
    updatedAt: ISODate("2024-01-22T09:45:00Z")
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0f3"),
    name: "Mechanical Gaming Keyboard",
    slug: "mechanical-gaming-keyboard",
    description: "RGB backlit mechanical keyboard with blue switches",
    category: "Electronics",
    subcategory: "Gaming",
    brand: "GameGear",
    price: 89.99,
    originalPrice: 89.99,
    discount: 0,
    stock: 67,
    images: ["keyboard-1.jpg", "keyboard-2.jpg", "keyboard-3.jpg"],
    specifications: {
      switchType: "Blue Mechanical",
      backlighting: "RGB",
      connectivity: "USB-C",
      layout: "Full-size"
    },
    ratings: {
      average: 4.3,
      count: 456
    },
    reviews: [
      {
        userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e5"),
        rating: 5,
        comment: "Perfect for gaming!",
        date: ISODate("2024-01-10T16:45:00Z")
      }
    ],
    tags: ["keyboard", "gaming", "mechanical", "rgb"],
    featured: false,
    status: "active",
    createdAt: ISODate("2023-10-20T11:30:00Z"),
    updatedAt: ISODate("2024-01-19T14:20:00Z")
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0f4"),
    name: "Portable Bluetooth Speaker",
    slug: "portable-bluetooth-speaker",
    description: "Waterproof portable speaker with 360-degree sound",
    category: "Electronics",
    subcategory: "Audio",
    brand: "SoundWave",
    price: 59.99,
    originalPrice: 79.99,
    discount: 25,
    stock: 0,
    images: ["speaker-1.jpg"],
    specifications: {
      bluetooth: "5.2",
      batteryLife: "12 hours",
      waterproof: "IPX7",
      weight: "400g"
    },
    ratings: {
      average: 4.1,
      count: 89
    },
    reviews: [],
    tags: ["speaker", "bluetooth", "portable", "waterproof"],
    featured: false,
    status: "out_of_stock",
    createdAt: ISODate("2023-12-01T08:15:00Z"),
    updatedAt: ISODate("2024-01-23T10:00:00Z")
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d0f5"),
    name: "4K Webcam",
    slug: "4k-webcam",
    description: "Professional 4K webcam with auto-focus and noise reduction",
    category: "Electronics",
    subcategory: "Accessories",
    brand: "ViewMax",
    price: 129.99,
    originalPrice: 149.99,
    discount: 13,
    stock: 15,
    images: ["webcam-1.jpg", "webcam-2.jpg"],
    specifications: {
      resolution: "4K (3840x2160)",
      fps: "30",
      fov: "90 degrees",
      microphone: "Dual noise-canceling"
    },
    ratings: {
      average: 4.6,
      count: 123
    },
    reviews: [],
    tags: ["webcam", "4k", "streaming", "video"],
    featured: false,
    status: "active",
    createdAt: ISODate("2023-11-25T13:20:00Z"),
    updatedAt: ISODate("2024-01-21T15:10:00Z")
  }
])

// Orders Collection
db.orders.insertMany([
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d101"),
    orderNumber: "ORD-2024-0001",
    userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e1"),
    items: [
      {
        productId: ObjectId("65a1b2c3d4e5f6a7b8c9d0f1"),
        productName: "Wireless Bluetooth Headphones",
        quantity: 1,
        price: 149.99,
        discount: 25
      },
      {
        productId: ObjectId("65a1b2c3d4e5f6a7b8c9d0f3"),
        productName: "Mechanical Gaming Keyboard",
        quantity: 1,
        price: 89.99,
        discount: 0
      }
    ],
    subtotal: 239.98,
    tax: 21.60,
    shipping: 10.00,
    total: 271.58,
    paymentMethod: "credit_card",
    paymentStatus: "paid",
    shippingAddress: {
      street: "123 Main St",
      city: "New York",
      state: "NY",
      zipcode: "10001",
      country: "USA"
    },
    status: "delivered",
    trackingNumber: "TRK123456789",
    orderDate: ISODate("2024-01-10T14:30:00Z"),
    shippedDate: ISODate("2024-01-11T09:00:00Z"),
    deliveredDate: ISODate("2024-01-15T16:45:00Z"),
    notes: "Leave at front door"
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d102"),
    orderNumber: "ORD-2024-0002",
    userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e2"),
    items: [
      {
        productId: ObjectId("65a1b2c3d4e5f6a7b8c9d0f2"),
        productName: "Smart Fitness Watch",
        quantity: 1,
        price: 249.99,
        discount: 17
      }
    ],
    subtotal: 249.99,
    tax: 22.50,
    shipping: 0,
    total: 272.49,
    paymentMethod: "paypal",
    paymentStatus: "paid",
    shippingAddress: {
      street: "456 Oak Ave",
      city: "Los Angeles",
      state: "CA",
      zipcode: "90001",
      country: "USA"
    },
    status: "shipped",
    trackingNumber: "TRK987654321",
    orderDate: ISODate("2024-01-20T11:15:00Z"),
    shippedDate: ISODate("2024-01-21T10:30:00Z"),
    deliveredDate: null,
    notes: ""
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d103"),
    orderNumber: "ORD-2024-0003",
    userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e5"),
    items: [
      {
        productId: ObjectId("65a1b2c3d4e5f6a7b8c9d0f5"),
        productName: "4K Webcam",
        quantity: 2,
        price: 129.99,
        discount: 13
      }
    ],
    subtotal: 259.98,
    tax: 23.40,
    shipping: 15.00,
    total: 298.38,
    paymentMethod: "credit_card",
    paymentStatus: "paid",
    shippingAddress: {
      street: "654 Maple Dr",
      city: "Austin",
      state: "TX",
      zipcode: "73301",
      country: "USA"
    },
    status: "processing",
    trackingNumber: null,
    orderDate: ISODate("2024-01-25T09:20:00Z"),
    shippedDate: null,
    deliveredDate: null,
    notes: "Gift wrap requested"
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d104"),
    orderNumber: "ORD-2024-0004",
    userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e1"),
    items: [
      {
        productId: ObjectId("65a1b2c3d4e5f6a7b8c9d0f2"),
        productName: "Smart Fitness Watch",
        quantity: 1,
        price: 249.99,
        discount: 17
      }
    ],
    subtotal: 249.99,
    tax: 22.50,
    shipping: 0,
    total: 272.49,
    paymentMethod: "credit_card",
    paymentStatus: "pending",
    shippingAddress: {
      street: "123 Main St",
      city: "New York",
      state: "NY",
      zipcode: "10001",
      country: "USA"
    },
    status: "pending",
    trackingNumber: null,
    orderDate: ISODate("2024-01-27T13:45:00Z"),
    shippedDate: null,
    deliveredDate: null,
    notes: ""
  }
])

// Categories Collection
db.categories.insertMany([
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d201"),
    name: "Electronics",
    slug: "electronics",
    description: "Electronic devices and gadgets",
    subcategories: [
      { name: "Audio", slug: "audio" },
      { name: "Wearables", slug: "wearables" },
      { name: "Gaming", slug: "gaming" },
      { name: "Accessories", slug: "accessories" }
    ],
    featured: true,
    productCount: 5,
    createdAt: ISODate("2023-10-01T10:00:00Z")
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d202"),
    name: "Fashion",
    slug: "fashion",
    description: "Clothing and accessories",
    subcategories: [
      { name: "Men", slug: "men" },
      { name: "Women", slug: "women" },
      { name: "Kids", slug: "kids" }
    ],
    featured: true,
    productCount: 0,
    createdAt: ISODate("2023-10-01T10:05:00Z")
  }
])
```

---

### Dataset 2: Social Media Platform

```javascript
// Switch to social media database
use social_media_practice

// Posts Collection
db.posts.insertMany([
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d301"),
    userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e1"),
    username: "john_doe",
    content: "Just finished my MongoDB tutorial! 🚀 #learning #mongodb #database",
    type: "text",
    media: [],
    likes: [
      ObjectId("65a1b2c3d4e5f6a7b8c9d0e2"),
      ObjectId("65a1b2c3d4e5f6a7b8c9d0e4"),
      ObjectId("65a1b2c3d4e5f6a7b8c9d0e5")
    ],
    comments: [
      {
        commentId: ObjectId("65a1b2c3d4e5f6a7b8c9d401"),
        userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e2"),
        username: "jane_smith",
        text: "Great job! MongoDB is awesome!",
        createdAt: ISODate("2024-01-15T15:30:00Z"),
        likes: [ObjectId("65a1b2c3d4e5f6a7b8c9d0e1")]
      },
      {
        commentId: ObjectId("65a1b2c3d4e5f6a7b8c9d402"),
        userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e5"),
        username: "charlie_davis",
        text: "Nice! What's next on your learning list?",
        createdAt: ISODate("2024-01-15T16:45:00Z"),
        likes: []
      }
    ],
    shares: 5,
    hashtags: ["learning", "mongodb", "database"],
    mentions: [],
    visibility: "public",
    isEdited: false,
    createdAt: ISODate("2024-01-15T14:20:00Z"),
    updatedAt: ISODate("2024-01-15T14:20:00Z")
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d302"),
    userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e2"),
    username: "jane_smith",
    content: "Beautiful sunset today! 🌅",
    type: "image",
    media: ["sunset-photo.jpg"],
    likes: [
      ObjectId("65a1b2c3d4e5f6a7b8c9d0e1"),
      ObjectId("65a1b2c3d4e5f6a7b8c9d0e4")
    ],
    comments: [],
    shares: 2,
    hashtags: ["sunset", "nature", "photography"],
    mentions: [],
    visibility: "public",
    isEdited: false,
    createdAt: ISODate("2024-01-20T18:30:00Z"),
    updatedAt: ISODate("2024-01-20T18:30:00Z")
  },
  {
    _id: ObjectId("65a1b2c3d4e5f6a7b8c9d303"),
    userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e5"),
    username: "charlie_davis",
    content: "Check out my new blog post about web development! Link in bio 🔗",
    type: "text",
    media: [],
    likes: [ObjectId("65a1b2c3d4e5f6a7b8c9d0e1")],
    comments: [
      {
        commentId: ObjectId("65a1b2c3d4e5f6a7b8c9d403"),
        userId: ObjectId("65a1b2c3d4e5f6a7b8c9d0e1"),
        username: "john_doe",
        text: "Will definitely read it!",
        createdAt: ISODate("2024-01-22T10:15:00Z"),
        likes: []
      }
    ],
    shares: 8,
    hashtags: ["webdev", "programming", "blog"],
    mentions: [],
    visibility: "public",
    isEdited: true,
    createdAt: ISODate("2024-01-22T09:00:00Z"),
    updatedAt: ISODate("2024-01-22T09:30:00Z")
  }
])
```

---

## Beginner Exercises

### Exercise Set 1: Basic CRUD Operations

**Exercise 1.1: Simple Inserts**
```javascript
// TODO: Insert a new user
// Name: David Miller
// Email: david.miller@email.com
// Age: 27
// City: Boston
```

**Exercise 1.2: Find All**
```javascript
// TODO: Find all users in the users collection
```

**Exercise 1.3: Find by Field**
```javascript
// TODO: Find the user with email "jane.smith@email.com"
```

**Exercise 1.4: Find with Multiple Conditions**
```javascript
// TODO: Find all users who are verified AND have role "customer"
```

**Exercise 1.5: Update Single Field**
```javascript
// TODO: Update john_doe's age to 33
```

**Exercise 1.6: Delete by Condition**
```javascript
// TODO: Delete all unverified users
```

---

### Exercise Set 2: Comparison Operators

**Exercise 2.1: Greater Than**
```javascript
// TODO: Find all products with price greater than $100
```

**Exercise 2.2: Range Query**
```javascript
// TODO: Find all users between ages 25 and 35 (inclusive)
```

**Exercise 2.3: Not Equal**
```javascript
// TODO: Find all products NOT in the "Gaming" subcategory
```

**Exercise 2.4: In Operator**
```javascript
// TODO: Find all products in categories: "Audio" or "Wearables"
```

---

### Exercise Set 3: Projection

**Exercise 3.1: Include Fields**
```javascript
// TODO: Find all users, return only username and email (exclude _id)
```

**Exercise 3.2: Exclude Fields**
```javascript
// TODO: Find all products, exclude images and reviews
```

---

### Exercise Set 4: Sorting and Limiting

**Exercise 4.1: Sort Ascending**
```javascript
// TODO: Find all users sorted by age (youngest first)
```

**Exercise 4.2: Sort Descending**
```javascript
// TODO: Find all products sorted by price (highest first)
```

**Exercise 4.3: Limit Results**
```javascript
// TODO: Find the 5 most expensive products
```

**Exercise 4.4: Pagination**
```javascript
// TODO: Get the second page of users (page size: 2)
```

---

## Intermediate Exercises

### Exercise Set 5: Complex Queries

**Exercise 5.1: Logical OR**
```javascript
// TODO: Find users who are EITHER:
// - From New York OR
// - Have loyalty points > 1000
```

**Exercise 5.2: Nested AND/OR**
```javascript
// TODO: Find products that are:
// - Featured AND
// - (Price < $100 OR Stock > 50)
```

**Exercise 5.3: Array Queries**
```javascript
// TODO: Find all products with tag "bluetooth"
```

**Exercise 5.4: Array Size**
```javascript
// TODO: Find all users with exactly 2 items in their wishlist
```

---

### Exercise Set 6: Update Operations

**Exercise 6.1: Increment**
```javascript
// TODO: Increase alice_brown's loyalty points by 500
```

**Exercise 6.2: Push to Array**
```javascript
// TODO: Add product ObjectId("65a1b2c3d4e5f6a7b8c9d0f4") to jane_smith's wishlist
```

**Exercise 6.3: Pull from Array**
```javascript
// TODO: Remove the first item from john_doe's wishlist
```

**Exercise 6.4: Update Nested Field**
```javascript
// TODO: Change bob_wilson's address city to "Boston"
```

**Exercise 6.5: Multiple Updates**
```javascript
// TODO: For all products with stock = 0, set status to "out_of_stock"
```

---

### Exercise Set 7: Aggregation Basics

**Exercise 7.1: Count by Group**
```javascript
// TODO: Count number of users by role
```

**Exercise 7.2: Sum and Average**
```javascript
// TODO: Calculate total and average price of all products
```

**Exercise 7.3: Group with Multiple Calculations**
```javascript
// TODO: For each order status, calculate:
// - Number of orders
// - Total revenue
// - Average order value
```

---

## Advanced Exercises

### Exercise Set 8: Complex Updates

**Exercise 8.1: Conditional Update**
```javascript
// TODO: For all users with loyalty points > 1000, set role to "vip"
```

**Exercise 8.2: Array Filter Update**
```javascript
// TODO: In the "Wireless Bluetooth Headphones" product,
// increment the rating by 1 for all reviews with rating >= 4
```

**Exercise 8.3: Upsert**
```javascript
// TODO: Update or insert a product with slug "laptop-stand"
// If it exists, increase stock by 10
// If not, create it with:
// - name: "Laptop Stand"
// - price: 39.99
// - stock: 10
```

---

### Exercise Set 9: Advanced Aggregation

**Exercise 9.1: Unwind and Group**
```javascript
// TODO: Find the total quantity sold for each product
// Hint: Unwind order items, then group by productId
```

**Exercise 9.2: Lookup (Join)**
```javascript
// TODO: Get all orders with complete user information
// Join orders with users collection
```

**Exercise 9.3: Multi-Stage Pipeline**
```javascript
// TODO: Find top 3 customers by total spending
// Steps:
// 1. Match only completed orders
// 2. Group by userId, sum total
// 3. Sort by total (descending)
// 4. Limit to 3
```

---

## Real-World Scenarios

### Scenario 1: E-commerce Analytics

**Task 1: Revenue Report**
```javascript
// TODO: Create a revenue report showing:
// - Total orders
// - Total revenue
// - Average order value
// - Revenue by status (pending, processing, shipped, delivered)
```

**Task 2: Product Performance**
```javascript
// TODO: Find products that:
// - Have average rating >= 4.5
// - Have been reviewed by at least 100 customers
// - Are currently in stock
// Sort by number of reviews (descending)
```

**Task 3: Customer Segmentation**
```javascript
// TODO: Create customer segments:
// - VIP: loyalty points >= 2000
// - Regular: loyalty points 500-1999
// - New: loyalty points < 500
// Count users in each segment
```

---

### Scenario 2: Inventory Management

**Task 1: Low Stock Alert**
```javascript
// TODO: Find all products with stock <= 15 and status = "active"
// Return: name, stock, price
// Sort by stock (ascending)
```

**Task 2: Restock Calculation**
```javascript
// TODO: For each product category, calculate:
// - Total products
// - Products in stock
// - Products out of stock
// - Total inventory value (price * stock for all products)
```

---

### Scenario 3: User Engagement

**Task 1: Active Users**
```javascript
// TODO: Find users who:
// - Logged in within the last 30 days
// - Have made at least one order
// - Are verified
```

**Task 2: Inactive Users Cleanup**
```javascript
// TODO: Find users who:
// - Haven't logged in for 90+ days
// - Have 0 loyalty points
// - Have no orders
// Mark them for deletion (add field: markedForDeletion: true)
```

---

## Challenge Problems

### Challenge 1: Complex Analytics
```javascript
// TODO: Create a comprehensive product analytics report that shows:
// For each category:
//   - Number of products
//   - Average price
//   - Total stock value
//   - Best selling product (by total quantity sold)
//   - Average rating
// Sort by total stock value (descending)
```

### Challenge 2: Social Media Engagement
```javascript
// TODO: Find the top 5 most engaging posts based on:
// Engagement score = (likes * 1) + (comments * 2) + (shares * 3)
// Return: username, content (first 50 chars), engagement score
// Sort by engagement score (descending)
```

### Challenge 3: Recommendation System
```javascript
// TODO: For user "john_doe", recommend 3 products that:
// - He hasn't purchased yet
// - Are in categories of products he previously bought
// - Have rating >= 4.0
// - Are in stock
// Sort by rating (descending)
```

### Challenge 4: Order Fulfillment
```javascript
// TODO: Create an order fulfillment pipeline:
// 1. Find all "pending" orders
// 2. Check if all items are in stock
// 3. If yes, update order status to "processing"
// 4. Decrease product stock by ordered quantity
// 5. Add processing date
// Handle this atomically (consider transactions)
```

---

## Solutions

### Beginner Solutions

**Exercise 1.1 Solution:**
```javascript
db.users.insertOne({
  username: "david_miller",
  email: "david.miller@email.com",
  firstName: "David",
  lastName: "Miller",
  age: 27,
  address: {
    city: "Boston",
    state: "MA"
  },
  role: "customer",
  isVerified: false,
  createdAt: new Date(),
  preferences: {
    newsletter: true,
    notifications: true,
    theme: "light"
  },
  wishlist: [],
  loyaltyPoints: 0
})
```

**Exercise 1.2 Solution:**
```javascript
db.users.find()
```

**Exercise 1.3 Solution:**
```javascript
db.users.findOne({ email: "jane.smith@email.com" })
```

**Exercise 1.4 Solution:**
```javascript
db.users.find({
  isVerified: true,
  role: "customer"
})
```

**Exercise 1.5 Solution:**
```javascript
db.users.updateOne(
  { username: "john_doe" },
  { $set: { age: 33 } }
)
```

**Exercise 1.6 Solution:**
```javascript
db.users.deleteMany({ isVerified: false })
```

**Exercise 2.1 Solution:**
```javascript
db.products.find({ price: { $gt: 100 } })
```

**Exercise 2.2 Solution:**
```javascript
db.users.find({
  age: { $gte: 25, $lte: 35 }
})
```

**Exercise 2.3 Solution:**
```javascript
db.products.find({
  subcategory: { $ne: "Gaming" }
})
```

**Exercise 2.4 Solution:**
```javascript
db.products.find({
  subcategory: { $in: ["Audio", "Wearables"] }
})
```

**Exercise 3.1 Solution:**
```javascript
db.users.find(
  {},
  { username: 1, email: 1, _id: 0 }
)
```

**Exercise 3.2 Solution:**
```javascript
db.products.find(
  {},
  { images: 0, reviews: 0 }
)
```

**Exercise 4.1 Solution:**
```javascript
db.users.find().sort({ age: 1 })
```

**Exercise 4.2 Solution:**
```javascript
db.products.find().sort({ price: -1 })
```

**Exercise 4.3 Solution:**
```javascript
db.products.find()
  .sort({ price: -1 })
  .limit(5)
```

**Exercise 4.4 Solution:**
```javascript
db.users.find()
  .skip(2)
  .limit(2)
```

---

### Intermediate Solutions

**Exercise 5.1 Solution:**
```javascript
db.users.find({
  $or: [
    { "address.city": "New York" },
    { loyaltyPoints: { $gt: 1000 } }
  ]
})
```

**Exercise 5.2 Solution:**
```javascript
db.products.find({
  $and: [
    { featured: true },
    {
      $or: [
        { price: { $lt: 100 } },
        { stock: { $gt: 50 } }
      ]
    }
  ]
})
```

**Exercise 5.3 Solution:**
```javascript
db.products.find({ tags: "bluetooth" })
```

**Exercise 5.4 Solution:**
```javascript
db.users.find({ wishlist: { $size: 2 } })
```

**Exercise 6.1 Solution:**
```javascript
db.users.updateOne(
  { username: "alice_brown" },
  { $inc: { loyaltyPoints: 500 } }
)
```

**Exercise 6.2 Solution:**
```javascript
db.users.updateOne(
  { username: "jane_smith" },
  {
    $push: {
      wishlist: ObjectId("65a1b2c3d4e5f6a7b8c9d0f4")
    }
  }
)
```

**Exercise 6.3 Solution:**
```javascript
db.users.updateOne(
  { username: "john_doe" },
  { $pop: { wishlist: -1 } }
)
```

**Exercise 6.4 Solution:**
```javascript
db.users.updateOne(
  { username: "bob_wilson" },
  { $set: { "address.city": "Boston" } }
)
```

**Exercise 6.5 Solution:**
```javascript
db.products.updateMany(
  { stock: 0 },
  { $set: { status: "out_of_stock" } }
)
```

**Exercise 7.1 Solution:**
```javascript
db.users.aggregate([
  {
    $group: {
      _id: "$role",
      count: { $sum: 1 }
    }
  }
])
```

**Exercise 7.2 Solution:**
```javascript
db.products.aggregate([
  {
    $group: {
      _id: null,
      totalPrice: { $sum: "$price" },
      avgPrice: { $avg: "$price" },
      productCount: { $sum: 1 }
    }
  }
])
```

**Exercise 7.3 Solution:**
```javascript
db.orders.aggregate([
  {
    $group: {
      _id: "$status",
      orderCount: { $sum: 1 },
      totalRevenue: { $sum: "$total" },
      avgOrderValue: { $avg: "$total" }
    }
  }
])
```

---

### Advanced Solutions

**Exercise 8.1 Solution:**
```javascript
db.users.updateMany(
  { loyaltyPoints: { $gt: 1000 } },
  { $set: { role: "vip" } }
)
```

**Exercise 8.2 Solution:**
```javascript
db.products.updateOne(
  { name: "Wireless Bluetooth Headphones" },
  {
    $inc: {
      "reviews.$[elem].rating": 1
    }
  },
  {
    arrayFilters: [{ "elem.rating": { $gte: 4 } }]
  }
)
```

**Exercise 8.3 Solution:**
```javascript
db.products.updateOne(
  { slug: "laptop-stand" },
  {
    $setOnInsert: {
      name: "Laptop Stand",
      price: 39.99,
      category: "Electronics",
      subcategory: "Accessories",
      status: "active",
      featured: false,
      tags: ["laptop", "stand", "accessories"],
      createdAt: new Date()
    },
    $inc: { stock: 10 }
  },
  { upsert: true }
)
```

**Exercise 9.1 Solution:**
```javascript
db.orders.aggregate([
  { $unwind: "$items" },
  {
    $group: {
      _id: "$items.productId",
      productName: { $first: "$items.productName" },
      totalQuantitySold: { $sum: "$items.quantity" },
      totalRevenue: {
        $sum: { $multiply: ["$items.quantity", "$items.price"] }
      }
    }
  },
  { $sort: { totalQuantitySold: -1 } }
])
```

**Exercise 9.2 Solution:**
```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "userInfo"
    }
  },
  {
    $unwind: "$userInfo"
  },
  {
    $project: {
      orderNumber: 1,
      total: 1,
      status: 1,
      "userInfo.username": 1,
      "userInfo.email": 1,
      "userInfo.phone": 1
    }
  }
])
```

**Exercise 9.3 Solution:**
```javascript
db.orders.aggregate([
  { $match: { status: "delivered" } },
  {
    $group: {
      _id: "$userId",
      totalSpent: { $sum: "$total" },
      orderCount: { $sum: 1 }
    }
  },
  { $sort: { totalSpent: -1 } },
  { $limit: 3 },
  {
    $lookup: {
      from: "users",
      localField: "_id",
      foreignField: "_id",
      as: "userInfo"
    }
  },
  {
    $project: {
      username: { $arrayElemAt: ["$userInfo.username", 0] },
      totalSpent: 1,
      orderCount: 1
    }
  }
])
```

---

## Practice Tips

### 1. Start Simple, Build Complex
- Begin with basic queries
- Gradually add filters
- Combine operators
- Test each step

### 2. Use .explain()
```javascript
db.users.find({ age: { $gt: 25 } }).explain("executionStats")
```

### 3. Always Verify Results
```javascript
// Before update
db.users.find({ username: "john_doe" })

// Update
db.users.updateOne(
  { username: "john_doe" },
  { $set: { age: 33 } }
)

// After update - verify
db.users.find({ username: "john_doe" })
```

### 4. Practice with Different Data
- Modify the sample data
- Add your own documents
- Create edge cases
- Test with large datasets

### 5. Learn from Mistakes
- Read error messages carefully
- Understand why queries fail
- Fix and retry
- Document solutions

---

## Next Steps

1. ✅ Complete all beginner exercises
2. ✅ Master intermediate exercises
3. ✅ Tackle advanced challenges
4. ✅ Build a real project using these patterns
5. ✅ Explore aggregation framework deeply
6. ✅ Study indexes and performance optimization

---

**Remember:** MongoDB mastery comes from practice. Don't just read the solutions—type them out, experiment with variations, and understand the "why" behind each approach.

**Keep practicing. Keep improving. Keep building!** 🚀
