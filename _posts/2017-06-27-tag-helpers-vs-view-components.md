---
published: true
title: "Discussing View Components vs Tag Helpers with myself (ASP.NET Core)"
comments: true
tags: [ASP.NET Core, web development, tag helpers, view components]
---
Why am I writing this?

This is a post meant to express my experience with Tag Helpers vs View Components and hopefully attract ASP.NET Core experts to shed some light in this area.  
Maybe I'm wrong, maybe I'm thinking of View Components the wrong way. If you think so, I invite you to make a case for them.
I haven't done any research regarding the internals of each; I don't know which one is faster. I'm prioritizing readability and code organization.

### What

I'm not aware of microsoft devs plans, but I truly hope they merge them in one at some point, because I think there's a big overlap between them.
Why would you use a View Component instead of a Tag Helper? I can't find a situation where I would, because they feel like the **same thing** to me

### Comparing invocations
Let's compare the way you use use/invoke them. Here's a Tag Helper invocation:

```html
<div>extra stuff</div>
<priority-list max-priority="4" is-done="true"></priority-list>
<div>some more extra stuff</div>
```

With ViewComponents, you can call them in an html friendly way as shown in this [dotnetthoughts post](http://dotnetthoughts.net/view-components-as-tag-helpers-in-aspnet-core/), intellisense and all:

```html
<div>extra stuff</div>
<vc:priority-list max-priority="4" is-done="true"></vc:priority-list>
<div>some more extra stuff</div>
```

That'd be great, except that I **don't have intellisense on that** even though I do have intellisense on normal tag helpers.

### Tag Helpers advantages 

#### Used by the core library
There's the oh so useful AnchorTagHelper (which matches to tags like `<a asp-controller>`, or you can even pass action arguments to a link like `<a asp-controller="Home" asp-route-parameter="true">`).  

#### Tag Helpers are more complete

A lot of useful features that would take too much time to properly describe in their entiretiy:
* A tag can match more than one tag helper.
* You can restrict a Tag Helper's children (which works incredibly well with intellisense, you get only those child tag helpers as suggestions)
* You can bind Tag Helpers properties to the actual html attributes with the `[HtmlAttributeName("name")]` decorator instead of binding them to method parameters.
* You `TagHelperContext` lets you access every html attribute (even the ones you didn't bind to anything).
* The `TagHelperOutput` that lets you control the result tag: 
	* Controlling if the the Tag has and end tag or not (as in `</div>`). You could make your single tag    `<script src=""/>` tag helper to output to a normal `<script src=""></script>` tag.
	*`Pre/PostElement`, `Pre/PostContent` are awesome.
* Sometimes, you don't even need to use a razor file, and you can do that with Tag Helpers easily.


### Tag Helpers disadvantages

#### No razor templating by default

* In order to pass a model to a razor (cshtml) file from a Tag Helper the same way View Components do, you have to whip out a generous amount of boilerplate code as shown in [this StackOverflow answer](`https://stackoverflow.com/a/40443258/796608`). And that makes your Tag Helper use a HtmlHelper to render itself.  
And this is the only thing that is really painful about tag helpers. If Microsoft makes this easier, the I definitely see no purpose in using View Components.
