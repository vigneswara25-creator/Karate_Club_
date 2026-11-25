Modularity on the Karate Club Graph

Course: DSC212 – Graph Theory Module (Research Assignment)
Deliverable: karate_modularity.ipynb, a Jupyter notebook that runs top-to-bottom without manual edits.

Overview

This project implements recursive spectral modularity partitioning from scratch on Zachary's Karate Club network (34 members, 78 edges). The club split into two factions, one following the instructor ("Mr. Hi", node 0) and one following the administrator (node 33). The goal is to see whether a method using only the graph structure can recover that split, and how node importance changes as the network is divided into communities.

The method follows Newman (2006). Only NumPy, NetworkX, Matplotlib and pandas are used. No community-detection routines are called. NetworkX's modularity is used only to verify the notebook's own Q values.

Method

1. Build the unweighted adjacency matrix A, the degree vector k, and the modularity matrix B = A - k kᵀ / 2m.
2. Verify that B is symmetric and that B·1 = 0.
3. For a community C, restrict B to B^(C) and compute its leading eigenpair with numpy.linalg.eigh.
4. If λ₁ ≤ 0, stop: no split of C increases modularity.
5. Otherwise split C by the sign of the leading eigenvector (> 0 goes to one group, the rest to the other) and recurse on both parts.
6. After every accepted split, record the partition, Q, and four node metrics, and draw the graph.

One iteration is one accepted split. Iteration 0 is the unsplit graph.

Notebook contents

Section
Content
1
Modularity matrix and numerical checks of its properties
2
Core routines: leading eigenpair, bisection, modularity
3
Recursive bisection, check of Q against NetworkX, comparison with the real split
3b
Comparison with Newman's generalized matrix B^(g)
4
Graph drawn after each split (fixed spring layout, one colour per community, node IDs as labels) and a plot of Q against iteration
5
Degree, betweenness, closeness and clustering after each split
6–7
Evolution plots for each metric (line plots, heatmaps, bar chart) and a mean-rank summary of the most consistently central nodes
8
Written discussion

Key results

• The first split (λ₁ = 4.98, Q = 0.372) gives groups of 16 and 18 that match the real fission for 33 of 34 members. Only node 8 is misplaced.
• Four splits give 5 communities with Q = 0.409, one of which is the singleton node 11.
• Nodes 0 and 33 stay the most central across iterations for degree, closeness and betweenness.
• Nodes 7 and 13 rank highest for clustering despite low degree and betweenness.
• The computed Q matches networkx.algorithms.community.modularity at every iteration.

Implementation notes

• Unweighted graph: recent NetworkX versions attach edge weights to the Karate graph. The notebook sets weight=None so that A ∈ {0,1}, as in the assignment.
• Node numbering: nodes are numbered 0–33 (NetworkX convention). Zachary's paper uses 1–34.
• Metrics per community: the four metrics are computed on the induced subgraph of each node's community, so they reflect importance within its group. The values change because of this normalisation. A singleton community gets 0 for every metric.
• Handout vs. Newman's matrix: the handout uses the plain block B^(C). It can have λ₁ > 0 with a single-signed leading eigenvector, in which case no split is possible and the community is treated as indivisible. Section 3b reruns the recursion with Newman's generalized B^(g), which gives 4 communities with Q = 0.393. The main results follow the handout.
• Greedy method: neither variant is guaranteed to find the global maximum of Q.

How to run

pip install -r requirements.txt
jupyter notebook karate_modularity.ipynb      # then Kernel -> Restart & Run All

Reference

M. E. J. Newman (2006). Modularity and community structure in networks. Proceedings of the National Academy of Sciences, 103(23), 8577–8582.

W. W. Zachary (1977). An information flow model for conflict and fission in small groups. Journal of Anthropological Research, 33(4), 452–473.
