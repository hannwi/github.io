---
layout: post
title:  "Dijkstra Algorithm"
crawlertitle: "Dijkstra Algorithm"
summary: "Study Dijkstra algorithm"
date:   2020-02-11
categories: posts
tags: 'dijkstra'
author: Hannwi
---

다익스트라(Dijkstra) 알고리즘

목적: 그래프 각 정점(node)의 최단경로를 구하는 알고리즘

조건: 단일 시작점을 갖는 그래프
  방향 그래프(directed graph)의 경우: 음수 사이클이 없는 그래프
  무방향 그래프(undirected graph)의 경우: 양수 가중치(positive weight)를 갖는 그래프
    무방향 그래프의 경우, 음수 가중치가 하나라도 있으면 음수 사이클이 형성된다.

방법 개요: 우선순위 큐(priority queue)를 사용한 너비 우선 탐색(breadth first search)

고찰:
우선순위 큐를 쓰는 이유?
	더 늦게 ‘발견＇한 노드라도 더 먼저 ‘방문＇할 수 있게 하기 위함
큐에서 요소를 꺼낼 때 말고, 요소를 넣을 때 최단거리 정보를 갱신하는 이유?
	조금이라도 빨리 dist[] 를 업데이트하게 되면 search space 가 줄어든다.

참고 문제: 알고스팟 BFS (breadth-first search) [link][problem_link]

코드:
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
