---
layout: post
title:      "I Promise() to CallBack()"
date:       2019-09-14 19:40:15 +0000
permalink:  i_promise_to_callback
---


I hated JavaScript when I first started learning it. It felt clumsy and arbitray. You had weird sounding things like Promises and Callback functions. However, despite all its weird parts, when I first  loaded some JSON data onto the DOM, using Javascript, my feelings for were changed. 

This project's objective is to take an existing Rails application, and incorporate Javascript. Namely, we are tasked with loading data, from our database, asyncrously onto the DOM. This means the page does not have to refresh, giving the user a better experience. The first step in achieving this is to convert your data to JSON. 

# Meet My Friend JSON
JSON, or Java Script Object Notation, is a method of formating data so it can be transferred from server to web applications. For its fancy, and fun to say, name, JSON is simply an object of strings. 

```
{
id: 1,
title: "Pappardelle alla Bolognese",
description: "Thick slices of French toast bread, brown sugar, half-and-half and vanilla, topped with powdered sugar. With two eggs served any style, and your choice of smoked bacon or smoked ham.",
instructions: "Smoked salmon, poached eggs, diced red onions and Hollandaise sauce on an English muffin. With a side of roasted potatoes.",
category: {
id: 3,
name: "Dinner"
}
```


This is JSON data. Once our data is converted to JSON, it makes it very easy to work with. There are several ways to get our data formated into JSON. For my application, I used the Rails gem Active Model Serializer. 

# Serializer()
It's easy to get the serializer gem up and working for us. We first need to install the gem in our gem file, and run `bundle install` in our console. 

```
gem 'active_model_serializers', '~> 0.10.2'
```

Now that we have serializer installed, and need to tell it which models we want it to work on. In the case of my application, we want to serialize the Recipe model and the Category model. We establihsed the necessary files by using rails generator. 

```
rails generator serializer recipe
```

and 

```
rails generator serializer category
```

This will generate a folder structure of `app/serializer`, with the two files inside the `serializer` folder, and  the serializer gem knows to look in the serializer folder. Inside these files we can choose which attributes we want serialized, and established relationships between the models. For instance, in my recipe app, recipes `belong_to` a category, and a category `has_many` recipes. 

```
class RecipeSerializer < ActiveModel::Serializer
  attributes :id, :title, :description, :instructions

  belongs_to :category
end
```

```
class CategorySerializer < ActiveModel::Serializer
  attributes :id, :name

  has_many :recipes
end
```

We establish these relationships so our data is easy to access and manipulate, once in JSON. Once we have our serializer up and running, we now use Javascipt to access and control the data. 

# JavaScript
With our data neatly formatted into objects of strings, we now need to use Javascript, or Jquery, to make a request for that data. For instance, we'll want to load all of our recipes on an index page. We know how to do this through Rails, but doing it through Javascript is a little different. To begin this, we will listen for a 'click.'

```
$('#recipe-index).on('click', (event) => {
}
```

This code targets the element with an id of 'recipe-index', using a jquery selector, and then says that `.on` a 'click' a function is going to run. This is represented by the `event`. So we can use this `event` to control things. For instance, we don't want the data to be loaded through Rails, which will load another view, refreshing the browser. We want to stop that by calling `.preventDefault()` on the 'click' `event`. We've effectively high-jacked the entire process, and now Javascript is driving. 

```
$('#recipe-index').on('click', (event) => {
    event.preventDefault();
```

We are now ready to `fetch()` our data.

# Fetch()

We now want to make an API request using `fetch()`. The fetch API allows us to access and make changes to data asychronously. We call `fetch()`, and pass in an url. 

```
fetch('/recipes.json')
```

The fetch returns a Promise.  Promise is either resolved or rejected. If the Promise is resolved, meaning we get data back from our fetch request, we can then call .then(). This will return a response. However, we must convert our response into useful JSON, and we do this by calling `.json()` on our response.  Once we've converted our response to JSON, we'll then call another promise. This promise will return the properly formatted data. 

`fetch('/recipes.json')
    .then(response => response.json())
    .then(recipes => {}`

Reading this code, as if it were a sentence, it would sound like this:

We request data from a url, using fetch(). This returns a promise, and if the promise is successful, returns a response. We convert this response to JSON, and 
then we call another promise that will return JSON data. 

From here we can then format and display our data. But we also want to make an Object Model. So each time we want to display a recipe, we pass our data into this Object Model. 

``
function Recipe(recipe) {
  this.id = recipe.id
  this.title = recipe.title
  this.description = recipe.description
  this.instructions = recipe.instructions
  this.category = recipe.category
}
``

With an object model, we can create prototype function, that we can call on our Object Model. This affords us functionality and versatility with our code. 

``
Recipe.prototype.formatIndex = function(){
  let recipeHTML = `
    <div class = "indexJSON">
      <p><a href = "/recipes/${this.id}" data-id="${this.id}" class = "recipe-link">${this.title}</a></p>
      <p>${this.description}</p>
    </div>`
  return recipeHTML
}
``

# Conclusion
Javascript, for me, is when coding went for intersting and challenging to really, REALLY fun. You begin to see the power of programming. This inpired a lot of create thoughts on what I could do with data, how I could improve existing applications, and future. I'm exciting to keep digging into Javascript, and learning as much as I can. And I'm looking forward to diving into React. 























