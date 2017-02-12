---
layout: post
title:  "Tensorflow summary event files as numpy arrays"
date:   2017-02-07 21:36:00 +0100
tags: [data, programming, python, tensorflow, tensorboard]
short_description: Bypassing Tensorboard to import event files as arrays.
image_preview: https://avatars2.githubusercontent.com/u/15658638?v=3&s=200
---

{% highlight python %}
from tensorflow.python.summary import event_accumulator
def get_lc(event_file):
    ea = event_accumulator.EventAccumulator(event_file)
    ea.Reload()
    lc = np.stack(
      [np.asarray([scalar.step, scalar.value])
      for scalar in ea.Scalars('error')])
    return(lc)
{% endhighlight %}
