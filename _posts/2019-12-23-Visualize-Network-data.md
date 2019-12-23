---
published: true
layout: post
title: Visualize Network Data
---

We have observed many different types of network based data points plotted. Many of data points like in case of Map have fixed coordinates. It may be easy to visualise how to place each data point based on geolocation like latitude and longtitude.
But what if you wanted to visualize the twitter following chart, StackOverflow questioner and answerer relationship or words in increasing frequency that make up a Charles Dickens novel.
It can be a daunting task to plot the graph specially if nodes are more than 10. The connections between nodes get mixed up and is not easy to make a informed decision on it.

Python provides a great library for it: **Networkx**

For the purpose of our tutorial, let's take an example of graphical nodes collected from the stack exchange web site Math Overflow. 
The dataset can be accessed from [SNAP by Stanford](http://snap.stanford.edu/data/sx-mathoverflow.html). 
The dataset has following format
```
SRC DST UNIXTS

where edges are separated by a new line and

SRC: id of the source node (a user)
TGT: id of the target node (a user)
UNIXTS: Unix timestamp (seconds since the epoch)
```

### Import dependencies

```python
import pandas as pd
from pandas import read_csv
import numpy as np
import matplotlib.pyplot as plt
```
### Import Data
We are only sampling 10000 data points initially as the the graph becomes increasing complex to visualise otherwise

```python
df = read_csv('sx-mathoverflow-a2q.txt',names=['SRC', 'DST', 'UNIXTS'], delim_whitespace=True, header=None)
df = df.sample(n=10000, random_state=1)
```

### Consider only top answerers
We want to visualise the top answer contributors for this exercise

```python
# Get the count of each value
value_counts = df['SRC'].value_counts()

# Select the values where the count is less than 60
to_remove = value_counts[value_counts < 60].index
df = df[~df.SRC.isin(to_remove)]

```

### networkx code

```python
import matplotlib.pyplot as plt
import matplotlib.patches as mpatches
import networkx as nx
from matplotlib.patches import Patch
from matplotlib.lines import Line2D


plt.figure(figsize=(12, 12))

# 1. Create the graph
# Build your graph
g = nx.from_pandas_edgelist(df,source='SRC',target='DST', edge_attr=None)

# 2. Create a layout for our nodes 
layout = nx.spring_layout(g,iterations=50)

# 3. Draw the parts we want
nx.draw_networkx_edges(g, layout, edge_color='#AAAAAA')

answerers = [node for node in g.nodes() if node in df.SRC.unique()]
answerers_size = [value_counts_src[node] for node in g.nodes() if node in df.SRC.unique()]
# for answerers we want the size of nodes to be representative of number of answers they have written
nx.draw_networkx_nodes(g, layout, nodelist=answerers, node_size=[c*40 for c in answerers_size], node_color='lightblue')

questioners = [node for node in g.nodes() if node in df.DST.unique()]
nx.draw_networkx_nodes(g, layout, nodelist=questioners, node_size=100, node_color='#AAAAAA',alpha=0.4)

answerers_dict = dict(zip(answerers, answerers_size))
nx.draw_networkx_labels(g, layout, labels=answerers_dict,font_color='white',font_size=19)

high_degree_questioners = [node for node in g.nodes() if node in df.DST.unique() and value_counts_dst[node] >3]
nx.draw_networkx_nodes(g, layout, nodelist=high_degree_questioners, node_size=100, node_color='#fc8d62', alpha=0.6)

# we do not need axes as there is no need for them
plt.axis('off')

plt.title("Math Overflow Answers vs Questions")

# custom legends
legend_elements = [Line2D([0], [0], marker='o', color='lightblue', label='User with more than 60 answers',
                          markerfacecolor='lightblue', markersize=15),
                   Line2D([0], [0], marker='o', color='#AAAAAA', label='User with questions',
                          markerfacecolor='#AAAAAA', markersize=15),
                   Line2D([0], [0], marker='o', color='#fc8d62', label='User with more than 3 questions',
                          markerfacecolor='#fc8d62', markersize=15)
                  ]

# Create the figure and show
plt.legend(handles=legend_elements,loc='upper right')
plt.savefig("Graph.png", format="PNG")
plt.show()

```

### Visualization
The image like this will be shown below:
![Math Overflow Answers vs Questions](https://raw.githubusercontent.com/pacificlion/pacificlion.github.io/master/images/Graph.png "Math Overflow Answers vs Questions")


### Inferences
For users who have answered (blue circles), the size of their circles increases with the number of questions they have answered, the number of answers is also mentioned over those circles. 

The visualization is a good representation of the contribution of users who have answered more than 60 questions. Moreover, it can be inferred that some of the top contributors to answers have also asked more than 3 questions

I think this example can be used for generating network data for good visualization. More datasets can be found from 
1. [Stanford Large Network Dataset Collection](http://snap.stanford.edu/data/index.html)
2. [UCI Network Data Repository](https://networkdata.ics.uci.edu/)
















### References

1:
> @misc{snapnets,
  author       = {Jure Leskovec and Andrej Krevl},
  title        = {{SNAP Datasets}: {Stanford} Large Network Dataset Collection},
  howpublished = {\url{http://snap.stanford.edu/data}},
  month        = jun,
  year         = 2014
}

2: 
>
Ashwin Paranjape, Austin R. Benson, and Jure Leskovec. "Motifs in Temporal Networks." In Proceedings of the Tenth ACM International Conference on Web Search and Data Mining, 2017.

3: 
>
http://jonathansoma.com/lede/algorithms-2017/classes/networks/networkx-graphs-from-source-target-dataframe/

