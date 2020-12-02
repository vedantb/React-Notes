## AXIOS

Axios is a very popular JavaScript library you can use to perform HTTP requests, that works in both Browser and Node.js platforms

### Introduction to Axios

Axios is a popular JS library you can use to perform HTTP requests, that works in both Browser and Node.js platforms.

It supports all modern browsers, including support for IE8 and higher.

It is promise-based, and this lets us write async/await code to perform [XHR](XMLHTTPRequest.md) requests very easily.

Using Axios has quite a few advantages over the native Fetch API:

- supports older browsers (Fetch needs a polyfill)
- has a way to abort the request
- has a way to set a response timeout
- has built-in CSRF protection
- supports upload progress
- performs automatic JSON data transformation
- works in Node.js

#### Installation

`npm install axios`

or

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

Remembering that API calls must enable CORS to be accessed inside the browser, otherwise the request will fail.

#### The AXIOS API

You can start an HTTP request from the `axios` object:

```js
axios({
  url: "https://dog.ceo/api/breeds/list/all",
  method: "get"
});
```

This returns a promise. You can use async/await to resolve that promise to the response object:

```js
(async () => {
  const response = await axios({
    url: "https://dog.ceo/api/breeds/list/all",
    method: "get"
  });

  console.log(response);
})();
```

For convenience, you will generally use the methods:

- `axios.get()`
- `axios.post()`

They offer a simpler syntax. For example:

```js
(async () => {
  const response = await axios.get("https://dog.ceo/api/breeds/list/all");
  console.log(response);
})();
```

Axios offers methods for all the HTTP verbs, which are less popular but still used:

- `axios.delete()`
- `axios.put()`
- `axios.patch()`
- `axios.options()`

and a method to get the HTTP headers of a request, discarding the body, `axios.head()`.

### GET Requests

This Node.js example queries the [Dog API](https://dog.ceo/) to retrieve a list of all the dogs breeds, using axios.get(), and it counts them:

```js
const axios = require("axios");

const getBreeds = async () => {
  try {
    return await axios.get("https://dog.ceo/api/breeds/list/all");
  } catch (error) {
    console.error(error);
  }
};

const countBreeds = async () => {
  const breeds = await getBreeds();

  if (breeds.data.message) {
    console.log(`Got ${Object.entries(breeds.data.message).length} breeds`);
  }
};

countBreeds();
```

If you don’t want to use async/await you can use the Promises syntax:

```js
const axios = require("axios");

const getBreeds = () => {
  try {
    return axios.get("https://dog.ceo/api/breeds/list/all");
  } catch (error) {
    console.error(error);
  }
};

const countBreeds = async () => {
  const breeds = getBreeds()
    .then((response) => {
      if (response.data.message) {
        console.log(`Got ${Object.entries(response.data.message).length} breeds`);
      }
    })
    .catch((error) => {
      console.log(error);
    });
};

countBreeds();
```

#### Add parameters to GET requests

A GET response can contain parameters in the URL, like this: `https://site.com/?name=Jack`

With Axios you can perform this by using that URL:
`axios.get('https://site.com/?name=Jack')`

or you can use a `params` property in the options:

```js
axios.get("https://site.com/", {
  params: {
    name: "Jack"
  }
});
```

### POST Requests

Performing a POST request is just like doing a GET request, but instead of `axios.get`, you use `axios.post`:
`axios.post('https://site.com/')`

An object containing the POST parameters is the second argument:

```js
axios.post("https://site.com/", {
  name: "Jack"
});
```
