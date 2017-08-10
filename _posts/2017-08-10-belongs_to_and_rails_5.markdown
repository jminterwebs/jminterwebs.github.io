---
layout: post
title:  "Belongs_to and Rails 5"
date:   2017-08-10 18:48:27 +0000
---


Rails 5 made some changes to belongs_to as seen [here](https://github.com/rails/rails/pull/18937). As one can see it is now a required by default, meaning you need the realation there upon creation. This is fine in most cases. A blog post belongs to the Author, and since the author wrote the blog post everything is valitdated nicely.

Lets change to making an app to handle a Tack and Field meet. A race has_many racers, and a racer belong_to a Race. In Rails 5 by default we cannot create a racer without assiging them to a race.  While this may be an easy work around of creating a racer once they sing up for a given race it can fall apart quickly.

What if the racers needed to submit previous race times, before they are assigned a race? We need to make a racer profile first then assign what races they belong to. In rails 5 by deafualt we can't. However there is a really easy fix to all of this. 

```
class Racer < ApplicationRecord
  belongs_to :race, optional: true
end
```

optional: true changes requiring a racer to belong to a race to being able to be optional. 




