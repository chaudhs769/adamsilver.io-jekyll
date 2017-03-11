---
layout: post
title: Progressively enhanced Javascript
date: 2015-08-16 09:00:01
categories: progressiveenhancement js a11y
description: The Javascript element of Progressive Enhancement, is quite possibly the most important and misunderstood aspect of client-side Javascript development, period. Find out how to write Javascript for the open web.
---

The Javascript element of Progressive Enhancement, is quite possibly the most important and misunderstood aspect of client-side Javascript development, period.

This article addresses these misunderstandings and provides techniques that can be considered cutting-edge, even though they have been around for a very long time and have been forgotten.

> &ldquo;The problems we have with websites are ones we create ourselves&rdquo;
<br>&mdash; <cite>Motherfuckingwebsite.com</cite>

The beauty of the web is that by default, it is accessible to *everyone*. It's us developers that come along and ruin it. I think most people have the right intention, which is to serve the users first, but often we fail in implementation which we'll explore shortly.

But before we do, what is the best way to define what Progressive Enhancement *really* is? I think the following description does it justice:

> Progressive Enhancement is the approach of providing a baseline **core** experience for everyone; and creating a better **enhanced** experience for people who use a more capable browser.

Whilst Progressive Enhancement doesn't just pertain to Javascript, it is definitely the technology that developers tend to struggle with the most. We just can't seem to answer the following question with sophistication:

> &ldquo;How am I meant to write Javascript in a Progressive Enhancement way?&rdquo;

Whilst this question is not the easiest one to answer, answers do exist, and they are not *that* difficult once you have taken the time to truly understand them.

## Progressive Enhancement Myths

