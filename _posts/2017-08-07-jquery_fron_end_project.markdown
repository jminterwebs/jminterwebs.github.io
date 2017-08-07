---
layout: post
title:  "JQuery Fron End Project"
date:   2017-08-07 00:34:09 +0000
---


For this project we needed to add a Jquery(AJAX) front end to our existing rails project.  The repo for the project can be found [here](https://github.com/jminterwebs/whiskytrails). A quick summary of the app is users can search for whiskeys, like them and add comments for about the wiskeys. It also split downs the whiskeys by distiller and region. There are several take aways I got from this project here are a few of them.



Trubolinks don't like JQuery. I soon found myself quite stuck trying to get JQuery to modify the DOM, I had no idea what the problem was. When ever I would call `.on()` on anthing the app would eventually break and nothing would work right. The reason for this Is the way Turbolinks work. From their github repo:

>   Turbolinks makes following links in your web application faster. Instead of letting the browser recompile the JavaScript and CSS between each page change, it keeps the current page instance alive and replaces only the body (or parts of) and the title in the head. Think CGI vs persistent process.
>   

As you can imagine the above could be very problematic when you need things to be loaded in a certain way for the DOM to be properly modified by JQuery. After alot of research I found a remarably simple fix. Besides calling `$( document ).ready()` which waits till the document is full loaded, which might never happen with Turbolinks. We need to change the line of code to `$(document).on('turbolinks:load', function() { }`, which hooks into a turbolinks event that is fired on every page visit. 






