---
layout: post
title:  "Fantasy Football CLI gem"
date:   2016-09-26 01:21:41 +0000
---




I'm a big fan of fantasy football.  For the Gem CLI project I created a gem that accesses the NFLs fantasy API, displays the top ten players and their projected points for any given Sunday and position. You are then able to pick one of those players in the list and access additional information on them. This information includes the most recent notes on the player  and their current status. 

I ended up using [NFLs fantasy API](http://api.fantasy.nfl.com/) because of its ease of use and the ease of access of all the information. I could not use Nokogiri. While very robust and feature rich it does not parse JSON, which was how all  the data used is presented.  Instead I used a gem called [HTTPARTY]( https://github.com/jnunemaker/httparty). This gem is able to take JSON and parse into a hash, which then can be manipulated like any normal hash. 

So what is JSON? JSON is not a very disgruntled camper who couldn’t swim, rather it stands for **J**ava**S**cript **O**bject **N**otation. It is a easy way to read and exchange data. Such as the example below.

```
{"employees":[
    {"firstName":"John", "lastName":"Doe"},
    {"firstName":"Anna", "lastName":"Smith"},
    {"firstName":"Peter", "lastName":"Jones"}
]}
```


First I accessed the [Player Stats API]( http://api.fantasy.nfl.com/v1/docs/service?serviceName=playersStats). From the page several options are given on to access what information you want. The also give an example url.  From here I wrote a method to modify an url based on user inputs. The following method takes two arguments and modifies the url to access the correct information. The method then uses HTTPARTY to parse the JSON obtained in to a hash.

```
def self.url(position, week)
    url = "http://api.fantasy.nfl.com/v1/players/stats?statType=weekProjectedStats&season=2016&week=#{week}&position=#{position}&format=json&returnHTML=1"
    response = HTTParty.get(url)
    @players_hash = response.parsed_response
  end
```


This is where the data started to become a slight pain. I assumed the data would be presented something like this.

```
[
    {"statType"=>"weekProjectedStats",
    "season"=>"2016",
    "week"=>"3",
    "id"=>"2506546",
    "esbid"=>"AND180512",
    "gsisPlayerId"=>"00-0023645",
    "name"=>"Derek Anderson",
    "position"=>"QB",
    "teamAbbr"=>"CAR",
    "stats"=>{"1"=>"1", "5"=>"9"},
    "seasonPts"=>0,
    "seasonProjectedPts"=>7.42,
    "weekPts"=>0.12,
    "weekProjectedPts"=>0.36},
 ]
```

But no that would be way to easy. The data was actually presented like this. 

```
statType"=>"weekProjectedStats",
 "season"=>"2016",
 "week"=>"3",
 "players"=>
  [{"id"=>"2506546",
    "esbid"=>"AND180512",
    "gsisPlayerId"=>"00-0023645",
    "name"=>"Derek Anderson",
    "position"=>"QB",
    "teamAbbr"=>"CAR",
    "stats"=>{"1"=>"1", "5"=>"9"},
    "seasonPts"=>0,
    "seasonProjectedPts"=>7.42,
    "weekPts"=>0.12,
    "weekProjectedPts"=>0.36}, …
```

at first glance that hash within a hash to get into the players looks like a real pain. Howerver at a second glance it really was not. The fact that players was an array of hashes in itself and I did not need the other information I could directly call the players hash and iterate through it and pull all the data I needed. Cool stuff, now I simply need to make a new hash that’s only the name, projected points, and the id (the id field will be very important later on). 

So now I have a brand new hash of anywhere between 32 through over 100 players. This is too much information to display in a console, so I need to cut it down `first(10)` will grab me the first ten indexes in the array so now I’m down to a list of 10 players.  This smaller list of players is much easier to deal with in a console window, but it’s the first 10 players in the order they came up in the JSON data. No one cares about 10 random players, that needs to be fixed.  A little `sort_by` and `reverse` took care of that.  

This is where this started getting cool for me. I had clean data that I could display to my console, that was being pulled from the NFL. Next step I need some more details on these 10 players. Back to the NFL API to find the right one to use. I ended up using the [details API]( http://api.fantasy.nfl.com/v1/docs/service?serviceName=playersDetails). This API will pull the status of any given player and the last 10 notes and videos. It also pulls the data by the player “id” field from earlier which is why I needed to store it.  

Next step was to display the notes and status by the selected player. The hash was structured much the same way as the previous one however this time I needed data from both levels of the hash. The orginal method pulled down all the notes at once but this caused issues with the amount of information that was displayed at once. To fix I placed all the notes in an array and  set  `#show_detaill_view` to display the first note only. Then I programmed the console to ask if more notes should be displayed in `#next_note(note_number)` that takes the previous note number and displays the next note for the player and asks if more notes should be displayed, until no is picked or there are no more notes. The #next_note(note_number)` method also takes care of exiting the program or the first method in the program to pick another position. 


 



