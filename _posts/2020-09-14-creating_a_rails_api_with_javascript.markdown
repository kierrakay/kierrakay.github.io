---
layout: post
title:      "' Creating a Rails API with Javascript.'"
date:       2020-09-14 14:56:22 +0000
permalink:  creating_a_rails_api_with_javascript
---




I will be giving a step by step guide on how I created a Rails API with a Javascript frontend. It will be a book tracker web app. In my last projects I strictly used Rails to render my views, store and grab data from the database. Now we will only use Rails to store our data. From there we will grab data from the api to render json.
Let's get started!
Phase 1:
Initial project setup
First you'll want to create a parent directory with two sub directories inside. One directory will be for your backend and one will be for your frontend. This will make it easier for you to push your commits and not run into any issues. To do that you'll go into your terminal and type the following:
Parent directory: You can name this whatever you'd like.

mkdir js_project
2. You'll want to cd into that parent directory and then create your backend folder with the following command:
rails new js_project_backend --api --skip-git
This api flag will give your app the necessary data it needs in order to turn your backend into an api. We use the skip git flag because we will be doing the initializing ourselves.
By default SQLite will be used but if you want to use something else such as PostgreSQL include the flag in this command

3. After you have done that (make sure you're in the parent directory) type"ls" to make sure the newly created backend folder is inside the parent directory.
4. Still inside the parent directory you'll create the frontend folder with the following command:
mkdir js_project_frontend
5.Type "ls" to make sure BOTH backend and frontend folders are there.
6. Type "git init" and hit enter. Then type "git status" and you should see that your backend folder is being tracked.
7. We will want to put something in the frontend folder so we can actually see it like how we did above for our backend. In order to do that you'll want to cd into the frontend folder and type the following command:
touch index.html
8. Cd back into the parent directory and type in "git status" and you should see that both your frontend and backend folders aren't added, committed or pushed. They should be in red font to signify that.
9. Now we will initialize. Type in the following:
git add .
git commit -m"Set up frontend/backend folders"
10. Then go into your browser and create a new repository for this project. Back in your terminal add the line that reads similar to "git remote add origin…" and then "git push -u origin master"
11. If you refresh your github page you should see your project up and ready with your new folders!




Phase 1 Complete!!
Phase 2:
Backend Setup
You should know how to set up a database for Ruby on Rails. Almost everything will be as expected. Instead of rendering erb files in our controller we will render json with the object and not instance because again we want our Javascript to handle our views.
Gems you will need:
rack-cors : This should already be in your gemfile, just uncomment it.
active_model_serializer: gem 'active_model_serializers', '~> 0.10.2'
pry: gem 'pry'
Next you'll want to go into the "config" file then the "cors.rb" file and scroll all the way to the bottom.
origins set to '*' will allow any routes to be able to access our api. Since this is a MVP we can do that but for other projects you'll want to change that. Depends on your situation.Nice, bundle install.
Creating BOOK and USER models.
Run the commands:
rails generate model USER name:string
rails generate model BOOK title:string author:string review:text rating:integer user:references
rake db:migrate
Your schema file and model folders should be created now. The BOOK model will have your " belongs_to :user" relationship already inside. You'll have to go into the USER model and add " has_many :books"
Creating BOOK and USER controllers.
I first created a "api" folder inside the "app" folder. Then I created a "v1" folder inside of the "api" folder. This just means version 1 incase you want to update the api in the future. Inside your "v1" folder you'll create the books_controller.rb and users_controller.rb files as usual.
Notice we aren't rendering the erb files for our views. In order to render javascript you just render json: (object). Our create function will find a user based on what name they input and if the DB can't find the name it'll create a new user. We don't have a true login form for this app. Correct spelling is essential for getting the correct info for that user. You can create your own login action using a Devise gem or Bcrypt. We won't have a password for this app either.Routes
Serializers
The reason we added the active_model_serializer is because it generates serializers for our models that will be implicitly called if we call render json: on a model! Which is pretty tight! You don't have to do anything but add the gem and files.
active_model_serializers | RubyGems.org | your community gem host
ActiveModel::Serializers allows you to generate your JSON in an object-oriented and convention-driven manner.rubygems.org
Using Active Model Serializer - Learn.co
Explain what ActiveModel::Serializer does. Use ActiveModel::Serializer to render JSON with associated objects Explain…learn.co
USERS route :
To see this data create a new user in the rails console. EX: u = User.new(name: "Kierra") … u.save!…. b = Book.new(title:"test", author:"test",review:"test",rating:1,user_id:1)…. b.save!BOOKS route:


---

BACKEND DONE YA'LL!Phase 3:
Javascript Frontend
Now the fun begins! We will be working on manipulating the DOM! Head on over to your frontend folder that we created earlier. In that folder you'll want to create a "styles.css" file and a "src" folder. In the "src" folder create a "book.js" file and a "user.js" file. Then create an "Assets" folder and in that folder create a "Images" folder and a "Fonts" folder. I won't be covering how to use the Assets folder or styles.css file. The Assets folders is handy for importing fonts, images, audio etc. Our main focus is the src folder and index.html.
The src folder holds our book and user js files. These files will hold our JS code. We can access these files through a script tag that will be in our index.html file. This will make our code more dynamic.Index.html
Let's start with a basic layout. We want to get our user input set up and some kind of message on top. Then we will use Javascript to manipulate the DOM with functionality outside of this template.
In your terminal you should be in the frontend folder. Type open index.html and you should see..
The submit button shouldn't be doing anything because there isn't any logic to it yet.

---

User.js file
Here is where we will start using OOJS (Object Oriented Javascript).
OOJS is a JavaScript library for working with objects. Features include inheritance , mixins , static inheritance and additional utilities for working with objects, arrays etc.
Static means we can only use this function by calling the User class on it. That's why we have User.createUser() above. When the submit button in our form is clicked it will run our POST request which will deliver that information to the DB(database). Lines 53 -54 is because I wanted to hide that books-container until after the user logs in. Feel free to ignore.Notice we have a console.log(user) on line 39. This is a great way to check that once you input a name and hit submit that user was sent to you DB correctly.


---

Your browser should look similar to this.

---

The id may vary. Depending on how many users you created. I accidentally created a user before this. So I have 3 users in my DB. You should only have 2 users at this point if you created a user in the rails console before. This is the console.log output.

---

Your API should look similar. User 2 and 3 were created with our new functioning submit button. This happened because we connect our frontend to our backend using that POST fetch to create a user.

---

Book.js
newBookForm will replace the newUserForm that we originally had in our index.html.Line 62 . Make sure to check that your book was created properly.

---

Browser

---

API

---

Books.js "Delete Fetch"
We will want a user to be able to delete books by using a delete fetch. This is a super simple task. When a user hits the "x" button it'll trigger an event listener on our frontend that will sent a DELETE HTTP request to our backend.
CONGRATULATIONS!
You have successfully created a fully functional web app using Ruby on Rails and Javascript! Feel free to use the Assets folder and styles.css file to customize! HAPPY CODING! :D
