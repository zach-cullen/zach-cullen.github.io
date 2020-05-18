---
layout: post
title:      "JavaScript with a Rails API"
date:       2020-05-18 14:57:30 +0000
permalink:  javascript_with_a_rails_api
---

For my JavaScript with Rails API project I made an app that allows users to create and practice Decks of Flash Cards. On the back-end of the app is a simple CRUD model where Users have many Decks and Decks have many Cards. This heavily relied on using nested resources to easiliy pass the user and deck id's as params for validating and building associations. 

## Starting with Auth

I challenged myself to start with Auth for this project, since I wanted users to be able to create their own private decks. The difficult part to wrap my head around was how to use the idea of a Session on the JavaScript side since there isn't an obvious way to parallel. By creating a service class called Auth to manage the current user, submit the login form and clear the data on logout, I learned how to implement separation of concerns in OOJS. It also pushed me to learn about CORS, which stands for Cross-Origin-Resource-Sharing. It's a pattern that is implemented on the server side to protect its resources by requiring HTTP requests from Clients to include credentials that it provides on every request, and also making sure that the origin (the client address) is whitelisted.

## Always Return Your Promises
By far the most common bug I had at first was writing promise-chained functions that forgot to actually return the promise at the top level (I would instead only return the .then callback function). But that error quickly became easy to spot. What was harder to grasp was the way that .then works and how to make sure that I was forcing JavaScript to operate in a procedural (i.e. synchronous) manner when necessary. In other words, making sure that my DOM doesn't re-render before it has the data. The key insight was learning that a series of .then statements chained together did not force those statements to wait for each other to finish, it only meant that they would start one after the ather as soon as the fetch promise was resolved. This meant that if any .then statement in a chain also returned a promise, it would end up being executed after the other .then statements. Learning to place the re-rendering in the correct place required me to at first place a console.log in every single function so I could read the console like a timeline. The pattern that I found most maintainable was to call as few .then statements for a given fetch as possible, and abstracting later steps into their own function. 

## Maintaining Multiple Views on a Single Page
Another concept that I had to wrap my mind around was creating a single page application that had multiple views. A single page application is one where the URL doesn't change, but behaves in a way that a user expects. You are essentially serving up an initial html template, and then re-writing the html in it by manipulating the DOM. There are a few different ways to handle this, but the method I chose was to use JavaScript 'component' classes that would provide html template strings to the DOM service class. Once I got the hang of it, it actually felt like an easier-to-maintain version of rails .erb templates. The beauty is that you can use the functional nature of JavaScript to encapsulate logic and create nested templates for rendering. 

The difficult part was coming up with a pattern for keeping track of which html templates to use and also track the model data that was relevant to a page. For this, I created a 'State' service class that had an object called currentView that had a string title of the currentView and an integer 'id' of the model currently being used. Then, I defined functions for setting the view to preset options so that the codebase would be more DRY and maintainable. 

## The 'Practice' Model

My favorite part of desigining this project was making the Practice model - which was essentially an instance of a game where the user reviews their flashcards and marks whether they answered them correctly or not. It required thinking in a new abstract way about Object Orientation that didn't have any connection to a persisted object. I also had to figure out how to create a practice session that would belong to a certain deck of flashcards, know which card to load next, keep track of correct answers, and shuffle the deck differently each time. Ultimately I decided to make the practice session instance be created prior to 'saving' a deck to my JavaScript content as an attribute of a deck. This allowed me to only have to keep track of the deck I was practicing, and access its practice session by simply calling deck.practiceSession. 

## Creating a Card Shuffler using Fisher-Yates Algorithm

Learning to properly shuffle my deck of flashcards required a little research, which uncovered the Fisher-Yates algorithm. Instead of just copying the code, I challenged myself to really understand it by writing out thorough comments, and stepping through the code in a Repl first. Here's the block of code I used with comments: 

```
  // returns an array of randomized integers representing indexes in array of cards of cardCount length
  static makeShuffler(cardCount) {
    // generate array of ints representing indexes of card array of cardCount length
    const arr = Array.from(Array(cardCount).keys())
    // implement Fisher-Yates algorithm 
    function shuffleArray(a) {
      let array = a
      // start at the end of array and count down
      for (let i = array.length - 1; i > 0; i--) {
        // generate a random number between 0 and i - 1
        // this number represents the index of a random position in an array of length i
        const j = Math.floor(Math.random() * i)
        // store the value at i of our original array because it's about to be changed and disappear
        const temp = array[i]
        // set the current value at i in our base array to the value at random index j
        array[i] = array[j]
        // set the value at random position j to the value we captured from base index
        // effectively, the two values have now 'swapped'
        // in the next loop, the random value we 'swapped out' will not be available for exchanging 
        // because our random integer selection decreases as we count down to no longer include the index it has moved to.
        // The number that was at position i is still in the mix because it was 'swapped in' to a lower index
        array[j] = temp
      }
      return array
    }

    return shuffleArray(arr)
  }
```

Here's a reference to the source code I used as well:
https://medium.com/@nitinpatel_20236/how-to-shuffle-correctly-shuffle-an-array-in-javascript-15ea3f84bfb

The reason I encapsulated the shuffling algorithm in a function that returned its own instance of an array rather than applying it directly to the array of cards, was that I wanted to be able to track the progression through the practice deck linearly, but always refer to the actual deck of cards in a shuffled way so that I wouldn't mutate the original array of cards. So the practice session would create an array of indexes as an attribute of itself each time it was instantiated in the constructor as:     this.shuffler = PracticeSession.makeShuffler(cardCount) 

Here's a bit of code showing the implementation of the shuffler in action, where the current Card is accessed with a function on the Deck level.

```
  currentCard() {
    // shuffled is an array of integers 0..cards.length that has been shuffled randomly
    const shuffled = this.practiceSession.shuffler
    // currentCardIndex progresses from 0 and counts up normally as the practice Session progresses
    const i = this.practiceSession.currentCardIndex
    return this.cards[shuffled[i]]
  }

```
