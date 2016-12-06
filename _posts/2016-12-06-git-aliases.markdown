---
layout: post
title:  "Git: handling whitespace changes"
date:   2016-12-06 17:59:36
categories: jekyll update
---
Some of the git aliases that I frequently use and that other find useful are
below.


### Add everything but the whitespace changes ###


{% highlight bash %}
  add-no-whitespace ="!f(){ \
    git diff -U0 --no-color -w $@ | \
        git apply --cached --ignore-whitespace --unidiff-zero -;\
  };f"

  anw =!git add-no-whitespace
{% endhighlight %}

### Add only whitespace changes ###

{% highlight bash %}
  add-only-whitespace = "!f(){ \
      git add -A; \
      git diff --cached -w | git apply --cached -R; \
  }; f"
  aws =!git add-only-whitespace
{% endhighlight %}

