---
author: "Lamont"
title: "Project: BC Daycare Map"
date: "2023-04-20"
description: "Daycare map for new parents in beautiful BC, Canada 🍁"
tags: ["project"]
draft: false
---

# Overview
I created a webapp to map licensed childcare facilities for parents in the Fraser Health region of BC.

Parents can intuitively search for licensed daycares near them and check Fraser Health's inspection reports. The data is pulled from the [Fraser Health Childcare database](https://www.healthspace.ca/Clients/FHA/FHA_Website.nsf/CCFL-Child)

It's used by new parents looking for daycares for their little ones. Parents can look at health inspections from Fraser Health and do further research before enrolling their child.

![BC Daycare Map](/images/bcdaycare-app/bcdaycare-screen.png)

# Why I Made It
We were searching daycares for our newborn daughter and was frustrated at the process. From talking to many parents, everyone just called a bunch of places nearby, be put on a waitlist with no timeline, and wait.

Although I couldn't fix the shortage of childcare spots, I noticed that searching for all childcare centers near me wasn't intuitive. Many had crude websites, others only had a Facebook page, and some weren't even listed online. And the Fraser Health's website did not have a map function, instead it only [listed all daycares alphabetically within a city](https://www.healthspace.ca/Clients/FHA/FHA_Website.nsf/CCFL-Child).

So, being an introvert, I put off phoning the childcare centers I could find and instead built a website to map out all the licensed daycares around in the Fraser Health region.

# How It Works
1. Data is scraped periodically from the Fraser Health website into PostgreSQL database
2. The website uses Flask framework and Google Maps API to render the map
3. Bootstrap5 for styling
4. Deployed on PythonAnywhere

# What I Learnt
* Government websites aren't the greatest 🙄
* Honestly a pretty straightforward project, it was nice to touch Python/Flask again after mostly using Ansible for 2 years
* Geocoding via Google Maps was great thanks to this [Python Client](https://pypi.org/project/googlemaps/)

# Possible Next Steps
I made this project to help out other parents and I think it's doing something 🥳!

From usage reports, I'm happy at least *__some__* parents have used it _(Jan 2023 shown below)_. It's not much, but I'm still ecstatic that this first-time side project has helped someone.

![January 2023 usage report](/images/bcdaycare-app/analytics.png)

I haven't worked on this website since last April, but some ideas for future improvements:
* I only fetched daycares in the Fraser Valley region since I was in that district. Fetching data from Vancouver Coastal would be the next step _(Each health authority has their own system)_
* Monitoring and CI/CD! _(Ashamed as a DevOps, but I needed to prioritize getting something out there between baby's naps 😴)_

If you have any bugs or feature requests, please add a [new issue](https://github.com/kronomon/fraser-health-childcare-map/issues) in the project.