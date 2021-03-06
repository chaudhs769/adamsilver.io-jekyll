---
layout: post
title: Buttons shouldn't have a hand cursor
date: 2016-07-15 09:00:01
categories: design ux
description: Designers are confused about the meaning of the hand cursor. Find out all the details in this article.
---

There's a belief that the hand (or pointer) cursor means clickable, but this is wrong and potentially problematic.

## The hand doesn't mean clickable

It’s no accident that browsers don’t give buttons (and other elements) a pointer cursor—it’s because they’re not meant to. See this screenshot:

<div class="image">
	<figure>
		<img src="{{ site.url }}/assets/img/buttons/chrome.jpeg" alt="Chrome on Mac OS" width="100%">
		<figcaption>Google's search page on Chrome Mac OS</figcaption>
	</figure>
</div>

Almost every element is interactive and clickable—the menu, the tabs, the whitespace, the browser buttons, the bookmark bar and Google’s search box—none of them have a pointer cursor.

There are interactive elements not shown above: select menus, sliders, checkboxes, radios, labels, images, empty space (e.g right click—view source) and text—again, none of them have the pointer cursor.

The same applies for the operating system. You can tap, drag, select, press, left click and right click on a plethora of different elements including buttons. But, buttons are not signified by the cursor changing to a pointer on hover.

<div class="image">
	<figure>
		<img src="{{ site.url }}/assets/img/buttons/license.jpeg" alt="" width="100%">
		<figcaption>License Agreement is a link and gets the pointer cursor. Buttons don’t.</figcaption>
	</figure>
</div>

Perceived affordance is signified by the way something looks regardless of the cursor. Remember, the cursor is only available when hovering with a pointing device such as a mouse.

This is why, for example, [checkboxes are never round](http://danieldelaney.net/checkboxes). This is also why [links are typically underlined](http://adrianroselli.com/2016/06/on-link-underlines.html). This is why links do, in fact, have a hand cursor.

## What the authorities say

[Microsoft’s Design Guidelines](https://msdn.microsoft.com/en-us/library/windows/desktop/dn742466%28v=vs.85%29.aspx) talk about weak perceived affordance:

> Text and graphics links use a hand […] pointer […] because of their weak affordance. While links may have other visual clues to indicate that they are links (such as underlines and special placement), displaying the hand pointer on hover is the definitive indication of a link.

> To avoid confusion, it is imperative not to use the hand pointer for other purposes. For example, command buttons already have a strong affordance, so they don’t need a hand pointer. The hand pointer must mean “this target is a link” and nothing else.

[Apple’s Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Pointers.html) says the hand cursor should be used when “the content is a URL link”. [W3C User Interface Guidelines](https://www.w3.org/TR/CSS21/ui.html#propdef-cursor) says the same thing again with “The cursor is a pointer that indicates a link.”

## The pointer cursor is for links

The pointer (and often underlined text) signifies a link. Links are not buttons. Links came along with the web. To help users understand that they're different, they're given the pointer cursor. It serves as an extra clue. Here’s why:

* Clicking a link opens a web page or resource.
* (On desktop) I can right-click on a link and do many things (that I can’t do with a button). Open in new tab/window, save a link, copy address, add to reading list, bookmark it and more.
* (On mobile devices) I can tap and hold on a link and get a similar context menu, like the previous point.
* A link also tells me that I am just going somewhere else. I am not modifying any data or making changes in anyway (like a button is likely to do).

## Summary

When a button has the pointer cursor, it subtly suggests that the user is interacting with a link when they’re not. If you want to give users feedback on hover, change the background colour, for example. A well-designed button doesn't need a hand cursor to help users realise it does something.

The pointer cursor is reserved for links. This is because their behaviour is unique. Browsers and Operating Systems have done the work for you—because contrary to popular belief—browser vendors often know best.

Links have always been handled this way since the web came along—this is the convention of the web that you need not innovate on. You can rest easy knowing that browsers have your back. This lets you and your team to solve real problems.

<!--
http://ux.stackexchange.com/questions/3788/default-cursor-on-mouse-over-of-a-button-is-not-a-hand-pointer
-->