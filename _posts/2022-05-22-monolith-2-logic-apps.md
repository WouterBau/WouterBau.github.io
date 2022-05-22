---
layout: post
title:  "Breaking a Monolith Data Import into Azure Logic Apps Quickly - Meetup & Techorama Presentation"
date:   2022-05-22
description: Want a real world example of making Azure Logic Apps and existing applications work together to simply import data from files?
tags:
 - Azure
 - Logic Apps
share: true
---

# What's it about?
Everyone probably has made them: Scheduled tasks to download files and import the data into a different database after some transformations and business logic. But those can get vulnerable quickly by piling up its purposes and external dependencies, creating a small monolith. Azure has different resource types to help you create these kind of tasks.

But if you start from an existing one, you can use Azure Logic Apps to offload your external connections without any code. Combine it with Azure Storage Accounts and Service Bus to create a whole extendable integration solution. We'll start with an Azure Function that was responsible for all steps of an import, and break it down with Logic Apps to the point where it can ingest files from different sources at the same time.

# Presentations
I've given a presentation about this topic during a livestreamed MeetUp event! You can rewatch it on demand on YouTube: [https://www.youtube.com/watch?v=QRb0aP9qVXg](https://www.youtube.com/watch?v=QRb0aP9qVXg){:target="_blank"}.

You can find the code examples on this GitHub Repository: [https://github.com/WouterBau/monolith-2-logic-apps](https://github.com/WouterBau/monolith-2-logic-apps){:target="_blank"}.

I'm also very proud to say I was given the opportunity to give a shorter version of it at [Techorama](https://techorama.be/){:target="_blank"}, a yearly international technology conference! A small dream that came to completion! And I hope to see you at the future ones as well.

![Techorama Calendar](/assets/images/techorama-calendar-2022.png "Techorama Calendar")