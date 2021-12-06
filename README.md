# Intro to REST

## Lesson Objectives

1. Describe REST and list the various routes
1. Create an Index route
1. Install JSONView to make viewing JSON easier
1. Create a Show route
1. Enhance the data in your data array

## URI Patterns

We want to build maintainable, scalable applications.

To do that, we want to have a plan on how we name our URLs.

Let's say we want to make an app that tracks bookmarks for us.

We want to be able to

- Create a bookmark
- See a list of our bookmarks
- See the details of one bookmark
- Update a bookmark
- Delete a bookmark

Let's take ~5 minutes to answer:
How should we organize our URLs?

## Good URIs shouldn't change

When you provide a link, it should always work.

[resource](https://www.w3.org/Provider/Style/URI)

Some URI Pain points

- version 2.0, reorganization to make the app better (while the app should improve, the URIs should be able to stay constant, if they were designed well)
- confidential/valid/up to date/out of date - how to organize?
- moving files - you should be able to move files without changing the URI
- making the name too specific/have too many details ie putting `/jane` in the URL because she worked on it. Then John takes over part of Jane's work. Yikes.

## Describe REST and list the various routes

Lucky for us, some brilliant minds have developed a URI pattern that solves a lot of the above problems. If we follow the pattern, we can alleviate a lot of pain points.

- REST stands for Representational state transfer
- It's just a set of principles that describe how networked resources are accessed and manipulated
- We have [7 RESTful routes](https://gist.github.com/alexpchin/09939db6f81d654af06b) that allow us basic operations for reading and manipulating a collection of data:

| **URL**          | **HTTP Verb** | **Action** |
| ---------------- | ------------- | ---------- |
| /photos/         | GET           | index      |
| /photos/:id      | GET           | show       |
| /photos/new      | GET           | new        |
| /photos          | POST          | create     |
| /photos/:id/edit | GET           | edit       |
| /photos/:id      | PATCH/PUT     | update     |
| /photos/:id      | DELETE        | destroy    |

## Create an Index route

## Setup our app

1. `mkdir fruits`
1. `cd fruits`
1. touch `server.js`
1. `npm init -y`
1. `npm install express`
1. require express and set up a basic server that logs listening when you start the app
1. start the app with nodemon and make sure it is working

Let's have a set of resources which is just a javascript array. To create an **index** (index = like table of contents, lists all the items) route, we'd do the following:

```javascript
// DEPENDENCIES
const express = require('express')
// CONFIGURATION
const app = express()
const PORT = 3000

// 'DATA'
const fruits = ['apple', 'banana', 'pear']

// ROUTES
// index
app.get('/fruits/', (req, res) => {
  res.send(fruits)
})

// Listener
app.listen(PORT, () => {
  console.log('listening on port', PORT)
})
```

Now go to http://localhost:3000/fruits/

Success?
![](https://i.imgur.com/v2sfviM.png)

**Thought question:** What happens when you go to http://localhost:3000 ?
Why?

## Install JSON Formatter to make viewing JSON easier

- JSON stands for Javascript Object Notation
- It's just a way to represent data that looks like a Javascript object or array
- JSON Formatter extension just makes it easier to view JSON data.

Install it:

1.  Go to https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa
1.  Click on "Add To Chrome"

## Create a Show route

To create a show route, we'd do this:

```javascript
// DEPENDENCIES
const express = require('express')

// CONFIGURATION
const app = express()
const PORT = 3000

// 'DATA'
const fruits = ['apple', 'banana', 'pear']

// ROUTES
// index route
app.get('/fruits/', (req, res) => {
  res.send(fruits)
})

//add show route
app.get('/fruits/:indexOfFruitsArray', (req, res) => {
  res.send(fruits[req.params.indexOfFruitsArray])
})

app.listen(PORT, () => {
  console.log('listening on port', PORT)
})
```

Now go to http://localhost:3000/fruits/1

## Enhance the data in your data array

- Right now are data array `fruits` is just an array of strings
- We can store anything in the array, though.
- Let's enhance our data a bit:

```javascript
// DEPENDENCIES
const express = require('express')

// CONFIGURATION
const app = express()
const PORT = 3000

// 'DATA'
const fruits = [
  {
    name: 'apple',
    color: 'red',
    readyToEat: true
  },
  {
    name: 'pear',
    color: 'green',
    readyToEat: false
  },
  {
    name: 'banana',
    color: 'yellow',
    readyToEat: true
  }
]

// ROUTES
// index
app.get('/fruits/', (req, res) => {
  res.send(fruits)
})

// show
app.get('/fruits/:indexOfFruitsArray', (req, res) => {
  res.send(fruits[req.params.indexOfFruitsArray])
})

// Listener
app.listen(PORT, () => {
  console.log('listening on port', PORT)
})
```


Next, we want to be able to create a new fruit. Let's review our 7 restful routes:


|#|Action|URL|HTTP Verb|EJS view filename|
|:---:|:---:|:---:|:---:|:---:|
|1| Index | /fruits/ | GET | index.ejs |
|2| Show | /fruits/:index | GET | show.ejs |
|3| **New** | **/fruits/new**| **GET** | **new.ejs** |
|4| Create | /fruits/ | POST| none |
|5| Edit ||||
|6| Update ||||
|7| Destroy |||||

## Create a new route and page

1. Let's create a page that will allow us to create a new fruit using a form
1. First, we'll need a route for displaying the page in our server.js file **IMPORTANT: put this above your show route, so that the show route doesn't accidentally pick up a /fruits/new request**

    ```javascript
    //put this above your show.ejs file
    app.get('/fruits/new', (req, res) => {
        res.render('new.ejs');
    });
    ```

1. Now lets's create the html for this page in our /views/new.ejs file

- `touch views/new.ejs`

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>New Form</title>
  </head>
  <body>

  </body>
</html>
```


1. Visit http://localhost:3000/fruits/new to see if it works

## Add interactivity to your site with forms

We can use forms to allow the user to enter their own data:


Breaking down the parts of a form

- `<form>` - this encompasses all the elements in a form
  - `action` where should this form be sent (for us it will be the relative path `/fruits`)
  - `method` - this will be a `POST` route, which is in line for our 7 RESTful routes pattern for creating a new fruit
- `<label>` - this is for visual formatting and web accessibility
  - `for` attribute that should match `id` in the companion `input` - again for web accessibility
- `<input />` - a self closing tag
  - `type` we'll use `text`, `checkbox` and `submit` for this project but there are many more like, `number`, `password`... you can always google for a more exhaustive list
  - `name` - this field **MUST** match your key value for your incoming data. This gets parsed as the key with `body-parser`


```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>
      <h1>New Fruit Page</h1>
      <form action="/fruits" method="POST">
        <label for="name">Name</label>
        <input type="text" name="name" id="name"/>
        <label for="color">Color</label>
        <input type="text" name="color" id="color" />
        <label for="isReadyToEat">Is Ready to Eat</label>
        <input type="checkbox" name="readyToEat" id="isReadyToEat" />
        <input type="submit" value="Create Fruit">
      </form>
    </body>
</html>
```

### Polishing

Right now, on successful POST, our data is just rendered as JSON. We should redirect it back to our index page or (bonus figure this out!) to the new show page of our new fruit.

```js
// create
app.post('/fruits', (req, res) => {
  console.log(req.body)
  if (req.body.readyToEat === 'on') { // if checked, req.body.readyToEat is set to 'on'
    req.body.readyToEat = true
  } else { // if not checked, req.body.readyToEat is undefined
    req.body.readyToEat = false
  }
  fruits.push(req.body)
  res.redirect('/fruits')
})

```

Put a link in the index page going to the new page

```html
<nav>
    <a href="/fruits/new">Create a New Fruit</a>
</nav>
```

## Create a static files folder for CSS/JS

- CSS/JS code doesn't change with server-side data
- We can toss any static files into a 'public' directory
    - static means unchanging
    - dynamic means changing depending on data

Let's set up a directory for our static code:

1. Create a directory called `public`
1. Inside the `public` directory create a directory called `css`
1. Inside the `css` directory, create an `app.css` file
1. Put some CSS in the `app.css` file
1. Inside server.js place the following near the top:

    ```javascript
    app.use(express.static('public')); //tells express to try to match requests with files in the directory called 'public'
    ```

1. In your html, you can now call that css file

    ```html
    <link rel="stylesheet" href="/css/app.css">    
    ```

Let's try some CSS

```css
@import url('https://fonts.googleapis.com/css?family=Comfortaa|Righteous');

body {
  background: url(https://images.clipartlogo.com/files/istock/previews/8741/87414357-apple-seamless-pastel-colors-pattern-fruits-texture-background.jpg);
  margin: 0;
  font-family: 'Comfortaa', cursive;
}

h1 {
  font-family: 'Righteous', cursive;
  background: antiquewhite;
  margin:0;
  margin-bottom: .5em;
  padding: 1em;
  text-align: center;
}

a {
  color: orange;
  text-decoration: none;
  text-shadow: 1px 1px 1px black;
  font-size: 1.5em;
  background: rgba(193, 235, 187, .9);
  /* padding: .25em;
  margin: .5em; */

}

a:hover {
  color: ghostwhite;
}

li {
  list-style: none;
}

li a {
  color: mediumseagreen;
}

input[type=text] {
  padding: .3em;
}

input[type=submit] {
  padding: .3em;
  color: orange;
  background: mediumseagreen;
  font-size: 1em;
  border-radius: 10%;
}

```

## Delete


|#|Action|URL|HTTP Verb|EJS view filename|
|:---:|:---:|:---:|:---:|:---:|
|1| Index | /fruits/ | GET | index.ejs |
|2| Show | /fruits/:index | GET | show.ejs |
|3| New | /fruits/new| GET | new.ejs |
|4| Create | /fruits/ | POST| none |
|5| Edit ||||
|6| Update ||||
|7| **Destroy** |**/fruits/:index**|**DELETE**|**none**||



### Create a Delete Route

Inside our server.js file, add a DELETE route:

```javascript
app.delete('/fruits/:index', (req, res) => {
	fruits.splice(req.params.index, 1); //remove the item from the array
	res.redirect('/fruits');  //redirect back to index route
});
```

Test it using:

```
curl -X DELETE localhost:3000/fruits/1
```

See our `index.ejs` has only two list item fruits, apple, banana
```
curl localhost:3000/fruits/
```

### Make the index page send a DELETE request

Inside our `index.ejs` file, add a form with just a delete button.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Index of Fruits</title>
    <link rel="stylesheet" href="main.css">
  </head>
  <body>
    <h1>Index of Fruits</h1>
    <nav>
      <a href="/fruits/new">Create a New Fruit</a>
    </nav>
    <ul>
    <% fruits.forEach((fruit, index) => { %>
      <li>
				<a href="/fruits/<%=index%>"> <%= fruit.name %></a>
				<!--  ADD DELETE FORM HERE-->
				<form>
					<input type="submit" value="DELETE"/>
				</form>
			</li>
    <% }) %>
    </ul>
  </body>
</html>
```

When we click "DELETE" on our index page (`index.ejs`), the form needs to make a DELETE request to our DELETE route.

The problem is that forms can't make DELETE requests.  Only POST and GET.  We can fake this, though.  First we need to install an npm package called `method-override`

```
npm install method-override
```

Now, in our server.js file, add:

```javascript
//include the method-override package
const methodOverride = require('method-override');
//...
//after app has been defined
//use methodOverride.  We'll be adding a query parameter to our delete form named _method
app.use(methodOverride('_method'));
```

Now go back and set up our delete form to send a DELETE request to the appropriate route

```html
<form action="/fruits/<%= index %>?_method=DELETE" method="POST">
```

## Edit


|#|Action|URL|HTTP Verb|EJS view filename|
|:---:|:---:|:---:|:---:|:---:|
|1| Index | /fruits/ | GET | index.ejs |
|2| Show | /fruits/:index | GET | show.ejs |
|3| New | /fruits/new| GET | new.ejs |
|4| Create | /fruits/ | POST| none |
|5| **Edit** |**/fruits/:id**|**GET**|**edit.ejs**|
|6| **Update** |**/fruits**|**PUT**|**none**|
|7| Destroy | /fruits/:index | DELETE |none |



## Update

### Create an update route

In order to UPDATE, we use the http verb PUT.

Inside server.js add the following:

```javascript
app.put('/fruits/:index', (req, res) => { // :index is the index of our fruits array that we want to change
	if(req.body.readyToEat === 'on'){ //if checked, req.body.readyToEat is set to 'on'
		req.body.readyToEat = true
	} else { //if not checked, req.body.readyToEat is undefined
		req.body.readyToEat = false
	}
	fruits[req.params.index] = req.body //in our fruits array, find the index that is specified in the url (:index).  Set that element to the value of req.body (the input data)
	res.redirect('/fruits'); //redirect to the index page
})
```

Test with cURL

```
curl -X PUT -d name="tomato" -d color="red" localhost:3000/2
```

```
curl localhost:3000/fruits
```

Our last fruit (banana) should now be a tomato


### Create an edit route

In our `server.js`, create a GET route which will just display an edit form for a single fruit

```javascript
app.get('/fruits/:index/edit', (req, res)=>{
	res.render(
		'edit.ejs', //render views/edit.ejs
		{ //pass in an object that contains
			fruit: fruits[req.params.index], //the fruit object
			index: req.params.index //... and its index in the array
		}
	)
})
```

Now let's grab our create form  and update it for editing in `views/edit.ejs`

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>
      <h1>Edit Fruit Page</h1>
      <form action="/fruits" method="POST">
        <label for="name">Name</label>
        <input type="text" name="name" id="name"/>
        <label for="color">Color</label>
        <input type="text" name="color" id="color" />
        <label for="isReadyToEat">Is Ready to Eat</label>
        <input type="checkbox" name="readyToEat" id="isReadyToEat" />
        <input type="submit" value="Edit Fruit">
      </form>
    </body>
</html>
```

### Create a link to the edit route

Inside our `index.ejs` file, add a link to our edit route which passes in the index of that item in the url

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Index of Fruits</title>
    <link rel="stylesheet" href="main.css">
  </head>
  <body>
    <h1>Index of Fruits</h1>
    <nav>
      <a href="/fruits/new">Create a New Fruit</a>
    </nav>
    <ul>
    <% fruits.forEach((fruit, index) => { %>
      <li>
        <a href="/fruits/<%=index%>"> <%= fruit.name %></a>
        <form action="/fruits/<%= index %>?_method=DELETE" method="POST">
          <input type="submit" value="DELETE"/>
        </form>
        <a href="/fruits/<%=index %>/edit">Edit</a>
      </li>
    <% }) %>
    </ul>
  </body>
</html>
```

### Make the edit page send a PUT request

When we click "Submit Changes" on our edit page (edit.ejs), the form needs to make a PUT request to our update route

```html

<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Edit a Fruit</title>
    </head>
    <body>
      <h1>Edit Fruit Page</h1>
      <form action="/fruits/<%=index%>?_method=PUT" method="POST">
        <label for="name">Name</label>
        <input type="text" name="name" id="name"/>
        <label for="color">Color</label>
        <input type="text" name="color" id="color" />
        <label for="isReadyToEat">Is Ready to Eat</label>
        <input type="checkbox" name="readyToEat" id="isReadyToEat" />
        <input type="submit" value="Edit Fruit">
      </form>
    </body>
</html>
```

What is frustrating, is that our users have to remember which fruit they clicked on and update/reenter all the values.

We should at least set values in the form for the user to update

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Edit a Fruit</title>
    </head>
    <body>
      <h1>Edit Fruit Page</h1>
      <form action="/fruits/<%=index%>?_method=PUT" method="POST">
        <label for="name">Name</label>
        <input type="text" name="name" id="name" value="<%=fruit.name%>"/>
        <label for="color">Color</label>
        <input type="text" name="color" id="color" value="<%=fruit.color%>"/>
        <label for="isReadyToEat">Is Ready to Eat</label>
        <input type="checkbox" name="readyToEat" id="isReadyToEat"
        <% if(fruit.readyToEat === true){ %>
  				checked
  			<% } %>
        />
        <input type="submit" value="Edit Fruit">
      </form>
    </body>
</html>
```
