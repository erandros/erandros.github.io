---
published: true
title:  "My thoughts on AngularJS (> 1.5.x) directives templates and transclusion"
---
Never use them and save yourself some scope related headaches (as in, "I can't access the damn scope of the parent/child").  

I'm sure there is some technical explanation that, if you take the time to read, makes them make sense.

But, here's a taken-out-of-context image from the "[Understanding scopes](https://github.com/angular/angular.js/wiki/Understanding-Scopes#directives)" page from the angular github wiki

![](https://camo.githubusercontent.com/4d9a7cbb029bb29d66cbbef0f0527b2d40202d90/687474703a2f2f692e737461636b2e696d6775722e636f6d2f41684f47482e706e67)

It doesn't seem too nice, amirite?

#### Quick directive template explanation for the newbies

Let's say you have a directive `bananaHolder` and that directive has a template with the html content `<div>bananas</div>`.  

This means that when you have your `index.html` file by like:

```html
...
<banana-holder></banana-holder>
...
```

When you browse it in your nice chromey/firefox browser, you'll get: 

```html
<banana-holder>
  <div>bananas</div>
</banana-holder>
```

#### Quick directive transclusion explanation for the newbies

Now, let's say that in your html file you do:  

```html
...
<banana-holder>
  <div>my custom bananatic features</div>
</banana-holder>
...
```

And you act like "I want that `my custom bananatic` text to show up somewhere".
I'd say, I agree, that's a good idea, it lets you customize your web elements. So, in the directive template, you do this:  

```html
//angular directive template
`<div>bananas</div><div ng-transclude></div>` 
//I think you also need to do "transclude: true" in the directive properties
```

Which means that the "transcluded" content, will be put in the second div (the one, with the ng-transclude, obviously).

And when you browsey browse the page, you'll get:

```html
...
<banana-holder>
  <div>bananas</div>
  <div>my custom bananatic features</div>
</banana-holder>
// Or something like that, I'm not sure if it replaces the entire tag, or the inner html
...
```
 

#### Ok, I get it now, thanks for the explanation
Yeah, well you can throw that in a garbage pail though, because transclusion and templates are just depression triggers.


#### But wait, those features seemed kind of cool, is there any alternative to them?

Yes.  
Use a template-less directive.   
Or, if you don't care about having an isolated directive, use a good old controller.

#### But what if I still want that cool customizable directive template dynamic?

That's a jolly good question, m8!  
Then make use of the templating engine your webapp framework has by default (I'm assuming you have a webapp framework).  

If you're in the `ASPNET core` world, use `TagHelpers`, and name them the same as the directives. Emulating the transclusion behaviour is pretty easy with them. (Please, in general try use cshtml files as templates of TagHelpers [as shown here](https://stackoverflow.com/a/40443258/796608), which is the only read good way for Tag Helpers with big html content, I will probably talk about it in a future post)

If you're in the Node.js world, with `pug.js` (before known as, `jade`), you can use [mixin blocks](https://pugjs.org/language/mixins.html#mixin-blocks) to achieve transclusion easily as well.

Having your templates coming from the server will be better because: 
 
* Server html processing speed >> Angular html processing speed
* Save you from painful scope problems.

A benefit of this is that the html comes already generated from the server side. You can `ng-cloak` the (and what I say ng-cloak, I mean hide the parts that look like crap until the whole thing is loaded)
