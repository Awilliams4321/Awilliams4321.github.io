---
layout: post
title:      "'Flatiron Project #2 - Sinatra App'"
date:       2020-06-07 19:46:31 -0400
permalink:  flatiron_project_2_-_sinatra_app
---


The Active Record/Sinatra module was challenging but at the same time allowed me to apply some of my OO Ruby knowledge. The requirements for this Sinatra Project was to create a CRUD(Create-Read-Update-Delete) and MVC(Model-View-Controller) application with a has_many & belongs_to relationship between a User model and another model. I chose to create a BillPro app that allows users to **create** a bill object and add it to a list, **read** the bills information, **update** a bills information, and **delete** a bill from the list. To start off my project i installed the corneal gem which creates the file structure necessary for a Sinatra app, including the **model**, **view**, and **controller** folders. This gem was especially helpful in getting started.

My next step was creating both a Users and Bills table with the column names(attributes) for each. I then created a User and Bill class in the models folder. After the relationship was built between the classes, i created a Users controller and a Bills controller in the controllers folder to hold my HTTP methods which consist of 4 verbs, GET, POST, PATCH, and DELETE.  

In total, i created 3 controllers, all which inherited from the main Application Controller. I  then had to make sure I mounted them in the config.ru file in order for my program to recognize the additional controllers.
```
use BillsController
use SessionsController
use UsersController
run ApplicationController
```


The majority of my CRUD methods were created in the Bills Controller. 
```
class BillsController < ApplicationController

    get '/bills/new' do 
        redirect to "/" if !is_logged_in?

        erb :'bills/new'
    end

    post '/bills' do #CREATE
        @bill = Bill.new(
            name: params[:name],
            creditor: params[:creditor],
            balance_owed: params[:balance_owed],
            monthly_payment: params[:monthly_payment],
            due_date: params[:due_date],
            user_id: current_user.id
        )
        if @bill.save
            redirect "/bills/#{@bill.id}"
        else
             erb :'/bills/new'
        end
    end 

    get '/bills/:id' do #READ
        redirect to "/" if !is_logged_in?
        @bill = Bill.find_by_id(params[:id])

        if @bill && @bill.user_id == current_user.id 
            erb :'bills/show'
        else
            redirect to "/bills"
        end
    end

    get '/bills' do 
        redirect to "/" if !is_logged_in?
        @bills = current_user.bills
        erb :'bills/index'
    end

    get '/bills/:id/edit' do #UPDATE
        redirect to "/" if !is_logged_in?
        @bill = Bill.find_by_id(params[:id])


        if @bill && @bill.user_id == current_user.id 
            erb :'bills/edit'

        else
            redirect to "/bills"
        end
    end 

    patch '/bills/:id' do #ADD
        redirect to "/" if !is_logged_in?
        @bill = Bill.find_by_id(params[:id])
        @bill.update(
            name: params[:name],
            creditor: params[:creditor],
            balance_owed: params[:balance_owed], 
            monthly_payment: params[:monthly_payment],
            due_date: params[:due_date]
        )
        redirect "/bills/#{@bill.id}"
    end 

    delete '/bills/:id' do #DELETE
        redirect to "/" if !is_logged_in?
        bill = Bill.find_by_id(params[:id])

         bill.destroy if bill && bill.user_id == current_user.id
            redirect to "/bills" 
    end

end
```

The connection between M-V-C is that the Models allow us to gather information from the tables and utilize them in our Controller methods. In our Controller methods, we can get/ post user information and display them on a View page.  

A portion of this project dealt with user authentication. I used the bcrypt gem to "salt" or encrypt a users password upon sign in so that their password stays private. With the assistance of helper methods and enabling sessions in the main controller, I was able to log users in and access a users profile via their session[:user_id].
```
class ApplicationController < Sinatra::Base

  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
    enable :sessions
    set :session_secret, 'secret'
    register Sinatra::Flash
  end

  get '/' do
    erb :welcome 
  end 

  helpers do 
    def is_logged_in?
      session.has_key?(:user_id)
    end 

    def current_user
      @current_user ||= User.find(session[:user_id]) if is_logged_in?
    end
  end 
end
```

This was a very fun project to work on because it allowed me to see how the things ive been learning connect. I am definitely looking forward to the upcoming Rails module.


