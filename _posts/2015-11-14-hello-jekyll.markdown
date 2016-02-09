---
layout:     post
title:      "Hello, Jekyll!"
subtitle:   "A new journey begins"
date:       2015-11-14 10:41:37
author:     "Krzysztof Marciniak"
header-img: "img/post-bg-04.jpg"
---

<p>Hello everyone! It has been a while since my last visit here, mostly because there is always something to be done - university, work, hobbies, etc. Although I cannot guarantee that it will be better this time, I will at least try to describe what is going on.</p>

<p>As you can see quite a lot has changed. Sick of playing around with WordPress (updates, permissions, themes, etc.) I decided - influenced by <a href="http://esenti.net/">esenti</a> - to switch to <a href="https://jekyllrb.com/">Jekyll</a>. It is light, powerful and gives me another excuse to use Vim all the time ;-)
</p>

<p>
The theme I use is Clean Blog by Start Bootstrap (the original Bootstrap theme can be downloaded <a href="http://startbootstrap.com/template-overviews/clean-blog/">here</a>, the Jekyll one can be obtained from <a href="https://github.com/IronSummitMedia/startbootstrap-clean-blog-jekyll">github</a>). If I recall correctly, the only changes that have to be made is to add <code>gems: ["jekyll-paginate"]</code> to _config.yml and install <code>pygments.rb</code> and <code>jekyll-paginate</code> gems.
</p>

<p>
If you are still in doubt, just look at this beautiful syntax highlighting:

{% highlight python %}
from django.db.models import Model

class MyModel(Model):
    def save(self, *args, **kwargs):
        # do some stuff
        Model.save(*args, **kwargs)

    def emptyMethod(self):
        pass
{% endhighlight %}

{% highlight scala %}
def splitList(list: List[Int], n: Int): List[List[Int]] =
  list.foldLeft(List[List[Int]](List[Int]()))(
    (acc: List[List[Int]], elem: Int) =>
      if(acc.head.length < n) List(acc.head ::: List[Int](elem)) ::: acc.tail
      else List[Int](elem) :: acc
  )
{% endhighlight %}

Of course, the highlighting subsystem (in this case <code>liquid</code>) is embedded into Jekyll, but it looks even better with this theme (by the way: the last piece of code is a part of a solution for a Scala excercise on <code>fold</code> usage).
</p>

<p>Not only is Jekyll great in terms of accessibility, it is also very secure due to it not having any kind of authentication. Since it is based on generating static files, any kind of security control despite server-side is obsolete. That saves me a lot of hassle with permissions/constant updates etc. Of course, it needs to get updated because of possible security flaws in the generation mechanism/subsystems, but that is still less time-consuming and less irritating in my humble opinion.
</p>

<p>Wrapping up this short entry I would like to say that I am looking forward to writing some new things in here. They will be, as always, posted on Facebook and Twitter as well, so stay tuned.
</p>
