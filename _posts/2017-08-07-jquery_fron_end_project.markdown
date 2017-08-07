---
layout: post
title:  "JQuery Fron End Project"
date:   2017-08-06 20:34:10 -0400
---


For this project we needed to add a Jquery(AJAX) front end to our existing rails project.  The repo for the project can be found [here](https://github.com/jminterwebs/whiskytrails). A quick summary of the app is users can search for whiskeys, like them and add comments for about the wiskeys. It also split downs the whiskeys by distiller and region. There are several take aways I got from this project here are a few of them.



Trubolinks don't like JQuery. I soon found myself quite stuck trying to get JQuery to modify the DOM, I had no idea what the problem was. When ever I would call `.on()` on anthing the app would eventually break and nothing would work right. The reason for this Is the way Turbolinks work. From their github repo:

>   Turbolinks makes following links in your web application faster. Instead of letting the browser recompile the JavaScript and CSS between each page change, it keeps the current page instance alive and replaces only the body (or parts of) and the title in the head. Think CGI vs persistent process.
>   

As you can imagine the above could be very problematic when you need things to be loaded in a certain way for the DOM to be properly modified by JQuery. After alot of research I found a remarably simple fix. Besides calling `$( document ).ready()` which waits till the document is full loaded, which might never happen with Turbolinks. We need to change the line of code to `$(document).on('turbolinks:load', function() { }`, which hooks into a turbolinks event that is fired on every page visit. 




Prototypes makes life easier sometimes. For my main navigation between Whiskeys, Distillers, and Regions. 

```
indexMaker = () => {
      $('nav a').on('click', function(event){
        event.preventDefault()
        let className = this.className
        if( className != "newWhiskey"){
      $.get(`/${className}`, function(data){
        let indexLists = new IndexLists()

          switch (className){

            case "whiskeys":
              indexLists.whiskeyList(data);
              break;
            case "distillers":
              indexLists.distillerList(data);
              break;
            case "regions":
              indexLists.regionList(data)
              break;
          }
      })
    }
      })

}
```

as you can see I also used `this` to pull the class name off the link that also corasonded to the proper route. I then used a switch statement to call the right method on the newly created indexLists. which then called the following code. 

```
IndexLists.prototype.whiskeyList = function(data){

  let whiskeyList = []

    for( let i=0; i <= data.length-1; i ++){

     whiskeyList.push(`<li id="${data[i].id}"> ${data[i].name} <span onCLick="addtoFavorites(${data[i].id})"> Add to Favorites </span> <span onClick="showInfo(${data[i].id})"> More info </li>`)
    }

  $('.showList').empty()
  $('.indexList').show().empty().append(whiskeyList)

}

IndexLists.prototype.distillerList = function(data){

  let distillerList = []
    for(let i=0; i <= data.length-1; i ++){
    distillerList.push(`<li> ${data[i].name} </li>`)
    }
  $('.showList').empty()
  $('.indexList').show().empty().append(distillerList)

}

IndexLists.prototype.regionList = function(data){

  let regionList = []
    for(let i=0; i <= data.length-1; i ++){
    regionList.push(`<li> ${data[i].country} </li>`)
    }
  $
  $('.showList').empty()
  $('.indexList').show().empty().append(regionList)

}

```

For the rest of the project I did run into some diffuclties at certain parts but nothing that I couldn't over come with relative east. Even my nested form to created new whiskeys was refactored to Jquery with out much of a problem. However I know I have several areas I can refactor for much cleaner code. But that for now is for another blog post and another day.



