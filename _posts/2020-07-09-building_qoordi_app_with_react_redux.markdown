---
layout: post
title:      "Building Qoordi App with React / Redux"
date:       2020-07-09 20:58:09 +0000
permalink:  building_qoordi_app_with_react_redux
---

### My Flatiron School Capstone Project

Qoordi is an app that helps users coordinate and plan for projects where multiple things need to happen at once by creating a visual interface that represents multiple timelines. This solution is inspired by my background in event videography, where I would have a team of multiple shooters working separately to capture different things happening simultaenously. I always wanted a way that could visually represent these different timelines side by side and have the interface be faster and more interactive than creating a spreadsheet.

![](https://i.imgur.com/MbfnK4H.png)

The UI precendent I used is Google Calendar because it allows users to create and move blocks of time in increments, and have those changes be persisted as data corresponding to specific times. 

## Highlighted Features

### Interactive Timeline Interface
![](https://i.imgur.com/eDoLFju.gif/)
![](https://i.imgur.com/tISl3Mk.gif)

### Filtering Projects by Category
![](https://i.imgur.com/YAcDyLT.gif)

### New Project Pop-up Form
![](https://i.imgur.com/knA0j9m.gif)
## Challenges

### Controlling the block from the outside (Working around React’s limitations)

One of the beautiful things about React is the way it tries to keep logic and data close together in components. But, when it comes to interacting with the DOM and attaching event listeners to components, this closeness can present challenges.

One such challenge I discovered comes when tracking the onMouseMove event. Because users move their mouse very quickly, the mouse can venture outside the component that is listening for that event for a moment, breaking the event. In the case of my calendar event blocks, this meant that it’s difficult or impossible to smoothly control the block and have it snap to increments using only the mouse movements within the block itself. To solve this, I had to create a conversation between the block being controlled and its parent component, so that the initial mouseDown event sent information about the block and mouse’s position to the parent, and then as the parent tracked the mouse’s movements it would make changes to the block.

### Creating ‘invisible updates’ for a Smoother UX

Each block of time is rendered using the entire instance of the event it represents, including its start and end time. This means that as it moves on the screen, it is not merely calculating the change in the DOM as a pixel distance, it is first changing its start and end time in redux, and then calculating its position on screen as a result. This allows every movement to be completely up to date in the redux store and display that information in the sidebar. 

The challenge with this is that because there are so many changes happening per movement, it would be very slow to communicate with the server to create every change in the redux store. So, in order to maintain a good user experience without losing functionality, I saved these micro changes in the redux store client-side as “proxy” changes, and had a separate action that persisted the endpoint of the change when the user stopped moving or resizing the block.

## Takeaways

### New Skills:

Since this was my first React / Redux project, it was for the most part an exploratory mission and a lot of simple features are things that took me a bit of research to figure out. 

In this project I learned to:

* Create a client-side redux ‘database’ by converting a nested API response into a normalized data structure based on [this model by Dan Abramov](https://redux.js.org/recipes/structuring-reducers/normalizing-state-shape)
* Setup a client side session in redux using the [react-redux-session](https://www.npmjs.com/package/redux-react-session) library.
* Create protected routes using react-router-dom
* Incorporate Material-UI icon components into a React project and style their size and color using css. (My method was to set both color and fontSize to inherit and wrap them in a span with a css class that provided their style)
* How to create a pop-up form with transparent overlay


### What’s Next?

The next features I would like to build for this app are:

* Allow users to share projects with each other and collaborate in comments.
* Give users the ability to create more than two timelines, and subscribe a single event to multiple timelines.
* Create a mobile view that reconfigures the sidebar into a bottom navigation row.
