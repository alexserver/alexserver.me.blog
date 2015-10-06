title: How Ractive changed my way to build web apps
tags: 'ractive, javascript, front-end, development'
date: 2015-09-15 06:35:35
---


## What is Ractive ?

Ok, first things first, in order to understand what's going here, you should hit this [link](http://www.ractivejs.org/) and embrace the awesomeness of [Ractive](http://www.ractivejs.org/).
Did you get it ? no more `jQuery.html('<div>some fancy html</div>')` anymore, right ?

## My first approach to ractive (January, 2014)
It was January, 2014, when I entered this new job. 
We were told we were going to disrupt the web apps development.
By that time, I used to called my self a front end senior developer. I felt very comfortable writing web applications, manipulating the **DOM** with jQuery, or vanilla javascript (most jQuery than vanilla, I accept that jQuery was my swiss knife). I was the king on ajax in the local realm (kidding), fetching JSON data from the server, injecting it into a template engine (mostly mustache), and puting the result into a div, dynamically with jQuery; all of this in client site. Yeah, the Single Page Apps era had come.
So, first week in this great job with skillful devs, and I hear about this **'Ractive'** library. I go into the web page, go to the docs, and see this code:

Html:
```html
<div id="container"></div>
<script type="text/ractive" id="scriptid">
  {{foo}}
</script>
```

Javascript:
```javascript
var ractiveInstance = new Ractive({
  el: '#container',
  template: '#templateid',
  data: {
    foo: "bar"
  }
});
```

Ok, not a big deal, right ?. This library had a template part, and a javascript part. And in the javascript code you set the data you want to be rendered. Not impressed.

But then, I started to work with it.
I was commanded to create an interface for the admin section, building forms and data tables, and fancy web controls. Suddenly I started to enter into the Ractive's mindset for working with data and views.
There is no need to touch the DOM, at least not that much for most of the things you want to achieve.

## Reactive templating
My first surprise was when I realized this thing is really **reactive** `DOM`: You write a template once, you set the data once, you start a new `Ractive` instance, and that's it. You don't need to recompile the template everytime the data changes. You only need to tell Ractive: 'Hey, this property called `foo` has changed, with a value of `bar`'. Or: `ractiveInstance.set('foo', 'bar');`. 
Voalá !

So, this code with jQuery:
```javascript
//data has changed
foo = 'another bar';
html = templateEngine.render('{{foo}}', {foo: foo});
$('#container').html(html);
```
Is the same as this with Ractive:
```javascript
//data has changed
foo = 'another bar';
ractiveInstance.set('foo', foo);
```

But the list of perks go further. Ractive goes deeper on objects properties when you set the `magic` option to true. Ractive listens objects accessors to update the template when you change an object property.

```javascript
var model = { message: 'hello' };

var ractive = new Ractive({
  el: container,
  template: '<div>message: {{message}}</div>',
  magic: true,
  data: model
});

// instead of doing `ractive.set( 'message', 'goodbye' )`...
model.message = 'goodbye';
```

## DOM being creating and destroyed on the fly

Another cool thing that Ractive has is the ability to create and destroy DOM elements on the fly, obeying conditionals in the template code.
We don't need anymore to set a `display: none` css style for every javascript event.
Let's say for example that we have a sort of accordion div, with a header and a body which will be shown or hidden every time the head is being clicked.

```html
<div class="head">Click me</div>
<div class="body">
  Now you see me, now you not...
</div>
```
With jQuery your approach would be something like this:
```javascript
$('.head').click(function(){
  $('body').toggleClass('hidden');
  //in Css you have something like: .hidden {display: none;}
});
```
But with Ractive, you go this way:
```html
<div on-click="toggleBody">Click me</div>
{{#open}}
<div> <!-- we don't need classes anymore for logic concerns ! -->
  Now you see me, now you not...
</div>
{{/}}
```
```javascript
ractiveInstance.on('toggleBody', function(){
  this.toggle('open');
})
```
That's it. You said in the template that everything enclosed in {%raw%}`{{#open}}{{/}}`{%endraw%} is conditioned to its value. If `open` is truthy, content will exists; if not, it won't.

### Why is this so cool ?
Remember the dynamic event handling in jQuery? Let me give you a refresh.
When you had some rendered template in where you had to assign events to some elements, but you had to re-render the template everytime the data changed, you had to manage those events handlers manually. That was extra logic you had to programm as a price to have a "cool web app":

```html
<div id="container">
  <button class="sayhello">One button</button>
  <button class="sayhello">Another button, LOL</button>
</div>
```
```javascript
//oops, data from server changed, let's recompile template and replace HTML content
data = someAjaxResponseFromServer(); //let's say for simplicity this is sync.
template = TemplateEngine.render(myTemplate, data);
//since DOM elements were going to be destroyed, we need to release handlers.
$('#container .sayhello').off('click');
$('#container .sayhello').on('click', function(){
  console.log('Hi, man');
});
```
If you were experienced, then you went for putting an event handler to its parent container, which never changed:
```javascript
//doesn't matter how many children are destroyed or created, event handler is
//in parent container, but is listening to children.
$('#container').on('click', '.sayhello', function(){
  console.log('Hi, man');
});
```

