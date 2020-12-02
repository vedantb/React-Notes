## XMLHttpRequest (XHR)

### An example XHR request

The following code creates an XMLHttpRequest (XHR) request object, and attaches a callback function that responds on the `onreadystatechange` event.

The xhr connection is set up to perform a GET request to `http://yoursite.com`, and it's started with the `send()` method:

```js
const xhr = new XMLHttpRequest();
xhr.onreadystatechange = () => {
  if (xhr.readyState === 4) {
    xhr.status === 200 ? console.log(xhr.responseText) : console.error("error");
  }
};
xhr.open("GET", "https://yoursite.com");
xhr.send();
```

#### Additional open() parameters

In the example above we just passed the method and the URL to the request.

We can also specify the other HTTP methods - (`get`, `post`, `head`, `put`, `delete`, `options`)

Other parameters let you specify a flag to make the request synchronous if set to false, and a set of credentials for HTTP authentication:

`open(method, url, asynchronous, username, password)`

#### `onreadystatechange`

The `onreadystatechange` is called multiple times during an XHR request. We explicitly ignore all states other than `readyState === 4`, which means that the request is done.

The states are:

- 1 (OPENED): The request starts
- 2 (HEADERS_RECEIVED): the HTTP headers have been received
- 3 (LOADING): the response begins to download
- 4 (DONE): the response has been downloaded

#### Aborting an XHR request

An XHR request can be aborted by calling the `abort()` method on the `xhr` object.

#### Comparison with JQuery

With jQuery these lines can be translated to:

```js
$.get("https://yoursite.com", (data) => {
  console.log(data);
}).fail((err) => {
  console.error(err);
});
```

#### Comparison with Fetch

With the Fetch API this is equivalent code:

```js
fetch("https://yoursite.com")
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.error(err);
  });
```

#### Cross Domain Requests

Note that an XMLHttpRequest connection is subject to specific limits that are enforced for security reasons.

One of the most obvious is the enforcement of the same origin policy.

You cannot access resources on another server, unless the server explicitly supports this using CORS (Cross Origin Resource Sharing).
