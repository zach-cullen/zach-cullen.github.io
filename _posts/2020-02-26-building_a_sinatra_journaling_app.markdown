---
layout: post
title:      "Building a Sinatra Journaling App"
date:       2020-02-26 10:45:46 -0500
permalink:  building_a_sinatra_journaling_app
---


### My App Idea

Something I have always struggled with is creating a consistent journaling practice. As a goal-oriented person, I'm constantly having ideas and visions of the future that I write in various Google Docs then try to organize and cull later. But this type of ad hoc practice leaves me with no organized way of consistently following my progress. So, I decided to make an app that might help me solve the problem: How can I track my progress towards my goals as well as cultivate the mindset that I need to every day? My solution was to design an app that would allow me to create journals that would each be a collection of recurring prompts so that I would never again have to face the tyranny of a blank page again. 

### Scaffolding the App

To begin this process, I had to learn how to build the basic working scaffold for a Sinatra Application. The most helpful way to understand this is to clearly understand the order in which Sinatra Files are read and utilized by Ruby. Once you understand this, everything makes a lot more sense:

**1. config.ru**

The first file that gets read is config.ru which is the configuration file for your Rack server interface. A server interface is essentially the program that allows your program to respond to and make HTTP requests. This is also the first thing that runs before any of the programmed interactions happen, so if you need to load in dependencies or pre-process any data this is the place to do it. My confg.ru contained a require link to my environment file, my SASS CSS pre-processor, a specific Rack override method that enabled me to use HTTP methods Patch and Delete in addition to the standard GET and POST. And mounting the controllers. The last line of your config.ru file should always Run your application controller, the thing that turns those HTTP requests into meaningful routes. 

**2. environment.rb**

Your environment is the the first thing loaded by config.ru, so it needs to make sure all of your gems are loaded (do this by using bundler and a Gemfile), that ActiveRecord has established it's connection to your database, and that every part of your program knows where to find the other parts of the app (Accomplish this easily by using the require_all gem but note that this method does not allow you to controll the order that modules and dependencies are loaded so you wil need to be explicit abou this when necessary). 

**3. Gemfile **

Here is a list of essential gems for a Sinatra MVC / CRUD application and what each of them does:

* sinatra - has methods that allow you to easily set up controller routes. All of your controllers will inherit from Sinatra::Base.
* activerecord - this has methods that allow you to easily build associations between your models that inherit their properties from your database tables. It translates the actions you perform on your models into SQL statements that make changes to your database.
* sinatra-activerecord - this is an essential link between sinatra and activerecord as it give you the rake tasks to make properly formatted changes to your database through migrations.
* sqlite3 - this ruby gem is a crucial translator between the ruby language and the sql commands needed for an sqlite database. It is utilized by the previous gems and essential for them to function properly.
* bcrypt - this gem works specifically with ActiveRecord to create secure passwords and validations for models that have the attribute has_secure_password.
* shotgun - This is the actual server that responds to http requests and serves up documents. Shotgun uses rack but has the advantage of reading your program every time rather than operating only from the cache. That means changes you make to the program will be reflected when you refresh the page without having to reboot your server.
* rake - allows you to automate tasks, you will use rake to create and execute changes to your database

I used a few more gems for my project, but they are not essential depending on the functionality of your app:
* sinatra-flash - allows you to create alert messages for the user that only persist for one http cycle
* require_all - allows you to require multiple files in one line of code
* sass - allows you to use the CSS preprocessor SASS

**4. Models Views and Controllers**

You will need to create your Models, Views and Controllers to add functionality to your app. But in keeping with our sequential based explanation it's helpful to think of it this way:

1. Controller - The first thing your app will do is start listening for http requests. If you run shotgun and navigate to the localhost, it goes to '/' first. You will need an application controller to tell it what to serve up when the request get '/' is made. 

2. Models - Even though you may not always have to interact with your models to perform every controller action, if you do it must happen before a view is rendered. The models are what provide the unique data that makes your website dynamic.

3. Views - The view is finally what is rendered to the user to either give them data or ask for data through a form. Once they perform an action in the view (like submitting a form or clicking a link) then another http request is made that will need to by handled by your controllers.



