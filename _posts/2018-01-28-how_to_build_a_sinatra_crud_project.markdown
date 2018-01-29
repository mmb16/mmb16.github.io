---
layout: post
title:      "How to Build a Sinatra CRUD Project"
date:       2018-01-28 20:43:59 -0500
permalink:  how_to_build_a_sinatra_crud_project
---


You'll notice a theme with my blogs for the past few months given the popularity of cryptocurrencies. I'll walk through how to build an ICO tracker with a very basic UI. All the code featured here is on my [github](https://github.com/mmb16/ico_tracker)

**Features**

* User Accounts
* Create ICOs with ticker, date, whitelist and name
* Read data on ICO's ordered by date
* Update ICO information
* Delete ICOs

Where to start I find to be the hardest part the links below were great resources for that

1. Create a git repository [click here to for a walkthrough](https://learn.co/tracks/full-stack-web-development-v3/git-and-github/git/git-repository-basics)
2. Set up the basic structure I've found this [link](http://blog.flatironschool.com/how-to-build-a-sinatra-web-app-in-10-steps/) to be a super helpful place to start

Now its time to start building the functionality

**User Accounts**
1. Create a method to check if a user is logged in the application controller
```
helpers do
   def logged_in?
     !!current_user
   end

   def current_user
     @current_user ||= User.find_by(id: session[:user_id]) if session[:user_id]
   end
 end
```

2. Create a User controller and inherit attributes from the application controller
```
class UserController < ApplicationController
...
end
```

3. Create a sign up process

```
get '/signup' do # retrieves the "/signup" view
  if logged_in? #check that user is logged in
    redirect to '/icos' #redirects to the "/Icos" view
  else
    erb :'/users/create_user' #if the user is not logged in they are directed to create a user
  end
end

post '/signup' do
  if params["username"] != "" && params["email"] != "" && params["password"] != "" #checks that the user inputs are not blank
	
    @user = User.create(params) #creates a new user with credentials 
    @user.save
    session[:user_id] = @user.id
    redirect to '/icos'
  end
  "Please fill in all fields" #error message that the user has not filled in all the information and the user is sent back to the signup page
  redirect to '/signup'
end
```

4. Create the login process

```
get '/login' do
  if logged_in?
    redirect to '/icos'
  else
    erb :'/users/login' 
  end
end

post '/login' do
  @user = User.find_by(username: params[:username]) #finds the user by username
  session[:user_id] = @user.id #changes the session id to the logged in user
  redirect to '/icos'
end
```

5. Create logout functionality
```
get '/logout' do
  session.clear #clears the session removing the user id from the session hash
  redirect to '/login'
end
```

**CRUD**

1. Create new entries
```
#directs the user to the create form if logged in
get '/icos/new' do
  if logged_in?
    erb :'/icos/create_ico'
  else
    redirect to '/login'
  end
end

#creates the ico if the ico name is not blank and the user.id matches the session id
post '/icos/new' do
  if logged_in?
    if params[:name] != ""
      @ico = Ico.create(params)
      @ico.user_id = session[:user_id] #prevents other users from creating things in your profile
      @ico.save
    end
    redirect to "/icos/#{@ico.id}"
  else
    redirect to '/login'
  end
end
```

2. Read entries
```
get "/icos/:id" do
  if logged_in?
    @ico = Ico.find_by_id(params[:id]) #finds the ico id that matches the url
    erb :'/icos/show_ico'
  else
    redirect to '/login'
  end
end
```

3. Update / edit entries (you'll notice it is very simliar to creating a new entry except it loads the old information first)

```
get "/icos/:id/edit" do
  if logged_in?
    @ico = Ico.find_by_id(params[:id])
    erb :'/icos/edit_ico'
  else
    redirect to '/login'
  end
end

post "/icos/:id/edit" do
  if logged_in?
    if params[:name] != ""
      @ico = Ico.find_by_id(params[:id])
      @ico.name = params[:name]
      @ico.save
    end
    redirect to "/icos/#{@ico.id}"
  else
    redirect to '/login'
  end
end
```
4. Delete entries
```
get "/icos/:id/delete" do
  if logged_in?
    @ico = Ico.find_by_id(params[:id])
    if @ico.user_id == session[:user_id]
      @ico.delete
    end
    redirect to '/icos'
  else
    redirect to '/login'
  end
end
```

**Views**
All the views for this project were basic forms for the most part, below is an example of the create ico form and the show ICO view.

Create ICO
```
<h3>New ICO</h3>
<form action="/icos/new" method="POST">
	<input type="text" name="ticker" placeholder="ticker">
	<input type="text" name="name" placeholder="name">
	<input type="date" name="ico_date" placeholder="DD/MM/YYY">
	<input type="boolean" name="whitelist?" placeholder="Y/N">
	<input type="submit" id="submit" value=" Add ICO">
</form>
```

Show ICO

```
<h3> <%= @ico.ticker %> </h3>
<h3> <%= @ico.name %> </h3>
<h3> <%= @ico.ico_date %> </h3>
<h3> <%= @ico.whitelist? %> </h3>

<div>
<form action="/icos/<%= @ico.id %>/delete" method="GET">
	<input type="submit" value="Delete ICO">
</form>

<form action="/icos/<%= @ico.id %>/edit" method="GET">
	<input type="submit" value="Edit ICO">
</form>
</div>
```

That is the basics of building a CRUD app in sinatra. Its pretty powerful for personal use, but probably slow if you have large amounts of data.





