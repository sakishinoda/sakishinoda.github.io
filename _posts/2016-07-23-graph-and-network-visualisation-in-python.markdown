---
layout: post
title:  "Graph and network visualisation in Python"
date:   2016-07-23 10:39:00 +0200
tags: [data, programming, visualization]
---

I've been playing around with graphical models in Python, and decided to pull together some of the resources I've seen and thoughts I've had regarding network/graph visualization in Python using Networkx. A few things to note:

* This is about visualization only, not analysis
* I hope to write further posts on using GraphViz using the python wrappers pydot and pygraphviz, and later on visualization in R (my data vis mother-tongue)
* I'm using python2, not python3, because that's what we use at work where I've been doing this stuff. I also think pygraphviz isn't supported for python3, though that's not strictly relevant here.

## Environment

I normally use Python 3, so I used [conda][conda] to set up a Python 2 environment. I highly recommend Continuum Analytic's Anaconda platform/bundle if you don't already have it, especially because it gets you easily set up with Jupyter notebooks. The python and figures that follow come from converting an ipython notebook .ipynb file into markdown using nbconvert as described by Brian Caffey [here](https://briancaffey.github.io/2016/03/14/ipynb-with-jekyll.html).

{% highlight shell %}
# Create an environment called 'python2env'
# with python2 and the networkx library installed
conda create --name python2env python=2 networkx
# Activate the environment
source activate python2env
{% endhighlight %}

Note that if you find yourself wanting to get more packages, you can either add them using conda, or install pip to install packages while in the environment.
{% highlight shell %}
# Install pip into the python2env environment
# It doesn't matter if you're not in the python2env environment
conda install --name python2env pip
# Install from pip
source activate python2env
pip install matplotlib
{% endhighlight %}


## Make some data

Let's make a dummy network by generating a random adjacency matrix. Networkx can turn a lot of different formats into its graph object.
A random adjacency matrix gives us a lot of nodes and edges, and the visualization demonstration that follows will focus on making it more interpretable.

This mirrors the situation I found myself in and which I expect is quite common: you start with some kind of mess of data and want to make it look presentable and understandable.

This might precede the step where you do some more quantitative analyses, or you might want to visualize the data so you can get expert opinions on it from people who might be able to tell you about the data without necessarily knowing how best to manipulate it.

{% highlight python %}
import numpy as np
import matplotlib.pyplot as plt
import networkx as nx
{% endhighlight %}



[conda]: http://conda.pydata.org/docs/
