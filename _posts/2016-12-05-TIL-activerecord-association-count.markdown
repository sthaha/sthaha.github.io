---
layout: post
title:  "TIL: ActiveRecord - Counting children"
date:   2016-11-25 17:59:36
categories: rails activerecord count
---

Given the following model `Post`

{% highlight ruby %}
# post.rb
class Post < ActiveRecord::Base
  has_many :comments
end
{% endhighlight %}

I wanted to find list of `posts` that has no `comments` on them.

{% highlight ruby %}
Post.includes(:comments).where(comments: {:post_id: nil})
{% endhighlight %}

A related one is to find the count of `children` for every `parent`

{% highlight ruby %}
Post.joins('LEFT OUTER JOIN comments on comments.post_id = post.id')
    .group('post.id')
    .select('post.id as id, count(comments.id) as comment_count')
{% endhighlight %}

which can be combined with `having`

{% highlight ruby %}
Post.joins('LEFT OUTER JOIN comments on comments.post_id = post.id')
    .group('post.id')
    .select('post.id as id, count(comments.id) as comments_count')
    .having('comments_count = ?', n)   # n can be 0
{% endhighlight %}

Reference: [Stack Overflow][so]

[so]:  https://stackoverflow.com/questions/5319400/want-to-find-records-with-no-associated-records-in-rails-3/5570221#5570221
