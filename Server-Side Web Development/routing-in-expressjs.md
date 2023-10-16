---
tags:
  - server-side-web-development
  - express
  - tips-and-tricks
  - nodejs
---
# Routing in express.js

routing in `express` can be tricky to understand at first, and while `express`' default template certainly demonstrates this functionality through the `Router` and `express.use()`, it leaves a lot to be desired in explanation! this guide should hopefully cover that.

in `express`, you can call `app.get()`, `app.post()`, etc. to provide an endpoint for your web application (often these methods contain a callback function which simply calls `res.render()`).

a `Router` is handy middleware provided by `express` which only contains these functions. we can then mount these to a route in our app with `app.use("/", someRouter)`!

let's look at an example:

```js
// app.js
const express = require('express');
const app = express();

const exampleRouter = require('./routes/example');

// mount `exampleRouter` and all its routes to "/example"
app.use("/example", exampleRouter);
```

in this example, we import our example router using `require('.routes/example')`. we then mount this router to the `/example` route using `app.use()`. the mount point is important, as every request made to the example router must now be prefixed with `/example`.

let's see how the router is implemented:

```js
// routes/example.js
const express = require('express');
const router = express.Router();

app.get("/hello", (req, res) => {
  // any time someone visits /example/hello, they will be sent "hello world!"
  res.send("<h1>hello world!</h1>");
};

// "for everything that `require`s bookings.js, give them this router"
module.exports = router;
```

since we mounted this example router to `/example` in `app.js`, this `GET` endpoint will now map to `/example/hello`. if we mounted it to `/`, this would map to `/hello`. mounting to `/world`, maps this to `/world/hello`, and so on!