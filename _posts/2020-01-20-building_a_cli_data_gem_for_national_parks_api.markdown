---
layout: post
title:      "Building a CLI Data Gem for National Parks API"
date:       2020-01-20 17:52:44 +0000
permalink:  building_a_cli_data_gem_for_national_parks_api
---


Completing my first code project for Flatiron School was a big step in my journey to become a software developer. While I have really enjoyed the lessons and labs up to this point, I was excited to design my own project and build the entire thing independently. I was challenged to set up my local environment, learn the git and github workflow, solidify my understanding of object oriented programming, and to dive into documentation for a the public API I chose. All of these experiences built my confidence as a programmer and made me feel ready to progress into some more complex concepts. 

### The Key Thing About API's

The first step in the project was choosing a source of data to interact with. Because I live near Shenandoah National Park and love to go hiking there every week, I searched for "Free API National Parks" and found that the National Park Service has created an API of its own with great documentation (https://www.nps.gov/subjects/developer/api-documentation.htm). The purpose of this API is to create a single source of truth for the entire National Park Service in order to provide up to date information about Parks, their hours, weather, conditions, addresses, campgrounds, and even staff. I can see this being a really great tool for keeping various informational websites to stay up to date, and also for the many businesses that operate in and around national parks, providing tours and other services. 

The first challenge for me, was to figure out how to access the data from my program. So, I signed up to acquire a free API key and read through the documentation to figure out how to access it. This particular API has an option that allows you to append the API key to the end of a secure http request and receive JSON data. I was able to use this technique and modify the url to learn how the data was structured just using my browser before I wrote any code.

When it came to coding my API interface, I utilized the rest-client Ruby gem to make http requests and used string interpolation to insert information into the uri including specific codes for the state and type of information I was looking for, as well as a constant set to the API key that was hidden in my .env file using the dotenv Ruby gem. Lastly, the data was parsed using the json ruby gem in order to return a hash of information about the parks. From this hash, I instantiated new park objects and allowed them to belong to the Park class as well as individual 'state' objects.

### Learning to commit

The second challenge of completing this project was learning the git workflow. This project was my first time working from a local environment and publishing the project to Github. At first, I struggled to make enough commits. I was working all over the place and when it came time to "save my work" I would have to retrace my steps to figure out what I had actually changed.

The breakthrough for me came when I started to keep notes as I worked. Instead of getting lost in my program and thought process, I would write a to do list of specific small pieces of code I needed to write. Then as soon as that small piece passed my test I would commit the changes with the message I had already written.


### Designing Features

Another really fun thing about this project was strategizing how to efficiently guide the user through the program. Because a CLI program offers so few choices for customizing the way the user can interact, the key considerations become using as few words as possible, and making it clear what the next option would be. 

As I worked through the project, there were many features that I wanted to build but it was important to first stick to the essentials and see the project through to completion before adding anything new. Right now, my app assumes that the user mainly wants to see what national parks are available to explore, so I thought that the best way to browse would be by state. That way, if they want to plan a trip to an area they might be able to find multiple parks to visit. In the future, I would like to allow the user to search for a specific park by name. That way, if they want to learn about a specific park they've heard of, like Yellowstone for example, they can look it up without having to know what state or states it's located in. Other features I would like to add are information about campgrounds and visitor centers, which would require making additional requests to the API since that information requires a differnt uri format to access. 



