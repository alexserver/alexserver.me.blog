title: "React or Ractive"
tags:
---
If you ask Google about a comparision between React and Ractive, you won't find that much about it.

The best link I got came from Ractive's [blog](http://blog.ractivejs.org/posts/whats-the-difference-between-react-and-ractive/).

## Prehistory

I started working with Ractive before hitting React source code. I came from jQuery mindset, and it blew my mind.
Before Ractive, I used to change DOM content in 2 ways:

- Asking the HTML from the server (usually PHP with template engine in the server), and replacing a container with `$(selector).html(content)`.
- Asking JSON from a REST API, and compiling the template in frontend, using mustache or a simil library, to finally, replace the container with the same jQuery instruction: `$(selector).html(content)`.

You would see this as a barbarian technique, but, hey, it was 2010 and Ajax was the goal. Templating and DOM manipulation was not a big problem. Or that's what we thought.

The problem was handling DOM events for all of components that were destroyed and created very often. On the way we learnt to use event handlers with sub selectors instead of the `made-for-begineers` jQuery `$.click`, `$.change` and many more event aliases.

## Ractive
Really Reactive
2 way binding
Observers
Troubleshoots
Performance

## React
1 way approach
Oh boy it is fast (javascript is fast, DOM is slow)
Flux (License Agreement)
Write twice, who cares, when website is faster ? $$$
For developers, not for designers

## What kind of priority do you have ?
Do you urge to build a prototype ? use Ractive
Do you work with non-technical web designers ? use Ractive
Do you really really care about load time response? use React