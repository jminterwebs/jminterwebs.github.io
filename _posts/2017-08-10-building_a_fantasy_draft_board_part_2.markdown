---
layout: post
title:  "Building a Fantasy Draft Board part 2"
date:   2017-08-10 18:29:07 +0000
---


So I decided to seed my own fantasty data from the [NFL fantasy](http://api.fantasy.nfl.com/v1/docs) api.

The seed was rather simple as you can see from below. HTTParty did almost all the heavy lifting. 

```
require 'httparty'
def player_seed(offset)
    url = "http://api.fantasy.nfl.com/v1/players/editordraftranks?count=100&format=json&offset=#{offset}"
    response = HTTParty.get(url)
    players_hash = response.parsed_response
    players_hash = players_hash["players"]

    players_hash.map do |player|
      if player["position"] == "QB" || player["position"] == "RB" || player["position"] == "WR" || player["position"] == "TE" || player["position"] == "DEF" || player["position"] == "K"

        Player.create(first_name: player["firstName"],
          last_name: player["lastName"],
          player_id: player["id"],
          teamAbbr: player["teamAbbr"],
          rank: player["rank"],
          position: player["position"]
          )
      end
    end

end

def seed_db
player_seed(0)
player_seed(100)
player_seed(200)
player_seed(300)
player_seed(400)
player_seed(500)
end
seed_db
```

I did have to clean the data a litte though. The player ranking api, ranks defensive platers and linemen as well. I only needed players playing at the positions I filtered out abouve. I also needed to run #player_seed 6 times because the NFL api limits returns to 100 at a time. 