There are many [myths about Progressive Enhancement](http://www.sitepoint.com/javascript-dependency-backlash-myth-busting-progressive-enhancement/). I want to point out 3 in particular.

**1. Unobtrusive Javascript is not Progressive Enhancement**. Simply placing your Javascript code in external files, does not, in any way, address the problem of Progressive Enhancement.

**2. Prepraring for users that disable Javascript is barely scratching the surface**. It's true &mdash; some people disable Javascript but there are so many other reasons why Javascript *is* going to fail as [Everyone has Javascript, Right?](http://kryogenix.org/code/browser/everyonehasjs.html) explains. The last point in that article is most common and most important: **using Javascript that the browser doesn't recognise**. Which leads on to...

**3. Unlike HTML and CSS, Javascript does not degrade gracefully without developer intervention.** HTML and CSS degrade (or enhance depending on the way you see things) without any extra effort. Consider `<input type="email">` and `border-radius`. When unsupported, the input reverts to a standard text control and forgoes curved borders. With Javascript, the browser will error when it tries to execute code it doesn't understand. As an example, try running the following in IE8:

	var form = document.forms[0];
	form.attachEvent('submit', function() {
	    window.event.returnValue = false;
        var widgets = document.getElementsByClassName('widget');
	});

The code above errors because IE8 doesn't support the retrieving of elements by class name. The problem here is that the page didn't *fully* enhance. The user didn't get the enhanced experience. Nor did they get the core experience. *No*, instead they got the *fuck you* experience.

The browser and feature in this example is not the relevant point here. It could be *any* browser and *any* feature. It makes no difference how new a browser is or what cutting-edge features it claims to support.

## What shouldn't you do?

Sometimes, it can be helpful to explore how others are tackling the problem, because when you find flaws, you can avoid them and explore a more successful path.

**1. Some ignore the problem exists.** If they haven't experienced a problem, then they often think one does not exist. Or perhaps, they believe it to be an edge case. Regardless, this is unfortunate to the people using the website and the potential loss to the business.

**2. Some also abdicate responsibility by using 3rd party libraries without checking under the hood for quality.** And often, these libraries support a subset of browsers i.e. it's [multi-browser as opposed to cross-browser](https://gist.github.com/david-mark/06b9879f963ebb0eed62) &mdash; a sure sign that the library does not practice Progressive Enhancement.

People who use *other* browsers get the aforementioned *fuck you* experience, often at times when it would be straightforward to provide a *core* experience. The same thing happens when a library releases a new version and happens to drop support for more browsers &mdash; this of course is a never ending cycle.

## Cutting The Mustard falls short

[Cutting The Mustard](http://responsivenews.co.uk/post/18948466399/cutting-the-mustard) (CTM) is a relatively new approach to Progressive Enhancement, one which has the premise of a reliable solution and is based on the concept of a core and an enhanced experience. However, it's implementation (shown below) falls short.

	if(document.querySelector && window.addEventListener && window.localStorage) {
        // bootstrap application
	}

It works by *detecting* a few *choice* browser APIs, in order to *infer* that the browser is "modern" &mdash; something that is impossible to determine and irrelevant anyway.

*Impossible*, considering the sheer amount of new browsers being released and *irrelevant*, because release date does not determine capability. Besides, every browser was new once, so it's quite obvious that an inference for modernity provides little value.

Once CTM determines it's "modern", the Javascript application starts and (attempts to) provide the enhanced experience. The emphasis on *browsers* as opposed to *features*, suggests this technique is frail. And, inference is little better than User Agent sniffing, which is something that Richard Cornford explains superbly in [Browser Detection (and What To Do Instead)](http://jibbering.com/faq/notes/detect-browser/).

More specifically, CTM has the following problems of note:

**1. Detecting host objects like this is dangerous**. [H is for Host](http://www.cinsoft.net/host.html) explains why this is dangerous and provides a simple solution to the problem.

**2. Detecting the presence of an API is not enough**. CTM only *detects* host methods but often APIs are buggy. This is why feature *testing* is important. Nicholas Zakas provides an excellent case study in his short ebook [The Problem with Native JavaScript APIs](http://chimera.labs.oreilly.com/books/1234000001655/index.html). Additionally, Peter Michaux's article [Feature Detection: State of the Art Browser Scripting](http://peter.michaux.ca/articles/feature-detection-state-of-the-art-browser-scripting) explains everything you need to know about feature detection and feature testing.

**3. CTM degrades the experience unnecessarily**. CTM can easily suppress a perfectly capable browser from providing the enhanced experience. For example, if you wanted client-side form validation, something that say IE8 (or 6 for that matter) is perfectly capable of, CTM disregards IE8 and will only give those users the *core* experience &mdash; resorting to server round trips  which is an *unnecessarily* poor experience.

**4. Some CTM implementations rely on Javascript polyfills to plug missing gaps**. Ignoring the fact that [polyfills are full of problems](/articles/the-disadvantages-of-javascript-polyfills/), it is clear that if developers are mixing them in with CTM, this more than indicates CTM is not enough on its own to determine whether the browser is capable of delivering an enhanced experience (or not).

**5. The CTM condition needs constant maintenance as new browsers are released**. Again it's that same old problem &mdash; when can you drop support for a browser? This question doesn't really ever have to be asked. Either the browser has the required working features or it doesn't &mdash; that is, it's about features, not browsers.

**6. It's unreliable**. If the application uses any API that is not within the CTM test, the chance of a *fuck you* experience is high. As an  example, it will break in browsers where `matchMedia` isn't provided, or even in browsers where it is provided but it's buggy. Furthermore, `querySelector` itself has many bugs depending on context and arguments supplied, further reducing the reliability of CTM. An example follows:

	if(document.querySelector && window.addEventListener && window.localStorage) {
	    // application that uses other APIs

	    window.addEventListener("load", function(e) {
	        // FAIL = ANOTHER FUCK YOU
	        var matches = window.matchMedia(...);
	        // ...other stuff...
	    }, false);
	}

I had a little chat with Jeremy Keith about this and he rightly says that you can use CTM better by detecting all the APIs. He is definitely right of course.

My point is that this is how the technique is advertised and often implemented, and that in addition to this there are several other points of failure to consider anyway.

## What *is* the solution?

> &ldquo;I’ve always maintained that, given the choice between making something my problem, and making something the user’s problem, I’ll choose to make it my problem every time.&rdquo;
> <br>&mdash; <cite>Jeremy Keith</cite>

If you have made it this far, you probably believe in people *first*, whether it's the users or the client. And, that Progressive Enhancement is the way to enable that belief.

In order to provide a core experience, the site *must* work without Javascript. Why? Because that is the experience a user will get, when Javascript *is* enabled, but incapable of running (for whatever reason).

Then, in order to determine that the browser can provide the enhanced experience, you must detect and where necessary, test *all* of the features used by your application *before* your application  uses them. This will ensure the page doesn't end up irrevocably broken, which is something your users will thank you for.

The only way to reliably do this is through wrappers, or *facades* if jargon is your thing. A library that employs Progressive Enhancement *must* provide a dynamic API. Dynamic, in that it adapts and changes based on the host environment i.e. the browser. This is what it basically looks like:

	if(lib.hasFeatures('find', 'addListener', 'storeValue')) {
        var el = lib.find('.whatever');
        lib.addListener(el, "click", function() {
		    lib.storeValue('key', 'value');
	    });
	}

**1. Notice how remarkably similar CTM *looks* in comparison.** The difference is that the application doesn't directly interface with browser APIs. Facades provide a leaner, context-specific API, that allows you to iron out bugs, all of which reliably enables Progressive Enhancement.

**2. Also note, the one-to-one mapping between what is *checked* in the condition and what is *used* by the application.** This is *vital*. If you break this rule, you significantly increase the chance of providing the *fuck you* experience.

**3. There is no need for polyfills.** The library either provides the method or it doesn't, no halfway houses, no caveats.

**4. Application logic is completely decoupled from browser logic.** This is something Nicholas Zakas writes about in many of his articles and books. Basically this is good for sanity and maintainability.

**5. In the event that Javascript is enabled and that the condition does *not* pass, the user gets the degraded experience.** In Cutting The Mustard lingo, it simply doesn't cut it.

At this point, some might say they don't concern themselves with browser problems, as libraries take care of them (the majority unfortunately don't). They might even portray themselves as application developers, but just because responsibility is abdicated, doesn't mean the problem isn't there.

The idea of abstractions are good, the idea of several abstractions i.e. a library, is also good. But if that library is monolithic in nature, context-less, lacks feature detection and feature testing, leans on polyfills (or browser sniffing or object inferences etc) and does **not** expose a dynamic API, then ultimately you are unable to Progressively Enhance your application and your users and the business you work for, will suffer for it.

At the very least, it is beneficial to be able to spot code that does not conform to the principles of Progressive Enhancement. Particularly, the kind that doesn't even attempt to degrade gracefully in the face of danger.

## How do I build a library like this?

To explain how to build a library that conforms to Progressive Enhancement would likely require a book of its own. Fortunately, Peter Michaux's article [Cross-Browser Widgets](http://peter.michaux.ca/articles/cross-browser-widgets) provides a detailed walkthrough all in one article. However, *this* article wouldn't be complete without a short example of its own would it?

This example will also demonstrate that Progressive Enhancement is not a drag in the way of "having to support old irrelevant browsers". Quite the opposite in-fact. The words "drops support for" changes to "degrades gracefully in". You also get to determine an appropriate degradation point suitable for your project.

	// library.js
	var lib = {};

	// Note: use isHostMethod - H is for Host.
	if(document.documentElement.classList.add) {
		lib.addClass = function(el, className) {
			return el.classList.add(className);
		};
	}

And then the calling application code looks like:

	// app.js
	if(lib.addClass) {
		// some application that must provide the ability to add a class to an element, in order to provide the enhanced experience

	}

Notice, that this application only enhances where the browser supports `classList`, which generally speaking are cutting edge browsers (at time of writing), meaning that this application will degrade in IE9 (and below) as well as a bunch of other browsers. That's not a problem though, they will just get the *core* experience. If you wanted to support those browsers, something which in this case is easy to achieve, then you could add another fork:

	// library.js
	var lib = {};

	// Note: use isHostMethod - Peter's article covers this
	if(document.documentElement.classList.add) {
		lib.addClass = function(el, className) {
			return el.classList.add(className);
		};
	} else if(typeof html.className === "string") {
		lib.addClass = function(el, className) {
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

This method now supports more browsers and your application code didn't change. Equally, you could remove this fork again in the future, when you think it's more acceptable to give those browsers the *core* experience. Perhaps the number of visitors naturally dropped for a particular set of browsers. Or maybe, it's not worth the development effort to write a second fork etc.

Regardless, a library never *has* to drop support in the traditional sense &mdash; it can simply drop *enhanced* support.

## Conclusion

Progressive Enhancement is something that puts users first. The misunderstandings of Progressive Enhancement, when broken down piece by piece are easy to understand, but if just one of those pieces falls down, technical implementations tend to fall short of the mark.

Unfortunately, this is quite common in the industry and it's the people that suffer the most, the same people that are interested in your business or content. It's just not good enough to let them endure the *fuck you* experience, they don't deserve it and it's circumventable.

Fortunately, when the real meaning of Progressive Enhancement is understood, the execution can be implemented correctly. This allows for robust, future-friendly, backwards-compatible Javascript code. This allows you to *responsibly* use cutting-edge browser APIs, leaving the majority of other browsers to degrade to the core experience.

<!--

Cornford:
The combination of the facts that it is impossible to determine which browser is executing the script, and that it is impossible to be familiar with all browser DOMs can be rendered insignificant by using feature detection to match code execution with any browser's ability to support it. But there is still going to be a diversity of outcomes, ranging from total failure to execute any scripts (on browsers that do not support javascript, or have it disabled) to full successful execution on the most capable javascript enabled browsers.

Veal:
I would add - and with a mobile connection you have no choice but to use the proxy because that is the way the network is configured. They are not as open as your home broadband and companies often employ a proxy to save bandwidth, and again you cannot avoid this
-->