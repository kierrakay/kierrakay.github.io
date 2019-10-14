---
layout: post
title:      "'#Project Portfolio: CLI Data Gem'. "
date:       2019-10-14 00:55:33 +0000
permalink:  project_portfolio_cli_data_gem
---


Hello! endangered_animals is my first portfolio project for the Flatiron School: Full Stack Web Development program.

My CLI Gem "endangered_animals"  is a CLI Ruby Gem that scrapes (https://www.worldwildlife.org/species/directory), to give the user data about a number of endangered animals in the world. This gem provides a CLI (Command Line Interface) for the user to choose their preferred animal to get a detailed list of information about it.

The project was a rewarding nightmare. I had no idea what I wanted to base my project off of and I was emmensly overwhelmed. Being put on the floor to test out the knowledge I've gained in these last 8 weeks made me extremely nervous, yet excited. I was not confident in myself at all but I dived in head first with the intention of completing my mission. Although this project is small in the world of IT it was a big first step for me. 

Not knowing where I wanted to start in my project I proceed to follow a tutorial video by Avi to get started, (https://www.youtube.com/watch?v=lDExWIhYKI). As Avi suggested, I outlined the interface and noted down what I wanted of the application first. To create the Ruby Gem, it was recommended to use bundler. Bundler will set up the architecture of the application, ensuring that the files and directories layout are properly set up. While following Avi's instructions I built my first working CLI gem.. until I broke it. I'm not sure how I broke it but it wouldn't load approprietly. I had spent countless hours getting this gem to work to end up with it breaking. I trashed it and started over. I realized that the way he had me set the gem up was too confusing for me to understand so I built a new one that lined up with my level of knowledge. This step worked for me and now I have a new working gem. I'm very proud of myself for not giving up and succumbing to my own self doubt and frustrations.  

The application uses Open-uri and Nokogiri. These are Ruby Gems that made it possible for me to access and scrape the World Wild Life website for information on endangered animals. Many programmers use debugging tools such as "byebug" or "pry" to help them decipher errors.  I on the other hand do not know how to properly use these tools. It would have made this project easier for me had I been able to utilize these them but I still managed to complete my task. This is one skill I am hoping to polish in the future. Especially if I'm ever going to need assistence with selecting the appropriate CSS for an HTML. 

While completing this project, there were many days I stood up way too late trying to fix ONE bug alone. I learned that although I have no problems asking for help I did struggle with using Google as a resource for me. I would sit and stare at my screen and try to figure out how to fix a bug/error I had never seen before based off the little knowledge I had. I did not know what to type in a google search to help me. I do not recommend this. I remember being told that we are basically "Professional Googlers". When I finally got the courage to step out into the world of Google I began to learn some really useful tips for fixing my current and future projects. Definitely use google and open forum sites!

One major issue I had with this particular project was scraping. I wasn't able to do the scraping lessons beforehand so I went in pretty blindly. One major issue I noticed was that the elements on the animals pages were all different. Meaning how I set up my scraper method was going to have to be super unique. One area where I scraped details for an animal had 7 elements and the next animal would have 4 elements. Meaning that I couldn't use the elements index to scrape details. I had to brainstorm how to gather specific elements without using their indexes.  I ended up searching in google how to extract elements from a 'UL' and pulling specific children from the "UL". This ended up working for me and I'm so glad! Now the user will be greeted with a greeting, given a prompt, given the ability to select an animal, and read details about it. 

This was a great first project! Although I was frustrated that I couldn't use the "ask for help" button on learn.co I'm glad we weren't given that option. It really forced me to go out of my comfort zone. Trial and error are key to programming. Our job is to fix what is broken. We are hired to research and explore. We are the ones that brainstorm and execute. This project really gave me a taste of what programming is all about and I love it!


