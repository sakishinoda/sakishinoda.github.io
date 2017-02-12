---
layout: post
title:  "Pretty graphs in NetworkX"
tags: [data, visualisation, python]
short_description: Hacking NetworkX for not-completely-ugly graphs.
image_preview:
---

## A guide to pretty(ish) NetworkX graphs

### Coloring subsets of nodes and edges

```
def make_gm_dict():
    MMid = pk.load(open('data/MMid.txt')).values
    MGid = pk.load(open('data/MGid.txt')).values

    gm_dict = dict()

    for m in MMid:
        gm_dict[m] = 'M'

    for g in MGid:`
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
    nx.draw_networkx_nodes(G, pos, node_size = 300, nodelist = nodelist,
                           node_color = node_color, alpha = 0.9)
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
    fname = 'data/edges/edges_shallow_%s.txt'% (''.join(e for e in root_node if e.isalnum()))
    f = open(fname, 'r')
    edges = pk.load(f)
    f.close()
    return edges


def plot_network(edges, root_node):

    ## Prepare the graph
    w_edge_list = list(edges[['src', 'dest', 'wt']].itertuples(index = False))
    G = nx.MultiDiGraph()
    G.add_weighted_edges_from(w_edge_list)
    # Create positions of nodes using a spring layout, with a relatively high spring strength to keep the graph small
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



```

### Drawing weighted edges

When passing colors or line widths to `nx.draw_networkx_edges()`, you need to make sure that your edge list and attribute list are in the right order. You could use a dictionary to look up the associated attribute for each edge, or you could generate the edge and attribute in the same order.

When coding edge weights by color, you want to pass `edge_color` the list of numerical weights, and a matplotlib colormap  to `edge_cmap` to convert those numerical values to colors. To get a colormap, use `plt.cm.get_cmap()`. If you want to make changes, note that this will affect the use of the colormap whenever you call it again from `plt.cm`.  Plenty of stuff on Matplotlib color maps on StackOverflow, but I haven't yet gotten my head around it.

When coding edge weights by line width, you need to pass `width` a list of edge widths in pixels. The default is 1px, so you can use the default lines drawn by NetworkX as your reference.

#### Using a dictionary

#### Generating edge and attribute lists together

### Layouts and spacing quick fixes
* Adjust your spring strength
*

### Labels

### Overall look and feel
#### Plot background and grid lines
Standard matplotlib manipulations. I like to get rid of the axis labels, which don't mean much to me, and make the background color dark so that the white text labels are visible.
```
ax = plt.gca()
```
#### Alpha
