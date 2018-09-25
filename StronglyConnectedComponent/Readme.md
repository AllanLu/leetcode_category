# Strongly Connected Component (SCC)


## To check if every node has both paths to and from every other node in a given graph:

DFS/BFS from all nodes:
---
[Tarjan's algorithm](https://en.wikipedia.org/wiki/Tarjan%27s_strongly_connected_components_algorithm) supposes every node has a depth `d[i]`. Initially, the root has the smallest depth. And we do the post-order DFS updates `d[i] = min(d[j])` for any neighbor `j` of `i`. Actually BFS also works fine with the reduction rule `d[i] = min(d[j])` here.

    function dfs(i)
        d[i] = i
        mark i as visited
        for each neighbor j of i: 
            if j is not visited then dfs(j)
            d[i] = min(d[i], d[j])

If there is a forwarding path from `u` to `v`, then `d[u] <= d[v]`. In the SCC, `d[v] <= d[u] <= d[v]`, thus, all the nodes in SCC will have the same depth. To tell if a graph is a SCC, we check whether all nodes have the same `d[i]`.

Two DFS/BFS from the single node:
---
It is a simplified version of the [Kosaraju’s algorithm](https://www.geeksforgeeks.org/strongly-connected-components/). Starting from the root, we check if every node can be reached by DFS/BFS. Then, reverse the direction of every edge. We check if every node can be reached from the same root again. See [C++ code](http://codeforces.com/contest/475/submission/8140615).

## Find SCC

The most useful and fast-coding algorithm for finding SCCs is Kosaraju.

1. DFS on the graph and sort the vertices in decreasing of their finishing time (we can use a stack).
2. Reverse the graph
2. Start from the vertex with the greatest finishing time, and for each vertex v that is not yet in any SCC, do : 
3. for each u that v is reachable by u and u is not yet in any SCC, put it in the SCC of vertex v.

If there is a forwarding path from `root` to `v`, then `root` ends up sitting above `v` in such stack. 
If `root` ends up sitting above `v` in such stack, then either there is a forwarding path from `root` to `v`, or they are disconnected.
In the transpose graph, if `root` still has a forwarding path to `v`, we eliminate the second case, thus, `root->v` and `v->root`. So we put `v` in the SCC of `root`.