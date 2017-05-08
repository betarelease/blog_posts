---
layout: post
title: Rails Concerns for Serializers
tags: [rails, ruby, serializers, concerns, protip]
---

## Serializers for Rails
ActiveModel Serializers pack a lot of punch when it comes to dealing with objects. They
allow you to use configuration that takes care of generating the proper serialization for
conventional rails objects.
Once your code matures to where finding reuse is not difficult, you may run into a situation
where you want to leverage shared functionality via mixins. I ran into such a situation and was
not able to find quick, clear documentation. Hence this effort to write it down.
<!--more-->

## Concerns

In our domain we often find our code converting values into normalized and denormalized forms.
When doing this de/normalization operation some of our code was scattered or worst duplicated in parts.
So I wrote up the common reusable functionality as a concern and moved it into
*app/serializers/concerns* folder. To my dismay this refused to work, throwing one of the common errors 'UndefinedConstant'. I ran into this to realize that I need to [follow a convention to find the concern](https://github.com/rails-api/active_model_serializers/issues/2091).The last but one comment on this issue describes the convention to be followed - and it is no surprise, just the same folder structure as where these concerns are included to find them. Makes sense.

I tried running specs after the above modifications I ran into the same error.

## Configuration in application.rb
I discovered that I do need to add this new concerns folder(not the folder structure underneath), just to the level of the concerns into the eager_load_paths as follows

{% highlight ruby%}
config.eager_load_paths << Rails.root.join(*%w(app serializers concerns)).to_s
{% endhighlight %}

## Result

It works. This is here so that it can help you in a bind. Code away!
