---
layout: post
title:  "Sinatra Project."
date:   2017-01-14 20:17:28 -0500
---



The basic idea my project, Scotchy was to let users make an account, then either add existing scotches they like to their account or make new ones. My orginal outline for the project was rather ambitious. The first Model I came up with looked something like this.

**User**

has_many :scotches
has_many :scotches through: user_scotches
username
password
email


**Scotches**

has_many :users
belongs_to :distiller
has_many :flavors
has_many :comments
name
age
abv%

**Flavor**

has_many :scotches

**Distiller**

has_many :scotches

**Comments**

Belongs_to :scotches


As you can see that’s a lot of migrations, models, and join tables. In addition to that I wanted everything to be need. A user would be able to look at a distiller page to find other scotches made by the same distiller. Scotch can be searched and organized by flavor as well. The problem I soon would run into is not so much setting up all the relationships and building out the schema and models but rather data validation. I felt that there were simply just to may distiller’s on top of flavor being really subjective to go the route I was going. So I refactored in a big way. I kept the basic premise of the app alive. My MVC outline became much easier to manage and I wouldn’t need to worry so much about validating everything. The new model changed to the following.

**User**

has_many :scotches
username
password
email


**Scotches**

has_many :users
has_many :comments
name
age
abv%
region


**Comments**

Belongs_to :scotches
scotch_id
user_id


This, I felt was a much easier model to follow. It allowed for users to add and like scotches as well as add comments. I would also still retain the ability to see what other users like certain scotches. 

Scotchy basic functionality is as follows. A user logs in, and is then taken to their profile page. Where it lists their Username links to add a scotch or make a new scotch. Then a list of scotches already liked. By each like scotch is a button to remove scotch from the like list, and finally a logout and delete account button. 

The removing a scotch in line was interesting. For such a simple click there were several things that needed to happen.  On button click the patch method would need to:

-	Find user by slug
-	Find the scotch the button is referring to by slug
-	Check to make sure the user is removing a scotch from his own user page and not someone else’s
-	Then remove said scotch from the has many relationship
-	Finally redirect back to an updated page with the scotch no longer liked.

The button set up on the front end was simple. I just needed to added a button inside the each loop I made for listing the scotches.

On the backend 

```
patch '/user/remove/:slug/:scotch_slug' do
    @scotch = Scotch.find_by_slug(params[:scotch_slug])
    @user = User.find_by_slug(params[:slug])

    if current_user.id == @user.id
      @user.scotches.delete(@scotch)
    else
      flash[:message] = "Please remove scotches from your profile only"
    end
    redirect "/user/#{current_user.slug}"
    
  end
```

I need to pass on 2 peices of data. Who the user is and what the scotch is. To do this I cam up with using `:slug` for user and refrenced the scotch as `:scotch_slug`. After that I just found each one by their slug method and removed the scotch from the user. 



For comments I wanted to have a user only leave one comment per scotch. To achive this i need to check to see if the whome ever is logged in has already left a comment. 

```redirect to  "/scotch/#{@scotch.slug}" unless !@scotch.comments.find_by(user_id: current_user.id)
```

This line of code took care of it for me by redirecting back to the scotch page unless a user did not leave a comment. 







