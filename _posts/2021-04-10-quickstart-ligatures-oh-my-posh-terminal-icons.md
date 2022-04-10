---
layout: post
title:  "Quickstart Fancy Editors & Powershell Terminal with Ligatures, Oh My Posh & Terminal Icons"
date:   2022-04-10
description: A quick guide with commands to get up and running quickly with Oh My Posh, Terminal Icons & Ligatures to have a fancy code editor and Powershell terminal experience
tags:
 - Powershell
share: true
---

It's been some time since I noticed the fancy looking terminals being used by presenters during tutorial clips. I was curious and eventually started to investigate how to set it up myself. I've collected the steps to do so, to use as a reference.  
**I won't go into deep details. So if you're following this, you better know what you're actually doing with below commands!**

# References
First things first, I figured it out using other blogs and the websites of the modules. I don't remember all of them, but let's show them some love:

- Ligatures:
    - [Nerd Fonts](https://www.nerdfonts.com/){:target="_blank"}
    - [https://worldofzero.com/posts/enable-font-ligatures-vscode/](https://worldofzero.com/posts/enable-font-ligatures-vscode/){:target="_blank"}
- Terminal:
    - [Oh My Posh](https://github.com/JanDeDobbeleer/oh-my-posh){:target="_blank"}
    - [Terminal Icons](https://github.com/devblackops/Terminal-Icons){:target="_blank"}
- Everything:
    - [Hanselman](https://www.hanselman.com/blog/take-your-windows-terminal-and-powershell-to-the-next-level-with-terminal-icons){:target="_blank"}
    - [https://medium.com/@hjgraca/style-your-windows-terminal-and-wsl2-like-a-pro-9a2e1ad4c9d0](https://medium.com/@hjgraca/style-your-windows-terminal-and-wsl2-like-a-pro-9a2e1ad4c9d0){:target="_blank"}
    - [https://dev.to/kasuken/cascadia-code-a-new-font-for-visual-studio-code-and-terminal-47oc](https://dev.to/kasuken/cascadia-code-a-new-font-for-visual-studio-code-and-terminal-47oc){:target="_blank"}

# Install CascadiaCode NerdFont
To get started with anything, we need to install a Nerd Font which supports ligatures, and which is supported by oh-my-posh. I didn't want to deviate too far from the standards. So I picked up the "CascadiaCode" version.

- Go to the Nerd Fonts website download page: [https://www.nerdfonts.com/font-downloads](https://www.nerdfonts.com/font-downloads){:target="_blank"}
- Search for "CascadiaCode" and download the latest one available. (Currently this was Caskaydia Cove Nerd Font)
- Install the font as you would any other font on your device

# Enable Ligatures in Editors
Do be sure to enable these, because they will start to override some default behaviors and views of characters. I like the sight of it, but it could be confusing for others.

## Visual Studio
- Open "Options"
- Open the "Fonts and Colors" tab
- Select "Cascadia Code" as your font for the text editor (Ligatures are supported out of the box)

## Visual Studio Code
- Open "Settings" and/or open "settings.json"
    - Ligatures can only be enabled in the json file
- Add `CaskaydiaCove Nerd Font, Cascadia Code` in front of the other options for the tag `editor.fontFamily`
- Add the value `true` for the tag `editor.fontLigatures`

# Enable Oh My Posh & Terminal Icons in Powershell
The next steps are best done in the "Terminal" Windows App. The changes will flow through to all other places where it is used, like Visual Studio Code.

- Configure `Cascadia Code` to be the default font for your Powershell Terminal. You can do this through the general configuration screens.
- Set the execution policy for the current user to unrestricted in Powershell, if needed for the following commands. I'll let you look up how to do this, so you also know how this affects your whole Powershell!
- Install the Posh GIT Module:  
{% highlight powershell %}
Install-Module posh-git -Scope CurrentUser
{% endhighlight %}
- Install the Oh My Posh Module:  
{% highlight powershell %}
Install-Module oh-my-posh -Scope CurrentUser
{% endhighlight %}  
If it doesn't want to install, add the `-Force` flag to the command.
- Install the Terminal Icons Module:  
{% highlight powershell %}
Install-Module -Name Terminal-Icons -Repository PSGallery
{% endhighlight %}
- Open your Powershell Profile with a code editor:  
{% highlight powershell %}
code $PROFILE
{% endhighlight %}
- Alter the Profile script to include these steps at start-up to start the modules and set a theme. Look up what theme you like on [https://ohmyposh.dev/docs/themes](https://ohmyposh.dev/docs/themes){:target="_blank"}:  
{% highlight powershell %}
Import-Module oh-my-posh
Import-Module -Name Terminal-Icons
Set-PoshPrompt -Theme pixelrobots
{% endhighlight %}
- Save, close, restart your terminal & enjoy!

# Postscript
And after writing this quickstart, I find a recent docs article from Microsft describing how to do all this too. That's what you get when you keep a large backlog of things you want to write about!  
Enjoy and use this too if you get stuck: [https://docs.microsoft.com/en-us/windows/terminal/tutorials/custom-prompt-setup](https://docs.microsoft.com/en-us/windows/terminal/tutorials/custom-prompt-setup){:target="_blank"}