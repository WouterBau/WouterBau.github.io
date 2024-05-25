---
layout: post
title:  ".NET DevOps Revolution : Flexibility and Quality Control with Docker's Adaptive Arsenal - Techorama 2024"
date:   2024-05-25
description: Let's see how we can bring flexibility, stability and control to our DevOps pipelines by using Docker to containerize our build and quality tasks.
tags:
 - Techorama
 - .NET
 - DevOps
 - Docker
share: true
---

This post accompanies my latest titular presentation and lighting talk I gave at [Techorama](https://techorama.be/){:target="_blank"} 2024. I will be sharing the [slides]("/assets/media/.NET DevOps Revolution with Docker.pdf"){:target="_blank"} I used during the talk as well.

The examples are all based on my sand box project [ShadoRoller](https://github.com/WouterBau/ShadowRoller){:target="_blank"}. I've made a [copy](https://github.com/WouterBau/ShadowRoller-NET-DevOps-Revolution-Docker){:target="_blank"} of the repo to keep it at this state. The links to this repo are in the slides.

I advice to scroll through the slide deck and check back here for more information!

# Scenario
ShadowRoller is a Discord Bot made in .NET with C#. We have automated Azure DevOps pipelines to perform Quality Control tests and create reports of their results. These include unit testing, code coverage collection, mutation testing and static code analysis. These are all made in Azure DevOps YAML tasks.

There is also a DockerFile for the application to have a consistent customized run environment for the application.

Now, let's see if we can also enjoy that promise of stability and flexibility for our Quality tasks by running them in a Docker container. Look into the slides for what possibilities this setup brings and what to take into account.

# Example 1: Unit Testing + Code Coverage
This is based on the "complex code" [example](https://github.com/dotnet/dotnet-docker/tree/main/samples/complexapp){:target="_blank"} from the .NET Docker documentation.

- Use the donet/sdk image as the base of ours
- Copy all the project files and the solution file to the image to restore dependencies
- Copy all other files to the image and build the solution
- Run the command to perform unit tests and code coverage collection with reporting enabled as the entry point.

By using an entry point to perform the tests, we can use volume binding to get the results back to the host machine.

# Example 2: Mutation Testing
We extend the previous example to also include generating mutation testing reports.

- Restoring the dependencies remains the same
- We moved building the solution to it's own stage, to be used by the unit testing stage
- A new stage is added, using the restore stage as base. This stage installs `dotnet-stryker` and run the mutation testing command as an entry point.

Mutation testing does multiple rebuilds of your application. So we've split it from the original build to keep the amount of builds to a minimum.

# Example 3: Static Code Analysis
The mutation testing example gets extended with steps to perform SAST with SonarQube.

- The restore and mutation testing stages remain the same
- A new stage is created where we
    - Install Java to use SonarQube Scanner
    - Install the SonarQube Scanner as a dotnet tool
    - Start SonarQube Scanner with the necessary parameters
    - Build the application and generate the unit test and code coverage reports
    - Stop the SonarQube Scanner, so it can send data to its server
    - Add a new stage with a copy command as entry point to get the reports back to the host machine

SonarQube requires actions before and after the build of our application. So we had to perform the steps of which we need the results in SonarQube as part of the build. So we had to get creative to get an entry point working for the volume binding

# Example 4: Automation with Azure DevOps Pipelines
We create an Azure DevOps Pipeline in YAML to build and run the docker images from our last example.

- All necessary variables are stored and collected in a safe fashion
- We create 2 seperate jobs to run in parallel, for the SAST scanning and Mutation Testing simultaneously
- The jobs have tasks to
    - Build the Docker image and run it with the necessary parameters, using volume binding to get the reports out
    - Publish the reports to the Azure DevOps Pipeline for clear visibility

This way we were able to use the strengths of all platforms and possibilities!

# Where to go next?
## Publish hybrid / desktop apps
The same setup can also be used to have a consistent way of publishing apps that are not optimal to run in a container. You can perform the command to publish the app as an entry point in the Dockerfile. Then you can also access the generated files through a volume binding on the host machine and bring them where necessary.

## How to handle NuGet packages
Publishing of NuGet packages in this kind of setup can be done by executing the pack command in the DockerFile. You can add the push to the NuGet source in the DockerFile or let it be performed by the host machine with binaries received from the dockerized publishing and packing. This depends on the authentication and connection requirements of your NuGet source.

## Moving to another CI/CD platform
Moving to another CI/CD platform should now only consist of making the necessary automation pipelines to trigger the same commands to build and run the expected stages of the Docker setup. These pipelines then perform the platform specific actions to have the integrated dashboarding. The runner hosts being used by this platform just needs to support containers to build and run.

![Techorama Calendar](/assets/images/techorama-calendar-2024.png "Techorama Calendar")