Now you don't need to do this. Ractive manages by itself all event handling. This is thanks to proxy events, which links to Ractive events. So you only need to tell Ractive that some DOM element will respond to a proxy event, and which Ractive event will be fired:
```html
<div on-click="sayHello">Click me</div>
```
```javascript
//sayHello is custom event
ractiveInstance.on('sayHello', function() {
  console.log('Hi, from Ractive');
})
```
Thanks to this, I was released from the responsibility of writing some repetitive code.
I'm not saying that dealing with advanced DOM programming is bad. Actually is good for polishing your skills. But when it comes to business, time is money, and Ractive saves you a lot of time, and it does its work pretty well !

## Creating components ? What's that ?

A Ractive component is an extension of Ractive's object, but with its own data and logic, well encapsulated and following the responsibility principle.
This helps us to separate concerns, and have a well designed application, and also let us reuse code for our many needs.

So, in Ractive, let's say you wan't to create a form, as a component. And for every field type, you also can create a component. That way, you will end having a component for text inputs, another for selects, one more for radio buttons or check boxes, and so on. And at the end putting all of them together organized withing the main form component.

I prefer to lead you to the Ractive doc page about [components](http://docs.ractivejs.org/latest/components) that confusing you more. This is a very extended subject and deserves another post.

## How hard it was to separate concerns those days ?

So, you want to create some components, and release them to the community, maybe in a github repo. Good. You only need a template part, and a javascript part. And, if you want to go fancy, some css part.
```
-- component
|
|-- css
|-- html
|-- js
```
How do you put'em together and make them easy to work with a main Ractive instance ?
Javascript files are easy, you can set them as common.js modules, and use webpack or browserify.
```javascript
module.exports = function() {
  return Ractive.extend({
    el: '#container',
    template: '#componentTemplate'
  });
}
```
But html files, and css files, those are hard to integrate with a running webapp. Because either you load them via AJAX and add them dynamically to the document, or you serve them from the begining.
Unlike `React`, where you can put the logic and the view in the same physical file (thanks to JSX). In Ractive you have no way to put html and javascript together.
One solution would be to accept a string template from the constructor:
```javascript
module.exports = function(template) {
  return Ractive.extend({
    el: '#container',
    template: template
  });
}
```
but, where's the encapsulation here ?
Another way to go (not a one I like) is to put all template into a long string in the js file:
```javascript
module.exports = function() {
  return Ractive.extend({
    template: '<div>Some Large template {{foo}}</div>'
  });
}
```
The template in this Component will be very hard to maintain.
So at the end we ended with a convention. Every file in html file should be added to the html document. Every css file should be also linked in the head as a style sheet.
This was easy for us, but won't for the community.

## Some monts later, what is new to my eyes ?

Yeah, I came to Ractive plugins directory, and stumbled upon this [repo](https://github.com/JonDum/ractive-datatable).

Apparently, Ractive evolved in such a better way, that releasing components is so much easier nowadays, that you only have to install it, and tell Ractive instance about its location.
Installation:
```
npm install ractive-datatable --save
```
Configuration:
```javascript
Ractive.extend({
    ...
    components: {
        datatable: require('ractive-datatable')
    },
    ...
});
```
Usage:
```
<datatable data='{{data}}' on-edit='dataedited' config='{{config}}' filter='{{filter}}'></datatable>
```

## Why is this so important ?

I don't know how aware you are about the front end scene nowadays, but it is growing faster than ever. And that's because of open source and active collaboration from the community. The most hype libraries are being cloned many times daily, a lot of issues are being raised, also not to forget the commits and pull requests.

React has a bunch of open source [components](http://react-components.com/) created by the enthusiasts, and I might bet angular has the same [movement](http://ngmodules.org/) going on.
But how about Ractive ? I think Ractive is one of the coolest libraries in the front end neighborhood, not only because it disrupts the way DOM manipulation used to be, but because it does pretty well its job about concerns separation: it separates the view from the logic. I mean, you specify some events in the template, and set control flow, but you don't have to write any line of javascript in there, because in the javascript layer you can do everything else.

So now, with the ability of adding plugins, decorators, components in such an easy way, I really hope the community will take off and create some cool stuffs.
I even feel myself motivated to write some components, but I'll save another post for that.

Meanwhile, visit [Ractive](http://docs.ractivejs.org) home page and its [plugins](http://docs.ractivejs.org/latest/plugins) section.

Long live Ractive !