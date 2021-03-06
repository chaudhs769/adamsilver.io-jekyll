---
layout: post
title: Progressively enhanced Javascript
date: 2015-08-16 09:00:01
categories: progressiveenhancement js a11y
description: Using Javascript to design progressively enhanced interfaces is probably the most important and misunderstood subject in web development. Find out why and what you can do about it in this article.
---

Using Javascript to design progressively enhanced interfaces is probably the most important yet, misunderstood subject in web development.

In this article we'll discuss these misunderstandings. Then, we'll explore long-forgotten techniques that have stood the test of time.

> &ldquo;The problems we have with websites are ones we create ourselves&rdquo;
<br>— <cite>Motherfuckingwebsite.com</cite>

By default, the web is accessible to everyone. That's the web's super power. It's us lot that take this natural super power and lace it with kryptonite, hurting users in the process.

Most of us care about users, but we also fail in the execution of that intent. Before we get to why, let's first define progressive enhancement.

Progressive enhancement first gives everyone a useful experience; Then, where necessary, it lets us create a better, enhanced experience for users using a more capable browser.

We don't seem to know how to write Javascript in a progressively enhanced way.

## Progressive enhancement myths

While there are many [myths about progressive enhancement](http://www.sitepoint.com/javascript-dependency-backlash-myth-busting-progressive-enhancement/) I want to point out two in particular.

First, unobstrusive Javascript (placing Javascript in external files), does not, in anyway, mean it's progressively enhanced.

Second, this is not about people who disable Javascript. Yes, some people do disable it but it's not the main problem. [Everyone has Javascript, Right?](http://kryogenix.org/code/browser/everyonehasjs.html) shows the many points of failure.

The last of those points—*using Javascript that the browser doesn't recognise*—is what we need to discuss. Javascript—unlike HTML and CSS—doesn't degrade gracefully without intervention.

For example, `<input type="email">` degrades gracefully into a text box. And `border-radius` degrades gracefully by not showing rounded corners. No problem.

Javascript, however, will error when the browser tries to execute code it doesn't recognise. Internet Explorer 8, for example, breaks on the last line here:

	var form = document.forms[0];
	form.attachEvent('submit', function() {
      window.event.returnValue = false;
      var foos = document.getElementsByClassName('foo');
	});

This is because it doesn't recognise `getElementsByClassName()`. The page didn't degrade nor did it fully enhance. The script intercepts the form's submit event, but gives users a broken interface.

So, when the user submits the form, nothing happens. Handling submit on the client (the enhanced experience) to save a round trip is fine. Handling submit on the server (the core experience) is fine too. But the implementation above doesn't allow for either experience.

Neither the browser nor the features in this example are relevant. It could be any browser and any feature. It makes no difference how new a browser is or what (cutting edge) feature it supports.

## What shouldn't we do?

It's often easier to deduce what it is we should do, once we found out what we shouldn't. Let's do that now.

### 1. Don't ignore the problem exists

I struggled a lot with this. I always thought about the current set of browsers that a particular project adheres to. But just because I ignored users who use ‘other’ browsers, doesn't mean they don't exist.

### 2. Don't abdicate responsibility

When we add a third-party script to the project, it becomes our responsibility. We should check under the hood for quality and watch out for the typical [multi-browser approach](https://gist.github.com/david-mark/06b9879f963ebb0eed62) that opposes the principles of progressive enhancement.

Moreover, when a library releases a new version, they often drop support for various browsers. This is a never ending cycle. This is what Jeremy Keith calls the [continuum](https://adactio.com/journal/6692).

### 3.Don't rely on Cutting The Mustard

[Cutting The Mustard](http://responsivenews.co.uk/post/18948466399/cutting-the-mustard) (CTM) is a relatively new approach which promises reliability by giving users a core or an enhanced experience. It's intent is great but the implementation falls a little short.

	if(document.querySelector && window.addEventListener && window.localStorage) {
      // start application
	}

The script detects a few choice methods, to then infer that the browser is ‘modern’. This is impossible because of the vast amount of new (versions of) browsers being released every day. And it's irrelevant because the release date doesn't determine capability.

If the check passes, the application starts and attempts to give users the enhanced experience. Inferences are almost as frail as user agent sniffing, something that Richard Cornford explains in [Browser Detection and What To Do Instead](http://jibbering.com/faq/notes/detect-browser/).

CTM has the following problems:

- Detecting host objects this way is dangerous. [H is for Host](http://www.cinsoft.net/host.html) explains why and provides a robust alternative.
- Merely detecting the existence of a method is not enough.  Nicholas Zakas' [The Problem with Native JavaScript APIs](http://chimera.labs.oreilly.com/books/1234000001655/index.html) demonstrates this. Peter Michaux's article [Feature Detection: State of the Art Browser Scripting](http://peter.michaux.ca/articles/feature-detection-state-of-the-art-browser-scripting) explains this further.
- It unnecessarily gives users the degraded experience. For example, due to the detection of a few choice ‘modern’ methods, it excludes Internet Explorer 8 (or 6 for that matter) from giving users client-side form validation even though these browsers are perfectly capable of providing a fast experience.
- CTM often relies on polyfills. [Polyfills are problematic](/articles/the-disadvantages-of-javascript-polyfills/) in their own right. But if CTM needs polyfills, it shows that on its own the technique isn't robust.
- The condition needs constant maintenance as vendors release new browsers. We shouldn't have to update scripts when new browsers come out. We should revolve our scripts around features, not browsers.
- It's unreliable because if the application uses a method that is not within the condition, then there's a high risk of exposing a broken interface.

## What is the solution?

Jeremy Keith says “I’ve always maintained that, given the choice between making something my problem, and making something the user’s problem, I’ll choose to make it my problem every time.”

To give users a core experience, we must ensure the interface works without Javascript. This is because that's the experience users will get when the browser doesn't recognise some method.

Then we must detect, and where necessary, test *all* of the features the application references before the application uses them. This ensures the page doesn't end up half enhanced and therefore irrevocably broken.

To do this reliably we need to use wrappers (facades). The library should expose a dynamic API that adapts to the browser, something like this:

	if(hasFeatures('find', 'addListener', 'storeValue')) {
      var el = find('.whatever');
      addListener(el, "click", function() {
        storeValue('key', 'value');
	  });
	}

### Notes

- It looks remarkably similar to CTM. The difference is that it doesn't reference browser methods directly. Facades are leaner and context specific, allowing the library to fix bugs enabling us to reliably enhance an interface.
- There's a one-to-one mapping between what's in the condition and what the application uses. If the condition doesn't pass, the user gets the core experience.
- There's no need for polyfills. The library provides the method or it doesn't, depending on the browser. No caveats.
- The application logic is decoupled from the browser.

## A dynamic API

Peter Michaux's [Cross-Browser Widgets](http://peter.michaux.ca/articles/cross-browser-widgets) has a detailed walk-through, but let's create a little something now.

Let's say our application needs to add a class to an element. First we'll create that function and expose a dynamic API:


	// use isHostMethod
	if(document.documentElement.classList.add) {
    var addClass = function(el, className) {
      return el.classList.add(className);
    };
	}

Then the calling application checks `addClass()` is defined before referencing it:

    if(addClass) {
      addClass(el, 'thing');
    }

The application is blissfully unaware that it only runs in browsers supporting `classList()`. Users using Internet Explorer 9 and below get the degraded experience and that's okay. If you want to support Internet Explorer 9, add a fork like this:

	var addClass;

	// Use isHostMethod
	if(document.documentElement.classList.add) {
    addClass = function(el, className) {
      return el.classList.add(className);
    };
	} else if(typeof html.className === "string") {
    addClass = function(el, className) {
      var re;
      if (!el.className) {
        el.className = className;
      } else {
        re = new RegExp('(^|\\s)' + className + '(\\s|$)');
        if (!re.test(el.className)) {
          el.className += ' ' + className;
        }
      }
    }
	}

This implementation supports almost every browser and the application didn't need to change. In future you can remove this fork once the number of visitors diminishes to a suitable level—something only you can determine.

Either way, users get a working experience, proving that a library never needs to drop browser support, at least not in the traditional sense.

## Summary

Progressive enhancement puts users first. Misunderstanding the application of progressive enhancement puts users last.

Progressive enhancement is not more work, it's less work. We don't have to endlessly play catch up with browsers. We don't have to give users a broken experience.

Instead we can write backwards-compatible and future-proof code that creates robust and [inclusive experiences](/articles/designing-inclusively/) for everyone.

<!--

Cornford:
The combination of the facts that it is impossible to determine which browser is executing the script, and that it is impossible to be familiar with all browser DOMs can be rendered insignificant by using feature detection to match code execution with any browser's ability to support it. But there is still going to be a diversity of outcomes, ranging from total failure to execute any scripts (on browsers that do not support javascript, or have it disabled) to full successful execution on the most capable javascript enabled browsers.

Veal:
I would add - and with a mobile connection you have no choice but to use the proxy because that is the way the network is configured. They are not as open as your home broadband and companies often employ a proxy to save bandwidth, and again you cannot avoid this


	if(document.querySelector && window.addEventListener && window.localStorage) {
	    // application that uses other APIs

	    window.addEventListener("load", function(e) {
	        // FAIL = ANOTHER FUCK YOU
	        var matches = window.matchMedia(...);
	        // ...other stuff...
	    }, false);
	}

The idea of abstractions are good, the idea of several abstractions i.e. a library, is also good. But if that library is monolithic in nature, context-less, lacks feature detection and feature testing, leans on polyfills (or browser sniffing or object inferences etc) and does **not** expose a dynamic API, then ultimately you are unable to Progressively Enhance your application and your users and the business you work for, will suffer for it.

At the very least, it is beneficial to be able to spot code that does not conform to the principles of Progressive Enhancement. Particularly, the kind that doesn't even attempt to degrade gracefully in the face of danger.

This example will also demonstrate that Progressive Enhancement is not a drag in the way of "having to support old irrelevant browsers". Quite the opposite in-fact. The words "drops support for" changes to "degrades gracefully in". You also get to determine an appropriate degradation point suitable for your project.

-->