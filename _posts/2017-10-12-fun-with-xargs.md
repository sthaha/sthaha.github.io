---
layout: post
title: Fun with xargs and pipes
date: Thu 12/Oct/17 11:10:02
categories: shell tools xargs
---

## xargs -I ##

`xargs` accepts option `-I <replace-str>` which lets you replace all
occurances of the `replace-str` with the command.

A trivial example: `echo '>>foo bar ' | xargs -I cmd echo cmdcmdcmd `
prints
{% highlight bash %}
>>foo bar >>foo bar >>foo bar
{% endhighlight bash %}


## using pipe in xargs command ##

Lets take an hypthetical example of sorting the filenames by length
A solution to that would be to use `wc -c` to compute the length of the
filename and pass that to `sort -n`

{% highlight bash %}
ls /tmp | xargs -I {} echo "$(echo {} | wc -c) {}" | sort -nr
{% endhighlight %}

Which produces an **"unexpected output"** (note 3 instead of actual length) below
{% highlight bash %}
3 tracker-extract-files.1337
3 tmux-7331
3 tmp.9UVFYcbNfh
{% endhighlight %}


This happens because `$(...)` get evaluated by the shell and `echo {} | wc -c ` returns 3

In order to delay the evalaution of that expression `sh -c` comes in handy.

{% highlight bash %}
 ls /tmp | xargs -I {} sh -c 'echo "$(echo {} | wc -c) {}"' | sort -nr

 # prints
 27 tracker-extract-files.1337
 15 tmp.9UVFYcbNfh
 10 tmux-7331
{% endhighlight %}


## Other Examples ##

### Cleanup unused rvm gemsets ###

Deleting rvm gemsets requires inputing or typing a confirmation "yes" which
isn't hard to automate using the `yes` command. e.g.

{% highlight bash %}
  yes yes | rvm gemset delete foobar
{% endhighlight bash %}

But when there are lots of gemsets that you want to prune, `xargs -I {}`
comes in handy. E.g.

{% highlight bash %}
rvm list gemsets |
  grep project- |
  tr -s ' ' |
  cut -f2 -d ' ' |
  xargs --max-lines=1 -I {} sh -c "yes yes | rvm gemset delete {} "
{% endhighlight %}

