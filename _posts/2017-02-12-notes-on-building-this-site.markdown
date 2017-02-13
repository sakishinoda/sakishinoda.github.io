---
layout: post
title:  "Notes on building this site"
date:   2017-02-12 22:47:00 +0100
tags: [web, programming]
short_description: "Some things I learned while building this github.io pages site using jekyll-theme-hackcss"
image_preview: https://www.sketchappsources.com/resources/source-image/jekyll-logo-sketch.png
---
I thought [jekyll](https://jekyllrb.com) and Github Pages would be an easy way to get a website up and running with minimal CSS and HTML hackery. Like many a millennial I learned CSS and HTML at a very early age, but I don't much find it particularly fulfilling to wrangle with these days. What has been a little more interesting is learning about Ruby (I guess).

## Custom typography
After some fiddling, my fix is to put in my `_sass/_styles.scss` (which came with the theme), some modifications on classes identified using Firefox 'Inspect Element' to specify my preferred fonts:
{% highlight css %}
@import 'https://fonts.googleapis.com/css?family=Fira+Mono';
body, .standard{font-family:Fira Mono,monospace}
code{font-family:Fira Mono,monospace; font-size: 85%;}
{% endhighlight %}


## Syntax highlighting
* Github only (apparently) supports syntax highlighting with rouge and not pygments

* [This post](http://anotherpeak.org/blog/tech/2016/09/22/syntax_highlighter_jekyll_rouge.html) shows how to use rougify to get syntax highlighting stylesheets. My usage:

{% highlight bash %}
# to list the styles
rougify help style
# style currently used
rougify style github > _sass/_syntax.scss
{% endhighlight %}


* Then add `syntax` to the list of partial files in `main.scss` which is looked for in the `_sass/` directory by default. An alternative is to just save `syntax.css` and import that in `head.html` (like the prism stylesheet)

* Remove the link to the Prism stylesheet in `head.html` since that is apparently a syntax highlighting library for CSS/JS/HTML, etc. and not so relevant for me – plus conflicts with rouge and other settings

* Make changes to the appearance of the code block bounding box by modifying the class `pre`, which I've done in `_sass/_highlight.scss`

## [Mathjax](http://gastonsanchez.com/visually-enforced/opinion/2014/02/16/Mathjax-with-jekyll/)
