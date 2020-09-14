---
layout: post
title:      "'Movie Review App Using Ruby on Rails.'"
date:       2020-09-14 14:59:39 +0000
permalink:  movie_review_app_using_ruby_on_rails
---


---


Welcome back! Today I'm going to teach you all how to build a movie review app using Ruby On Rails! Let's do this!


---



---

Goals:
Use the Ruby on Rails framework.
Include a has_many relationship, a belongs_to relationship, and a has_many :through relationship.
Models will include a few validations for the simple attributes.
The application will provide standard user authentication; including signup, login, logout, and passwords. We will also allow Github to authenticate a users login.
Create a nested route.
Implement reviews, ratings and users.
Use a few gems such as Searchkick, Bootstrap, Paperclip, and many more.
Make sure you already have Javascript and Java set up on your device.

Step 1:
First, create a project by running the command " rails new movie_review_app" . Then navigate into the folder that holds your project.
In the Gemfile add these gems.
The devise gem will handle the login, logout, and signup for our users. As well as keep up with their sessions. The dotenv-rails gem we will use to create a .env file to hold our secret ID and keys for our OmniAuth gem. The OmniAuth gems are so we can have Github authenticate our user if they choose to login or signup through Github. The Paperclip gem is for importing images to our application. The bootstrap gem is for our designing process and last but not least we will use the SearchKick gem to allow our users to search for a particular movie instantly.Run the following command to lock these gems in.
$ bundle install
Check out these articles for setting up devise and omniauth.
heartcombo/devise
Devise is a flexible authentication solution for Rails based on Warden. It: Is Rack based; Is a complete MVC solution…github.com
Devise Authentication Guide with GitHub OmniAuth for Rails Application
Introductionmedium.com
Devise_ - Learn.co
Set up a Rails server with Devise to let users log in with a username and password. We're going to use Devise and Rails…learn.co
Once you have that set up run the following command to get your server running at localhost:3000
$ rails s
If you have done this correctly you should see the following.
Step 2:
Let's create our models and their attributes(the user model you should have already created when you set up the devise gem). Luckily for us we can do this fairly quickly by using the rails command ..
Below we will create the Review model my using a model generator. We will use the Review model as our join table. We will use it to join the user model and the movie model. That is why we give it the foreign keys of movie_id and user_id along with two of its own attributes. Comment and rating.
$ rails generate model Review rating:integer comment:text user_id:integer movie_id:integer
$ rails generate model Movie title:string description:text movie_length:string director:string rating:string user_id:integer
t.string "title"
ignore t.string "image_file_name", t.string "image_content_type", t.integer "image_file_size", t.datetime "image_updated_at" for now. These are generated when we get ready to utilize our paperclip gem.I didn't explain how to set up your user model as it was explained in the devise articles but this is what your migrations and schema should look like for it. As well as what your migration should look like for OmniAuth.
you may have noticed that you don't have "t.string "username" in your schema. That is because devise doesn't create a username attribute for a user. If you want a user to have a username you must manually add it with the following command..$ rails generate migration add_username_to_users username:string
Awesome, now run the following command.
$ rake db:migrate
The files created should look as such.
invoke active_record
create db/migrate/20190618010724_create_movies.rb
create app/models/application_record.rb
create app/models/movie.rb
At a high level, this has created:
A database migration that will add a movies table and add the columns user_id, title, and description, movie_length, director, rating .
A model file that will inherit from ApplicationRecord (as of Rails 5)

Note: Up to Rails 4.2, all models inherited from ActiveRecord::Base. Since Rails 5, all models inherit from ApplicationRecord. If you've have used an older version of Rails in the past, you may be wondering what happened to ActiveRecord::Base? Well, not a lot has changed, actually. This file is automatically added to models in Rails 5 applications:
 # app/models/application_record.rb
 class ApplicationRecord < ActiveRecord::Base
 self.abstract_class = true
 end
Now time to build our controllers. You could use a controller generator or resource generator but I prefer to start with the model generator. To each his own.
Next let's create the controllers.
$ rails g controller Users 


---

$ rails g controller Movies


---


