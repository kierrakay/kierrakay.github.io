---
layout: post
title:      "'How to build a CRUD, MVC app using Sinatra!'"
date:       2020-09-14 15:00:47 +0000
permalink:  how_to_build_a_crud_mvc_app_using_sinatra
---



Welcome, Everybody! This blog is going to teach you how to create a custom CRUD, MVC app using Sinatra. Sinatra is a free and open source software web application library and domain-specific language written in Ruby. Sinatra is designed to be lightweight, flexible and provide you with the bare minimum requirements/abstractions for building simple and dynamic Ruby web applications. Let's get started!
Our Goal: Create a simple journaling app.
Show errors to our user

GET /failure

2. Create a user account
GET /signup
POST /signup

3. Log in as a user
GET /login
POST /login

4. CRUD on Entry
GET /entries
GET \/entries/new
POST /entries
GET \/entries/:id/edit
PATCH \/entries/:id
DELETE \/entries/:id

First step is to open your command-line and create a root directory for your journaling app. Name the file whatever you'd like (after the "mkdir"). Hit return and then cd into the file.
mkdir sinatra_app
and then
cd sinatra_app
You will have an empty project directory to fill with all the tools you will need to build the structure for the "Sinatra Magic".
Second step is to create your files.
I like to begin by running bundle init in the terminal to create a Gemfile in your root directory. Here we will list all the dependencies we will need for our app.
bundle init
Lines 1–7 will be created for you but you will need to input lines 8–19. (Line 8) will help you keep track of your models. It has a different name when you require it from when you install it. Make sure you don't forget to specify that with the require attribute. (Line 9) You will need for the Rakefile. (Line 10) You will use to keep track of data. (Line 11) Is an adapter to keep track of data as well. (Line 12) Is a password hashing function used for encryption to make your data more secure. (Lines 15–19) Will be gems you will only be using for the development phase of the app. They will help with any debugging issues. Once this is all written run bundle install. Once you run bundle install in your terminal it'll create a Gemfile.lock file for all your gems.
bundle install
Create a new folder called config in your root directory. In this folder you will want to insert an environment.rb file and a database.yml file. Afterwards create a config.ru file.
This will pull in everything that we need for our project. To test that you created the file successfully you can type ruby config/environment.rb in your terminal. If no errors appear you have created it successfully. Ignore lines 4–7 for now. We haven't gotten that far yet!If you ever get this error .. create a database.yml file to fix it.This states that when you're in the development mode use the adapter sqlite3 and the database file db/entries.db (Name the database what you want to store. In our case it'll be entries because this is a journaling app.) Run rake db:create and it''ll create the db file for you. which will be your database. You can also type ls in your terminal to make sure all the previous files you created are there or ls db to see the db file specifically.Create a Rakefile in your root directory. In this file you will require your sinatra/activerecord/rake gems and the environment. You will also set up your custom console (ensuring pry is installed) in a do block so you can test your code in your terminal. When you run rake console in your terminal it'll automatically put you into a pry console. Run "rake -T" and you will see all of the rake tasks you can use for your project.
Rake TasksIn the config.ru file you will require your environment, run your main controller (ApplicationController) and use any other controllers you need for your web app (EntriesController). You can only run one controller. You'll have to use any other controllers. These controllers have to be in order. Rack::MethodOverride is forCONGRATS! YOU HAVE SUCCESSFULLY CREATED PART OF YOUR WEB APP! LETS MOVE ON TO THE MVC PORTION OF OUR APP!
First you will need a folder called app. Here is where you will create your models, views, and controllers (MVC).
I know I know! There's a lot in here. In the beginning the only thing you should have are lines 1,2,10,11,12. Replace line 11 with "<h1>Hello World</h1>". Type shotgun in your terminal and go to the local host. You should see "HELLO WORLD" on the screen! If you do congrats you have a working web application! All of the routes in these photos are rendering erb files that will show data that you want the user to see. Work one at a time based off of our goals listed above. It'll make things easier. Lines 3–8 is a block that will ensure that your views (erb files) will show, that the data for a specific user is saved (session request) by enabling sessions. Then you'll set the session and use a complex string but for now we will put something basic. You normally would avoid committing this to your GitHub repo. The views (erb files) are also set here.The helper methods are custom methods you can use in your code to do specific things. You can call these methods in other methods in different controllers as long as the other controllers are inheriting this parent controller where the helper methods are created.Under private (private acts as a helper) are the params. Params is a hash ( key-value pair). It is a way to encapsulate data that is passed around through our encoded URL/HTTP requests. We are using routing parameters for this project.There is a lot going on here so let me break it down. This is your parent controller. You can set it up however you like but this is how I set it up for mine. Inside this folder we are creating routes that will render erb files (Embedded Ruby). Erb files are what the user will see in the browser through forms that take in a users input. Thanks to Sinatra our controllers will be written in Ruby code but our erb files will automatically convert to html files that can have embedded Ruby code. Pretty cool right!
Remember when I said to ignore lines 4–7? We are here now! You add these lines as you start to build your app. You require them so you can call shotgun in your terminal and see all the goodies you created in your browser! You will only need to require your models and your controllers here (in order). The views you will set in the application controller. For reference look at the application controller photo above.Example of what a signup.erb file can start off as. (Line 2) Here is where we will create things and to do that you need method="POST" and to what url you want it to go to is where action="/signup" comes in. Your erb files should be named the same thing as the GET request. You will put your html code in the erb file and link your css file in here for the design of your site. Again, remember to work on each route one at a time and use your failure page (erb: failure in your GET/POST requests ) to test that your code is functioning properly.Once you get that to work you want to create your migrations. Type what is above in your terminal.Once you do that active record will create this database migration called CreateUsers. The migrations is where you'll create the tables and columns of data you want stored in your database. Notice the password column is inputed as password_digest. We are doing this so we can use the bcrypt gem to encrypt the password. We do this so that no one knows a users password except for the user. Not even the developers will know a users password.rake db:migrate
After you save the table run rake db:migrate in your terminal to migrate your table. Then check your schema file to ensure that it is there. If you don't see your table in your schema file it is most likely because you didn't save your table in your text editor before running rake db:migrate. Repeat the process to fix it.
In your app folder create a models folder that has a user.rb file. (Line 2) will add some methods for us just like how inheriting ActiveRecord does. Don't forget to require this file above your controllers in your environment folder. Picture below for reference (Line 4). After type rake console in your terminal to confirm that User is a class. If you type User.methods you should see a bunch of methods added that Active Record wouldn't have added before. You also want to validate that the inputs for the attribute :username is true and that it is unique so no one can have the same username. You'll want to validate that a password was entered as well. Notice line 3 where we state that user has . a has_many relationship to entries which will connect to our entry model and in our entry model we will state that an entry has a belongs to relationship to a user. You create your models after you create their db migrations.TIME FOR CRUD ACTIONS!!
Here is where you will create your create, read, update, delete actions (CRUD). This controller will allow a user to create an entry, read their entries, delete their entries and update their entries (patch is for update). This is possible because in our model we stated that a user has_many entries and in the entry model we stated that an entry belongs_to a user using their user_id which is created automatically with active record. This controller also renders erb files.
def current_user
User.find(session[:user_id])
end
A user is also able to see only their stuff because of this method we created in the application controller that our entries controller inherits from.
Sinatra has middleware that allows us to use PATCH, PUT, DELETE requests. This is the only way we can use these requests. In an application with multiple controllers, Rack::MethodOverride must be placed above all controllers in order to access the middleware functionality.The hidden input field shown above uses Rack::MethodOverride. this method will interpret any requests with name="_method" by translating the request to whatever is set by the value attribute. In this example, the post gets translated to a patch request.Ooof. You still there? If so…
I hope this was helpful and informative!
