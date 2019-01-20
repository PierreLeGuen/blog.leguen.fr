---
title: Fix HP Spectre 13 not going to sleep mode.
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
sidebar:
  nav: main
classes: wide
excerpt: How to fix HP Spectre 13 (v001nf) not going to sleep/hibernate mode on Linux.
header:
  teaser: images/suspend-fix/problem.jpg
  og_image: images/suspend-fix/problem.jpg
tags:
- how-to
- systemctl
- issues
---

# The problem

After switching from Windows to Linux I faced a problem with my HP Spectre, when I close the lid the laptop doesn't go in sleep mode.

I looked for a workaround on the web without success.

{% include image.html file="problem.jpg" alt="The problem" img_link="true" fig_caption="Photo by Hans-Peter Gauster on Unsplash" %}

# Fixing the issue

The way to fix this is by browsing the /proc/acpi/wakeup file. This way you can find what wake up the laptop from suspend.

It appears that the power buttokhn is the issue here. One thing to know here is that the /proc/acpi/wakeup file can't be edited directly. To achieve what we want, we need to create a service that will change the value for us.

## The script

Let's create the service that will start a bash script for us at start up.

* Create the file, e.g : ```$ touch suspendfix_pwrb.service```

* Edit the file with the followging service :

```bash
[Unit]
Description=fix to prevent system from waking immediately after suspend, this is due to a bug with the power button.

[Service]
ExecStart=/bin/sh -c '/bin/echo PWRB > /proc/acpi/wakeup'
Type=oneshot
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

## Starting the service

* Now enable our service using : ```# systemctl enable ./suspendfix_pwrb.service```

This finally resolve our issue. After some research I really can't find why the PWR button sends a wake up signal immediately after suspend.

# Conclusion

I hope this solves your issue with suspend on your HP Spectre 13. Feel free to contact me if it did not.