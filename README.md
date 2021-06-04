[![Binder](https://binder.pangeo.io/badge_logo.svg)](https://binder.pangeo.io/v2/gh/taylorreiter/2021-sgc-domset-viz.git/main)

# Experimenting with dominating set visualization 

[spacegraphcats](http://spacegraphcats.github.io/spacegraphcats/) facilitates (meta)genome assembly graph queries through assembly graph compression.
Compact de bruijn assembly graphs (cDBG) are compressed using a dominating set algorithm, where each node in the cDBG is at more distance *r* from a dominating node.
This carves the cDBG into *pieces*, each of which contains one dominator node. 

This repository plays with spacegraphcats dominating sets and common graph viz libraries to (hopefully) produce intuitive visualizations of dominating sets.

I anticipate wanting to visualize metadata (annotations, differential abundance of nodes, etc).

A conda environment file is available in `binder/environment.yml` that provides installation instructions for the software used in this repo.
 
## create some toy graphs to play with using the spacegraphcats test data

The method `domination_graph`, defined in `spacegraphcats.catlas.rdomset` and used by `spacegraphcats.catlas.catlas` returns a dictionary whose keys are the dominating vertices and whose values are the respective sets assigned to them. 
While we could access this method in the python API, the command line script `python -Werror -m spacegraphcats.catlas.catlas dory_k21 dory_k21_r1 1` generates checkpoints (when the flag `--no_checkpoint` is not invoked. 
These checkpoints contain the output from the `domination_graph` method, thereby providing a file with the dominating set data.

The python API is still necessary to reformat the checkpoint object into a format that can be read by other tools.

```
import spacegraphcats.catlas.catlas
import pandas as pd

in_dir = "dory_k21"         # cDBG directory
out_dir = "dory_k21_r1"     # catlas directory
r = 1                       # radius size
level = 1                   # catlas level to extract; I think only level 1 and top level contain all nodes

proj = spacegraphcats.catlas.catlas.Project(in_dir, out_dir, r)
proj.load_checkpoint(level)

proj.graph.inarcs_by_weight # list of dictionaries where dominators are keys and nodes that belong in each domset are values

d = proj.graph.inarcs_by_weight[0]

domset_df = pd.DataFrame([{"dominator":k, "set_node": v} for k,v in d.items()])
domset_df = domset_df.explode('set_node')
domset_df.to_csv("dory_k21_r1_domset_df.csv")
```

