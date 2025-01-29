### 📚 Neo4j Book Recommendation System using Cypher

## 📌 Overview
This project implements a **book recommendation system** using **Neo4j** and **Cypher queries**. It leverages **graph-based relationships** between users, books, genres, and ratings to provide personalized book recommendations.

## 🏗️ Key Features
- **Graph-Based Recommendations**: Uses relationships between users, books, and genres.
- **Collaborative Filtering**: Recommends books based on users with similar interests.
- **Content-Based Filtering**: Suggests books based on genre preferences.
- **Efficient Querying**: Uses **Cypher** to retrieve recommendations quickly.

---

## ⚙️ Tech Stack
- **Database**: `Neo4j`
- **Query Language**: `Cypher`
- **Graph Model**:
  - `(:User {id, name})`
  - `(:Book {id, title, genre})`
  - `(:Genre {name})`
  - `(:Rating {score})`
  - Relationships:
    - `(:User)-[:READ]->(:Book)`
    - `(:User)-[:LIKES]->(:Genre)`
    - `(:Book)-[:BELONGS_TO]->(:Genre)`
    - `(:User)-[:RATED]->(:Book)`

---

## 🚀 Sample Dataset (Cypher Queries)
```cypher
CREATE (u1:User {id: 1, name: "Alice"})
CREATE (u2:User {id: 2, name: "Bob"})
CREATE (u3:User {id: 3, name: "Charlie"})

CREATE (b1:Book {id: 101, title: "1984", genre: "Dystopian"})
CREATE (b2:Book {id: 102, title: "Brave New World", genre: "Dystopian"})
CREATE (b3:Book {id: 103, title: "The Hobbit", genre: "Fantasy"})
CREATE (b4:Book {id: 104, title: "The Catcher in the Rye", genre: "Classic"})

CREATE (g1:Genre {name: "Dystopian"})
CREATE (g2:Genre {name: "Fantasy"})
CREATE (g3:Genre {name: "Classic"})

MERGE (u1)-[:READ]->(b1)
MERGE (u1)-[:RATED {score: 5}]->(b1)
MERGE (u1)-[:LIKES]->(g1)

MERGE (u2)-[:READ]->(b2)
MERGE (u2)-[:RATED {score: 4}]->(b2)
MERGE (u2)-[:LIKES]->(g1)

MERGE (u3)-[:READ]->(b3)
MERGE (u3)-[:RATED {score: 5}]->(b3)
MERGE (u3)-[:LIKES]->(g2)
```

---

## 🔍 Recommendation Queries

### 📌 1️⃣ Get Books Liked by Users with Similar Interests
```cypher
MATCH (u:User {id: 1})-[:LIKES]->(g:Genre)<-[:BELONGS_TO]-(b:Book)
WHERE NOT (u)-[:READ]->(b)
RETURN b.title AS RecommendedBooks
```
🔹 **Recommends books** in the same genre the user likes but hasn’t read yet.

---

### 📌 2️⃣ Collaborative Filtering - Books Read by Similar Users
```cypher
MATCH (u1:User {id: 1})-[:READ]->(b:Book)<-[:READ]-(u2:User)
WHERE u1 <> u2
MATCH (u2)-[:READ]->(rec:Book)
WHERE NOT (u1)-[:READ]->(rec)
RETURN rec.title AS RecommendedBooks, COUNT(*) AS Score
ORDER BY Score DESC
LIMIT 5
```
🔹 **Recommends books** that users with similar reading habits have read.

---

### 📌 3️⃣ Top-Rated Books in a Genre the User Likes
```cypher
MATCH (u:User {id: 1})-[:LIKES]->(g:Genre)<-[:BELONGS_TO]-(b:Book)<-[:RATED]-(otherUsers)
WITH b, AVG(otherUsers.score) AS avgRating
RETURN b.title AS RecommendedBooks, avgRating
ORDER BY avgRating DESC
LIMIT 5
```
🔹 **Suggests highly-rated books** in the genres the user likes.

---

## 🏗️ Setup & Usage

### 🔹 1️⃣ Install & Run Neo4j
- Download **Neo4j Community Edition** from [Neo4j Download Page](https://neo4j.com/download/).
- Start the Neo4j database.

### 🔹 2️⃣ Load the Dataset
Run the dataset queries in **Neo4j Browser**.

### 🔹 3️⃣ Execute Recommendation Queries
Run any of the **recommendation queries** in the Neo4j **Cypher Query Console**.

---

## 📌 Future Enhancements
- **Enhance recommendations** using embeddings or machine learning.
- **Incorporate real-time user interactions** for better personalization.
- **Expand relationships** (e.g., authors, book series, user reviews).

---

🚀 **Graph-Powered Book Recommendations with Neo4j & Cypher!** 📚
