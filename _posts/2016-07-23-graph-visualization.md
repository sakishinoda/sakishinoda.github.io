---
layout: post
title:  "Graph visualization"
date:   2017-02-13 01:14:00 +0000
tags: [data, programming, visualization]
image_preview: http://habrastorage.org/storage2/249/57e/c1a/24957ec1abe44ea13146dd7f3cb43fd2.png
short_description: "Miscellany on visualizing graphs in Python using NetworkX and in R using VisNetwork."
---
There was going to be a series of posts about how to make pretty(-ish) graphs using NetworkX in Python and VisNetwork in R. I started working on a few back in July. However, I don't really have time to condense all the various notes I made while working at NIBR. So instead, a few bits of miscellany follow and may be added to in the future.

***

## R -- VisNetwork

VisNetwork is fantastic. You can hook it up to Shiny to make pretty, interactive web apps that allow people to explore data in visual graph format. Everything you need to know is in [the best tutorial for using VisNetwork in R](http://kateto.net/network-visualization).

***

## Python -- NetworkX
So there seem to be a few possibilities: pygraphviz, pydot among them. I went for NetworkX because it had the most documentation.

### Colouring node/edge subsets
Code for colouring subsets of edges and nodes in NetworkX in Python (2.7). In this case I use M and G to denote subsets of nodes (since I was combining metabolomic and genomic datasets), and have three different groups of edges.

{% highlight python %}
import numpy as np
import matplotlib.pyplot as plt
import networkx as nx
import pickle as pk

def make_gm_dict():
    MMid = pk.load(open('data/MMid.txt')).values
    MGid = pk.load(open('data/MGid.txt')).values
    gm_dict = dict()
    for m in MMid:
        gm_dict[m] = 'M'
    for g in MGid:
        gm_dict[g] = 'G'
    return gm_dict


def identify_gm_nodes(gm_dict, g):
    node_names = g.nodes()
    g_nodes = []
    m_nodes = []
    for n in node_names:
        n = str(n)
        if gm_dict[n] != "M":
            g_nodes.append(n)
        else:
            m_nodes.append(n)
    return g_nodes, m_nodes


def draw_nodes(G, pos, nodelist, node_color):
    nx.draw_networkx_nodes(G, pos, node_size = 300,
        nodelist = nodelist, node_color = node_color, alpha = 0.9)
    return None

def draw_edge_layer(G, pos, edge_alpha, edge_color, edge_list, weight_list, width_scaler = 1):
    width_list = [width_scaler * w for w in weight_list]
    nx.draw_networkx_edges(G, pos, edgelist = edge_list,
                           edge_color = edge_color,
                           alpha = edge_alpha,
                           arrows = False,
                           width = width_list)   
    return None

def get_edges_from_file(root_node):
    fname = 'data/edges/edges_shallow_%s.txt'% (''.join(
        e for e in root_node if e.isalnum()))
    f = open(fname, 'r')
    edges = pk.load(f)
    f.close()
    return edges


def plot_network(edges, root_node):
    ## Prepare the graph
    w_edge_list = list(edges[['src', 'dest', 'wt']].itertuples(index = False))
    G = nx.MultiDiGraph()
    G.add_weighted_edges_from(w_edge_list)
    # Create positions of nodes using a spring layout
    # Use a high spring strength to keep the graph small
    pos = nx.layout.fruchterman_reingold_layout(G, k = (2/np.sqrt(len(G.nodes()))))
    g_nodes, m_nodes = identify_gm_nodes(gm_dict, G)    

    ## Draw nodes
    draw_nodes(G, pos, m_nodes, 'r')
    draw_nodes(G, pos, g_nodes, 'b')
    highlight_nodes = []
    highlight_nodes.append(root_node)
    draw_nodes(G, pos, highlight_nodes, 'm')
    #draw_edge_layer(G, pos, 0.3, 'k', *layer_edge_list(edges, 3))
    draw_edge_layer(G, pos, 0.5, 'y', *layer_edge_list(edges, 2))
    draw_edge_layer(G, pos, 0.7, 'c', *layer_edge_list(edges, 1))
    draw_edge_layer(G, pos, 0.9, 'm', *layer_edge_list(edges, 0))

    nx.draw_networkx_labels(G, pos, font_size = 10, font_weight = 'bold', font_color = '0.95')
    ax = plt.gca()
    ax.set_axis_bgcolor('0.5') # Grey background (0 is darker)
    ax.grid(b = False) # No gridlines
    ax.get_xaxis().set_visible(False) # hide x-axis (to avoid confusing labels)
    ax.get_yaxis().set_visible(False) # hide y-axis

    return None

{% endhighlight %}
