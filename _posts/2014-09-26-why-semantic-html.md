---
layout: post
title:  "Why Semantic HTML"
date:   2014-09-26 09:00:01
categories: html
---

Semantic HTML is the use of mark-up to reinforce the meaning and *intention* of the content. It does *not* describe how the content looks or how it behaves. Semantic HTML has many benefits:

## Accessibility

Some disabled users utilise the functionality of a screen reader and when HTML is semantic, there is a much higher chance it will be read out meaningfully to the user.

## Responsive Design

> &ldquo;Responsive web design (RWD) is a web design approach aimed at crafting sites to provide an optimal viewing experience—easy reading and navigation with a minimum of resizing, panning, and scrolling—across a wide range of devices (from mobile phones to desktop computer monitors).&rdquo;
> <br> &mdash; Wikipedia

The relevance here is that something that is styled one way for *large* screens may look quite different on *small* screens, and so semantic HTML is advantageous.

For example, it is quite common to find elements littered with a Class attribute value of *clearfix*. This ensures floated elements are cleared without the need for extra mark-up. In small screens the elements might stack below one another, meaning that *clearfix* has no meaning here; infact it becomes rather misleading and confusing in this context. Additionally, the CSS rule would need overriding for small screens.

This is tricky because if another component *does* require the *clearfix* in small screens, then the override becomes problematic. This wouldn't be a problem if the component was given a semantic Class attribute value, because the styles can be applied based on what it is.

## SEO

Search engine crawlers rank the page based on the content. The content is easier to understand if semantic HTML is used, therefore improving search engine ranking.

## Developer understanding

When using semantic HTML, it is easier to read and understand; developer on-boarding is easier and quicker too.

## Maintainability

It is easier to update a site's look and feel, requiring less effort to maintain the HTML i.e. a *heading* is always a *heading* no matter what it looks like; if the colour is changed from red to black, the HTML will not need updating.

## Automated functional testing

Functional tests are easier to write because the hooks are mapped to features. For example, if the feature contained a continue button, and the automated test scenario was "Given I click the continue button" then there is a clear 1-2-1 mapping between the feature and the HTML element. Without the semantic hook, automation testing becomes difficult to impossible.

## Performance

The last and most minor benefit is that of performance, as the page weight is likely to be smaller when using semantic HTML. Unsemantic HTML might use inline styles or stylistic elements such as `<font>`. It also increases the likeliness of elements having multiple class names, increasing bloat.

**HTML is the foundation of the web, and like all foundations, they are extremely important to get right. Everything else follows.**