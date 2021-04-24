# nodejsguide
Source: https://gist.github.com/anotheruiguy/10674846


$ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"

$ brew install node

$ node --version

brew update
brew doctor
brew upgrade node

$ curl https://www.npmjs.org/install.sh | sh

npm --version

Telling your system where Node is
This is very important. Depending on how you have your Bash set up, you may want to put this into your .bashrc or bash_profile. If you do not have a .bashrc or bash_profile, it is ok to just create them. If Bash is totally new to you, I'd suggest reading life-inside-terminal first.

You need tell Bash where Node is by updating the $PATH variable. In your file you may have the following:

export PATH=/usr/local/bin:$PATH
Basically, you need to point Bash to where the code for Node lives.

$HOME/local/node/bin
When you have existing paths in the $PATH variable, adding a new path is delineated by the : character. Adding the Node path would possibly look like the following:

export PATH=$HOME/local/node/bin:/usr/local/bin:$PATH
But remember, your environment may be different depending on how you install all the things. For example, my $PATH looks like this, Node and NPM is installed in my /usr/local/bin path.

PATH=/usr/local/bin:/usr/bin:/bin:/usr/local/bin/npm:$PATH
If you are installing Node for the first time and things like node --version returns an error, be sure to validate where Node was installed and make sure that you have PATH updates correctly.



## Express.js

> express web application framework for node

