---
layout: post
title:  "TIL: Do not rescue Exception"
date:   2016-11-25 17:59:36
categories: ruby exception-handling
---

Note to self: Do not `rescue Exception` unless there is a very good reason (
log exception and re-raise )as it is results in handling all exceptions
including Syntax Errors, `Ctrl + C` (Interrupt)

{% highlight ruby linenos %}
# bad_exception_handling.rb
def main
  puts "try: kill -HUP #{$$}"

  begin
    sleep 20
  rescue Exception => e
    puts "Error:  #{e.class}: #{e}"
    retry
  end
end

main if __FILE__ == $0
{% endhighlight %}

Instead simply use `rescue => e` which is the same as writing
`rescue StandardError => e`

Ref: [StackOverflow post][so]

[so]: http://stackoverflow.com/a/10048406
