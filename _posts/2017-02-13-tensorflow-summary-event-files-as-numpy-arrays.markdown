---
layout: post
title:  "Tensorflow summary event files as numpy arrays"
date:   2017-02-13 01:58:00 +0000
tags: [data, programming, python, tensorflow, tensorboard]
short_description: Bypassing Tensorboard to import event files as arrays.
image_preview: https://avatars2.githubusercontent.com/u/15658638?v=3&s=200
---

Pass this function a directory path, and it returns the path to the most recently modified file in that directory. This can be used to get the most recent event file in your logdir.

{% highlight python %}
import glob, os
def get_event(dir_path):
    return max(
      glob.glob('{}/*'.format(dir_path)),
                key=os.path.getctime)
{% endhighlight %}

I usually organise my logdirs into `logs/{}/train/` and `logs/{}/test/` where `{}` is filled by the string ID of whatever task/exercise the events are for. This makes for nice categorisation in Tensorboard as well as ease of extraction using the function above, which we might use to return the tuple of most recent logs for training and test respectively.

{% highlight python %}
def get_events(ex_id):
    return (
      max(
        glob.glob('logs/{}/train/*'.format(ex_id)),
                  key=os.path.getctime),
      max(
        glob.glob('logs/{}/test/*'.format(ex_id)),
                  key=os.path.getctime))
{% endhighlight %}  

Let `scalar_str` be the string name of the scalar summary value you want to retrieve, e.g. in the following example, the first argument to `tf.summary.scalar` is the value you would use as `scalar_str`.

{% highlight python %}
    # op defining some metric you want to track
    accuracy = tf.reduce_mean(tf.cast(correct_prediction, tf.float32))
    tf.summary.scalar('accuracy', accuracy)
{% endhighlight %}

Given paths to event files, we can use built in Tensorflow functionality plus some list-to-array conversion and array manipulation to retrieve the values of the desired summary scalar. The following function returns a numpy array with one column corresponding to the global step and the other to the metric you have retrieved (`lc` stands for learning curve in this context).

{% highlight python %}
import numpy as np
from tensorflow.python.summary import event_accumulator
def get_lc(event_file, scalar_str):
    ea = event_accumulator.EventAccumulator(event_file)
    ea.Reload()
    lc = np.stack(
      [np.asarray([scalar.step, scalar.value])
      for scalar in ea.Scalars(scalar_str)])
    return(lc)
{% endhighlight %}