[Getting started](http://expressjs.com/guide.html) with Express is pretty exhaustive. No doubt, a great read. But I am going to try and boiled this down into the basic steps to get Node and Express up and running. 

To get Express installed, now that Node and NPM is installed, we can use it's magical package install solutions. Because we will want Express available as an executable from everywhere within our environment, pass in the `-g` flag for 'global'. 

	$ npm install -g express
	
Depending on how you installed Node/Express, you may or may not have permission to install something globally. If you encounter errors during the install or after, you may need to use the `sudo` command: 

	$ sudo npm install -g express
	
Using `sudo`, your computer will ask for your account password. 

### Using Express

Now that Express is installed, using it is very simple. You can create a project just about anywhere on your computer. For simplicity sake, I am going to crate my new app on my Desktop. 

To generate our first Node/Express app called `donuts`, we will run the following:

	$ express donuts
	
There are other flags you can pass in to alter the setup, but for now we will go with the defaults. 
	
GREAT! You just created your first app! Go celebrate! In your terminal, you should see the following output:

	14:34:33: Desktop$ express donuts
	
	   create : donuts
	   create : donuts/package.json
	   create : donuts/app.js
	   create : donuts/public
	   create : donuts/public/javascripts
	   create : donuts/public/images
	   create : donuts/public/stylesheets
	   create : donuts/public/stylesheets/style.css
	   create : donuts/routes
	   create : donuts/routes/index.js
	   create : donuts/routes/user.js
	   create : donuts/views
	   create : donuts/views/layout.jade
	   create : donuts/views/index.jade
	
	   install dependencies:
	     $ cd donuts && npm install
	
	   run the app:
	     $ node app
	
	14:34:37: Desktop$ 
	
In the last few lines, there are some instructions. The fist one we need to run is the command to install your new app's dependencies. Running `npm install` inside the dir of your new project folder of `donuts` will execute the `package.json` file. If you were to open that up, you would see the following: 

	{
	  "name": "application-name",
	  "version": "0.0.1",
	  "private": true,
	  "scripts": {
	    "start": "node app.js"
	  },
	  "dependencies": {
	    "express": "3.4.4",
	    "jade": "*"
	  }
	}
	
Pretty simpleton read and understand. The only thing I am going to point out is that by default Express uses [Jade](http://jade-lang.com/) as it's templating language. There are other templating languages you can use, but I highly suggest using Jade.

Notice that `express` is a dependency even though we have Express installed globally? The reason is simple. Now that we did use Express to build the app, wherever the app goes, Express needs to be installed too. Wherever it gets deployed too or if someone clones your project. If they don't have Express installed, running `npm install` will make them install it and then your app will run. 

Ok, let's do what it said. You can either run the `cd donuts && npm install` as instructed, or do each step independently. It doesn't matter. 

Once you run that command, you should see a whole lot of good things happen. 
	
	npm http GET https://registry.npmjs.org/express/3.4.4
	npm http GET https://registry.npmjs.org/jade
	npm http 200 https://registry.npmjs.org/express/3.4.4
	npm http GET https://registry.npmjs.org/express/-/express-3.4.4.tgz
	npm http 200 https://registry.npmjs.org/express/-/express-3.4.4.tgz
	npm http 200 https://registry.npmjs.org/jade
	npm http GET https://registry.npmjs.org/jade/-/jade-1.3.1.tgz
	npm http 200 https://registry.npmjs.org/jade/-/jade-1.3.1.tgz
	...
	
GREAT! Looking in your new dir, you should see the following:

	|-app.js	
	|-package.json	
	|-routes
	|-node_modules
	|-public
	|-views

If you are interested, you can also run `npm ls` to see a tree of all your dependencies. 

	├─┬ express@3.4.4
	│ ├── buffer-crc32@0.2.1
	│ ├─┬ commander@1.3.2
	│ │ └── keypress@0.1.0
	│ ├─┬ connect@2.11.0
	│ │ ├── bytes@0.2.1
	│ │ ├── methods@0.0.1
	│ │ ├─┬ multiparty@2.2.0
	...
	
### Run your new app

At this point you have the bare bones for a new Node/Express app. To get things up and running, you simply need to run:

	$ node app.js
	
You should see:

	Express server listening on port 3000
	
To see your new website, go to:

	http://localhost:3000
	
### Is there an error?

At the time of writing this, the default layout template installed using Jade has a bug in it. 

Line 1 of the file has `doctype 5`. This is depreciated. You need to open the `./views/layout.jade` file and edit line 1 to be `doctype html`.


## What's in app.js?

Open the `app.js` file and I am going to quickly talk about a few things. 

The following lines are some boilerplate things, you can delete them.

	var user = require('./routes/user');

	app.get('/users', user.list);

The following are things that establish how your app is working:

	var express = require('express');   // framework
	var routes = require('./routes');   // where routes will be defined
	var http = require('http');         // protocol 
	var path = require('path');         // node module to resolve paths

The following is what sets the port of your local app, feel free to change that if needed. 

	app.set('port', process.env.PORT || 3000);
	
This line is what turns on the server logger in the terminal. 

	app.use(express.logger('dev'));
	
Further down, you will see this for instructions you only want to fire in development mode:

	if ('development' == app.get('env')) {
	  app.use(express.errorHandler());
	} 

You could delete that previous line and put it in the development mode instructions like so if you only want this to run in dev mode.

	if ('development' == app.get('env')) {
	  app.use(express.errorHandler());
	  app.use(express.logger('dev'));
	} 
	
The following line is what tells Node where your static assets are located. Things like CSS, images and JavaScript.

	app.use(express.static(path.join(__dirname, 'public')));
	
The following line is what tells Node where the root route is:

	app.get('/', routes.index);
	
### Fun with routes

Now that we have some basic understanding of how the `app.js` file works, lets get into setting up some routes. 

Just some basics first. In the current setup, `var routes = require('./routes');` is setting a variable to the path of `./routes` to look for where all the RESTful routes will be kept. 

Right now, there is an `index.js` file in the `routes/` dir. In the `app.js` file, there is the line `app.get('/', routes.index);`. 

What this is doing is, Node has `./routes` loaded into memory. When it get to the line where `app.get` is stating that the path of `/` is defined in the value of `index` inside the key of `routes`. 

In the `index.js` file, you will see `exports.index`, what's that?

> `module.exports` is the object that's actually returned as the result of a `require` call.

Basically, the return of this function is set equal to this export object. Kind of like an alias. 

In `index.js` it is going to look for the `index` file and pass in the value for the `title` variable. 

	exports.index = function(req, res){
	  res.render('index', { title: 'Express' });
	};
	
If we were to make this more simple, we could comment out the following lines:

	var routes = require('./routes');
	app.get('/', routes.index);
	
Then in the `app.js` file, let's add the following. Make sure it is above the server line `http.createServer(app)`

Let's break this down. `app.get` is the function that will 'get' the URL path of `/`. Then we need to crate a function that will make a `req` or request, `res` or response, and `next` for chaining events. 

	app.get('/', function(req, res, next) {
		... 
	});
	
Ok, now that we have that frame of a route, we need to pass in what we want into the function. One thing we can do is `res.render()` and this will output the rendered HTML from our Jade template and any arguments we pass into it. 

	res.render('index', { title: 'Express' });

Another thing we can do is `res.send()` and what ever we put in there will get sent directly to the browser. For example: 

	res.send('holy crap')

NOTE: When we make changes to the `app.js` file we need to go back to terminal, kill the app and restart it. Once you restart the server, go to the browser and then refresh the browser. 

Using the `res.send()` we can do fun things like even send in JSON objects. 

	res.send({'name':'Bob Goldcat', 'age': '41'})
	
Restart, refresh and open the inspector. Open the 'network' tab and sure enough, app type `application/json`. Cool. 

Ok, let's get this back so that it is using the correct routes for this app. But, let's make is more personal. Update the `title` object to something like `It's a jelly!`

Restart, refresh and POW!

	app.get('/', function(req, res, next) {
	  res.render('index', { title: "It's a jelly!" });
	});
	
Now, let's say that we want to add a new view to the site. We have a couple of steps here to make this work. First being, we need to add the route.

	app.get('/foo', function (req, res, next) {
	  res.render('foo', { title: "This is totally foo"});
	});
	
Then we need to add the new file to the `/views` directory. What's interesting to note here is that this is not a file based URL. This is a RESTful URL path. The first argument is `/foo`, this is the URL path that the app will look for. Inside the `res.render()` function, the first argument is the file name, in this case it is `foo`. 

Remember, this is a templating language and we want to make sure that this new view uses all the goodness that we have in our `layout.jade` file. 

`extends layout` is a Jade concept that will extend `layout.jade` into the new view file. 

`block content`, again, is a Jade concept for interjecting content into the extended layout file for the final view. Removing any of the following concepts will break how this UI is rendered to the browser. 

	extends layout
	
	block content
	  h1= title
	  p This is some serious foo right here!

Let's say that over time marketing comes back and says that we need a new canonical URL, something like `this-article-is-really-foo`. Cool, we can make this change really easy and not break any links out in the internet already. Just add:

	app.get('/this-article-is-really-foo', function (req, res, next) {
	  res.render('foo', { title: "This is totally foo"});
	});
	
Now, `/this-article-is-really-foo` and `/foo` both point to the same file of `foo`. 
	
### Will you really define your routes in the app.js file?

Ok, this is fun, but when building a real app, you will not want to define your routes inside the `app.js` file. Unless it is a really small site with very few routes, you will want to put all these instructions into a separate file. 

The boilerplate solution that was installed when we ran the generator is one way to go, but there is a slightly simpler approach, but is still pretty rock solid. 

Remember how we commented out these following lines? Let's delete them. 

	// var routes = require('./routes');
	// var routes = require('./routes/user');
	
Also, if `app.get('/', routes.index);` is still in `app.js`, let's get rid of that too. 
	
Down where we have been writing our routes, as long as it is above `http.createServer(app)` you are ok, put in the following line:

	require('./routes');

Up at the top, for `var app`, you will need to update 

	var app = module.exports = express();

Now, open `/routes/index.js` and pretty much delete everything in there. First, put in the following line so that this file will reference the `app.js` file:

	app = require('../app');
	
Then below that, let's take all the routes we wrote in the `app.js` file and place them in this new file.

	

## Please, make the restarts stop!

Ok, making changes in the `app.js` file and having to restart the server is pretty lame. How make better? Node works this way because at server start, it loads all it's resources into memory and then on request it will then pass in that data using callbacks. That's what makes it fast. But in development, this is crap! How make better? The answer is Nodemon!

> Monitor for any changes in your node.js application and automatically restart the server

	npm install -g nodemon
	
Now, instead of running `node app.js` you will run `nodemon app.js`. 

Start your server and then make some simple edits to your `app.js` file and watch how Nodemon watches for this and restarts your server for you. 

YAY!

For those who like this kind of thing, in your `.bash_profile` you can create an alias like so:

	alias node="nodemon"
	
Some may say "HOW DARE YOU OVER-RIDE A NATIVE COMMAND!!!" Whateva! It's your damn computer, do what you want!

One word of warning though, remember that you did this. If there is a case where things just go weird, you will want to comment out that alias so that you can get your alerts all straightened out. 


## 404 errors?

Ok, people get things wrong and we need to have a way to handle that. Let's open up the `app.js` file and just below the `require('./routes');` line, let's add the following:

	app.use(function (req, res, next) {
	  res.status(404);
	  res.render('404', { url: req.originalUrl });
	});

Using the `app.use()` function, we are going to pass in another function much like creating a URL path using request, response and next.

Looking for a 404 status by using the `res.status(404);` function and passing in `404` as the argument we will then go to `res.render` and pass in the template file we want to use and pass in the incorrect URL. 

This code assumes that you know that this will be a HTTP response. If there is a case where this is not always true, you could update this with an `if` statement like the following: 

	app.use(function (req, res, next) {
	  res.status(404);
	  if (req.accepts('html')) {
	    res.render('404', { url: req.originalUrl });
	    return;
	  } else {
	  	...
	  }
	});

 
## Conclusion

Express with Node makes it really simple to set up a web app in just a few minutes. With some custom configurations, you can clean up unused boilerplate code and be up and running in no time. 
