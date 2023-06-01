const express = require('express');
const mongoose = require('mongoose');
const booksRouter = require('./routes/books');

const app = express();
const port = 3000; // or any other port you prefer

// Connect to MongoDB
mongoose.connect('mongodb://localhost/bookstore', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
  .then(() => console.log('Connected to MongoDB'))
  .catch((error) => console.error('Failed to connect to MongoDB:', error));

// Middleware
app.use(express.json());

// Routes
app.use('/books', booksRouter);

// Start the server
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