$ rails g controller Reviews
You should get this in your terminal for each.
no routes created.It will create a users folder under views that connects to the users controller. As well as for movies and reviews.In your config folder go toroutes.rb and add resources :users , resources :movies , resources :reviews so we can implement our CRUD actions for the users, reviews, and movies controllers. This will give us the following routes.
you should see routes for reviews and movies as well.Define controller actions in your controllers and also in the app/views/movies , app/views/reviews, app/views/users folder add your view files for each action. Example: app/views/movies/index.html.erb . Don't forget to create a "_form.html.erb" in the views as well. This will be our partial view for new and edit.
class MoviesController < ApplicationController
def index 
@movies = Movie.all
end
def show
@movies = Movie.find(params[:id])
end
end
Write the corresponding views in the app/views/movies/index.html.erb folder.
Index:
<h1> Movie Index Page </h1>
When you visit localhost:3000/movies, you should see:
Show:
<h1> Movie Show Page </h1>
When you visit localhost:3000/movies/1, we should see:
Great! Let's get some views up for our users.
class MoviesController < ApplicationController
def index 
@movies = Movie.all
end
def show
@movies = Movie.find(params[:id])
end
def new 
@movie = Movie.new
end
def edit
end
def create 
@movie = Movie.new(movie_params)
if @movie.save
redirect_to @movie, notice: 'Movie was successfully created.' 
else
render :new
end
end
def update
if @movie.update(movie_params)
redirect_to @movie, notice: 'Movie was successfully updated.' 
else
render :edit 
end
end
def destroy
@movie.destroy
redirect_to redirect_to movies_path 
end
private
def set_movie
@movie = Movie.find(params[:id])
end
def movie_params
params.params.require(:movie).permit(:title, :description, :movie_length, :director, :rating, :image, :user_id )
end
end
app/views/movies/index.html.erb
<h1>Movies</h1>
<table>
<thead>
<tr>
<th>Title</th>
<th>Description</th>
<th>Movie length</th>
<th>Director</th>
<th>Rating</th>
<th colspan="3"></th>
</tr>
</thead>
<tbody>
<% @movies.each do |movie| %>
<tr>
 <td><%= movie.title %></td>
