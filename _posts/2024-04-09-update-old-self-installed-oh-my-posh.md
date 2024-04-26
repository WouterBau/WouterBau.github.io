---
layout: post
title:  "Update an old self-installed Oh My Posh version to a version from the Windows store and get it working"
date:   2024-04-09
description: I had a way old self installed version from Oh My Posh, and I wanted to update it to the Windows Store version. Here's how I did it.
tags:
 - Powershell
share: true
---

I had a way old self installed version from Oh My Posh, and I wanted to update it to the Windows Store version. Here's how I did it.

# Install the new version from the Windows store
Following the link to the Windows Store found on the [documentation](https://ohmyposh.dev/docs/installation/windows){:target="_blank"} page of Oh My Posh, I installed the new version from the Windows Store. But you can use any distro of your liking.

# Configure to use the new version
The information on the store page mentioned to afterwards follow the instructed [next steps](https://ohmyposh.dev/docs/installation/windows#next){:target="_blank"}. I already had a font installed and in use. So I mainly had to replace the command to initialize the Oh My Posh.

{% highlight powershell %}
oh-my-posh init pwsh | Invoke-Expression
{% endhighlight %}

It took a few attempts to restart the terminal and profile. But at first it was the theme that was broken before it switched over to the default theme in the new version.

{% highlight powershell %}
. $PROFILE
{% endhighlight %}

# Install a new supported theme
Setting a theme works differently in this new version as well. The theme is configured as an argument on the initiliazation command in the Profile. You can choose one from the list. I opted to always download it from the raw file source on GitHub.

{% highlight powershell %}
oh-my-posh init pwsh --config "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/clean-detailed.omp.json" | Invoke-Expression
{% endhighlight %}

# Update NerdFont
When using the new version and having configured the new theme, I noticed some icons were missing in it. I looked up the latest version of the font I was using at [Nerd Fonts](https://www.nerdfonts.com/font-downloads){:target="_blank"} and installed it by unpacking the zip and dragging the `.ttf` files into the Fonts install drop space.

I had to fully restart the terminal again to see the finished result.

# Previous Version $PROFILE
{% highlight powershell %}
oh-my-posh init pwsh | Invoke-Expression
Import-Module -Name Terminal-Icons
Set-PoshPrompt -Theme pixelrobots
{% endhighlight %}

# New Version $PROFILE
{% highlight powershell %}
oh-my-posh init pwsh --config "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/lambdageneration.omp.json" | Invoke-Expression
Import-Module -Name Terminal-Icons
{% endhighlight %}