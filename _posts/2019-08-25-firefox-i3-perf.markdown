---
layout: post
title:  "Fix poor firefox performance on i3"
date:   2019-08-25 22:44:50 +0300
categories: arch linux i3 wm compton firefox telegram issue performance
---
![Firefox on Arch](/assets/post1_title.jpg)
On my new laptop with i7-8750h and fresh Arch Linux install - Firefox was laggy as hell. Whant to know why and how to fix it? 
<sub> (TLDR: install compton)</sub>

Recently I've purchased a new laptop: Dell XPS 9570.
I've installed Arch Linux, installed the same packages that I'd on my desktop system for several months, copied [dotfiles][dotfiles-github]

I'd chosen i3 as a window manager, without any DE or display managers.
I run it from bare `.xinitrc` with
{% highlight shell %}
exec /bin/i3
{% endhighlight %}

And something strange happened - Firefox on that i7-8750h system was laggy as hell. When I tried to scroll a page it was hanging for 2-5 seconds.
Chromium worked fine, everything worked fine - except Firefox. 
Oh, and also Telegram. And it's like my main communication method for the past 2 years, so I needed to fix it. 
I may be able to replace Firefox with Chromium, but Telegram... I can use web-client, but it's not the same as native desktop app experience. 

I tried to find any info regarding this: maybe somebody had issues like and fixed it.
I'd encountered this:

>i3 does not properly implement double buffering hence tearing or flickering may occur. [See Compton.][arch-i3-tearing]

>So, the culprit was a recent kernel [issue with the Intel drivers][reddit-firefox-issue] ... a workaround is actually installing a composite manager

So, I'd installed compton, added this to `.config/i3/config`

{% highlight shell %}
exec --no-startup-id "compton -b -C -f --config /home/gunter/.config/compton/compton.conf"
{% endhighlight %}

Created that `.config/compton/compton.conf` mentioned above - ping me on twitter if you need my config, I will publish it to [dotfiles][dotfiles-github] eventually, but now I'm too lazy to do backups. But you can use any config you want.

As far as I can tell - compton fixed firefox and telegram lags, I guess it may be related to double-buffering, or some driver issue mentioned above.
But for now - it works perfectly.

Btw - desktop PC on i5-8600k with RTX2070 works well without compton `¯\_(ツ)_/¯`

[dotfiles-github]: https://github.com/SuddenGunter/dotfiles
[arch-i3-tearing]: https://wiki.archlinux.org/index.php/I3#Tearing
[reddit-firefox-issue]: https://www.reddit.com/r/i3wm/comments/4wqwcf/why_would_an_application_run_slow_in_i3_and_fast/d6loavq?utm_source=share&utm_medium=web2x