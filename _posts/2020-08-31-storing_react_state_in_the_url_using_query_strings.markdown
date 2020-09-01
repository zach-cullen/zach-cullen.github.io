---
layout: post
title:      "Storing React State in the URL using Query Strings"
date:       2020-08-31 23:45:26 -0400
permalink:  storing_react_state_in_the_url_using_query_strings
---


## When to Store State in the URL

You may be thinking: Why would I want to store state in the URL rather than using Redux or a React class component? It's a valid question. The URL is just a string, after all, and that is a pretty limited data structure. But, there are some cases where it comes in handy. 

### 1. To Keep Redux 'Clean'

If you are using Redux, you are creating one overarching state that any component within your top-level Provider component can 'connect' to. There are no rules about what aspects of your application's state you can and can't keep in your redux store, but the [general guidelines](https://redux.js.org/faq/organizing-state#do-i-have-to-put-all-my-state-into-redux-should-i-ever-use-reacts-setstate) are that you only want to keep data that will be shared across components in redux. Otherwise, you want to keep that data as close as possible to the components that actually care about it.

Many developers prefer to keep their redux free of any UI state, and have it only contain data representing back end models and information pertaining to the session or user. 


### 2. To Avoid (Too Much) Prop-drilling

Many times we have information that needs to be shared across components that we don't necessarily want to keep in our Redux store. One React pattern for achieving this is to store shared state in a "container" component, and then pass prop functions down to child components so that they can set the state of the parent. The parent then provides its state down to children as props. 

According to [this article](https://kentcdodds.com/blog/prop-drilling) by Kent Dodds, prop drilling is great... until it's not. Sometimes, as an application grows you will have to pass these props through many layers of components that have nothing to do with the interaction between the container and the child component controlling its state.

A common example of this is when you have a view that displays a collection of components and you want to filter or sort that collection using a button or input field. The two components will be on separate branches of the component tree, but they will need to share information. Additionally, you may have multiple components within this view that depend on the same information to render themselves. 


