# Simple Server Using Express

```
const express = require('express');
const app = express();

// Endpoint: /post
app.get('/post', (req, res) => {
  // Generate 20 example posts
  const posts = [];
  for (let i = 1; i <= 20; i++) {
    posts.push({ id: i, title: `Post ${i}`, body: `This is the body of Post ${i}` });
  }
  
  // Send the posts as a response
  res.json(posts);
});

// Start the server
const port = 3000;
app.listen(port, () => {
  console.log(`Server is listening on port ${port}`);
});

```

> To use this code, make sure you have Node.js installed. Then follow these steps:
>- Create a new directory for your project.
Open a text editor and paste the code into a new file.
>- Save the file with a .js extension, for example, server.js.
>- Open a terminal and navigate to the project directory.
>- Install Express by running `npm install express`.
>- Run the server by executing `node server.js`.

>Once the server is running, you can access the endpoint by visiting http://localhost:3000/post in your browser or making a GET request to http://localhost:3000/post using tools like cURL or Postman. The server will respond with an array of 20 example posts in JSON format.