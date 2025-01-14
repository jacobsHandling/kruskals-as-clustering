# Clustering with Kruskal's algorithm and the Union Find data structure
Kruskal's algorithm and the union find data structure applied to the clustering problem as presented in week 2 of Tim Roughgarden's Stanford Online course entitled, ["Greedy Algorithms, Minimum Spanning Trees, and Dynamic Programming"](https://www.coursera.org/learn/algorithms-greedy/exam/bBxNY/programming-assignment-2). See **The Problem** below for a paraphrasing of that presentation.

## The Problem:

Apply [Kruskal's algorithm](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm) in the clustering of a graph so big that the distances (i.e., edge costs) are only defined implicitly, rather than being provided as an explicit list.

The data set is found in `clustering_big.txt`, which has the format:

[# of nodes] [# of bits for each node's label]

[first bit of node 1] ... [last bit of node 1]

[first bit of node 2] ... [last bit of node 2]

...

The distance between two nodes *u* and *v* is defined as the [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between the two nodes' labels.

**What is the largest value of *k* such that there is a *k*-clustering with spacing at least 3?  That is, how many clusters are needed to ensure that no pair of nodes with all but 2 bits in common get split into different clusters?**

## This solution:
1. A minimum spacing of 3 dictates that all edges that will be considered by Kruskal's algorithm for being incorporated into connected components (which represent clusters) will be either of cost 1 or 2. Specifically, EVERY edge of cost 1 and EVERY edge of cost 2 will be considered.

1. The possible neighbors to consider for any one node, then, are those that contain either 1 bit or 2 bits complemented from the node. This leads to 300 possible neighboring nodes that would be 1 or 2 away per node: 24 possible with 1 changed bit + 276 (n choose 2) possibilities with 2 changed bits.

1. Calculate all 300 possible neighbors per each of the ~200,000 nodes (actually less - store them in a set to see how many discreet nodes are in the data set) giving < 60,000,000 possible neighboring nodes, a manageable number.

1. For each node, determine which of its 300 possible neighboring nodes are actually present in the data, and for each, add the appropriate edge with the corresponding cost (1 or 2) to your edges to consider with Kruskal's.

1. Finally, run Kruskal's algorithm on the entire graph, considering all edges of cost 1 followed by all of cost 2, halting once these edges have been exhausted. Since only edges up to the desired minimum spacing (3 in this case, so edges of cost 1 or 2) were considered, and precisely all of them were considered, the number of remaining connected components / clusters is the maximum *k*. Any higher *k* would require at least 2 clusters to have a spacing > 2.


## On optimality:
This implementation could be significantly sped up by using `ints`, `bitmap`s, and the bitwise `xor` operator instead
of `string`s, `itertools.combination`s, and index replacement.

## To run: ##
1. Make sure you have python3 installed in your current environment
2. cd into the `kruskals_as_clustering` directory
3. run `python3 big_cluster.py`
