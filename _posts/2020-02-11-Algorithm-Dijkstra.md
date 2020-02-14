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
알고리즘 문제를 풀다 보면, 문제를 `그래프`로 모델링하여 그래프의 최단거리를 구해야 할 때가 있다.

예시 문제: [알고스팟_BFS][problem_link]

그래프는 `정점(node)` 과 `간선(edge)`으로 이루어진다. 그리고 각 간선은 `가중치(weight)`를 가지고 있다. (가중치가 없을 때는 모든 간선이 1 의 가중치 값을 갖는다고 가정한다.)  
또한, 간선 종류에 따라 정해진 방향이 있는 간선으로 이루어진 `방향 그래프(directed graph)`와, 양 방향으로 이동이 가능한 간선으로 이루어진 `무방향 그래프(undirected graph)`로 분류된다.

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/undirected_graph.png"><br>
	<em>undirected graph with weighted edges</em>
</p>

그래프의 특정 정점으로부터 특정 정점으로 이동하기 위해 지나가는 간선들의 집합을 `경로`라고 하자. 이 때, 경로의 가중치 합이 그 경로의 `거리`가 된다.  
그래프의 **최단거리**란, 특정 시작 정점 s에서 출발하여 종료 정점으로 도착하기까지 지나는 모든 경로 중에서 가장 거리가 작은 경로의 거리 값을 의미한다.  

<p align="center">
	<img width="400" src="/assets/images/algorithm/dijkstra/undirected_graph2_min_distance.png"><br>
	<em>minimum distances (red figures) from <strong>s</strong> to each node.</em>
</p>

그래프의 최단거리를 구하는 알고리즘은 여러 가지가 존재하지만, 이 포스트에서는 그 중 가장 유명한(?) 다익스트라(Dijkstra) 알고리즘에 대해 공부한 내용을 정리해본다.

### 목적
다익스트라 알고리즘은 정해진 시작 정점 s로부터 다른 모든 정점까지의 최단거리를 구해주는 알고리즘이다.  
그래서 시작점이 정해져 있는 문제에서 사용되며, 시작점이 정해져있지 않을 때는 다익스트라를 한 번 만 실행해서는 답을 구할 수 없다.

### 조건
어떤 그래프에서 다익스트라를 사용할 수 있을까?  
`무방향 그래프`의 경우, 모든 간선의 **가중치 값이 양수**여야만 알고리즘을 적용할 수 있다. 만약 가중치 값에 음수가 존재하면, 다익스트라 알고리즘이 최단거리를 빠르게 찾기 위한 전제조건이 성립되지 않아 적용이 불가능하다.  
게다가, 만약 동일한 간선을 여러번 지날 수 없다는 조건이 따로 있지 않다면, 음수 가중치를 갖는 간선을 무한대로 왕복하면 최단거리가 마이너스 무한대가 나올 것이다. 문제 자체가 성립되지 않는다.  
`방향 그래프`의 경우엔, `음수 사이클`이 없다면 적용 가능하다. 음수 사이클이란, 무한히 같은 경로를 돌면서 거리가 마이너스 무한대로 발산할 수 있는 경로를 의미한다.
설명의 편의를 위해, 여기서는 모든 간선 가중치 값이 양수인 무방향 그래프를 가정하고 설명을 진행한다.

### 방법 개요
우선순위 큐(priority queue)를 사용한 너비 우선 탐색(breadth first search)

방법
초기화
모든 node 의 dist[] 값을 infinite 으로 설정
시작 node 의 dist[] 값을 0으로 설정
(0, s) 를 queue 에 넣는다.
loop (while queue is not empty)
queue 에서 요소를 꺼낸다.
꺼낸 요소의 dist 값이 dist[] 값보다 크다면 loop 를 skip 한다.
인접 node 들을 조사한다. (BFS)
인접 node 로 이동할 때의 dist 총 합이 dist[node] 보다 작아진다면 dist[] 값을 갱신한다.
갱신된 node 와 dist 쌍만 queue에 넣는다.

우선순위 큐를 사용하지 않는 Dijkstra
핵심 논리는 똑같음
우선순위 큐 대신 매번 for 문을 돌면서 아직 방문하지 않은 node 중 최소 거리의 node 를 방문
visited[] 사용하여 한 번 방문한 node 는 다시 방문하지 않음
아직 방문하지 않은 인접 node 들의 dist[] 를 갱신
node 개수가 적거나 edge 개수가 많은 경우 유리

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
