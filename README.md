[![Binder](https://binder.pangeo.io/badge_logo.svg)](https://binder.pangeo.io/v2/gh/taylorreiter/2021-sgc-domset-viz.git/main)

# Experimenting with dominating set visualization 

[spacegraphcats](http://spacegraphcats.github.io/spacegraphcats/) facilitates (meta)genome assembly graph queries through assembly graph compression.
Compact de bruijn assembly graphs (cDBG) are compressed using a dominating set algorithm, where each node in the cDBG is at more distance *r* from a dominating node.
This carves the cDBG into *pieces*, each of which contains one dominator node. 

This repository plays with spacegraphcats dominating sets and common graph viz libraries to (hopefully) produce intuitive visualizations of dominating sets.

I anticipate wanting to visualize metadata (annotations, differential abundance of nodes, etc).

A conda environment file is available in `binder/environment.yml` that provides installation instructions for the software used in this repo.
 
## create some toy graphs to play with using the spacegraphcats test data

