---
layout: post
title:  "Vibe coded Jekyll site with GitHub Copilot"
date:   2025-10-13
description: "Why and how I used GitHub Copilot to quickly create a Jekyll website for my mother's art business."
tags:
 - Jekyll
 - GitHub Copilot
share: true
---
I've just done the last bits of configuration and changes to bring the new website online for my mother's art business: [https://www.clairedepaepe.be](https://www.clairedepaepe.be){:target="_blank"}.
I literally done almost no coding myself at all on this Jekyll website.
I've used GitHub Copilot to generate all the files and code.
The only thing I provided were the images and links.
The [repository](https://github.com/WouterBau/clairedepaepe.be){:target="_blank"} is located on GitHub where you can see the different commits I made with every step.

The basis and first version of the website was done in just about an hour.
Perfect since I was short on time and knew styling was needed as well.
Therefore I used this project as an example for a presentation at my employer to showcase how we could use AI tools to speed up our development.
Feel free to check the [slides](/assets/media/AI Bites 202507.pdf){:target="_blank"} if you're interested.

A word I didn't include or think of at that time was "vibe coding".
While it was a great example of it:
- I had a vague idea of what I wanted to achieve
- I provided a base of imagesin tthe repository
- I used GitHub Copilot agent mode to generate everything for the project
- I iterated on the output until I got what I wanted
- I ended up with a great result in a fraction of the time it would have taken me to do it myself

And most importantly:
- It was low stakes securitywise
- It was very small in scope
- It was a personal project
- I could easily redo it if needed

For professional projects I would be more careful and selective in letting all reigns loose to GitHub Copilot or something similar.
There is so much more to think about when you bring in authentication, data storage, business logic, security, ...
I still believe that AI tools can be a great help in speeding up design and development!
Still, you better keep an eye out on what it generates to make sure it's enterprise production worthy.

But for small projects, prototypes, experiments, ... vibe coding brings a great deal of value and you can focus on the fun interesting parts of the project.

We'll see what the future brings when multi-agent setups become mainstream...