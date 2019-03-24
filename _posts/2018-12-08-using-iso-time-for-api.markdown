---
layout: single
title:  "ISO-8601 UTC datetimes in web-applications"
date:   2019-03-02 12:23:00 +0500
---

The best way to handle dates in web-apps (especially a backend side) is using ISO-8601 standart (DB dates, input params, responses).

An example of an ISO-8601 UTC datetime: '2019-03-02T13:27:40Z'.

Usage:
* npm install moment --save or npm install moment-timezone --save:
{% highlight javascript %}
const moment = require('moment-timezone');
{% endhighlight %}

* get a Moment (UTC mode) by a current system time:
{% highlight javascript %}
moment.utc();
{% endhighlight %}

* print a current system time (my system time is GMT+5):
{% highlight javascript %}
moment().format();
// > '2019-03-02T18:27:40+05:00'
{% endhighlight %}

* print a current system UTC time (my system time is GMT+5)
{% highlight javascript %}
moment.utc().format();
// > '2019-03-02T13:27:40Z'
{% endhighlight %}

* get a Moment (UTC mode) by a UTC date (string)
{% highlight javascript %}
const utcDateString = '2019-03-02T13:27:40Z';
moment.utc(utcDateString);
{% endhighlight %}

* get a JS Date by a UTC date (string)
{% highlight javascript %}
moment.utc(utcDateString).toDate();
{% endhighlight %}

* check is a valid ISO 8601 date (string)
{% highlight javascript %}
const isValidDate = moment(utcDateString, moment.ISO_8601).isValid();
{% endhighlight %}

* convert a UTC date (string) to a Moment with changed timeozne
1. get a JS Date by a UTC date (string)
1. change the timezone to 'America/Los_Angeles'
{% highlight javascript %}
moment.utc(utcDateString).tz('America/Los_Angeles');
{% endhighlight %}

* convert a timezone date (string) to a Moment (with a timezone offset)  
1. get a Moment by a date (string) and a timezone  
1. get a Moment (UTC mode)  

{% highlight javascript %}
const losAngelesDate = 'March 2nd 2019, 5:27:40 am';
moment.tz(losAngelesDate, 'MMMM Do YYYY, h:mm:ss a', 'America/Los_Angeles').utc();
// > 2019-03-02T13:27:40Z
{% endhighlight %}

