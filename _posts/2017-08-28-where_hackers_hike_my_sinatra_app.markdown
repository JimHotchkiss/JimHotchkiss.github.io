---
layout: post
title:  "Where Hackers Hike: My  Sinatra App"
date:   2017-08-28 14:07:47 +0000
---


While I worked my way through the Sinatra curriculum, I found that I needed a way to completely remove myself from code.  I found a long, scenic hike was the best rememdy to recharge my programing battery.  Now, I  have some favorite hikes that I like to share with friends, and I was wondering where other hackers may like to hike.  With that question, an app was born.  

The idea for this app, HackerHIkes, was very simple, you can find a linke  [here](https://github.com/JimHotchkiss/sinatra-fwitter-group-project-v-000).  I wanted the user to be able to: 

1. Signup to the app
2. See a list of all the hikes
3. Create their own hike 
4. Edit or delete one of their hike

In order to get this basic CRUD (Create, Read, Update and Delete) functionality working, I used an MVC model.  Here work and responsibilities are divided up into *Models, Views and Controllers.* The models is where the logic lives, these are classes, in this case `User`, `Hike` and `Difficulty.` The views is what the user sees, this is the webpage.  The controller is how the programmer mediates between these two models and the views.  The controller determines whether to render an html page, instantiate objects from users inputs, or edit or delete existing objects.  To get these seperations of responsibilities correctly interacting, I first had to establish a logical, and working database that would link database tables to my classes.  

To connect my database to my models, I used ActiveRecord.  This provides the powerful functinality of saving, or persiting, objects created from the models.  For instance, when a user signsup, the information they provided is used to instantiate a new user.  ActiveRecord links this newly created object to the appropriate database table.  Now that information, the object and its attributes, is stored, and lies in wait for a controller action to call on it.  Another powerful part of ActiveRecord is how it assosiates, or relates, datatables to one another.  Objects can `belongs_to` another object, or an object can `has_many` of another object, and even other associations.  As you can imagine, in this app, HackersHikes, and in most other apps, objects are related to one another. 

For instance, in HackersHike, there are three models, 'User,' 'Hike' and 'Difficulty,' and these models are all related to one another.  A user will have many (`has_many`) hikes; a hike will belong to (`belongs_to`) a user and difficulty; conversely, a user `has_many`hikes; while a difficulty `has_many` hikes.  It is these relationships, that allowed me to be able to ask the users about their hikes, or a hike about its user or difficulties about its hikes.  Now, in my experience, the database is both where I begin  when I begin to think about the functionality of my app, and this is the part of the process that requires a lot of attention.  After some trial and error, and many, many rakes of the console, I ended up with a schema (the structure of my database tables) like this. 

![schema](http://i.imgur.com/8aIGcwfl.png)

The key to getting your database tables set up correctly is playig with data.  Jump into your console and play around with instantiating objects, correctly associating your objects and confirming that those associations hold up to scrutiny.  Once I felt confidate that I had a working database table that was appropriately linked to my models, I began to build out my CRUD acions.  

The first thing I worked on, once I was able to render an index page, was creating a user.  To do this there was a couple things I had to do.  I first had to render a signup page that provided a form for the user to input their data.  Prior to rendering this page, I had to confirm that the user wasn't already logged in.  This is funtionality that was used many times through out the application.  Consequently, I built out a helper-method.  Once the user submited the form, the params must be checked for completenes, if this conditional is met, an object of the user is then instantiated and the newly created user is signed in by assigning them a sessions has.  The user is then redirected to a list of hikes.  This is what that controller action looks like. 

![post /users/new](http://i.imgur.com/Wqi8f6bl.png)

Once a user has signedup, the user can then scroll through hikes that other users have posted.  Additionally, if the user wants to contribute their own hike they can create their own hike.  First, much like signing up, the it must first be confirmed that the user is logged in, this is where that helper method came in handy.  Then a page is rendered for the user that includes a form for the user to input  the data of the hike they would like to create.  This data is sent to the action that creates a hike.  The users input is first checked for completeness.  If the input is complete, an object of the hike is instantiated.  Here, the newly instantiated hike is associated to the user creating the hike (this is where correctly constructed data tables is very important).  Lastly, the object and its attributes are saved, and a page is rendered that shows the user their newly created hike.  This is what that controller action looks like.

![](http://i.imgur.com/Izgar9vl.png)

Now that a user has signed up, created a hike, they can also update or even delete that hike.  In order to do this, I had to include some logic that prevented a user from editting or deleting a hike that was not theirs, in addition to checking that the content that the user was submitting was complete (like the orginal creation of the hike).  This was accomplished by comparing the users sessions hash with the hikes users_id (again, this is where smartly constructed database tables are wildly important).  Authenticating a user was done with this line of code.  

![](http://i.imgur.com/CP3iUYml.png)

This was a wonderful experience.  I had some wonderful hicups with github, where I had to start over THREE TIMES, that in the end prove to be wonderful learning experiences.  I found, that when creating a database table, even one that is as simple as the one I created, it is best make a diagram of what you intend to create, write out each object, by name, draw a line or lines connecting each object to one another, and then verbally, to yourself or someone bored enough to listen to you, the exact schematics of your design.  Ask youself of the intended outcome, and constantly throughout the development, make sure that that functionality is there and robust.  Once I felt I was done, I asked my wife to open the app, create a user, browse the listed hikes, create a hike and finally, edit or delete a hike.  This uncovered some gaps, not in the functionality, but rather the interface.  For instance, I had not built a message to notify a user if they were trying to edit or delete a hike that was not theirs.  And while watching my wife move through the functionality of the app, it was very suddenlty clear that she was completely confused on why she now seeing the page she was, and what exactly to do next.  A few lines a code, and a couple pushes to github, that functionality became far more intuitive.  

My affinity for coding has turned to a genuine love.  


















