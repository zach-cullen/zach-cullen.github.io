---
layout: post
title:      "Building a Workout App with Ruby on Rails"
date:       2020-04-07 22:05:59 -0400
permalink:  building_a_workout_app_with_ruby_on_rails
---


One of the things I spent the most thought on was creating a map of my object relationships. It's important to get this right since it is the foundation of all your data. Once you have a solid foundation, you can fly through features and have a lot of fun coding them intuitively. If you make your model overly complex or too simplistic, you will have to work a lot harder to figure out how to build every single feature down the line, and you'll set yourself up for creating bugs.

## Join Models
Sometimes a join model exists purely for the purpose of creating a many-to-many relationship. In that case, it is invisible to the user. Other times, the join model between two objects is the main place a user is going to interact with the program and the background models are invisible. 

For example, in my project there is a relationship between a Workout and a Metcon (a type of workout component). Many workouts may use the same component, and a component would then both belong to and have many workouts. The user visits a workout and can see the components that have been added to it.

Next the user wants to score their workout, so we build a join on this join called scores. The score belongs to a user and a user has many scores. But the user's score is not the only score for a workout component, since there are other users on their team. Thus, we have another many-to-many relationship built on a join model. 

Over time, your object relational map can start to look like a spiderweb, so it's really important to draw it out. There are often many different ways you can join the data, just like there are many ways to solve a maze. However, you want to be strategic about how you do that. Here are a few considerations to optimize for:

**1. Single Source of Truth. **

You don't want to be replicating the same data all over the place since it will get complicated to make sure it is all updated. Apart from the id's that reference a particular row in another table, you want to make sure there is only one place any unique piece of data is stored.

**2. Less empty columns**

If you build your models too big so that they contain multiple variations on variations with incapsulated logic, you will end up with a lot of empty columns and a huge database since many of your columns will only apply to certain rows. This also makes it tougher to validate and handle each case. While building my project, I ended up doing a large refactor in order to separate the types of workout components since the map had become too simplified and the code too complex. 

**3. Shorter Chains**

When you are deciding where to connect something (like for example the score model I mentioned), connecting to a join can create shorter chains since each piece of information (like the workout id or the component id) can be retrieved directly from that object rather than digging through layers of an object's associations multiple times.

