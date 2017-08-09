---
layout: post
title:  "Building a Fantasy Draft Board"
date:   2017-08-08 21:58:45 -0400
---


At the very start of my course work on Lear.co I wrote a blog post about a Draft Board. I did build a very basic one using NodeJS a few years ago. While it worked Ok, it really wasn't special in any regard. It had no database, was not presistent at all, as well as many other things that could be better. So for my final project I have choosen to go back to begining. I am going to remake the draft board using a Rails back end, a React front end. In addition I will be pulling relveant data from [NFL's Fantasy API](http://api.fantasy.nfl.com/). 

The NFL api does have its limits. A few examples would be limiting lists to 100 players, and not getting all the data I need in one call. I may use it to see my own database. However the API does give me the data in JSON which will make things nice and easy regardless of what I do with it. 

I addition my own back end will support user accounts and league managment. I plan on having the ablity for a user to create a league and become commisoner, and the other user join that league. From their they will be able to draft players to their respected team.  

As of now my Schema looks as follows. 

```
USERS
  has_many Teams
  userName
  email
  password
  



TEAM
  belongs_to League
  belongs_to User
  has_many Players
  teamName
  



LEAGUES
  has_many teams
  LeagueName




PLAYERS
  belongs_to Team
  Name
  NFL Team
  Position
  Rank
	```
	
	
	There are numerous ways to realate all this data and I'm also leaving several data points out right now, namely all the projected stats. 
	
	



