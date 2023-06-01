const Book = require('../models/Book');

module.exports = {
  getAllBooks: (req, res) => {
    Book.find()
      .then((books) => {
        res.status(200).json(books);
      })
      .catch((error) => {
        res.status(500).json({ error: 'An error occurred while retrieving the books.' });
      });
  },

  getBookById: (req, res) => {
    const bookId = req.params.id;
    Book.findById(bookId)
      .then((book) => {
        if (!book) {
          return res.status(404).json({ error: 'Book not found.' });
        }
        res.status(200).json(book);
      })
      .catch((error) => {
        res.status(500).json({ error: 'An error occurred while retrieving the book.' });
      });
  },

  createBook: (req, res) => {
    const { title, author, description, publishedYear } = req.body;
    const newBook = new Book({
      title,
      author,
      description,
      publishedYear,
    });
    newBook.save()
      .then((book) => {
        res.status(201).json(book);
      })
      .catch((error) => {
        res.status(500).json({ error: 'An error occurred while creating the book.' });
      });
  },

  updateBook: (req, res) => {
    const bookId = req.params.id;
    const { title, author, description, publishedYear } = req.body;
    Book.findByIdAndUpdate(bookId, { title, author, description, publishedYear }, { new: true })
      .then((book) => {
        if (!book) {
          return res.status(404).json({ error: 'Book not found.' });
        }
        res.status(200).json(book);
      })
      .catch((error) => {
        res.status(500).json({ error: 'An error occurred while updating the book.' });
      });
  },

  deleteBook: (req, res) => {
    const bookId = req.params.id;
    Book.findByIdAndDelete(bookId)
      .then((book) => {
        if (!book) {
          return res.status(404).json({ error: 'Book not found.' });
        }
        res.status(200).json({ message: 'Book deleted successfully.' });
      })
      .catch((error) => {
        res.status(500).json({ error: 'An error occurred while deleting the book.' });
      });
  },
};
