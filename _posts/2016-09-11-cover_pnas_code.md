---
layout: post
title: Cover of PNAS - code
---

<img src="/images/2016/code_pnas_cover_header.png" class="fit image">
I received questions from a couple of people asking me how I drew the network featured on the cover of PNAS (read about it [here](/2016/09/06/pnas_cover)).
Well, this blogpost is for you, and anybody else.

Before we get started we first need some data, go ahead and grab a small sample of the [Copenhagen Network Study](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0095978) data from [here](/data/edges_pnas_cover.csv) &mdash; of course heavily anonymized.
The csv-file is just a list of edges which denote that ```node i``` is connected to ```node j```.

Before we start plotting stuff we first need to import some libraries. 
Note that I am using python 2.7 and, and at the time of writing, [networkX](https://networkx.github.io/) version 1.11.
If you have problems with importing the graphviz_layout package, please upgrade your pydot module to pydot2.

``` python
import matplotlib.pyplot as plt
import networkx as nx
import numpy as np
from networkx.drawing.nx_agraph import graphviz_layout
```

Then lets load some [data](/data/edges_pnas_cover.csv). 

``` python
edges = []
with open('edges_pnas_cover.csv') as f:
	next(f) # skip first line - header
	for line in f:
		edges.append(tuple(line.strip().split(',')))
```

Proceed by constructing an empty graph, populate it with the edge-data, and use NetworkX to position the edges nicely.
For positioning nodes you can choose one of the many built-in algorithms in NetworkX, my favorite is graphviz, especially using the _fdp_ or _neato_-layouts.


``` python
G = nx.Graph() # create empty graph
G.add_edges_from(edges) # populate it with edges

# layout graphs using graphviz
pos = graphviz_layout(G,prog="fdp")
```

Now we can already draw the network using ```nx.draw_networkx(...)```, but because we want the network to look nice we are not going to use networkX to draw the network (_nothing personal against networkX_).
Instead we are going to use [matplotlib](http://matplotlib.org/).

``` python
plt.figure()

# draw edges
for ei,ej in G.edges():
	plt.plot([pos[ei][0],pos[ej][0]],[pos[ei][1],pos[ej][1]],'-',color='crimson',alpha=0.3,lw=0.8)

# draw nodes
# for n in G.nodes():
#	plt.plot(pos[n][0],pos[n][1],'o',color='crimson',markersize=2.5,markeredgecolor='#cccccc',markeredgewidth=0.1,clip_on=False)

plt.axis('off')
plt.savefig('pnas_cover_network.png',dpi=200,bbox_inches='tight')
plt.close()
```

This code produces something like the figure below. 
Note that I have not drawn the actual nodes, only links are shown.
If, however, you are interested in the nodes as well you can draw them by commenting in the lines regarding nodes.

<img src="/images/2016/pnas_cover_network.png"  width="300px" class="fit image">

This look OK, but all edges have the same color and the figure is strangely elongated &mdash; we can do better then that.
Lets color edges differently depending on their distance from a certain point, say from the bottom left.
First we need to estimate the extend of the network, and calculate the minimal and maximal distance form all nodes to the reference point.

``` python
# find extent of network
x,y = zip(*pos.values())
min_x = min(x); max_x = max(x)
min_y = min(y); max_y = max(y)

# calculate distance from nodes to bottom left (min_x,min_y)
dist = dict([(n,np.sqrt((min_x-pos[n][0])**2 + (min_y-pos[n][1])**2 )) for n in G.nodes()])
min_d = int(min(dist.values())); max_d = int(max(dist.values()))
```

Notice we store the distance from each node to the reference point a dictionary named ```dist```.
[_If you have a huge network with millions or even billions of nodes this might not be the best approach_].
Now we are ready to assign a unique colors to each edge.
Go ahead a select a colormap from [matplotlib colors](http://matplotlib.org/examples/color/colormaps_reference.html), or create your own custom colormap see [here](http://matplotlib.org/examples/pylab_examples/custom_cmap.html) and [here](http://stackoverflow.com/questions/16834861/create-own-colormap-using-matplotlib-and-plot-color-scale).
I personally prefer _viridis_, but in this case we will use _coolwarm_.

For each distance unit we move away from the reference point we want the color of the edges/nodes to be slightly different. 
To achieve this we are going to create a dictionary named ```colors`` with ```max_d - min_d + 1``` number of colors, basically this is the total number of distance units which we are going to distribute colors across.
To separate colors evenly we use numpy's [_linspace_-function](http://docs.scipy.org/doc/numpy/reference/generated/numpy.linspace.html) and to be able to reference colors we wrap it all into one big dictionary we use the zip function. 

``` python
colors = dict(
		zip(
			range(min_d,max_d+1),
			plt.cm.coolwarm(np.linspace(0,1,max_d-min_d+1))
		)
	)
```

Now we can draw the network again using the following code:

``` python
plt.figure(figsize=(8.3,8.9)) # close to required size by PNAS (in inches)

# draw edges
for ei,ej in G.edges():
	dist_e = int(np.sqrt( (min_x - (pos[ei][0]+pos[ej][0])/2)**2 + (min_y - (pos[ei][1]+pos[ej][1])/2)**2))
	plt.plot([pos[ei][0],pos[ej][0]],[pos[ei][1],pos[ej][1]],'-',color=colors[dist_e],alpha=0.3,lw=0.5,clip_on=False)

# draw nodes
#for n in G.nodes():
#	plt.plot(pos[n][0],pos[n][1],'o',color=colors[int(dist[n])],markersize=2.5,markeredgecolor='#cccccc',markeredgewidth=0.1,clip_on=False)

# set axis
plt.xlim(min_x-1,max_x+1)
plt.ylim(max_y+1,min_y-1,)

plt.axis('off')
plt.savefig('pnas_cover_network_final.png',dpi=200,facecolor='black',bbox_inches='tight')
plt.close()
```

### End result
Using a black background, our colormap, and the correct (_PNAS_) figure size.
Congratulations! You can now use this code to draw your own versions of the network featured on the cover of PNAS.
Maybe even do a startup where you print cool networks on t-shirts &mdash; I would definitely buy one.
<img src="/images/2016/pnas_cover_network_final.png"  width="300px" class="fit image">


