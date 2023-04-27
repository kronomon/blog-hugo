---
author: "Lamont"
title: "Project: Fantasy Hockey Assistant"
date: "2023-04-25"
description: "Chrome web extension for Yahoo Fantasy Hockey"
tags: ["project"]
draft: false
---

# Overview
I play in a weekly head-to-head categories hockey league on Yahoo Sports. This means that every week, you're matched with an opponent and compete in a variety of categories _(ex. goals, assists, etc)_. It doesn't matter how much you win by in each category, but the goal will be to win the most categories every week.

![Yahoo Fantasy Hockey Weekly Matchup](/images/fantasy-hockey-app/matchup.png)

When preparing for matchups I used to manually calculate expected production from my players to optimize matchups in Excel. This was really cumbersome as I would have to copy-paste player stats every week. This was a repeated process that started from scratch every week so I thought _"This could be automated!"_

I created a Chrome Extension that automates predicted performance based on a player's per-game stats and the number of games they have during the week within the Yahoo website. This helps managers optimize their lineup throughout the season and win more matchups.

![Chrome Extension Screenshot](/images/fantasy-hockey-app/screenshot.png)

You can check out the Chrome Extension [here](https://chrome.google.com/webstore/detail/fantasy-hockey-assistant/ojcnjbfcabkjbndahgpiocpkofldojpk?hl=en)


# Why I Made It
A very simplified method of projecting how a team will perform is taking each hockey player's per-game stats and multiplying them by the number of games he has during the week.

Overall, this strategy worked well for me, as I got 1st place in the second year of playing in my 14-team league 🏆.

![The Crosmonauts Trophy](/images/fantasy-hockey-app/trophy.png)

When calculating the projected stats, I had to manually go into Excel and paste the data from numerous sources. This was an extremely tedious task.

So I automated it with this extension!

Now each player's expected stats for a given timeframe can be easily viewable straight from the Yahoo Fantasy Sports website.

---

Personally, I wanted to try something new. I saw a few reddit posts of others creating an [extension](https://www.reddit.com/r/fantasyhockey/comments/mc6fr6/i_made_a_chrome_extension_that_embeds_the_lineup/) and was curious.


# How It Works

The main meat of the project is a content-script.js file which modifies the Yahoo Fantasy Hockey website whenever it sees a player name in a table. It fetches data from the unofficial [NHL API](https://gitlab.com/dword4/nhlapi).

A popup window can be accessed via the browser's toolbar. This page has settings users can change to fit their league settings, which are then stored in Chrome's local cache.

Lastly, a service worker runs in the background to detect any events and triggers updates to the page content.

```goat
           .---------.
           | NHL API |
           '---------'
                ^
                |
        .-------+-----------.             .------------.
        | content-script.js |<------------+ popup.html |
        '-------------------'    update   '------------'
                ^                configs
                | send update
                | message
        .-------+-----------. 
        | service-worker.js |
        '-------------------'
                                                                
```
* _content-script.js_ - this script runs in the context of the web page and is the file that modifies what the user sees
* _service-worker.js_ - background script that catches events in the Chrome browser, such as when a new tab is created. Ajax requests don't refresh pages, so I use this script to send an update message to content-script
* _popup.html_ - a page to change extension options using the browser cache
* _NHL API_ - content-script.js requests the NHL API to fetch players' in-season stats


# What I Learnt

Even for a pretty simple extension, there are a few moving parts that I learned:
* Chrome's capabilities like handling browser events and caching
* async requests using _await fetch_ 
* refresher on CSS and importance of [colours](https://coolors.co/contrast-checker/112a46-acc8e5)
* a new word: [diacritic](https://en.wikipedia.org/wiki/Diacritic) _(thankfully I can [normalize](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/normalize) players' names like Tim Stützle easily)_

---

I was also happy to be able to make something from scratch, and release it into the public on the Chrome Web Store.

It turns out publishing a chrome extension is [quite painless](https://developer.chrome.com/docs/webstore/publish/). It only costs a $5 registration fee and my extension was published after a few days.

I've also discovered it is a deep ecosystem, including monetized extensions that show off [whimsical cats](https://chrome.google.com/webstore/detail/tabby-cat/mefhakmgclhhfbdadeojlkbllmecialg) and [inspiring landscapes](https://chrome.google.com/webstore/detail/momentum/laookkfknpbbblfpciffpaejjkokdgca). Even large companies like [Jungle Scout](https://chrome.google.com/webstore/detail/jungle-scout/bckjlihkmgolmgkchbpiponapgjenaoa) and [Honey](https://chrome.google.com/webstore/detail/honey-automatic-coupons-r/bmnlcjabgnpnenekpadlanbbkooimhnj) started off as purely extensions. 


# Possible Next Steps
Overall I'm happy with the results. The extension shows a player's expected stats and shares convenient links for further research.

![Screenshot](/images/fantasy-hockey-app/screenshot-stats.png)

Some things that could be improved:
* a more sophisticated model of predicted stats
    * the NHL API is not granular and I can only get a players' stats per season
    * an option to get average stats in the last 30 days could show hot & cold streaks
    * I can take strength-of-schedule into account _(more points are expectted if the opponents have an above-average GAA)_
* currently the app shows each players' stats, but can display the team's total points for a matchup
    *  just not sure where I'd display it on the page with nice UI
