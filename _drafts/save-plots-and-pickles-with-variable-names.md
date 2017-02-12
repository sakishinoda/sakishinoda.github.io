---
layout: post
---
## Saving plots and pickles to files based on variable values
This is a quick tip on how to avoid `File not found` errors.

Example scenario 1: You are trying to generate a list of edges that start at some `root_node` and making a network plot. You want to pickle or maybe save the list of edges as a DOT language text file, and also save the PDF of the network plot.

Example scenario 2: You have a big data frame of sales associated with many different products. You want to save the analysis and plots associated with only a few products.

In these cases, you may want to automate the process of analysis and saving by looping over a list of the names you are selecting.

### Without Regex
This solution is deterministic and reproducible, and for a given variable can be reliably used for both reading in and writing out files.

Try, in Python 2.7 string formatting:
```
fname = 'data/%s.txt' % (''.join(e for e in root_node if e.isalnum()))
```

In Python 3 formatting:
```
fname = 'data{}.txt'.format(''.join(e for e in root_node if e.isalnum()))
```

This solution courtesy of this [StackOverflow answer](http://stackoverflow.com/questions/5843518/remove-all-special-characters-punctuation-and-spaces-from-string).

Note that for a quick guide on Python new and old string formatting, the [PyFormat guide](https://pyformat.info/) is the clearest that I have found.

### Alternative: multi-page PDF
Have you considered putting all of your plots into one plot? This is straightforward in R:
```
pdf(file_to_save_to.pdf)
# make plots that will be saved to the file
dev.off()

```
In Python, it's a little more complicated but pretty straightforward one you've read [this demo](http://matplotlib.org/examples/pylab_examples/multipage_pdf.html).
