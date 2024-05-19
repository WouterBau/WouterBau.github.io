---
layout: post
title:  ".NET DevOps Revolution : Flexibility and Quality Control with Docker's Adaptive Arsenal - Techorama 2024"
date:   2024-05-16
description: Let's see how we can bring flexibility, stability and control to our DevOps pipelines by using Docker to containerize our build and quality tasks.
tags:
 - Techorama
 - .NET
 - DevOps
 - Docker
share: true
---

This post accompanies my latest titular presentation and lighting talk I gave at [Techorama](https://techorama.be/){:target="_blank"} 2024. I will be sharing the [slides]("/assets/media/.NET DevOps Revolution with Docker.pdf"){:target="_blank"} I used during the talk as well.

The examples are all based on my sand box project [ShadoRoller](https://github.com/WouterBau/ShadowRoller){:target="_blank"}. I've made a copy of the repo to keep it at this state. The links to this repo are in the slides.

I advice to scroll through the slide deck and check back here for more information!

# Scenario
ShadowRoller is a Discord Bot made in .NET with C#. We have automated Azure DevOps pipelines to perform Quality Control tests and create reports of their results. These include unit testing, code coverage collection, mutation testing and static code analysis. These are all made in Azure DevOps YAML tasks.

There is also a DockerFile for the application to have a consistent customized run environment for the application.

Now, let's see if we can also enjoy that promise of stability and flexibility for our Quality tasks by running them in a Docker container.
