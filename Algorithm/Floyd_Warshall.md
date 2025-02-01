# 플로이드 워셜 (Floyd-Warshall)
- 모든 정점에서 모든 정점까지의 최단 경로를 구하는 알고리즘
- 음수 가중치가 있는 그래프에도 사용할 수 있지만, 음수 사이클이 존재하면 사용할 수 없다.
## 동작 원리
- 노드 i에서 j까지의 경로가 존재할 때 노드 k를 경유하는 경우가 더 짧은 경로라면 해당 경로로 갱신한다.
## 구현

```cpp
#include <iostream>
#include <algorithm>
#define MAX_NODE 1000
#define INF 987654321

using namespace std;

int n, m;

int dist[MAX_NODE][MAX_NODE];

int main(){
    
    cin>>n>>m;
    
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= n; j++){
            if(i == j) dist[i][i] = 0;
            else dist[i][j] = INF;
        }
    }
    
    for(int i = 0; i < m; i++){
        int u, v, w;
        cin>>u>>v>>w;
        
        dist[u][v] = min(dist[u][v], w);
    }
    
    for(int k = 1; k <= n; k++){
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= n; j++){
                if(dist[i][k] == INF || dist[k][j] == INF) continue;
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
            }
        }
    }
    
    return 0;
}

```

## 시간복잡도
- 노드 개수 N에 대해 O(N^3)의 시간 복잡도를 가진다.
- 시간복잡도가 매우 크므로 N이 작을 때만 사용가능하다.