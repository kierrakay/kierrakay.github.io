---
layout: post
title:      "'Bind,Call, Apply..huh?'"
date:       2020-09-14 18:28:33 +0000
permalink:  bind_call_apply_huh
---

Bind, Call, Apply…huh?
Ok, so I'm going to explain these methods and how they're used. I'll first start off by saying I wanted to avoid these concepts completely because for some reason I thought they were going to be overly complicated…I later found out that's not the case so let's talk about it!
Traditionally objects have properties and methods. In Javascript you can create objects with properties and use bind, call or apply on separate methods. This allows you to create a common method for multiple objects.
Important note: Apply and Call execute a function that will return a value immediately. Bind creates a new function that you can execute at a later time.
First Ex: Call
We have an obj with a property/key of num with a value of 4. Line 5 is a method that is supposed to return a + num. As we can see above our object doesn't have a method and our method doesn't have a property of num. However it does have "this.num".If we want to use our new method with our object we can use the CALL method.
Notice line 10. This is the proper syntax for using call. You want to first call the function name which in this case is "addToThis". Then you chain ".call" to it. In parenthesis you put the object name which is "obj". The last part is adding in the data you wanna use for method you created. We are just adding the data we want to add to this function. Because 4 and 2 are numbers JS will add them together by using this.num to get our result of 6. If we replaced 2 with "John" we would get a result of "4John".Here we have two separate objects and one function. We called the "addToThis" function on the "obj2" object! The property of obj2 was accessed using this.num and the argument of 10 was added to it. So "a" is 10 and "this.num" is 3.We can also have multiple args in our function.
Second Ex: Apply
If you want to use multiple args you can use apply or call to return a value. The only difference is that with apply you can only use it with arrays. Whether you hold your array in a separate variable or input your array directly into the callback .
ex 1ex 2Third Ex: Bind
Remember that when using bind you are creating a function that binds an obj to be executed at a later time…lets take a look!
We had to call our new bound function with args in order to get our desired results.
