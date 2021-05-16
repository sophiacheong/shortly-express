# shortly-express #
Get ready for full-stack app development with Shortly! Shortly is a URL shortener service similar to Bitly - but is only partially finished. Your goal is to build out an authentication system and other features that will enable users to have their own private set of shortened URLs.
> This pair programming sprint was assigned to me when attending Hack Reactor.

While some aspects of the authentication system have been provided, you will need to think about how to approach authentication from both a user perspective and a technical perspective. Some questions to think about:
  1. What additional steps will the user need to take when interacting with the application? More specifically, what additional routes will the application need to handle?
  2. What strategies do I need to employ to secure existing site functionality?
  3. How often should the user need to enter their username + password?

**Exercise repo: `https://github.com/hackreactor/<cohort>-shortly-express`**

## What's in this Repo ##
This repo contains a functional URL shortener designed as a single page app. It's built using [Backbone.js](https://backbonejs.org/) on the client with a Node/Express-based server, which uses [EJS](https://ejs.co/) for server-side templates.

It uses [MySQL](https://www.mysql.com/), an open-source relational database management system (RDBMS).

Server side, the repo also uses [express 4](http://expressjs.com/). There are a few key differences between express 3 and 4, foremost that middleware is no longer included in the express module, but must be installed separately.

Client side, the repo includes libraries like [jQuery](https://jquery.com/), [underscore.js](http://underscorejs.org/) and [Backbone.js](https://backbonejs.org/). Templating on the client is handled via [Handlebars](https://handlebarsjs.com/).

This repo includes some basic server specs using [Mocha](https://mochajs.org/). It is your job to make all of them pass, but feel free to write additional tests to guide yourself. Enter __`npm test`__ to run the tests.

Use [nodemon](https://nodemon.io/) so that the server automatically restarts when you make changes to your files. To see an example, use __`npm start`__, but see if you can improve on this.

## How to use the provided code ##
This repo contains many lines of provided code, and spending too much time reading this code will make it hard for you to complete the sprint. You **should not** need to inspect the client side code to complete the bare minimum requirements. You **will** need to use methods in __`server/models/`__ and __`server/lib/`__. Documentation for these methods has been provided in the __`docs/`__ directory - just open __`docs/index.html`__ in your browser. ___Note: the documentation is generated as part of the `postinstall` npm script - so if you haven't run `npm install` yet, the docs will not be available.___

## Reference Material ##
  * [Express 4 API](http://expressjs.com/en/4x/api.html) 
  * [Sessions and Security](https://guides.rubyonrails.org/security.html) - this is a Rails resource, but it's a really good explanation.
  * [Node crypto Module](https://nodejs.org/api/crypto.html) 
  * [How does a web session work?](https://machinesaredigging.com/2013/10/29/how-does-a-web-session-work/) 
  * [What is a cookie?](https://www.youtube.com/watch?v=I01XMRo2ESg) 
  * [How does cookie-based authentication work?](https://stackoverflow.com/questions/17769011/how-does-cookie-based-authentication-work) 
  * [Beginner's guide to REST](https://code.tutsplus.com/tutorials/a-beginners-guide-to-http-and-rest--net-16340) 
  * [REST and RESTful responses](https://pixelhandler.dev/posts/develop-a-restful-api-using-nodejs-with-express-and-mongoose) 
 
## Your Goals ##

### Bare Minimum Requirements ###
___Build a simple session-based server-side authentication system - from scratch. Use the tests in `test/ServerSpec.js` and the following outline to guide you in your implementation.___

### Usernames and passwords ###
The database table, __`users`__, and its corresponding model have been provided. Use this to store usernames and passwords. The model includes useful methods for encrypting and comparing your passwords.
  - [ ] Add routes to your Express server to process incoming POST requests. These routes should enable a user to register for a new account and for users to log in to your application. Take a look at the __`login.ejs`__ and __`signup.ejs`__ templates in the __`views`__ directory to determine which routes you need to add.
  - [ ] Add the appropriate callback functions to your new routes. Add methods to your user model, as necessary, to keep your code modular (i.e., your database model methods should not receive as arguments or otherwise have access to the request or response objects).

### Sessions and cookies ###
You will be writing two custom middleware functions to configure your Express server. The first will be a cookie parser and the second will be a session generator. Though Express has its own middleware packages named __`cookie-parser`__ and __`express-session`__, you will be building this functionality from scratch (read as: you **should not** be using the __`cookie-parser`__ and __`express-session`__ middleware packages anywhere in your code). The database table, __`sessions`__, has been provided as a place to store your generated session hashes.
  - [ ] In __`middleware/cookieParser.js`__, write a middleware function that will access the cookies on an incoming request, parse them into an object, and assign this object to a __`cookies`__ property on the request.
  - [ ] In __`middleware/auth.js`__, write a __`createSession`__ middleware function that accesses the parsed cookies on the request, looks up the user data related to that session, and assigns an object to a __`session`__ property on the request that contains relevant user information. (Ask yourself: what information about the user would you want to keep in this session object?)
    * Things to keep in mind:
      * An incoming request with no cookies should generate a __`session`__ with a unique hash and store it the sessions database. The middleware function should use this unique hash to set a cookie in the response headers. (Ask yourself: How do I set cookies using Express?).
      * If an incoming request has a cookie, the middleware should verify that the cookie is valid (i.e., it is a session that is stored in your database).
      * If an incoming cookie is not valid, what do you think you should do with that session and cookie?
  - [ ] Mount these two middleware functions in __`app.js`__ so that they are executed for all requests received by your server.

### Authenticated Routes ###
  - [ ] Add a __`verifySession`__ helper function to all server routes that require login, redirect the user to a login page as needed. Require users to log in to see shortened links and create new ones. Do NOT require the user to login when using a previously shortened link.
  - [ ] Give the user a way to log out. What will this need to do to the server session and the cookie saved to the client's browser?

### Tests ###
  - [ ] Write at least 3 new meaningful tests inside of __`test/ServerSpec.js`__
