title: The tech stack behind this blog
date: 2015-10-06 07:19:42
tags:
- tech
- programming
---


When I decided to start this project: a space to write about technology and software engineering, I wanted a minimalist tool to work with.

Developers are a different race. We don't usually mind about style, font type, text columns, photos, videos or any other fancy thing a usual wordpress blog would have. Developers usually only care about text, and code (which is text but in a special format).

In the last years I noticed how the best developers I was following, had minimalist blogs and were using such basic tools for write that didn't deviate them from the main goal: to write a post.

* [Tom Preston-Werner](http://tom.preston-werner.com/) uses [jekyll](http://jekyllrb.com/)
* [substack](http://substack.net/) uses [markdown](https://github.com/substack/blog) to store his posts, and some black magic for putting them together into fancy html.
* [Kyle Simpson](http://getify.me/) a.k.a @getify uses 90s plain unstyled html,

And so on.

A static generator was the tool I was looking for.

So, that being said, there is no much to explain. The tech stack behind this blog is simple, but effective. It does the work and does it well.

I'm using:

* [hexo.io](https://hexo.io/) , a static content generator, for blog engine.
* [carbon](https://github.com/icylogic/carbon) theme, now forked and customized by [myself](https://github.com/alexserver/carbon).
* [github](https://github.com) to host my blog source posts, and also my generated static html files.
* [vagrant](https://www.vagrantup.com/) to set the stack in my local machine without to deal with the host OS.
* [commando.io](https://commando.io/) for automated deployment on blog updates.

And that's it.

So creating and publishing a post usually goes like this:

I ask hexo to create a post in my vagrant machine:
```
$hexo new 'this is a new post'
```
It creates a new text file with some prefilled information. Every post is written in markdown syntax. This let's me focus on the content and releases my attention from insignificant matters like style.

Every time I want to check how the post looks, I have to ask hexo to update the static html files, and serve the app.
```
$hexo generate && hexo server
```

Once the post is complete, after many revisions and corrections, I commit the changes to the github repo. This will update the repo source, but not the static html.
For updating the static html I have to ask hexo to deploy it to my github repo.
```
$hexo deploy
```
In order to have this working, I have to configure my blog config file, and tell hexo what is the github repo to where it will commit the changes. 
If you want to use hexo for your personal blog, I encourage you to read the [docs](https://hexo.io/docs/) and give it a try, it's a nice tool to have.

When hexo commits the static html file to my static blog repo, github will send a message to [commando.io](https://commando.io/) and then, commando will run a task in my server that will update my blog content.

Voalá.

I find this process pretty nice. How I can write a post as a physical file in a virtual machine that is hosted in my computer, then, after I decide it is good enough for going to public, I only have to type one command, and by the magic of internet and beautiful tools such as [hexo](https://hexo.io/), [github](https://github.com) and [commando.io](https://commando.io/), such post is sent right away to the cloud, available for everyone.

Every tool has its own merit, but the one that saves me from doing the hardest and most unsecure step is commando.
Because, once that my blog is updated in github, without commando I would need to log into my server via ssh and run the update task. Not every wireless connection is secure enough to connect to my server.
I could set a public POST entrance and set a hook in github, but that would make me nervious. And to make it safe with some sort of authentication, I'd have to spend some time on building it.
I really wanted to go simple, so this time, I came across this cool service that let me to connect securely to my server and run a task everytime I do a push to my repo.

I also encourage you to go to [commando.io](https://commando.io/) and give it a try. This is a nice and simpler service than any other CI/deployment tools I came across.




