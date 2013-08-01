---
layout: post
title: "How to disable Mac OS X Dashboard"
date: 2013-01-03 09:52
comments: true
categories: [mac os x]
---
For me, Dashboard is one of the worst features of Mac OS X. If Apple were to release it today, there would be all kinds of comments on how they are losing it and that it's just not the same since Steve left. But, did you know you can remove it? After a little searching, I found out that it's just another preference that can be set via the `defaults` command.

### Turn it off
To turn off Dashboard, open your terminal and enter the following

`defaults write com.apple.dashboard mcx-disabled -boolean YES`

Then logout your user, and log back in. Dashboard should be gone.

### Bring it back
If you decide that you want it back, it's just a matter of flipping the boolean.

`defaults write com.apple.dashboard mcx-disabled -boolean NO`

As before, you'll need to logout and login to see the difference.
