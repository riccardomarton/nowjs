lightly.js - a lightweight js app platform
=====

**lightly.js** is a simple application container. It's aim is to provide a **simple, dependency-free framework** to manage a one page web app.

It's not really powerful, and is very limited in usage, but it is designed this way. An the other hand it's so simple it's easily extended.

It is made for use primarly with Cordova/Phonegap, so it doesn't change the actual URL of the page. Possibly it will be a future implementation.

## Initialization ##
To initialize an app using lightly.js simply invoke the *lightly* function inside a variable.

	var app = lightly();

## Container ##
The container is the element which contains the entire app. The container is the element which throws all the events triggered by lightly.


By default it is the *body*, but you can change it by using the *setContainer* method.
	
	app.setContainer(elem);		//elem must be a valid DOM element

## Pages ##
Pages are objects representing pages of the application. A page object must have this structure.

   	var page = {
		id: 'identifier_of_page', 	//string
		title: 'Title of page', 	//string - optional
		contents: {},				//object - optional
		callback: function() {}		//function - optional
	};

You can add a page to your app using the *addPage* method
	
	app.addPage(page);

You can get a list of the assigned pages by using the *getPages* method

	var pages = app.getPages();

	//returns
	pages = {
		"id1": {...}, 	//page object 1
		"id2": {...}, 	//page object 2
		...
	}
	


## Actions ##

Actions are objects containing functions to trigger. lightly.js mantains an history of actions triggered in order to offer en easy *back* action. A action object must have this structure

   	var action = {
		id: 'identifier_of_action', 	//string
		callback: function() {}			//function - optional
		history: true | false			//boolean - optional
	};

*back* and *navigate* are built-in actions and cannot be ovewritten. If you try to do so the *container* will throw an Exception. 
You can add an action to your app using the *addAction* method
	
	app.addAction(action);

You can get a list of the assigned actions by using the *getActions* method

	var actions = app.getActions();

	//returns
	actions = {
		"id1": {...}, 	//action object 1
		"id2": {...}, 	//action object 2
		...
	}

To execute an action you can use the *executeAction* method. Alternatively, you can use the shorthand method *do*

	app.executeAction('identifier_of_action');
	app.do('identifier_of_action');

You can pass a other arguments to these methods, which will be passed as arguments to the action function.
	
	var action = {
		id: 'identifier_of_action',
		callback: function(a, b, c, ...) {...}
	}
	...
	app.do('identifier_of_action', a, b, c, ...);

You can use the *getHistory* method to retrieve the current history array. The last element is the most recent.
	
	var history = app.getHistory();

	//returns
	history = [
		{...},	//action object 1
		{...},	//action object 2
		...
	]

### Built-in actions ###
The *back* action can be used to re-do back to the previous action performed.

The *navigate* action can be used to show a page. You'll need to specify the id of the page you want to load, and, optionally, a object containing other paramethers. Those paramethers will be passed as an argument to both the contents and the callback functions of that page.

The ***navigate* action** is tracked in the history by default. If you need to avoid tracking of the navigation, you can define a custom action using the ***navigate* method**

	var custom_navigate = {
		id: 'custom_navigate',
		callback: navigate,
		history: false
	}
	app.addAction(custom_navigate);

### Other methods ###
* **navigate:** it works in the same way of the *navigate action*, but it doesn't use the history array, so you can define your own navigation action.
* **newPageElement:** it takes the same paramethers of the navigate methods but, instead of showing the new page, it returns a div element containing the new page. Using this method you can add effects to navigation.  

## Events ##
Some custom events are thrown by the container of the application. The event object contains, if needed, a property named *vars* which contains option informations, depending on event.

* **lightly-page-added**: thrown when a new page has been added. A reference to the page object is contained in the *vars* property of the event.
* **lightly-page-load**: thrown when a new page has finished loading, after the callback. A reference to the page object is contained in the *vars* property of the event.
* **lightly-action-added**: thrown when a new action has been added. A reference to the action object is contained in the *vars* property of the event.
* **lightly-action-executed**: thrown when an action has been performed. A reference to the action object is contained in the *vars* property of the event.
* **lightly-action-back**: thrown when a *back* action has been performed. A reference to the action object is contained in the *vars* property of the event.

## Unit testing ##
To test the code, lightly uses the [QUnit library](http://api.qunitjs.com/QUnit.test/). The test unit file is contained in the *tests* folder. To test lightly.js simply open *tests/index.html* in your browser.
