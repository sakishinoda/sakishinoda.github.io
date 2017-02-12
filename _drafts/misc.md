---
layout: post
---
## Input validation using list comprehensions in Python

Take as input a list to loop over. Validate that the selection exists in your dataframe using:
```
# For a straightforward output of a boolean vector
[name in df_names for name in names_list]
```
Combine with any() and all() to reduce it to a single boolean which you can use as a switch.

## Filtering a list in Python and R
Python, using a list comprehension:
```
[e if e > 0 for e in elements_list]
```
R, using the Filter [higher-order function](https://www.r-bloggers.com/filtering-a-list-with-the-filter-higher-order-function/):
```
Filter(function(x) x > 0, elements.list)
```

## Indexing a dataframe in Pandas vs Dplyr
`df$ColName` vs `df.ColName`
`df['ColName']` in both
`filter(df, ColName > 0)` vs  `df[df.ColName > 0]`


## Using bnlearn and igraph together

The default layout for a graph generated using `bnlearn::graphviz.plot` is a Sugiyama layer layout which can be replicated in igraph using `layout_with_sugiyama` or `with_sugiyama`, which however seem to function slightly differently to all the other layouts, e.g. `layout_on_grid`.
```
library(bnlearn)
library(igraph)
(as.data.frame(arcs(dag)), as.data.frame(nodes(dag)))

layout_with_sugiyama(net)$layout
```

## Converting an igraph object to a bnlearn

Simple little script.


## A really useful iPython Notebook  trick
http://blog.rtwilson.com/how-to-rescue-lost-code-from-a-jupyteripython-notebook/
