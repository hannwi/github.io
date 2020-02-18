---
layout: post
title:  "Dijkstra Algorithm 01 (Intro)"
crawlertitle: "Dijkstra Algorithm"
summary: "Study Dijkstra algorithm"
date:   2020-02-11
categories: posts
tags: 'dijkstra'
author: Hannwi
---

### 최단거리 문제
알고리즘 문제를 풀다 보면, "어떤 점수의 최대값, 최소값 등을 구하시오." 같은 문제를 본다.
이 때 문제가 `그래프`로 모델링 가능하다면, 대부분 그래프의 최단거리를 계산함으로써 답을 구할 수 있다.

예시 문제: [알고스팟_BFS][problem_link]

#### 그래프(Graph)란?
그래프는 `정점(node)` 과 `간선(edge)`으로 이루어진 집합을 의미한다. 각 간선은 `가중치(weight)`를 가지고 있을 수 있으며, 가중치가 없을 때는 모든 간선이 1 의 가중치 값을 갖는다고 가정한다.  
또한, 간선의 방향성 종류에 따라 두 가지로 그래프를 분류할 수 있는데, 간선에 방향성이 있으면 `방향 그래프(directed graph)`와, 방향성이 없으면 (양 방향으로 이동이 가능) `무방향 그래프(undirected graph)`라 부른다.

<p align="center">
	<img src="/assets/images/algorithm/dijkstra/undirected_graph.png"><br>
	<em>undirected graph with weighted edges</em>
</p>

#### 그래프의 경로(path)와 거리(distance)
그래프의 특정 정점으로부터 특정 정점으로 이동하기 위해 지나가는 간선들의 집합을 `경로`라고 하자. 이 때, 경로의 가중치 합이 그 경로의 `거리`가 된다.  
그래프의 **최단거리**란, 특정 시작 정점 s에서 출발하여 종료 정점으로 도착하기까지 지나는 모든 경로 중에서 가장 거리가 작은 경로의 거리 값을 의미한다.  

<p align="center">
	<img width="400" src="/assets/images/algorithm/dijkstra/undirected_graph2_min_distance.png"><br>
	<em>minimum distances (red figures) from <strong>s</strong> to each node.</em>
</p>

그래프의 최단거리를 구하는 알고리즘은 여러 가지가 존재하지만, 이 포스트에서는 그 중 가장 유명한(?) 다익스트라(Dijkstra) 알고리즘에 대해 공부한 내용을 정리해본다.

### 다익스트라(Dijkstra) 알고리즘
#### 목적
다익스트라 알고리즘은 정해진 시작 정점 s로부터 `다른 모든 정점`까지의 최단거리를 구해주는 알고리즘이다.  
그래서 시작점이 정해져 있는 문제에서 사용되며, 시작점이 정해져있지 않을 때는 다익스트라를 여러 번 실행해야 한다.

#### 방법 개요
우선순위 큐(priority queue)를 사용한 너비 우선 탐색(breadth first search)

[problem_link]: https://algospot.com/judge/problem/read/BOJ
