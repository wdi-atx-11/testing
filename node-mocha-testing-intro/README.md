<!-- ---
title: Testing Node with Mocha/Chai
type: lesson
duration: "1:45"
creator:
    name: Gerry Mathe
    city: London
edited by:
    name: Wes Hoover
    city: Austin,TX
competencies: Testing
--- -->
# Intro to Testing

### Objectives
*After this lesson, students will be able to:*

- Describe TDD and the benefits it brings
- Describe the importance of testing your code programmatically
- Write basic test specs to validate attributes working
- Use describe and assertion functions to do basic testing

### Preparation
*Before this lesson, students should already be able to:*

- Explain basic file and folder structure in a simple RESTful node app
<!-- - Run Ruby Scripts
- Explain OOP in Ruby -->


## What is TDD? What is testing? Intro (15 mins)

#### What is testing?

Testing is the process of making sure your code does what it's supposed to.

Manual testing, or error-driven development, is just what it sounds like: checking all the code works as expected after you change any source code, including testing your application from your web interface. This is limited by the time you need to test *everything* whenever you change *anything*. The larger the code base gets, the harder it is to check every line and every page every time a change is made.

Automated testing is achieved by writing code that checks your code. This may involve writing some code that plays through scenarios that address various possible input values and what the expected outcome is.

When you write very small tests that check very small sections of classes or models, we'd call that "unit" testing.

As your code base grows, so does you test coverage. You should get to a situation where you can run your test code at any time, and every single line of your code gets passed through to ensure it's still returning what you expected it to when it was first written.

Now, you can try to test after you've developed a product, but here are some problems with that approach:

  - Will you have the time?
  - Will your PM let you do this? Or will you have to begin creating the next bit of functionality?

#### What is TDD? For that matter, what is BDD?

BDD is behavior-driven development. We can utilize TDD within a BDD framework. BDD is a more holistic approach to TDD. With BDD we describe __*behaviors*__ that our app should achieve and we develop from the start with these in mind. This way we have clearly defined goals and standards to hold our app to. The goal of BDD is to have your __whole team__ gaining value from your tests, not just one part of the team (i.e. developers or engineers). BDD makes it easier for your Product Manager to visualize the necessity of TDD, which means more overall efficiency and possibly lower costs in the long run.

> BDD can, and should be, used together with TDD and unit testing methods.

TDD stands for test-driven development. Also called red/green development, in TDD, you write the tests first, before writing any code and then write code that makes the test pass.

The test will initially fail - that's the point of the 'red' - and the expectations of the test will drive how you will write your actual code - this is referred to as your implementation - until the test passes, or goes 'green'.

Frequently, TDD is approached with pair programming - two developers working together at one machine. Often, one person writes a test; then, the other writes the implementation, and they alternate throughout the day. In an interview, you might be given some test code and be asked to write the implementation code; or you might be asked to write the tests for some outline functionality to demonstrate your familiarity with this process.

