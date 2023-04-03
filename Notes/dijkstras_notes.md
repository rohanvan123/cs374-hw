Psuedocode implementation of Dijkstra's Algorithm:

```
Dijkstra(G, s, t)
    for each vertex in V:
        dist[v] = infinity
        prev[v] = Null
        if v != s:
            pq.add(v)

    dist[s] = 0

    while pq is not empty:
        u <- extract_min(pq)
        for each edge u -> v:
            if v is unmarked:
                temp_dist <- dist[u] + w(u -> v)
                if temp_dist < dist[v]:
                    // relax edges
                    dist[v] = temp_dist
                    prev[v] = u



```
