title: "Modern web apps at frontend masters"
date: 2015-07-01 15:53:15
tags: development
---

# FrontEnd Masters Course
June 25, 2015 to June 26, 2015


## Overview
In the last weekend of June, a friend of mine and myself took a course in order to elevate our skills in web development. The course name is [Building Modern Web Apps Workshop](https://frontendmasters.com/workshops/web-apps/), and it was given by [Henrik Joreteg](https://github.com/HenrikJoreteg), 

## The instructor, Henrik Joreteg
Henry is an active web developer, working at [&yet](https://andyet.com/).
His personal blog is http://joreteg.com/
He has participated in js conferences and given talks in &yet, as you can see [here](https://andyet.com/speaking).
He also has written a book: [human javascript](http://humanjavascript.com/), and contributed in open source javascript framework [ampersand](https://ampersandjs.com/)

## Course content
In these 2 days, participants were able to create a modern web app (one single page app) using new javascript libraries/frameworks:
* React for visual layer
* Ampersand as a backbone framework for the app
* Webpack as an application bundler
* Gulp as a task manager to start the app, track file changing, and app build process.

Henrik sent us an email before the course started to set up a github [repo](https://github.com/HenrikJoreteg/masters) for the workshop. This repo had all the codebase just to start the application, with empty controllers. All package.json configured to run the app from scratch the first day.

The first thing we did was to create a route for the main app entrance and render content with a React class.
```
// router.js
import Router from 'ampersand-router'
export default Router.extend({
  routes: {
    '': 'public',
    'repos': 'repos',
  },
});
public () {
  this.renderPage(<PublicPage/>, {layout: false})
},

repos () {
  this.renderPage(<ReposPage repos={app.me.repos}/>)
},
```
As you can see, this was our first approach with ampersand library. The ampersand library is not a full web-framework, but a losely coupled bunch of libraries that we can use together in our web app. Although they can collaborate together, we are not tied to use all of them in any page we need, which makes our web app lighter in comparision with another heavy web frameworks, such as ember or backbone.

In the web app, we have all React classes responding for pages rendering into `/pages` folder. Each file is a React class that matches with a route page.
For example, to display public page, in the app router we only have to import the React class and use the component into the code:
```
// /router.js
import PublicPage from './pages/public'

public () {
  React.render(<PublicPage/>, document.body)
}
```
and, in the `/pages/public.js` file, we define the React class:
```
// /pages/public.js
import React from 'react'

export default React.createClass({
  render () {
    return (
      <div className='container'>
        <header role='banner'>
          <h1>Labelr</h1>
        </header>
      </div>
    )
  }
})

```
From this point we started to move forward on the application building, adding more pages and code in a horizontal way, and complexity in a vertical way, replacing simple code by more complex but straightforward code.

We learned how to use a React component inside another. How to pass properties through it.
At first glance React seems to be very easy to learn and apply. Even when you put many pieces and make them work together it is not hard to keep up as long as the app complexity raises.

We made our login process with github credentials API.
Github API is very straightforward to use. In the course we learned how to redirect, and get the auth token back. Also we learned how to get a user repos list, and its details.
We extended an ampersand-model to match with github user, in order to help us to maintain session into the app. In this part the complexity raised a lot, and I confess it was hard to me to keep up all these new libraries working together: ampersand libraries plus github auth process and github user structure.
If you want to take a look at the code, here is the [me.js](https://github.com/HenrikJoreteg/masters/blob/master/src/models/me.js) file.
As fas as I can tell at this point. Ampersand has some mixins classes, which help us to couple our own classes with external API classes. In this case github has its own structure for API calls and responses. So the Ampersand classes help us to define a model structure (properties, behaviors), specify a url and it takes care of communication for fetch data.
For example, in this helper, we defined the way it interacts with token:
```
// /helpers/github-mixin.js
import app from 'ampersand-app'

export default {
  ajaxConfig () {
    return {
      headers: {
        Authorization: 'token ' + app.me.token
      }
    }
  }
}
```
And so, we can use it to define what and how to get data from github API:
```
// /models/repo.js
import Model from 'ampersand-model'
import githubMixin from '../helpers/github-mixin'

export default Model.extend(githubMixin, {
  url () {
    return 'https://api.github.com/repos/' + this.full_name
  },

  props: {
    id: 'number',
    name: 'string',
    full_name: 'string'
  },

  fetch () {
    Model.prototype.fetch.apply(this, arguments) //call its super
  }
})
```
This way we can focus on the app behavior and less care about API communication.
The app we were working with in the course is of course more complex than these simple lines of code. If you want to dive in, feel free to checkout the github [repo](https://github.com/HenrikJoreteg/masters) and follow install instructions. And Luke, read the source !

## Was there value found in the course?
By the end of the first day I was not so satisfied with the course. I have to say that Henrik, although he is a high-end web developer with a great background as a javascript dev, he might not be the best in teaching matters. This is why: he was coding and changing between files and windows faster that many of us could follow and understand code that was presented to us the first time.
Fortunately, Henrik, as a smart guy, created a funny npm command called *YOLO* . This command let him at any time, commit and push his code to github so any of us could do git pull and keep up. See by yourself in `package.json`:
```
"yolo": "git add --all && git commit -am \"$(date)\" && npm version minor && git push origin master --tags && npm run build && npm run deploy"
```

I've discovered the way I learn and understand things is this way:
  - *1st pass* The knowledge comes to me, it breaks the wall of ignorance and presents by itself in my mind. At this point, I might not be good enough to see the whole picture, I can only read, imitate, and obey instructions (for example, reading a tutorial, or any npm package instructions on how to install or use a library). This is the introduction.
  - *2nd pass* At this point I start using the new knowledge, typing, calling commands, following tutorials related to the tecnology (for 1 library documentation, there is about 10 other pages in internet that explains the same but slightly different), making errors, finding solutions, and moving on in the darkness towards the light. This is the practice.
  - *3rd pass* At this point somehow my brain start to not just get used to the technology (library names, function calls, build processes), but also can connect the dots, imagine how the technology works in background, or even finding out. I start to see the whole picture and being good at it. This is the eureka moment, or domination of concepts.

So this can happen in my mind in matter of weeks, days, hours or minutes, this is up to the complex of the technology I'm presented to. It can be as easy as an inoffensive function, or as hard as an intimidating framework.

So, I can say I had obviously a complete introduction to the libraries in the course. I had practice for 2 days and many errors. But the part that I can say I'm not satisfied is that I could not reach the Eureka, or full understanding. This is because Henrik teaches by the method of practicing. Do it before you understand it. 

So we were typing in ES6, including React classes, connecting Ampersand libraries but having no strong theory that can support all that typing festival.

I was not happy at the end of the second day of the course. But I'm sure that if I read more about Ampersand, Github API, React and ES6 (which is now mandatory in my job), and then come back to this course again, then I'll find the Eureka moment and have the desired closure for this knowledge.

## Extra Value

Ok, so I paid $100 USD for this course and got not enough value. Should I ask my money back ?
I don't think so.
With the workshop attendance, I also got 6 months mebership to see all past courses given by the company.
And here is where I finf the value I was expecting.
There are a lot of good courses (the ones that I was interested in are more theorical which fits better to me) that can help me to improve my skills in javascript development:
- ES6 course
- Javascript The good parts (given by the self Douglas Crockford)
- Javascript functional
- React
- Hardcore functional programming in javascript
- Lean Front-End engineering
And so many others.

I started learning ES6 as soon as I saw it. I'm currently at 40% of advance right now, and I have plans to finish it this week. Next step is to take React course and move on. And, as always, keep moving and following the source Luke !


