---
layout: post
title: "Rescue lost code in Jupyter/IPython notebooks"
short_description: "One of the most useful tricks I've ever found for working with notebooks."
date:   2016-07-24 12:00:00 +0200
tags: [python, notebook]
image_preview: https://raw.githubusercontent.com/jupyter/design/master/logos/Favicon/favicon.png
---
I found this in a moment of great need [here](http://blog.rtwilson.com/how-to-rescue-lost-code-from-a-jupyteripython-notebook/). It explains it far better than I do so go check it out. In short though, to rescue a function that is still in your environment, but that you defined in a cell that you've since deleted, use the following nifty little function.
{% highlight python %}
def rescue_code(function):
    import inspect
    get_ipython().set_next_input("".join(inspect.getsourcelines(function)[0]))
{% endhighlight %}