The process is also referred to as red/green/refactor because once the test passes (and it's "green"), you can review the code you've written and any other parts of the code that's affected to see if it can be cleaned up at all. No new functionality is added at this stage - the desired outcome is still for the tests to pass, just as they had before, but with more efficient code.


<p align="center">
<img src="http://www.pathfindersolns.com/wp-content/uploads/2012/05/red-green-refactorFINAL2.png">
</p>



# Testing Node with Mocha/Chai

## Mocha, Chai And Javascript Testing - Intro (10 min)

We've now created several Express applications.  All these apps cover a single topic, so most of the time, they are quite small.  But when you create a larger application, the codebase will become bigger and more complex every time you add some features.  At some point, adding code in file A will break features in file B, and to avoid these "side-effects", we need to test our app.

To do so in Node, we will use two libraries: one to run the tests and a second one to run the assertions.

Mocha will be our testing framework. From the mocha Website:

_"Mocha is a feature-rich JavaScript test framework running on Node.js and the browser, making asynchronous testing simple and fun. Mocha tests run serially, allowing for flexible and accurate reporting, while mapping uncaught exceptions to the correct test cases."_


For assertions, we will use Chai. From the Chai website:

_"Chai is a BDD / TDD assertion library for node and the browser that can be delightfully paired with any javascript testing framework."_


To be able to make HTTP requests inside tests, we will use a framework called `supertest`.

## Let's Test! Codealong (35 mins)

#### Setting up the app

Take the starter code provided - this is an app we've created when we talked about routing in Express.

To test this app, we need to install a couple of dependencies.

First, let's install mocha, and we will install this package globally using `-g`:

```bash
npm install -g mocha
```

Then we will install chai using --save-dev

```bash
npm install chai --save-dev
```

Last dependency we need to install is supertest:

```bash
npm install supertest --save-dev
```

#### Files and Folders

Now that we're configured, let's set up our file and folder structure. All the tests will be written inside a folder `test` at the root of the app:

```bash
mkdir tests
```

Then we will write the tests inside a file called `candies_tests.js`:

```bash
touch tests/candies_tests.js
```

> Note: Explain that because our tests will request the application through HTTP, students have to make sure they are running the app while running the tests

#### Let's write some tests

Open the file `candies_tests.js`. We now need to require some dependencies at the top of this file:

```javascript
var should    = require("chai").should(),
expect        = require("chai").expect,
supertest     = require("supertest"),
api           = supertest("http://localhost:3000")
```

Make sure you set the url correctly, as this will be used to request the app and analyze the response.

All the tests need to be inside a `describe` function.  We will use one describe block per route:

```javascript
describe("GET /candies", function(){
  //tests will be written inside this function
});
```


First, we will write a test to make sure that a request to the index path `/candies` returns a http status 200:

```javascript
describe("Candies", function(){
  it("should return a 200 response", function(done){
    api.get("/candies")
    .set("Accept", "application/json")
    .expect(200, done)
  })
})
```

> **Note:** Make sure that the done is in the it block and not the describe block!

Now go in the command line and type `mocha`, you should get an output looking like :

![CLI Screenshot](http://s2.postimg.org/htp4cex15/Screen_Shot_2015_08_12_at_12_17_01.png)


This test is passing!

Every block of code that starts with `it()` represents a test.

The `callback` represents a function that Mocha will pass to the code so that the next test will be executed only when the current is finished and the `done` function is called - this allows tests to be executed once at a time.

Now, let's verify the content of the response by looking at the data sent back by hitting the `/candies` endpoint:

```javascript
[{
    "id": 1,
    "name": "Chewing Gum",
    "color": "Red"
}, {
    "id": 2,
    "name": "Pez",
    "color": "Green"
}, {
    "id": 3,
    "name": "Marshmallow",
    "color": "Pink"
}, {
    "id": 4,
    "name": "Candy Stick",
    "color": "Blue"
}]

```

We can write a test that verifies the response is an array:

```javascript
it("should return an array", function(done){
  api
    .get("/candies")
    .set("Accept", "application/json")
    .end(function(error, response){
      expect(response.body).to.be.an('array');
      done()
    })
  })
```

We can write another test that verifies the presence of a field in the response:

```javascript
it("should return an object that have a field called 'name' ", function(done){
  api
    .get("/candies")
    .set("Accept", "application/json")
    .end(function(error, response){
      expect(response.body[0]).to.have.property('name');
      done()
  })
})
```

We can also send data to the server and test the behavior - in our case, we want to make sure that when we post some JSON to `/candies`, a new object is added to the array candies.

Because we are going to test another route, let's add another describe block:

```javascript
describe("POST /candies", function(){

})
```

For this test, we need to:

1. Create a new object by sending a `POST` request
2. Verify that a new object has been "saved" by requesting the index route

For this, we will use `before` blocks. A `before` block will be executed for every `it` function is called inside a `describe` block.

Add this inside the new `describe` block:

```javascript
  before(function(done){
    api.post("/candies")
    .set("Accept", "application/json")
    .send({
      "id": 5,
      "name": "Lollipop",
      "color": "Red"
    }).end(done)
  })
```

This code will be called for every test we will add into the current `describe` block.

Now, we can verify that calling "POST" will add an object to candies:

```javascript
it("should add a candy object to the collection candies and return it", function(done){
  api.get("/candies")
  .set("Accept", "application/json")
  .end(function(error, response){
    expect(response.body.length).to.equal(5);
    done()
  })
})
```

Run the `mocha` command in the terminal, you should now have four passing tests!

#### One caveat: Is the test always right?

Humans write the tests too, so this is not infallible.

What situations can we anticipate that would make our code misbehave?

<!-- - What happens if you try to withdraw more than is in an account?
- What happens if you try to deposit money into a non-existent account?
- Or if you try to withdraw from a non-existent account? -->


## Independent Practice (20 minutes)

> ***Note:*** _This can be a pair programming activity or done independently._

Add tests to the suite:

1. Write a test that make sure the object returned when you call show with a specific ID contains the right fields.
2. Write a test that ensure an object is deleted from the array candies when you call delete.
3. Write a test that ensure a property is updated when you call `PUT /candies/:id`


## Conclusion (10 mins)
> Review the answers to the tests specs above

We've covered the principles of testing in JavaScript, but Chai offers a lot of different expectations syntaxes. Check the [Chai Documentation](http://chaijs.com/api/)

- How does Mocha work with Chai to write tests in your JavaScript application?
- Describe how to configure your app to use Mocha and Chai.
