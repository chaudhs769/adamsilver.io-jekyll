---
layout: post
title: An introduction to DOM scripting forms
date:   2014-01-02 09:00:59
categories: forms js
description: Modifying and enhancing forms is easy with the DOM0 API.
---

The HTML DOM has dedicated APIs for retrieving and manipulating form controls. The [DOM0](http://docs.oracle.com/cd/E19957-01/816-6408-10/) Forms Collection API is perfect for this but unfortunately often forgotten. This article describes how to use this API.

## Accessing a form

Accessing a form with a Name attribute value of *login* is as follows:

	var loginForm = document.forms.login;

## Accessing form controls

Accessing the controls within the form is as follows:

	var loginControls = loginForm.elements;

Note: If the login form contained a username and password control then `loginControls` would be a collection of two controls.

## Accessing a control by name

There are several types of control including input, select, textarea, etc. Having retrieved the controls earlier we can loop over each control or retrieve a particular control by index or name. Retrieving a control by name is as follows:

	var usernameControl = loginControls.username;

## Inspecting a control value

Once we have a control we can interrogate its value:

	var usernameValue = usernameControl.value;

This is useful if you want to perform client side validation, for example.