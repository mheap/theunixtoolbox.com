---
title: Special cron entires
layout: post
date: 2015-05-05
---

There are some special entries that can be used when creating a crontab entry (`crontab -e`), most of which are just shortcuts for the standard crontab entries that we all know and love.

You can use them in your crontab just like you would a normal entry:

{% highlight bash %}
# m h dom mon dow  command

# Say hello each time the machine boots up
@reboot echo 'Hello world. I just booted up'

# Say Happy new year, using both forms of entry
@yearly echo 'Happy new year'
0 0 1 1 * echo 'Happy new year from me too'
{% endhighlight %}

The `@reboot` entry could be useful for keeping track of when a machine is rebooted. Just add the crontab and each time the machine restarts you'll get an email with any output from the crontab (in this case, "Hello world. I just booted up")

### Possible entries

There are nine possible aliases, some of which are just aliases for each other (such as `@yearly` and `@annually`)

| Entry     | Description          | Equivalent entry |
|-----------|----------------------|------------------|
| @reboot   | Run once, at startup | None             |
| @yearly   | Run once a year      | 0 0 1 1 *        |
| @annually | Same as @yearly      | 0 0 1 1 *        |
| @monthly  | Run once a month     | 0 0 1 * *        |
| @weekly   | Run once a week      | 0 0 * * 0        |
| @daily    | Run once a day       | 0 0 * * *        |
| @midnight | Same as @daily       | 0 0 * * *        |
| @hourly   | Run once an hour     | 0 * * * *        |

I'm not sure I'd recommend using any of these (except maybe `@reboot`) as the standard syntax is well known by anyone that should be editing a crontab, but it's interesting to know that there are aliases in there for common time periods.


(via [mkaz (now offline)](https://mkaz.com/2006/05/29/unix-crontab/))
