---
layout: post
title:  "Putting all my Test Commands in a Script File."
date:   2016-09-03 20:56:21 +0000
---

**and a little bit of command line**
Today I am working on the Music CLI lab. It's big, it uses bands I never heard of which lead me down the dark abyss that is YouTube. Watching to a bunch of Neutral Milk Hotel, some cat videos, I got back to work on this lab. There are a lot of tests I would guess this lab has the most tests of any lab at the point. They are divided into 12 nice little files. 

Running rspec throws a pretty big error, it deals with not having the modules hooked up yet. Same thing happens when we try and run rspec --f-f, learn, learn --f-f. So that leave running each test on it's own. 

That means running all these commands in the terminal to test each file indivdually.

```
rspec spec/001_song_basics_spec.rb
rspec spec/002_artist_basics_spec.rb
rspec spec/003_genre_basics_spec.rb
rspec spec/004_songs_and_artists_spec.rb
rspec spec/005_songs_and_genres_spec.rb
rspec spec/006_artists_and_genres_spec.rb
rspec spec/007_findable_songs_spec.rb
rspec spec/008_findable_module_spec.rb
rspec spec/009_findable_artist_and_genres_spec.rb
rspec spec/010_music_importer_spec.rb
rspec spec/011_music_libary_controller_spec.rb
rspec spec/012_musiclibary_cli_spec.rb
```


I for one don't like having to type out rspec spec/001_song_basics_spec.rb in the command prompt to run the test. Even the fact that the up arrow gives me access to previous used commands, it still doesn't sit right with me. I also feel it isolates the tests way to much. It could be possible for me to get one set of tests passing that will in turn make the previous set fail. 

So that leaves me with a very good option. A script file. Sorry Windows folks this will be very MAC and Linux focused. We have already learned about CLI for Ruby and how how its just a file that calls the code you want to run without writing the code in the Terminal. Well a Script file works in much the same way. So we can take the above and place it into a file. 

The steps to do so is create a file in the root folder, mine was called testing. Using either the touch command or make new from your text editor. Next step is to make sure that we can run the file. 

We need to change the file to be excutable. If we run ls -l we get this  


![](http://i.imgur.com/x7eLqKs.png)



Those -rw--r-- lines repsersent what can be done with the file. Right now the 'sampletest' file i made is showing this -rw-r--r--. Which means it's no excutable. To fix this we can run `chmod 700 sample test`. This will change sample test to read -rwx-r--r--.  Awesome we made sampletest able to execute. So now when we can do something like this in sampletest.

```
#!/bin/bash
echo Hello World
```

and then run `./sampletest` and the output will be Hello World. same as if re just type `echo Hello World` into the terminal. So from the we can expand this out quite a bit. We can paste the code below in. Then we just need to run `./sampletest` and our test will run. We can go further and add --f-f to the end of some lines to make them fail fast. We can even comment out some lines if we don't want those test to run. Then we just save and run `./sampletest`.  After the set up it makes things simple and fast, for me at least. It also enabled me to write a blog post about it. Which is always good. 


```
#!/bin/bash
rspec spec/001_song_basics_spec.rb
rspec spec/002_artist_basics_spec.rb
rspec spec/003_genre_basics_spec.rb
rspec spec/004_songs_and_artists_spec.rb --f-f
rspec spec/005_songs_and_genres_spec.rb
rspec spec/006_artists_and_genres_spec.rb
rspec spec/007_findable_songs_spec.rb
rspec spec/008_findable_module_spec.rb
rspec spec/009_findable_artist_and_genres_spec.rb
rspec spec/010_music_importer_spec.rb
rspec spec/011_music_libary_controller_spec.rb
rspec spec/012_musiclibary_cli_spec.rb
```



