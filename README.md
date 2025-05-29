# 📘 Book management REST API

This is a simple **REST API** to manage a list of books using **Node.js** and **Express**. All data is stored **in memory**, meaning it will reset when the server restarts.

---

## 🚀 Features

- **GET** `/books` – Get all books  
- **POST** `/books` – Add a new book  
- **PUT** `/books/:id` – Update a book by ID  
- **DELETE** `/books/:id` – Delete a book by ID  

---

## 🛠️ Requirements

- Node.js & npm
- Postman or any REST client (for testing)

---

## 📦 Setup Instructions

### 1. Initialize a Node.js project

```
mkdir book-api
cd book-api
npm init -y
```

### 2. Install Express

```
npm install express
```

### 3. Create `index.js`

```js
const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json());

let books = [];
let nextId = 1;

app.get('/', (req, res) => {
  res.send('Welcome to the Book API! Use /books to interact.');
});

app.get('/books', (req, res) => {
  res.json(books);
});

app.post('/books', (req, res) => {
  const { title, author } = req.body;
  const newBook = { id: nextId++, title, author };
  books.push(newBook);
  res.status(201).json(newBook);
});

app.put('/books/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const { title, author } = req.body;
  const book = books.find(b => b.id === id);

  if (book) {
    book.title = title;
    book.author = author;
    res.json(book);
  } else {
    res.status(404).json({ message: 'Book not found' });
  }
});

app.delete('/books/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = books.findIndex(b => b.id === id);

  if (index !== -1) {
    const deletedBook = books.splice(index, 1);
    res.json(deletedBook[0]);
  } else {
    res.status(404).json({ message: 'Book not found' });
  }
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

## ▶️ Running the Server

```
node index.js
```

You should see:

```
Server running on http://localhost:3000
```

---

## 🧪 Testing the API with Postman

### ➕ Add a Book (POST)

- **URL:** `http://localhost:3000/books`
- **Method:** POST
- **Body (JSON):**
```json
{
  "title": "Harry Potter",
  "author": "J.K. Rowling"
}
```

---

### 📚 Get All Books (GET)

- **URL:** `http://localhost:3000/books`
- **Method:** GET

---

### ✏️ Update a Book (PUT)

- **URL:** `http://localhost:3000/books/1`
- **Method:** PUT
- **Body (JSON):**
```json
{
  "title": "Harry Potter and the Chamber of Secrets",
  "author": "J.K. Rowling"
}
```

---

### ❌ Delete a Book (DELETE)

- **URL:** `http://localhost:3000/books/1`
- **Method:** DELETE

---

## 📝 Notes

- This app uses an in-memory array. Data will be lost when the server restarts.
- Use `Content-Type: application/json` header for POST and PUT requests.
