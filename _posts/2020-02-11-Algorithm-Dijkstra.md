---
layout: post
title:  "다익스트라(Dijkstra) 알고리즘"
crawlertitle: "Dijkstra Algorithm"
summary: "Study Dijkstra algorithm"
date:   2020-02-11
categories: posts
tags: 'dijkstra'
author: Hannwi
---
### 최단거리 문제
알고리즘 문제를 풀다 보면, 문제를 그래프로 모델링하여 그래프의 최단거리를 구해야 할 때가 있다.

예시 문제: [알고스팟_BFS][problem_link]

보통 그래프에는 `정점(node)`과 `간선(edge)`이 존재하고, 간선에 `가중치(weight)`가 부여된다. (가중치가 없을 때는 모든 간선이 1 값을 갖는다.
`최단거리`란, 특정 시작 정점에서 종료 정점으로 이동하기 위해 지나는 간선의 가중치 합이 가장 작은 경우를 의미한다.

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/undirected_graph.png"><br>
	<em>undirected graph with weighted edges</em>
</p>

그래프의 최단거리 알고리즘은 여러 가지가 존재하지만, 그 중 가장 유명한(?) 다익스트라(Dijkstra) 알고리즘에 대해 정리해본다.

### 목적
다익스트라 알고리즘은 정해진 정점 s로부터 다른 임의의 정점 v까지의 최단거리를 모두 구해주는 알고리즘이다.

그래서 시작점이 하나로 정해져 있을때만 사용한다.

### 조건
그래프는 정해진 방향이 있는 간선으로 이루어진 **방향 그래프(directed graph)** 와, 양 방향으로 이동이 가능한 간선으로 이루어진 **무방향 그래프(undirected graph)** 로 분류된다.  
보통 무방향 그래프로 모델링이 가능한 문제들이 많고, 이런 경우엔 모든 간선의 가중치 값이 양수여야만 알고리즘을 적용할 수 있다. 만약 가중치 값에 음수가 존재하면, 다익스트라 알고리즘이 최단거리를 빠르게 찾기 위한 전제조건이 성립되지 않아 적용이 불가능하다. 게다가 만약 동일한 간선을 여러번 지날 수 있는 문제라면 음수 간선을 무한대로 왕복하면 최단거리가 마이너스 무한대가 나올 것이다. 물론 이런 경우는 문제 자체가 성립되지 않겠지만...  
방향 그래프의 경우엔, 무한히 돌면서 마이너스 무한대로 갈 수 있는 음수 사이클이 없는 그래프일 때만 적용 가능하다.
여기서는 모든 간선 가중치 값이 양수라고 가정하고 설명을 진행한다.

### 방법 개요
우선순위 큐(priority queue)를 사용한 너비 우선 탐색(breadth first search)

### 고찰
우선순위 큐를 쓰는 이유?
	더 늦게 ‘발견＇한 노드라도 더 먼저 ‘방문＇할 수 있게 하기 위함
큐에서 요소를 꺼낼 때 말고, 요소를 넣을 때 최단거리 정보를 갱신하는 이유?
	조금이라도 빨리 dist[] 를 업데이트하게 되면 search space 가 줄어든다.



### 코드
VS2015 에서 작성하였다.

{% highlight cpp %}
#include <iostream>
#include <queue>
#include <vector>
#include <functional>	// std::greater
#include <utility>		// pair

using namespace std;
typedef vector<vector<int>> array2d;
typedef pair<int, int> pos2d;

void Dijkstra(array2d& input, array2d& dist, const int h, const int w)
{
	// priority queue
	priority_queue<int, vector<pair<int, pos2d>>, greater<pair<int, pos2d>>> q; // sort the smallest element at the top

	// Initialization (start node)
	q.push(pair<int, pos2d>(0, pos2d(0, 0))); // push starting node
	dist[0][0] = 0; // update distance

	// Loop
	while (!q.empty())
	{
		pair<int, pos2d> e = q.top(); // pop element with the smallest distance
		q.pop();

		int here_x = e.second.first;
		int here_y = e.second.second;
		int here_min_dist = dist[here_y][here_x];
		int here_dist = e.first;

		// path with smaller dist. already exists
		if (here_min_dist < here_dist) // equal is missing because of the first iteration
			continue;  // skip the node

		vector<vector<int>> delta;
		delta.push_back(vector<int>({ 0, -1 })); // top
		delta.push_back(vector<int>({ -1, 0 })); // left
		delta.push_back(vector<int>({ 1, 0 })); // right
		delta.push_back(vector<int>({ 0, 1 })); // dowm

		for (auto d : delta)
		{
			int there_x = here_x + d[0];
			int there_y = here_y + d[1];

			// out of index
			if (there_y < 0 || there_y >= h || there_x < 0 || there_x >= w)
				continue;

			int there_min_dist = dist[there_y][there_x];
			int there_dist = here_min_dist + input[there_y][there_x];

			// if path with smaller dist is found
			if (there_dist < there_min_dist)
			{
				cout << "(" << there_x << ", " << there_y << ")" << endl;
				// push neighbor node, update distance
				q.push(pair<int, pos2d>(there_dist, pos2d(there_x, there_y)));
				dist[there_y][there_x] = there_dist;
			}
		}
	}
}

void main()
{
	// Input: 2D matrix
	const int height = 4;
	const int width = 3;
	array2d input;
	input.push_back(vector<int>({ 0, 1, 1 }));
	input.push_back(vector<int>({ 1, 1, 1 }));
	input.push_back(vector<int>({ 1, 1, 0 }));
	input.push_back(vector<int>({ 1, 1, 0 }));

	// Output: array for minimum distance
	array2d dist;
	dist.push_back(vector<int>({ INT_MAX, INT_MAX, INT_MAX }));
	dist.push_back(vector<int>({ INT_MAX, INT_MAX, INT_MAX }));
	dist.push_back(vector<int>({ INT_MAX, INT_MAX, INT_MAX }));
	dist.push_back(vector<int>({ INT_MAX, INT_MAX, INT_MAX }));

	// Run algorithm
	Dijkstra(input, dist, height, width);

	// Print output
	for (int y = 0; y < height; y++)
	{
		for (int x = 0; x < width; x++)
			cout << dist[y][x] << ", ";
		cout << endl;
	}

	// Pause
	char a;
	cin >> a;
}
{% endhighlight %}

[problem_link]: https://algospot.com/judge/problem/read/BOJ
