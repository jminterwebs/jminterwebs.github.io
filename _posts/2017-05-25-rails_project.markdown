---
layout: post
title:  Rails project
date:   2017-05-25 02:38:54 +0000
---



For my Rail project I expanded on my Sinatra project Scotchy by adding full functionality for whiskey from anywhere in the world.


My set up was as follows

User set up with Devise
	Omniauth with facebook
     has_many 	Whiskeys 
     has_many Whiskeys through user_whiskey
     :name
     :email
     :password

Whiskey
	has_many Users
	has_many Users through user_whiskey
	belongs_to Distiller
	:name
	:proof
	:distiller_id

Distiller
belongs_to Whiskey
belongs_to Region
:name

Region
	belongs_to Distiller
	:country
	:sub_region


The idea behind the application is that a user can create or like a whiskey. A user can also see other whiskeys made by certain distillers, and see what other distillers are nearby in that country.

The biggest issue I ran into is creating a double nested form.  The create whisky form has a nested form for creating new distillers, which has a nested form for creating new regions. This was even more complicated when fields_with_errors. To pass through both nests the whiskey controller looks as follows.


```
def new
	@whiskey = Whiskey.new
	@distiller = @whiskey.build_distiller
	@region = @whiskey.distiller.build_region
end
```
	

then the disitller controller is as follows

  

```
def new
    @distiller = Distiller.new
    @region = @distiller.build_region
  end
```

The nest works basicly the same way except each level need to relate to all lower levels. The big diffrence though is the belongs_to relationship. Due to the face belongs_to assicates with nil if the is nothing created yet .build cannot build off of it. To fix this the build_ fixes this for us. 







	


