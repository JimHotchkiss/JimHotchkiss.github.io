---
layout: post
title:      "My Rails Portfolio Project"
date:       2017-10-18 11:15:09 -0400
permalink:  my_rails_portfolio_project
---


> *Me, I can't usually get them 'cause my girlfriend's a vegetarian, which pretty much makes me a vegetarian. I do love the taste of a good burger.*                   - Jules Winfield



Often my wife and are trying to decide on something to cook, and with her being a vegan, our choices seem to be inherently limited.  This made me consider building a recipe organizer app for vegans.  The functionality is straight forward enough.  A user can browse recipes,  search recipes by ingredients, contribute a recipe and leave a comment on a recipe.

### Users and Authentication

I started things by entering, rails new simply_vegan_app, in my terminal, and that Rails magic did not disappoint.  I started by getting the User up and running.  With `rails g resource User`, I stubbed out  the necessary migration, model, routes and controller for a user.  I built out the necessary user signup form, and controller action neseccary to instantiate a user.  I then moved on to authentication.  I utilized the gem 'bcrypt', with the macro `has_secure_password`.  This gave me acess to useful authentication methods, like `authenticate` and `password_confirmation`.  We were required to use Omniauth to login by a third-party provider, like Facebook, Twitter or Github.  Omniauth is gem that provides a secure way for you to authenticate a user.  Omniauth initiates a kind of 'hand-shake', where the the provider, Github, in this case, responds to a GET request by sending a token back to the browser.  The browser then sends the token back to the provider, proving they are who they say they are.  The provider then sends a hash of the user's data.  In order to correctly extract this data, we need to grab the hash, and put it in a variable.

We first setup an if-statement, to confirm whether the user authenticating from third party.  If so, we have access to our user's data, and set it to a variable.

```
auth_hash = request.env['omniauth.auth']
```

Once we have access to this data, we can build a method that searches our data base to either find or create a user.

```
def self.find_or_create_by_omniauth(auth_hash)
    self.where(username: auth_hash['info']['name']).first_or_create do |user|
      user.password = SecureRandom.hex
      user.save
    end
  end 
```

Finally, we log the user in by using the SessionHelper method, `log_in`.  If not, we find our user, using our find_by method, and authenticate their password by using the authentication method. 

### Associating Objects

But perhaps the crux of this project is the associations between objects.  In order to unlock the power of Activerecord, a builder needs to make sure the objects they are working with are appropriately wired up.  In this case, I had a recipe model.  Recipes have to have ingredients.  But ingredient is its own model.  So, what I'd like to do is to take a `recipe`object, and be able to ask it about its ingredients, like `recipe.ingredients`.  I want this to return to me an array of ingredients specific to that recipe.  We want the user to be able to provide the recipe's title, instructions for the recipe, but these are direct attributes of the recipe.  Additionally, we want to user to be able to select ingredients, already instantiated by other users, or provide their own.  However, an ingredient is an object.  We need to relate these two objects, recipe and ingredient, by using nested forms.   

I'm using form_for to build out the recipe form.  We have an option for the user to selected from already existing ingredients.


```
<%= f.collection_check_boxes :ingredient_ids, Ingredient.all, :id, :name %><br></br>
```

However, we'd also like the user to be able to add their own ingredient.

```
<%= f.fields_for :ingredients, @recipe.ingredients.build do |ingredients_fields|%>
  <%= ingredients_fields.text_field :name %>
<% end %>
```

Here I'm using the fields_for, which is a FormBuilder object that associates the input with an existing object, in this case, `@recipe` to our `:ingredient` arguement.  Here, we are able to use the power of our associated objects.  Namely, we have `@recipe.ingredients.build`. This returns an array of ingredients.  The fields_for will now associated the input with the `@recipe` object.    

We have the input we need, and the necessary associations at the form level, but our recipe model does not have an ingredient attribute.  We must include this association in our strong params, allowing a @recipe to be instantiated with and newly created ingredient.  

```
def recipe_params
    params.require(:recipe).permit(:title, :instructions, ingredient_ids:[], ingredients_attributes: [:name])
  end
```

Here, we are 'whitelisting' the params we want permited.  Note, the `ingredients_attributes: [:name]`.  This is a setter method that takes the user's input, searches the data base, and either finds and returns that object, or creates a new ingredient object.  

```
def ingredients_attributes=(ingredient_attributes)
    ingredient_attributes.values.each do |ingredient_attribute|
      if !ingredient_attribute[:name].blank?
        ingredient = Ingredient.find_or_create_by(ingredient_attribute)
        self.ingredients << ingredient
      end
    end
  end
```

Here, we are taking the user's input, interating over the array, assuring there is not blank input.  Then it will search ingredients, and either find or create that ingredient.  Then the ingredient object will be shoveled into our @recipe.ingredients array.  

### Nested Resources

Another important feature of this app is for the user to be able to comment on a particular recipe.  This means that the user will navigate to a recipe url, `/recipes/recipe_id`, and then to create a new comment, `/comments/new` associated with that recipe.  In order to do this, we need to use nested resources.  

```
resources :recipes, only: [:show] do
    resources :comments, only: [:new]
  end
```

By nesting these resources, we are now able to use the url path of `/recipes/recipe_id/comments/new`.  This assure that each comment that is instantiated is properly associated with the correct recipe. 

Additionally, to add another level of functionality and convenience for the user, I decided to build a class level scope method.  This allows for more precision when searching and retrieving objects.  I wanted the comments to present the most recent comments first.  

```
  scope :most_recent, -> (limit) { order("created_at desc").limit(limit) }
```

This created a method, called `:most_recent`, which uses the provided method of `created_at`, and returns an array of objects, our comments, in this case, that are ordered from newest to oldest.  We, again, are taking advantage of the power of  Activerecord's associations.  Here, a comment belongs_to a recipe, and a recipe has_many comments.  This allows us to ask a recipe about its comments, like `@recipe.comments`.  This will return for us an array of those recipe's comments.  Our scope method takes an argument of 'limit'.  To address the 'limit', I simply used the count of each array.  

```
@recipe_comments = find_recipe.comments.most_recent(@recipe.comments.count)
```


### Final Thoughts

I really enjoyed this process.  There were times of enormous frustration.  For instance, I had moved away from authentication, and was working on views and forms.  My signup, login and logout had been working perfectly, and just before a signed off for the day, a double checked all the app's functionality.  Nothing worked.  I kept getting an error regarding  CSRF token authenticity.  Login links were dead, or just breaking the code.  I Googled the question, and got many explenations and suggestions that I didn't understand.  I reached out to the Slack community, and was immediately pointed toward accessible resources.  In my attempt to use link_to for a delete button, I added some JavaScript.  This, I learned, caused me to loose my authentication token.  Then I came across the prettiest, little line a code, `<%= csrf_meta_tags %>`.  This, effectively, acts as a hidden field, but allows JavaScript easy access to the authentication token.  

I'm always amazed at how challenging and rewarding programing can be.  There are times I feels as if I'm struggling with everything, and it isn't until I reflect on the process that I see just how much I know and how far I have come.  It  is definately hills and valleys, ebbs and flows (please pick you metaphore), please read my blog "Climb High, Sleep Low: Coding and Climbing.  

I'm already thinking about other project, big and small, can't wait.  













