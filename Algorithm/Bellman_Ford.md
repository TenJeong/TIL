# 벨만 포드 (Bellman-Ford)
- 음수 가중치가 있고 음수 사이클이 있는 그래프에서 단일 출발점 최단 경로를 구하는 알고리즘
- 음수 사이클 존재 유무 파악 가능

## 동작 원리
- 모든 간선을 반복적으로 확인하며 최단 경로를 갱신함
1. 초기화: 출발 노드까지의 거리는 0, 다른 노드까지의 거리는 매우 큰 값으로 초기화
2. 최단 경로 갱신: 노드 개수 V에 대해 모든 간선을 V-1번 확인하면서 최단 경로를 갱신한다.
3. 음수 사이클 확인: 최단 경로가 확정된 후에도 다시 간선을 확인했을 때 최단 경로가 더 짧아지면 음수 사이클이 존재한다.

## 구현

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <utility>
#include <queue>
#define INF 987654321
#define MAX_NODE 1000

using namespace std;

int N, M;
int dist[MAX_NODE];

// next_node, weight
vector<pair<int, int>> graph[MAX_NODE];

int main(){
    
    fill(dist, dist + MAX_NODE, INF);
    
    cin>>N>>M;
    
    for(int i = 0; i < M; i++){
        int u, v, w;
        cin>>u>>v>>w;
        
        graph[u].push_back({v, w});
    }
    
    dist[0] = 0;
    queue<int> q;
    
    for(int i = 0; i < N; i++){
        for(int u = 0; u < N; u++){
            for(pair<int, int> next : graph[u]){
                int v = next.first;
                int weight = next.second;
                
                if(dist[u] != INF && dist[u] + weight < dist[v]){
                    if(i == N - 1) q.push(v);
                    dist[v] = dist[u] + weight;
                }
            }
        }
    }
    
    if(q.size()) cout<<"negative cycle\n";
    else{
        for(int i = 0; i < N; i++){
            cout<<"node:"<<i<<" distance: ";
            if(dist[i] == INF) cout<<"-1\n";
            else cout<<dist[i]<<"\n";
        }
    }
    
    
    return 0;
}

```

## 시간 복잡도
- 노드 개수: V
- 엣지 개수: E
- O(VE)