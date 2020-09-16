---
layout: post
title:      "'React/Redux problems I faced.'"
date:       2020-09-16 23:12:25 +0000
permalink:  react_redux_problems_i_faced
---



I'm going to explain a few issues I ran into in my project and how you too can overcome them.
Problem One:
The browser cannot access props on the first render.
I have my routes set up as such:
I'm using switch so that each path is going to exactly that and that also these components can only be accessed through the URL. Lines 21–22 are virtually the same path so it's important that you put new before :id because if you switched them :id could just as easily be seen as new. Again, we are no longer rendering the components directly. They are specifically rendered based on the URL. That is the only way to see the components.When creating the blog's show page I noticed that on the first render my props were undefined with the route below.
<Route path='/blogs/:id render={() => <Blog blogPosts={this.props.blogPosts} />} />
That is because for line 22 we are sending all of the blogs to the Blog Component through a URL. In order to get a specific Blog we need to pass in {…routerProps} to match the :id in the URL that our Blog component can use to render a specific Blog.
We pass those router props in here on line 9.Now that we have those router props we can use them to give our component our props. We have to use a ternary for this. I set up a variable blogPost that gets our regular props and our router prop of id. We call params on props.match. Params are holding our id for our blog. We plug in "-1" because redux starts at 0 and id in our backend start at 1. This is how we compensate for the difference.
After that, in order to access our props to render to the DOM we first check that the blogPost is a match in our ternary which it won't be so it runs null first. It's not until the second redirect that it can access the props we desire to show.
Second Problem:
Not being able to delete comments associated with a blog and also have the idx of a blog still match once a blog gets deleted..
When I tried to delete a blog it was rolling back in my DB.I couldn't figure out what was the cause of this until I saw "ActiveRecord::InvalidForeignKey". This triggered a memory I had when I was working on a previous project. When you delete a record that has_many associated records you need to remember to add one small detail to your model.
dependent: :destroy
By default, those associated records are left in the database. If you want associated records to be removed when their parent is removed, you can include the dependent: :destroy option to the has_many association in the model class.
This allows for everything to be removed from the DB.Now, I was still running into a problem with my blogs.
Remember I stated that we only wanted to access components through the URL? Well, previously passing props to the components worked fine this way until I went to delete a blog.
Two blogs being held in the BlogsContainerOne comment added to the blog with the id of 1.DB is correct.State is correctNow when I delete a blog this was happening.
I can add a new blog no problem.When I went to click to comment on the blog with test 2 it instead rendered the blog after it which is the new blog test 3. I also can't submit a new comment to this blog. Notice the url.The first blog successfully deleted from our DB and blog with id: 2 is suppose to be rendering test2 for title and component so why is it rendering test 3?Here I discovered that idx and id were different.I thought ok well maybe my state isn't updating in the Blogs Container with my ComponentDidMount() method.
console.log confirmed that wasn't the case.So if my method is correct, my DB is correct and my Redux dev tools are showing my props correctly besides the idx and id issue…what am I missing?
I tried adding state to my NavBar component and figured maybe I needed to add props to my Blogs Container route there but that didn't make sense because I was already using MapStateToProps there and passing props to the Blog and Blogs component. I was also already connected to the store which accesses state for fetching blogs by using the ComponentDidMount() method.
It hit me that it has to be a routing issue…which it was.
The problem lies here. The only way to access a blog's show page is through a link using a blog's id.Here we were originally trying to access a blog based on the idx in the blogs' array. This is what causes issues when you delete records. The filter approach is better because we are creating a new array by finding that blogs' id and grabbing the first and only existence of it.idx and id are still different but that is normal.Now we are successfully rendering the proper URL with the proper blog.Those were the two issues I ran into and I hope this was helpful to anyone else who may run into these issues!