<td><%= movie.description %></td>
<td><%= movie.movie_length %></td>
<td><%= movie.director %></td>
<td><%= movie.rating %></td>
<td><%= link_to 'Show', movie %></td>
<td><%= link_to 'Edit', edit_movie_path(movie) %></td>
<td><%= link_to 'Destroy', movie, method: :delete, data: { confirm: 'Are you sure?' } %></td>
</tr>
<% end %>
</tbody>
</table>
<br>
<%= link_to 'New Movie', new_movie_path %>
app/views/movies/show.html.erb
<p>
<strong>Title:</strong>
<%= @movie.title %>
</p>
<p>
<strong>Description:</strong>
<%= @movie.description %>
</p>
<p>
<strong>Movie length:</strong>
<%= @movie.movie_length %>
</p>
<p>
<strong>Director:</strong>
<%= @movie.director %>
</p>
<p>
<strong>Rating:</strong>
<%= @movie.rating %>
</p>
<%= link_to 'Edit', edit_movie_path(@movie) %> |
<%= link_to 'Back', movies_path %>
app/views/movies/new.html.erb
<h1>New movie</h1>
<%= render 'form' %>
<%= link_to 'Back', movies_path %>
app/views/movies/edit.html.erb
<h1>Editing movie</h1>
<%= render 'form' %>
<%= link_to 'Home Page', movies_path %> |
<%= link_to 'Back', @movie %> |
<%= link_to "Delete", movie_path(@movie), method: :delete, data: { confirm: 'Are you certain you want to delete this?' } %>
app/views/movies/_form.html.erb
<%= form_for @movie do |f|%>
<%= form_for @movie, html: { multipart: true } do |f|%>
# { multipart: true }is for paperclip gem above
<%= f.hidden_field :user_id, value: current_user.id %>
#gives a movie a user_id so only the person who creates it can delete it
<div class="form-group">
<% if @movie.errors.any? %>
<div id="error_explanation">
<h2><%= pluralize(@movie.errors.count, "error") %> prohibited this movie from being saved:</h2>
<ul>
<% @movie.errors.full_messages.each do |message| %>
<li><%= message %></li>
<% end %>
</ul>
</div>
<% end %>
<%# <div class="field"> %>
<%= f.label :title %><br>
<%= f.text_field :title, class: "form-control", placeholder: "Title" %>
</div>
<div class="field">
<%= f.label :description %><br>
<%= f.text_area :description, class: "form-control", placeholder: "Description" %>
</div>
<div class="field">
<%= f.label :movie_length %><br>
<%= f.text_field :movie_length, class: "form-control", placeholder: "Movie Length" %>
</div>
<div class="field">
<%= f.label :director %><br>
<%= f.text_field :director, class: "form-control", placeholder: "Director" %>
</div>
<div class="field">
<%= f.label :movie_rating %><br>
<%= f.text_field :rating, class: "form-control", placeholder: "Rating" %>
</div>
<div class="actions">
<%= f.submit %>
<%# f.submit will automatically give it a create or update button %>
</div>
<% end %>
At this point you should be able to fill out a basic form at localhost:3000/movies/new
In your routes file add root 'movie#index' to set it up as your homepage.
Time to add our has_many , belongs_to and many-to-many relationships.
In your movies controller to make sure a movie gets created with the current user we will need to add a before_action authenticate!, and add the method current_user to our new and create actions. The before_action is from devise and it will ensure the current user is actually a user. Then we state except: [index] to allow anyone to see our index page.
Go into your movies model and add belongs_to :user
ignore the rest.Go into your movies model and add has_many :movies
ignore the rest.BREAK TIME!!!!
Take a quick 15… this is a lot!If you haven't added your paperclip gem, do so now. We're gonna start adding photos. https://github.com/thoughtbot/paperclip this gem also requires that you install imagemagick. Follow the repo for how to install. Then scroll to the QuickStart section.
For the migration for paperclip type rails generate paperclip movie image .Thenrake db:migrate .
you should have all the image attributes in your movie schema once you rake db:migrate.Add the html: { multipart: true } to this view now.
app/views/movies/_form.html.erb
<%= form_for @movie, html: { multipart: true } do |f|%>
# { multipart: true }is for paperclip gem above %>
.
.
.
and where your attributes are below the code above add 
<div class="field">
<%= f.label :image %><br>
<%= f.file_field :image %>
</div>
app/views/movies/show.html.erb
<%= image_tag @movie.image.url(:medium) %>
app/views/movies/index.html.erb
<% @movies.each do |movie| %>
<tr>
<td> <%= image_tag @movie.image.url(:medium) %> </td>
<td><%= movie.title %></td>
<td><%= movie.description %></td>
<td><%= movie.movie_length %></td>
<td><%= movie.director %></td>
That is all for the paperclip gem! You should be able to add an image file to a newly created movie.
Styling
If you haven't added the bootstrap-sass gem to your Gemfile add it now.
https://github.com/twbs/bootstrap-sass
ignore where it says gem 'sassc-rails', '> = 2.1.0'. Make sure to add both @imports and only those to the app/assets/stylesheets/application.scss.app/views/layouts/application.html.erb
Add the following into this file.
Line 14 will take us to partial. "_header.html.erb"app/views/layouts/_header.html.erb
Add the following in this file. This is all bootstrap and you can find it the bootstrap documentation. https://getbootstrap.com/docs/3.4/css/
when you refresh your server you should see a new nav-bar at the top.
search bar shouldn't do anything…yet!app/views/layouts/index.html.erb
Here we are adding a big jumbotron message that will go off if a user isn't signed in. It will direct them to sign up. If the user is signed in this will not appear on the homepage. Go into incognito mode to test it out.
Line 3 change the reviewed typo. The mumbo jumbo in the <p> tag is just a filler message. Add whatever you would like your user to see!At this point your movie show page should look something like the following.
We are gonna add the reviews to the right.
We are putting our content in a panel and have two rows on the page. The left side will be the movie info and the right side will be for the reviews. This image is just for the movie info right now.app/assets/stylesheets/application.css.scss
Add this to add a little "umpf" to your application. Feel free to change what you would like! :)
Reviews Controller
let's fix our reviews side. Your reviews controller should look something like this.
When you create a review using the form it should take you to the review show page. There you should see the rating you gave and the comment you wrote.
Lets add a review to a user and a movie!
Your models should look like the following. These are validations I used. Feel free to use them or other ones. You can also wait till your application base is completed to add the validations.
dependent: :destroy allows review to get deleted with a user when they cancel their account. A user has an id associated to their reviews and their movies. So only they can edit them once we use current_user in our controllers for new, create and edit actions in the movie and review controller. We also will use current_user for the review show page later on when we nest our movie and review route.We add " belongs_to :user" here so a movie.id gets add to a particular user. So if that user wants to delete the movie only they can.Updated reviews controller. We have added a before_action to make sure only a signed in user can create a review and line 21 to assign a review to a user now that we have those associations in our models.
under line 35 add " redirect_to root_path.I hope everything is working well so far. Hang in there!Time to fix our routes!
Here we have our devise routes that was created when we set up our devise gem. We changed our movies routes to have the reviews routes nested inside. The only route we will not be using for reviews (unless you want to) is the index page. For the users routes we added only: [:show] because I only want users to have a show page that will list all the movies they have reviewed. You can ignore the collection do get 'search' end for now. We will implement our search feature later.
Run rake routes in your terminal to see your newly created routes and nested routes.
Bravo! Nested routes for the win!Go to your reviews controller and addbefore_action :set_movie the the top and add theset_movie methodunder private.
for the create action in the review controller add movie.id to the review upon creation.
For the show and edit action add the following.
For the show action we are saying if the @review.user_id is EQUAL to the current_user.id show them their show page with their reviews. Otherwise go back to the movie path. For the edit action we are saying if the @review.user_id is EQUAL to current_user.id show them the edit form otherwise redirect them to the movie path.Remove the paths for now. This is the app/views/reviews/edit.html.erb and app/views/reviews/new.html.erbHopefully you have at least 1 or 2 movies created right now. If so go to the url localhost:3000/movies/2/reviews/new to get to your review new form. If you find that once you create a review you get an error it might be because in your reviews controller you don't have your create action redirecting to @movie which is the movie_path. You can do that now because the review routes are nested inside the movie routes. If you get redirected to the movie after hitting sumbit on your review creation it's working properly. You can always confirm in your rails console by typing rails c in the console and checking @movie = Movie.find(insert the movies id). and then @review = Review.last and checking to see the movie.id matches the movie.
app/views/movies/show.html.erb
Add this <%= link_to "write a review", new_movie_review_path(@movie), class: "btn btn-danger" %> to create a button a user can hit to take them to the new review form.app/views/reviews/new.html.erb
Add this path so a user can have the option to go back to the movie they were on if they don't want to create a review.
app/controllers/users_controller.rb
This controller is for the users to see their reviews and only them. Through the before_action :authorize_user .
app/views/users/show.html.erb
The link for this will be in the nav bar look below for code. Here we are giving the user access to movies they reviewed. They are able to click the image of the movie they created and it will redirect them to the movies shows page. We also add the .uniq to the loop just incase they make multiple reviews for the same movie their user show page won't repeat the same image.app/views/layouts/_header.html.erb
<% = link_to "Your Movie Reviews", current_user %>app/controllers/reviews_controller.rb
Make sure you have this in the show action. This allows for only the current_user to see their show page.
app/views/movies/show.html.erb
You should have the right row for the reviews set up to something like this.
<h5> Posted by: <%= link_to "#{review.user.username}", movie_review_path(@movie, review) %></h5> will take the user to the nested review show page. Your rating should work because we have the blank stars method in the review model. Here is the documentation for the star rating. https://medium.com/@toodimes/how-to-implement-a-simple-5-star-rating-system-in-rails-using-native-bootstrap-48d5be205fdcOr you can have something like this if you don't want stars for ratings.
add another <% end %> after the first one.app/views/reviews/show.html.erb
app/views/reviews/edit.html.erb
Add these paths so a user can have the option to go back to the movie they were on or delete a review.
Your movies controller should look like the following
For the movies show page we have the reviews matching the movie id to go down the page by "created_at DESC".Congrats! Everything should be working accordingly!
SEARCHKICK GEM TIME!!!!
searchkick | RubyGems.org | your community gem host
RubyGems.org is made possible through a partnership with the greater Ruby community. Fastly provides bandwidth and CDN…rubygems.org
Instructions on how to install.
Searchkick handles:
stemming - tomatoes matches tomato
special characters - jalapeno matches jalapeño
extra whitespace - dishwasher matches dish washer
misspellings - zuchini matches zucchini
custom synonyms - pop matches soda

Add collection do get 'search' end into your nested movie/reviews routes.
Add this to you app/views/layouts/_header.html.erb
starting from line 29.Add a view for the search form now!
THANK YOU SO MUCH FOR READING! I HOPE THIS WAS HELPFUL! GOD BLESS!
