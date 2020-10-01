### Explaination
https://cp-algorithms.com/graph/dinic.html

https://www.geeksforgeeks.org/dinics-algorithm-maximum-flow/
### Code
```c++
struct Dinic 
{
	struct Edge
	{
		int from, to, cap, flow, idx;
		Edge(int from, int to, int cap, int flow, int idx) :
				from(from), to(to), cap(cap), flow(flow), idx(idx) {}
	};
	
	int N;
	vector<vector<Edge> > G;
	vector<Edge *> dad;
	vector<int> Q;
	Dinic(int N) :	N(N), G(N), dad(N), Q(N) {}

	void AddEdge(int from, int to, int cap)
	{
		G[from].push_back(Edge(from, to, cap, 0, G[to].size()));
		if (from == to)
			G[from].back().idx++;
		G[to].push_back(Edge(to, from, 0, 0, G[from].size() - 1));
	}

	long long BlockingFlow(int src, int snk) 
	{
		fill(dad.begin(), dad.end(), (Edge *) NULL);
		dad[src] = &G[0][0] - 1;
		int head = 0, tail = 0;
		Q[tail++] = src;

		while (head < tail) 
		{
			int u = Q[head++];
			for (auto &e : G[u]) 
				if (!dad[e.to] && e.cap - e.flow > 0)
				{
					dad[e.to] = &e;
					Q[tail++] = e.to;
				}
		}

		if (!dad[snk])	return 0;

		long long totflow = 0;
		for (int i = 0; i < G[snk].size(); i++)
		{
			Edge *start = &G[G[snk][i].to][G[snk][i].idx];
			int path_flow = INT_MAX;

			for (Edge *e = start; path_flow && e != dad[src]; e = dad[e->from]) {
				if (!e)
				{
					path_flow = 0;
					break;
				}
				path_flow = min(path_flow, e->cap - e->flow);
			}

			if (path_flow == 0)
				continue;

			for (Edge *e = start; path_flow && e != dad[src]; e = dad[e->from]) 
			{
				e->flow += path_flow;
				G[e->to][e->idx].flow -= path_flow;
			}

			totflow += path_flow;
		}

		return totflow;
	}

	long long GetMaxFlow(int src, int snk) 
	{
		long long totflow = 0;
		while (long long flow = BlockingFlow(src, snk))
			totflow += flow;
		return totflow;
	}
};
```