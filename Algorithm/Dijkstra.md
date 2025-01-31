# 다익스트라 알고리즘 (Dijkstra's Algorithm)

## 다익스트라 알고리즘
- 양의 가중치만을 가지는 그래프에서 한 정점에서 다른 모든 정점까지의 최단거리를 구하는 알고리즘
- 우선순위 큐(Priority Queue)를 사용해서 가장 가중치가 작은 경로부터 확인하면서 경로를 확인한다.

## 동작 원리
1. 우선순위 큐에서 가장 짧은 경로의 정점을 꺼낸다
2. 현재 정점에서 이동할 수 있는 경로에 대해 거리를 계산하여, 이전의 거리보다 더 짧다면 새 거리로 갱신하고 방문한 정점을 우선순위 큐에 넣는다
3. 더 이상 반복이 불가능할 때까지 위 과정을 반복한다.

## 구현

```cpp

#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
#include <utility>
#define NUM_EDGE 1000
#define INF 987654321

using namespace std;

struct edge{
    int vertex;
    int weight;
};

vector<edge> graph[NUM_EDGE];
int dist[NUM_EDGE];

void dijkstra(int start){
    // {sum of cost, vertex}
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
    
    fill(dist, dist + NUM_EDGE, INF);
    
    dist[start] = 0;
    pq.push({0, start});
    
    while(!pq.empty()){
        int here_cost = pq.top().first;
        int here_vertex = pq.top().second;
        pq.pop();
        
        if(dist[here_vertex] != here_cost) continue;
        
        for(edge next : graph[here_vertex]){
            int next_cost = here_cost + next.weight;
            
            if(next_cost < dist[next.vertex]){
                dist[next.vertex] = next_cost;
                pq.push({next_cost, next.vertex});
            }
        }
    }
}


```