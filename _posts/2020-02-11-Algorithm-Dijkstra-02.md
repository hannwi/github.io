---
layout: post
title:  "Dijkstra Algorithm 02 (Procdure)"
crawlertitle: "Dijkstra Algorithm"
summary: "Study Dijkstra algorithm"
date:   2020-02-11
categories: posts
tags: 'dijkstra'
author: Hannwi
---

이번 포스트에서는 다익스트라 알고리즘의 자세한 진행 과정에 대해서 정리한다.

우선순위 큐(priority queue)를 사용한 너비 우선 탐색(breadth first search)이 대표적이며, 

#### 알고리즘 예제
우선순위 큐 
(Node 번호, node 까지 최단거리)를 쌍으로 저장
최단거리 값을 기준으로 항상 최저값을 가장 아래에 정렬해줌
결과 행렬
모든 노드까지의 최단거리가 저장됨

알고리즘 진행
항상 최단거리인 node 를 우선적으로 방문
모든 node 를 한 번 씩만 방문하는 것을 보장


<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_01.png">
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_02.png">
	노드 <strong>s</strong> 에서부터 시작 (방문여부, 결과행렬 초기화)
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_03.png">
	연결된 모든 인접 노드(<strong>a</strong>, <strong>b</strong>)를 큐에 추가<br>
	가장 작은 거리를 가진 항목이 top 으로 이동
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_04.png">
	큐에서 top 값 (<strong>a</strong>, 2)을 빼냄과 동시에 해당 노드를 방문<br>
	빠진 거리값(2)이 해당 노드까지의 최단거리이므로, 이를 결과 행렬에 저장
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_05.png">
	두 번째로 방문한 노드(<strong>a</strong>)에서 같은 동작을 수행<br>
	인접 노드(<strong>c</strong>)를 큐에 추가 (이미 방문한 노드(<strong>s</strong>)는 다시 갈 필요 없으므로 제외)<br>
	현재 노드까지의 최단거리(2)와 현재 노즈에서 인접 노드까지의 거리(4)를 더한 값(6)이 인접 노드까지의 총 거리가 되므로, queue에는 (<strong>c</strong>, 6)이 입력됨
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_06.png">
	우선순위 큐에 의해 추가된 항목 중 가장 작은 거리를 갖는 노드가 top 으로 이동
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_07.png">
	큐에서 top 항목을 꺼내며 꺼낸 노드(<strong>c</strong>)를 방문 (결과 행렬 갱신)
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_08.png">
	방문한 노드의 인접 노드(<strong>b</strong>)를 다시 큐에 추가
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_09.png">
	우선순위 큐의 항목들 중 가장 작은 거리를 갖는 노드가 맨 밑으로 온다.<br>
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_10.png">
	큐에서 값을 꺼내며 방문 (결과 행렬 갱신)<br>
	(<strong>b</strong>, 12)가 큐에 먼저 입력되었지만, 우선순위 큐에 의해 더 거리가 가까운 (<strong>b</strong>, 9)가 먼저 꺼내어지는 것을 확인할 수 있다.
</p>

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/procedure_11.png">
	우선순위 큐의 top에 있는 (<strong>b</strong>, 12) 를 꺼낸다. 이미 방문했으므로 skip.
	우선순위 큐가 empty 상태가 되면 알고리즘이 종료된다.
</p>

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

#### 알고리즘 과정 예제


#### 고찰
우선순위 큐를 쓰는 이유?
	더 늦게 ‘발견＇한 노드라도 더 먼저 ‘방문＇할 수 있게 하기 위함
큐에서 요소를 꺼낼 때 말고, 요소를 넣을 때 최단거리 정보를 갱신하는 이유?
	조금이라도 빨리 dist[] 를 업데이트하게 되면 search space 가 줄어든다.

#### 조건
어떤 그래프에서 다익스트라를 사용할 수 있을까?  
`무방향 그래프`의 경우, 모든 간선의 **가중치 값이 양수**여야만 알고리즘을 적용할 수 있다. 만약 가중치 값에 음수가 존재하면, 다익스트라 알고리즘이 최단거리를 빠르게 찾기 위한 전제조건이 성립되지 않아 적용이 불가능하다.  
게다가, 만약 동일한 간선을 여러번 지날 수 없다는 조건이 따로 있지 않다면, 음수 가중치를 갖는 간선을 무한대로 왕복하면 최단거리가 마이너스 무한대가 나올 것이다. 문제 자체가 성립되지 않는다.  
`방향 그래프`의 경우엔, `음수 사이클`이 없다면 적용 가능하다. 음수 사이클이란, 무한히 같은 경로를 돌면서 거리가 마이너스 무한대로 발산할 수 있는 경로를 의미한다.
설명의 편의를 위해, 여기서는 모든 간선 가중치 값이 양수인 무방향 그래프를 가정하고 설명을 진행한다.

#### 코드
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
