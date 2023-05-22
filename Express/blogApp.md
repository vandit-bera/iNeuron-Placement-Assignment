# A Complete Blog App

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');

const app = express();
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// Connect to MongoDB
mongoose.connect('mongodb://localhost/blog_app', { useNewUrlParser: true, useUnifiedTopology: true });
const db = mongoose.connection;
db.on('error', console.error.bind(console, 'MongoDB connection error:'));

// Define the Blog schema
const blogSchema = new mongoose.Schema({
  title: String,
  content: String,
});

// Define the Blog model
const Blog = mongoose.model('Blog', blogSchema);

// Create a new blog post
app.post('/blogs', (req, res) => {
  const { title, content } = req.body;
  const blog = new Blog({ title, content });
  blog.save((err) => {
    if (err) {
      res.status(500).send('Error saving blog post');
    } else {
      res.status(201).json(blog);
    }
  });
});

// Get all blog posts
app.get('/blogs', (req, res) => {
  Blog.find({}, (err, blogs) => {
    if (err) {
      res.status(500).send('Error retrieving blog posts');
    } else {
      res.status(200).json(blogs);
    }
  });
});

// Get a single blog post by ID
app.get('/blogs/:id', (req, res) => {
  const { id } = req.params;
  Blog.findById(id, (err, blog) => {
    if (err) {
      res.status(500).send('Error retrieving blog post');
    } else if (!blog) {
      res.status(404).send('Blog post not found');
    } else {
      res.status(200).json(blog);
    }
  });
});

// Update a blog post by ID
app.put('/blogs/:id', (req, res) => {
  const { id } = req.params;
  const { title, content } = req.body;
  Blog.findByIdAndUpdate(id, { title, content }, { new: true }, (err, blog) => {
    if (err) {
      res.status(500).send('Error updating blog post');
    } else if (!blog) {
      res.status(404).send('Blog post not found');
    } else {
      res.status(200).json(blog);
    }
  });
});

// Delete a blog post by ID
app.delete('/blogs/:id', (req, res) => {
  const { id } = req.params;
  Blog.findByIdAndDelete(id, (err, blog) => {
    if (err) {
      res.status(500).send('Error deleting blog post');
    } else if (!blog) {
      res.status(404).send('Blog post not found');
    } else {
      res.status(200).send('Blog post deleted');
    }
  });
});

// Start the server
const port = 3000;
app.listen(port, () => {
  console.log(`Server is listening on port ${port}`);
});

```

> In this example, we use Mongoose as the ODM (Object Data Modeling) library for MongoDB. We define a Blog model using the blogSchema and set up the necessary routes for each CRUD operation.
>- The POST /blogs endpoint allows users to create a new blog post by sending a JSON payload with title and content properties.  
>- The GET /blogs endpoint retrieves all blog posts.
>- The GET /blogs/:id endpoint retrieves a single blog post by its ID.
>- The PUT /blogs/:id endpoint updates a blog post by its ID, with the new title and content provided in the request body.
>- The DELETE /blogs/:id endpoint deletes a blog post by its ID.