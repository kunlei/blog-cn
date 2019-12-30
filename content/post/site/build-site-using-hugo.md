---
title: Recent Updates and Blog Redesign using Hugo
subtitle: 
date: 2019-08-17
draft: false
comments: true
tag: ["web", "hugo", "blog"]
---

### Recent updates
I have a blog setup using Hugo with the theme called *mainroad* and it has been working fine, functionally, but I always feel something missing from that design.
All the contents, including the navigation menu and posts, are centered on a paper-like area, which looks great at first impression.
On the other hand, much of the screen space seems wasted, which is especially true when I have to big monitors and the design only takes up a small portion of the screen.
Another issue is with source code rendering.
I have been sharing codes on the blog and find the outputs ugly without beautiful syntax highlighting.
My interests in composing posts gradually declined as the idea of looking at mediocre output is unbearable.


In the meantime, lots of things happened since my last post, the most significant thing being my new journey of frontend development.
Initially tasked with translating an optimization algorithm written in VBA into a more modern language (Java is the final choice), I ended up taking the responsibility of developing a frontend user interface as well as backend optimization engine.
I've always admired real software engineering works and I jumped into this opportunity without much idea of what I signed up for.
I will write more posts later sharing my experience of acquiring some [*Spring Boot*](https://spring.io/projects/spring-boot) knowledge to provide backend Restful API to be consumed by the frontend application.



On the frontend site, a new world opened up in front of me since I learned skills of html, css and javascript. 
To people outside this field, frontend developers are really luck since they have at least three fancy skills to put on their resume no matter which framework they use, and React is the choice in my case.
As I tap into the world of frontend development, much more blog generation techniques came into my attention, [Gatsby](https://www.gatsbyjs.org) being the most intriguing one.
However, it has some significant learning curve with React as the pre-requisite and GraphQL to drive its data layer.
I am still playing with Gatsby with the ultimate goal of creating a blog that's totally under my control - I don't feel like confident when I have to use something without understanding how it works.


### Blog redesign
As it won't be any time soon that I build my own blog using Gatsby, and I can't live the mainroad theme, the temporary solution is locating a new theme that looks better.
The rest of the post will focus on how to create a similar looking site using Hugo and a theme called *beautifulhugo*.

#### Install Hugo
I use Mac for development and Hugo can be installed via *brew*:
```
brew install hugo
```

If you already have Hugo installed, it's good to check the version you have with 
```
hugo version
```
and the output on my machine looks like this:
```
Hugo Static Site Generator v0.57.1/extended darwin/amd64 BuildDate: unknown
```

In case of having an outdated version, Hugo can be updated using
```
brew upgrade hugo
```
I did have some issues with this newest version, which I will detail how to address at the end of this post.

#### Create a new site locally
With Hugo installed, the next step is to create a site locally on your machine.
To do this, navigate to a directory in which you site will live, in my case, it's **~/dev/blog/**.
Create a new site using the following command
```
hugo new site name-of-your-site
```
This will create a new directory named *name-of-your-site*.
I created an example site named as *tutorial* and now I have my directory of **~/dev/blog/tutorial/** and Hugo automatically generated a couple of files and directories within it:
```
total 8
drwxr-xr-x  3 klian  74715970    96B Aug 18 10:27 archetypes
-rw-r--r--  1 klian  74715970    82B Aug 18 10:27 config.toml
drwxr-xr-x  2 klian  74715970    64B Aug 18 10:27 content
drwxr-xr-x  2 klian  74715970    64B Aug 18 10:27 data
drwxr-xr-x  2 klian  74715970    64B Aug 18 10:27 layouts
drwxr-xr-x  2 klian  74715970    64B Aug 18 10:27 static
drwxr-xr-x  2 klian  74715970    64B Aug 18 10:27 themes
```

#### Install the theme
At this point, we have a local directory that is not version controlled.
To do this, execute the following command at *~/dev/blog/tutorial/*:
```
git init
```
This effectively initializes a git repository on our machine.
The Hugo community makes available lots of themes for us to choose from, and this site uses a theme called **beautifulhugo**, which is available on github and can be cloned into the *themes* directory.
Run the command below at *~/dev/blog/tutorial/*:

```
git submodule add https://github.com/halogenica/beautifulhugo themes/beautifulhugo
```

Now the *themes* directory looks like below:
```
total 0
drwxr-xr-x  15 klian  74715970   480B Aug 18 21:36 beautifulhugo
```
We've successfully installed a theme ready to be used by our site.

#### Start blogging
All of our site contents are placed within the, not surprisingly, *content* directory.
As a start, we can create markdown file *about.md* to say something about the author of the site:
```
---
title: About Me
date: 2019-08-17
tags: ["site"]
---

This is John Doe's site.
```

The site can be viewed locally by running the Hugo command:
```
hugo server -t beautifulhugo
```


#### Build site and deploy
For now, the site only lives on your machine and only by deploying it to some hosting servers that others can access it.
The easiest way to get a free hosting server is probably using something you already have, that is, github.
[This page](https://gohugo.io/hosting-and-deployment/) provides awesome guides to other deploying options, but in this post I'll just show how to deploy our site to github.

Github offers two options to host user sites: user pages or project pages.
The former can be achieved by creating a special repository with name *username.github.io*.
Note that the *username* is your github account name.
This repository is where we should put our generated site files produced by Hugo.
It is not the place we save the source file of our site.
We should create a separate repository to save the source files and the repository I created for this tutorial is
```
https://github.com/kunlei/hugo-tutorial.git
```
Now we can push our local source files to this repository
```
git add .
git commit -m "first commit"
git remote add origin https://github.com/kunlei/hugo-tutorial.git
git push -u origin master
```

Next, we need to link the user page repository to the *public* directory:`
```
rm -rf public
git submodule add -b master git@github.com:kunlei/kunlei.github.io.git public
```

Then we can ask Hugo to create our site files by running
```
hugo -t beautifulhugo
```
This will create all our site files and put them into the *public* directory.

Last, we only need to push all the files inside the *public* directory to github:
```
cd public
git add .
git commit -m "update"
git push origin master
```

Congratulations! Now your site can be viewed anywhere at ```https://username.github.io```. 


