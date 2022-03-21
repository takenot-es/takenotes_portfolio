---
layout: writing
date: 2022-03-16
title: Componentizing common elements in Jekyll
subheadline: Ready to be bored with a simple concept that really isn't that applicable to anyone that doesn't use an SSG or Jekyll in general? You're in the right spot.
---

First things first: I have no idea as to if “compententizing” is an actual word. Grammarly says it isn’t, but Grammarly doesn’t own me. Plus, it makes for a more enticing title.

Second things second: I spend a lot of time in Figma building out various components and trying my best to build stronger design systems. Part of that never-ending endeavor is the Atomic approach to building out elements. Ya know? Starting small at text styles, smallest base elements, building up to molecules (example being: Label, Input, Button), and then all of the ways to templates.

I’m not a JS dev, and I certainly don’t know much about dev in a Gatsby/React environment. It’s not that I can’t learn it, but it is that it isn’t the best use of my time to learn to write it. There will be a day when that isn’t true, but for now, I usually have a JS library/plugin I can utilize or there is a developer around that is doing the hard stuff.
All of this applies to people like me that are using Jekyll not a React-based/component-based JS framework.

### This brings me to the meat and potatoes of what I'm trying to do in Jekyll.
##### Build a library of reusable components that I don't have to hard code the HTML into the blocks, and I can reuse them. 

We'll look at buttons applied to cards, and sections holding a CTA (really common in a Jumbotron/header). We'll figure out how to nest that into both.

![Button Diagram](https://s3.us-east-2.wasabisys.com/takenotes/blog-posts/button-diagram.png)

My file tree looks something like this. ```Blocks``` being sections larger templates, and ```components``` being smaller elements, and I'll pull out a basic button using getbootstrap.com classes.

###### Filetree
```
_includes
-- _blocks
---- home-header.html
---- home-about.html
---- three-column-blog-loop.html
-- _components
---- button.html
---- card.html
``` 
###### Buttoncomponent.html
```
<a href="{% raw %}{{ include.url }}{% endraw %}" class="btn btn-{% raw %}{{ include.button_background }}{% endraw %}">
    {% raw %}{{ include.button_text }}{% endraw %}
</a>
```
If you’re noticing all the includes — they're important. Those includes help us pass through front-matter variables or just pass through plain text that we are going to use. This means that, as I said, the button is now a repeatable component that we can manipulate the structure of without having to hunt it down in ALL Of the Blocks or other components. If we’re making structural changes to that element we find the base component. Similar to Figma, or any other component-based system. Let’s look at it in action.

###### Button in action
```
/// Header Text using page_section section front-matter variables.
<h1>Headline goes here.</h1>
<h2>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed elementum augue risus.</h2>
{% raw %}{% include components/button.html 
   url=section.cta_url
   button_background=primary
   button_text=section.cta_text
%}{% endraw %}

/// Bootstrap Cards
<div class="card">
  <img class="card-img-top" src="{% raw %}{{ include.card_image }}{% endraw %}" alt="{% raw %}{{ include.image_alt }}{% endraw %}">
  <div class="card-body">
    <h5 class="card-title">{% raw %}{{ include.card_title  }}{% endraw %}</h5>
    <p class="card-text">{% raw %}{{ include.card_text  }}{% endraw %}</p>
    {% raw %}{% include components/button.html 
        url=include.button_url
        button_background=include.button_background
        button_text=include.button_text
    %}{% endraw %}
  </div>
</div>
```

You can see where the card and headline CTA differ. The headline CTA is going to pull either just text or a front-matter variable dictated by the section it is in. The card is going to pull in another include variable that will either be dictated by live text OR a loop variable. The button stays the same though and is immediately editable through the button.html component.