# Using fetch() - POST and PATCH

## Learning Goals
- Explain how to add new data to a backend using POST
- Explain how to update data on the backend using PATCH
- Use forms to initiate data transfer to the server

## Setup

This is a code-along lesson. Please fork and clone this lesson before continuing.

## Introduction

In the previous lesson, we learned how to using `fetch` to ask our server for data - how to perform a GET request.

In this lesson, we'll tackle the next step in communicating with the server - sending data _from_ the Frontend _to_ the Backend.

There are two key HTTP methods we'll be playing around with in this lesson - `POST` and `PATCH`.

A POST request is used when we want to add entirely new data to our database (in this case, our `db.json` file).

Imagine that you're YouTube, and you need to allow users to post new videos. Or, imagine you're Instagram, and your users want to post new pictures.

In each case, you'll need a way to send that data from the Frontend to the Backend to create a brand new entry in your database. That's what the `POST` HTTP method is for.

We also send data from the Frontend to the Backend with PATCH, but for different reasons. Instead of _creating_ new data, we use PATCH to _update_ existing data. 

Let's say one of our Instagram users wants to update the caption for an image. Instagram will need to run a PATCH request to change that information on its Backend.

We'll dive into talking about POST first!

## Creating New Data - POST Requests

As you may remember, sending a basic `GET` request just involves passing a URL to fetch:

```
fetch('http://localhost:3000/data')
```

A `POST` request is a little bit more complicated. We need to include information telling our server that we're POSTing data, the type of the data we're POSTing, and the actual information we want to POST.

In order to do this, we need to pass fetch a second argument - an object containing all that information.

Here's what that object will look like. We've included a second example object containing data we want added to our server (let's imagine we have a dog-walking application and are entering a new dog in our database).

```
const newDog = {
  breed: "Boston Terrier",
  age: 7,
  name: "Porky"
}

const postObj = {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    Accept: 'application/json'
  }, 
  body: JSON.stringify(newDog)
}
```

In the example above, the `newDog` object represents the data we want to send to our server, and the `postObj` object represents the object that we'll pass to `fetch` as our second argument:

```JavaScript
fetch('http://localhost:3000/dogs', postObj)
```

Let's break down the different pieces of our POST Object

### The POST Object

You'll notice that our postObj has three keys - `method`, `headers`, and `body`.

#### Method
The `method` key references the HTTP method that we're using in the request. Our Backend will look at this HTTP method to determine what it should do with the data that's being sent up from the Frontend. By using the `POST` HTTP header, we're telling our backend that we want it to create a new piece of data in our database.

```JavaScript
method: 'POST' // tells our server how to handling the incoming request
```

#### Headers
The `headers` key includes additional "meta-data" about the request. In this case, our `headers` key references another object, which itself contains two keys.

```JavaScript
headers: {
   'Content-Type': 'application/json',
    Accept: 'application/json'
}
```

The `Content-Type` key tells the server the type of content that's being set up from our frontend to our backend. Note that the `Content-Type` key is wrapped in quotes, as its format means it's a non-standard key.

The `Accept` key specifies the type of data the client will be able to understand when the server sends back its response.

Both of these are set to the value 'application/json', which is a Multipurpose Internet Mail Extention (or [MIME](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)) type.

#### Body

Finally, we have the `body` key, which references the actual data we want to send up!

In order to send this data from our Backend to our Frontend, we first need to convert it into JSON. This is where the `JSON.stringify` method comes in. 

We pass in our data object to the `JSON.stringify` method which receives it and returns a JSON-ified version of that object. (You can think of it as the _opposite_ of the `.json()` method we discussed in the previous lesson.)

```JavaScript
body: JSON.stringify(newDog)
```

### Handling the Response

