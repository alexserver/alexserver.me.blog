title: How Ractive has evolved for integrations
tags: ractive, javascript, front-end, development
---

## What is Ractive ?

Ok, first things first, in order to understand what's going here, you should hit this link and embrace the awesomeness of [Ractive](http://www.ractivejs.org/).
Did you get it ? no more `jQuery.html('<div>some fancy html</div>')` anymore, right ?


## My first approach to ractive (January, 2014)
```html
<div id="container"></div>
<script type="text/ractive" id="scriptid">
  {{name}}
</script>
```
```javascript
var Ractive = new Ractive({
  el: '#container',
  template: '#templateid',
  data: {
    name: "Stranger"
  }
});
```

## Creating components ? What's that ?

## How hard it was to separate concerns those days ?

## Some monts later, what is new to my eyes ?

## Why is this so important ?