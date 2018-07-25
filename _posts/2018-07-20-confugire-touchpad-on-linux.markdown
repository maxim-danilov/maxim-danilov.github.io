---
layout: single
title:  "Adjust your touchpad on Linux"
date:   2018-07-06 14:50:30 +0300
---

Linux systems support most of devices right out of the box. But default configurations of input devices are not always fine. Though Ubuntu has GUI settings to configure your pointing device it's not enough:

![image-title-here](/assets/img/notcompressed/0021.png){:class="img-responsive"}

So you can't remap buttons, set scrolling speed, tap time and etc using the settings. 

We're going to adjust your touchpad using `xinput` utility.

### Get input device list:
{% highlight bash %}
xinput --list
{% endhighlight %}

![image-title-here](/assets/img/notcompressed/0017.png){:class="img-responsive"}

### Get properties list of your input device:
{% highlight bash %}
xinput --list-props 'Maxim Danilov’s Trackpad'
{% endhighlight %}

![image-title-here](/assets/img/notcompressed/0018.png){:class="img-responsive"}

### Adjust property of your input device:
Device Accel Constant Deceleration
{% highlight bash %}
xinput --set-prop 'Maxim Danilov’s Trackpad' 'Device Accel Constant Deceleration' 0.1
{% endhighlight %}

![image-title-here](/assets/img/notcompressed/0020.png){:class="img-responsive"}

### Make your xinput commands permanent
{% highlight bash %}
vim .xsessionrc
{% endhighlight %}

