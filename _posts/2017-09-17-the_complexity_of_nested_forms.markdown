---
layout: post
title:  "The Complexity of Nested Forms "
date:   2017-09-17 12:06:17 -0400
---

**The Big Picture**

In this lab, we are working with two models.  Namely, we are working with a `Post` and a `Tag`.  There are other models but we are focusing on these two.  We are looking at, exploiting, the association between these two models.  So, in our app, a `user`can create a new `post`, and they can add any number of `tags` they would like from a list of provided `tags`.  We want to create the functionality on our `post` form to allow the user to create a new `tag`.  This is reasonable enough.  The difficulty comes when we need to both instantiate a new `tag` and associate that `tag` with the newly instanatiated `post`.  To do this we must look at * nested forms.*  

**The Frame Work**

In our application, our Post model `has_many` `tags, :through`, and our `Tag` model `has_many` `posts, :through` the `post_tags` table.  This association is provided by a joins-table, `post_tags`.  We join these two models by connecting them through the `post_tags` joins-table.  

```
class Post < ActiveRecord::Base
  has_many :post_tags
  has_many :tags, :through => :post_tags
```

```
class Tag < ActiveRecord::Base
  has_many :post_tags
  has_many :posts, :through => :post_tags
```

What this does, out of the box, is allows us to ask a `@post` how many `tags` it has, with `@post.tags`.  Ultimately, what we want to be able to do is to nest the `tag` attribute `name` with our `post` on our `post` form page so it looks like, `@post[:tag][:name]`.  Rails will do a lot of heavy lifting for use, to accoplish this.  

**The Role of Rails**

At this point, we have our associations wired up: we have our datatables, with the appropriate joins-table; we have our models with the necessary `has_many` relationships.  We now need to add some functionality to our `post` form.  Rails has a powerful metho, `accepts_nested_attributes_for`, which provides us the ability to nest data structures in our `post` form.  We add this method to our `Post`model, and allow to accept the attributes of `tags`.  

```
class Post < ActiveRecord::Base
  has_many :post_tags
  has_many :tags, :through => :post_tags
	
	accepts_nested_attributes_for :tags
```

Now we can go to our form page, and write the functionality that will allow us to enter a new `tag`, and associated with the newly instantiated `post`.  We start with a form_for, passing it the instance variable `@post`.  This instance variable, `@post`, is made in the `posts#new`action, and provides rails with an object to work with.  

```
<%= form_for(@post) do |f| %>
```

For the `post`, we pass the variable `f` the rails' method `.label` and `.text_field`.  This provides the html to allow the user to enter a the content of their post.  We wrap this in a <fieldset>.  

```
<fieldset>
    <%= f.label :name %><br>
    <%= f.text_field :name %><br>
    <%= f.label :content %><br>
    <%= f.text_area :content %><br>
  </fieldset>
```

Additionally, we can provide a list of checkboxes for the user to pick the tag or tags they would like to associate with their `post`.  We can do this using the rails' method `.collection_check_boxes`.  

```
    <%= f.collection_check_boxes :tag_ids, Tag.all, :id, :name %>
```

But what we want to do is add a new `tag` with its attributes.  This is where the functionality of `accepts_nested_attributes_for`is put on full display.  We call the rails' method `.fields_for`, and pass it two arguements.  The first is the parent object, in this case, our `@post`.  This is done with the variable `f`.  Then we pass it the variable `:tags`.  

```
<%= f.fields_for :tags do |tag_forms| %>
      <%= tag_forms.label :name %>
      <%= tag_forms.text_field :name %>
    <% end %>
```

Now, the variable `tag_forms` gives us access to a ActionView::Helpers::FormBuilder.  This builder method gives us access to two objects: `post` and `tag`.  And, this is where the magic happens, it nests for us the attributes of our newly formed `tag`, and associates it with our newly instantiated `post`.  Here is the html that `tag_forms` produces.  

```
#<input type="text" name="post[tags_attributes][0][name]" id="post_tags_attributes_0_name">"
```

There's a few things that I did that perhaps you can learn from.  

**Remember to...**

When I began this lab, I started with the database.  Next, I made sure the models were correctly associated, and then I began to work on the `post` form.  After some review, I was able to build the `fields_for`that I wanted.  However, when I updated the browser, I got nothing!  The code didn't break, it just didn't render the output that I wanted.  After more review, I was reminded that, like the `@post` in the `posts#new`controller action, I needed to start the building process for `tags`.  I used the rails method `build`, which is a method like `new`, but `build`is smart enough to set the foreigh key.  So, once I updated my `posts#new`controller action, everything worked beautifully.  


```
def new
    @post = Post.new
    @post.tags.build
  end
```

Okay, now the new data is passed to the `posts#create`controller action, and however, when I look at the `post_params`, I am unable to find my nested data.  I needed to update my strong params, `post_params`, to be able to instantiate a new `tag`and its attributes.  I updated my `post_params` with `:tags_attributes =>[:name]`.  

```
def post_params
      params.require(:post).permit(:name, :content, :tags_attributes =>[:name], :tag_ids => [])
    end
```

After this last addition, I was able to instantiate both a new blog post with a new tag and its attribute(s).  

**Conclusion**

I have been enjoying learning Rails.  The power and functionality that is at your finger tips is wonderful.  However, sometimes, you have to look under the hood, in order to make sense of all the moving parts.  What seemingly in straight forward functionality you are trying to initiate, becomes very abstract with Rails doing much of the heavy lifting.  It is important to go back, and review the underlying methods provided to us through Rails.  












