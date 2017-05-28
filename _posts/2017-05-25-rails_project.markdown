---
layout: post
title:  Rails project
date:   2017-05-24 22:38:55 -0400
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
	


The idea behind the application is that a user can create or like a whiskey. A user can also see other whiskeys made by certain distillers, and see what other distillers are nearby in that country.

The biggest issue I ran into is creating a double nested form.  The create whisky form has a nested form for creating new distillers, which has a nested form for creating new regions. This was even more complicated when fields_with_errors. To pass through both nests the whiskey model looks as follows.


def distiller_attributes=(distiller)
      self.distiller = Distiller.find_or_create_by(name: distiller[:name])
      self.distiller.region_attributes=(distiller[:region_attributes])
      self.distiller.update(distiller)
  end
```
	

then the disitller model is as follows

  

```
def region_attributes=(region)
    self.region = Region.find_or_create_by(country: region[:country])
  end
```

The controllers

Whiskey
```
def new
    @whiskey = Whiskey.new
    @distiller = @whiskey.build_distiller
    @region = @whiskey.distiller.build_region
  end
```

```
def new
    @distiller = Distiller.new
    @region = @distiller.build_region
 end
```


The most difficult part of using a double nested form was making sure things wouldn't update twice and I would end up with redundant data. This was solved by calling update only once on the distiller_attribute method. 

	


