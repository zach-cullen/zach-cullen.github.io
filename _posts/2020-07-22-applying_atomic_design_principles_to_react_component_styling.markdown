---
layout: post
title:      "Applying Atomic Design Principles to React"
date:       2020-07-22 16:58:23 -0400
permalink:  applying_atomic_design_principles_to_react_component_styling
---


## What is Atomic Design?

Atomic Design is a metaphor for design systems created by Brad Frost in and presented in 2015 to describe the varying levels of complexity at which design decisions are made. It attempts to answer the question: 

“What are the web’s Lego bricks?”

The core idea is that digital products are like complex organisms composed of simpler elements in a hierarchical chain of compositions getting less and less complex until we reach an irreducible ‘Atomic’ level. 

Watch this:
[“Atomic Design” by Brad Frost—An Event Apart Austin 2015](https://www.youtube.com/watch?v=W-h1FtNYim4)


## Why Should Developers Care?

Modern Front End development very closely mirrors the concept of Atomic Design. In React, our ‘Lego bricks’ are called components. A React component is a composible element that encapsulates data, style, content, behavior, and functionality. Components are nested within each other and vary in complexity.

Because React is non-prescriptive about the way we add CSS styles to our components and the way we structure our files, we need to organize our projects in such a way that reflects both good design principles and utilizes React’s uni-directional data flow pattern to ensure that changes to the interface are introduced at the correct layer of complexity. 

#### The Balance: DRY vs SRP

Good programming principles require us to write code that is DRY (Don’t Repeat Yourself) in order to increase the maintainability and scalability of a project. When we need to be able make a change that effects our product broadly, we want to be able to make as few changes to our code or data as possible to create that change. 

However, we also want to follow the Single Responsibility Principle (SRP) in order to make sure that the most fine-grained changes will not break others.

> “We’re not designing pages, we’re designing systems of components” - Stephen Hay



## What Does the Atomic Design Hierarchy Look Like?

![](https://bradfrost.com/wp-content/uploads/2013/06/atomic-design.png)

### Atoms

Frost argues that the smallest level of design decision is the Atom, these are building blocks that are supposedly irreducible in complexity, and are not useful in isolation. Examples of atoms are buttons, labels, and input fields.

### Molecules

A molecule is a composition of atoms, such as an input field containing an an input, a button, and a label. These three elements are grouped together visually, and serve a single programmatic purpose such as serving a piece of content to a user, or collecting one piece of information from them.

### Organisms

An organism is a composition of molecules, and can solve an entire user problem such as allowing them to navigate a website’s many pages, scan a gallery of images, or sign up using a complex form. 

### Templates

A template is a composition of organisms to create an entire view or page of a product. The template will present the user with options of what to do next. However, when we’re thinking of at the template level, we’re not yet thinking of our product in terms of specific data. You can think of a template as a species rather than an individual. 

### Pages (Hydrated Content)

A page is a template that has been hydrated with real content. It is like an individual person rather than the species Homo sapiens. it contains real data and allows us to answer questions about edge cases and capture feedback from real users.


## Two Critical Additional Layers

### Particles: Subatomic Design Systems

Frost’s Atomic design model had a major impact on the discussion around design systems and what is digital design’s deliverable product. But, in 2018 designer Daniel Eden posited a new take on it called Subatomic Design Systems that proposed Frost’s idea of Atoms was not small enough. 

When we think of a Button, for example, we are still talking about a composition of simpler primitives: Color, Shape, Size, Positioning, Shadow, Animation, etc. Eden calls these simpler primitives ‘Particles’. 

It is critical to recognize this fact, because in creating a consistent Design System (and a good programmatic representation of one), we will be sharing as many of these primitives across different components as possible. This means we are using the same colors, size and spacing increments, animation timings, and even cornder-roundedness to compose many low level components. 

[Subatomic Design Systems by Dan Eden](https://daneden.me/blog/2018/subatomic-design-systems)

### Environment

Just as every individual exists and changes according to the environment it is inhabiting (for example you may wear a tux to a wedding and a t-shirt at home), every digital product has a context created by the device it is being used on, the preferences of the user, and the data that the program has or doesn’t have. 

This is where we must think about Responsive Design. The context of an application has the potential to have the most dramatic effect on its composition. A page may completely rearrange itself based on the width of its screen and still serve the exact same purpose using the same basic elements. 

However, it is not the case that a change of context won’t effect the smallest level of a design. For example, a change of context from dark to light mode may have no effect at the macro level, but instead it will alter only the colors (which are subatomic primitives) being used. 


## How to Structure a React Project to follow Atomic Design

### Guiding Principles

The insight we gain from incorporating these ideas is that the highest level design decisions impact the lowest level styling of elements, and the lowest level decisions about our design will have the broadest impact across our product.

Therefore, we need the most universally accessed variables to be provided at the top-most layer of our application in the form of a Theme that provides color variables, sizing primitives, typography, and animation types. 

We also need to ensure that components at every layer of the application need to have access to context information such as screen breakpoints and color modes. 

Lastly, we want to make sure that every component is only responsible for styling itself, and composing its children. So, that means every component will need to have its own isolated set of CSS styles. 


### Ways to Implement Atomic Design in React

**1. Wrap Your App in a ThemeProvider**

Installing a package like Styled System or Styled Components will provide you with a simple ThemeProvider component that you wrap your entire app with in order to access variables referring to CSS styles across components. This can enable you to add contextual styling such as dark mode to your app or allow you to make sweeping design changes with only a single change to your code. Gone are the days of hunting through css files to make 400 changes in order to have all of your buttons look the same. 

[Styled System](https://styled-system.com/)
[Styled Components](https://styled-components.com/docs/advanced)


**2. Use Tokens**

Tokens take the abstraction of your Theme up a level by storing these primitive values in a JSON object that can be referenced by multiple different apps to create a consistent look without having to translate those styles into a different data structure for different environments like iOS vs web. 

https://css-tricks.com/what-are-design-tokens/


**3. Provide Breakpoints as Props**

Some design systems such as Chakra-UI and Rebass that are based on Styled System allow you to provide an array of style props to a component corresponding to the different breakpoints. This can be a very fast way to programmatically and consistently change the size of text as your user resizes the page or views it on a different device.

https://chakra-ui.com/responsive-styles
https://rebassjs.org/props/

One challenge though, is that they don’t support style prop arrays for major changes like re-arranging your grid. For that you will need to look another package like Atomic Layout (not coincidentally named), which provides hooks like useBreakPointChange and useResponsiveValue that you can use to create specific changes at key breakpoints while maintaining the values of those breakpoints at the Theme level

https://redd.gitbook.io/atomic-layout/api/hooks


**4. Use CSS in JS (e.g. Styled Components)**

As I previously mentioned in #1, using styled components allows you to programmatically change the style of your components without relying on a library of CSS Classes and stylesheets. This means you can dynamically alter the style of an element using props. This is useful for everything from composition and sizing, to creating data visualizations without relying on a heavy library like D3. 

It also allows us to encapsulate our styles as close as possible to the level of complexity they effect, and isolate those styles from other components. This means we can make breaking changes to one component (such as deleting styles) and mitigate the side-effects of that change. With the help of a library such as Styled Components, we can also make our code easier to debug by programmatically creating unique class names for each component, preventing global naming conflict.  

**5. Group Components By Complexity**

Because React uses ES Modules, it really doesn’t care how you organize most of your files. So, this last one has the least effect on the performance of your program, but I find it very helpful from a developer experience perspective. 

The goal is to make highly reusable components easy to access, and more specific components easy to find. We want to avoid having to dig deep into directories to find components, and we want to avoid developers re-inventing the wheel because they are unaware of what is already built.

So rather than creating one folder called Components and placing everything in there, I like to separate my components based on their level of complexity and specificity. 


#### Particles
* Store as tokens in their own top level file
* Reference tokens in ThemeProvider that wraps our app’s top level component

#### Atoms
* Components Folder at Top Level

#### Molecules
* Molecules are often reusable so I also place these within Components but sometimes group them by type using subdirectory or (e.g. Forms, Grids)
* If you have many variations of a single molecule, try to see if there is away to dynamically change a single molecule by providing it a prop indicating where and how it is being used. 

#### Organisms
* Create a top level folder called Views
* Create groups of organism types in subdirectories such as Forms, Navigation, SideBar, Photos, Video, Galleries, etc. 

#### Templates
* Depending on the framework you used to build your React app, you may be restricted to a certain format for this. Either way, I recommend having a top level folder called Pages that corresponds to your app’s routes. 
* This pattern is one reason I prefer Next.js over Create React App. Next dynamically creates routes based on the files in your pages folder, which just feels more intuitive to me. 


## Conclusion

This is just one way of thinking about things, and there are a million different ways to implement these ideas in React. But, I think that the trend of creating cross-platform design systems is here to stay. And I love when we can pull metaphors from nature because these ideas tend to be the most evergreen and valuable across disciplines.








