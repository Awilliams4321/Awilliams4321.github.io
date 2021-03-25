---
layout: post
title:      "Rails App | Flatiron Project #3 "
date:       2020-08-07 15:49:18 -0400
permalink:  rails_app_flatiron_project_3
---


In this last Rails module, I learned about nested routes, generators, OmniAuth, and multiple other things that help a Rails app run smoothly. This project required me to have a true understanding of the Rails MVC flow, which honestly, i struggled with quite a bit. After reviewing quite a few lessons and exploring Google and Youtube for additional resources, I was really able to help bring my project together. Below are a few things I learned that absolutely helped me understand a few concepts i had a hard time with:

#1. Form_for vs. Form_tag

In my project, I had to decide which form was the approriate form to use and it required me to have a clear understanding of what I was trying to accomplish and the differences between the two. So what are the differences? The biggest difference is that a form_tag is not model dependent.

- A form_for tag is used to interact directly with a model in your application. You pass in an instance variable "object" and then you have access to that objects attributes and how it is displayed. Ex:
```
 <%= form_for @review do |f| %>

    <p><%= f.label 'Headline:' %><br>
    <%= f.text_area :headline %><br>

    <%= f.label 'Content:' %><br>
    <%= f.text_area :content %><br>
    <br>

    <%= f.label 'Name:' %><br>
    <%= f.text_field :public_name %><br>
    <br>

    <%= f.submit 'Update Review' %>
<% end %>	
```

In my above example, the 'f' variable is my "object binder" and i am able to call methods on my review obj. such as text_field. It would be better to use a form_tag if i am creating a basic form, like a sign-up form for new users. Ex:
```
<%= form_tag login_path do %>
  <p><label>Email: </label>
  <%= text_field_tag :'session[email]' %>

  <label>Password:</label>
  <%= text_field_tag :'session[password]' %><br>
  <br>
 
  <%= submit_tag "Sign In" %></p><br>

<% end %>
```

In this example, No objects are needed to create this form as im not interacting with any particular model.

#2. Strong Params

Private methods are methods that are in/available to a controller that tell Ruby "All methods from this point on can be called from withihn the object, but not from outside."
```
private

def review_params
        params.require(:review).permit(:musical_id, :headline, :content, :public_name)
end
```

This specific private method is called "strong params". This is because we are telling the method, within an object(:review), you can permit theses given attributes (:musical_id, :headline, etc.). Then, we have access to those permitted attribute in methods like so:
```
        @review = Review.new(review_params)
```
		
#3. Defining a Nested Route

In config/routes.rb is where we define all routes including nested ones. In order for Ruby to know a route is nested, you use a 'do' block. In my project, I nested reviews under musicals:
```
resources :musicals, only: [:index] do
    resources :reviews, only: [:show, :index, :update]
  end
```

The routes that are now provided allow us to access a specific musical_ by id and that specifc musicals review(s).
Ex:
```
/musicals/1/reviews/1
```

The above link will display the FIRST musical objects FIRST review.

~ My knowledge was tested and tried during the creation of this project but it demanded that I have a deeper understanding of how Rails apps operate.
