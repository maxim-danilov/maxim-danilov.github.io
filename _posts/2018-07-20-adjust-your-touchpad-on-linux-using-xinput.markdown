---
layout: single
title:  "Adjust your touchpad on Linux using xinput"
date:   2018-07-06 14:50:30 +0300
---

Linux systems support most devices right out of the box. But default configurations of input devices are not always fine. Though Ubuntu has GUI settings to configure your pointing device they're not enough:

![image-title-here](/assets/img/notcompressed/0021.png){:class="img-responsive"}

So you can't remap buttons, set scrolling speed, tap time and etc using the settings. 

We're going to adjust your touchpad using `xinput` utility.

### Get input device list:
This command shows your input device list:
{% highlight bash %}
xinput --list
{% endhighlight %}

My touchpad has name `Maxim Danilov’s Trackpad`:
![image-title-here](/assets/img/notcompressed/0017.png){:class="img-responsive"}



### Get properties list of your input device:
Next we retrieve property list of the touchpad:
{% highlight bash %}
xinput --list-props 'Maxim Danilov’s Trackpad'
{% endhighlight %}

![image-title-here](/assets/img/notcompressed/0018.png){:class="img-responsive"}

### Adjust the properties of your input device:
I'd like to increase my pointer speed using `Device Accel Constant Deceleration` property. The default value of the property is 2.5. I will change it to 0.1 that will increase speed.

{% highlight bash %}
xinput --set-prop 'Maxim Danilov’s Trackpad' 'Device Accel Constant Deceleration' 0.1
{% endhighlight %}

![image-title-here](/assets/img/notcompressed/0020.png){:class="img-responsive"}

### Make your xinput commands permanent
Xinput resets your adjustments after reboot. We can make the adjustment permanent using `~/.xsessionrc`. Just put your xinput commands to `~/.xsessionrc` file.
