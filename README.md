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
The `spacegraphcats.catlas.catlas` script relies on the the cDBG directory, `dory_k21`. 
Although somewhat wasteful, this can be generated with `python -m spacegraphcats build dory-test`; this is wasteful because it will run the `spacegraphcats.catlas.catlas` script with `--no_checkpoint`, which needs to be rerun with the `--no_checkpoint` parameter removed.
In the long run, this could be remedied by:

+ allowing checkpoints to be configurable in the configuration file
+ running the ~7 steps orchestrated by the spacegraphcats Snakefile prior to `spacegraphcats.catlas.catlas` as separate scripts.

In this test setting, the "wasted" compute is trivial and so the `spacegraphcats.catlas.catlas` script was just rerun after `python -m spacegraphcats build command`.

After the checkpoint files are created, the python API is still necessary to reformat the checkpoint object into a format that can be read by other tools.

## graphs and visualization

@mogproject created some experimental code to visualize the CAtlas and dominating sets.
This code can be executed via binder by running the notebook `01_visualize_sgc.ipynb`.
As this code is experimental, it may have bugs.
The visualization uses the script `visualize.py`. 
For more information about `visualize.py`, please see this [blogpost](https://www.cs.utah.edu/~yos/2021/02/02/plotly-python.html).

I generalized these functions to output a gml format graph in `02_visualize_sgc.ipynb`.
