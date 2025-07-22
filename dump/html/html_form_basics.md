Forms allow users to submit data to the server.

```html
<form action="process.php" method="post">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" placeholder="Your name" required>

  <label for="email">Email:</label>
  <input type="email" id="email" name="email" placeholder="Your email" required>

  <label for="message">Message:</label>
  <textarea id="message" name="message" rows="4" cols="50" placeholder="Write your message here"></textarea>

  <input type="submit" value="Send">
</form>
```

- **`action`**: Specifies the URL to which the form data will be sent.
- **`method`**: Defines the [[HTTP]] method for sending data (`get` or `post`).