#include <iostream>  
#include <cstring>  

using namespace std;  

#define INF 9999999   

// Кількість вершин у графі (V=8)  
#define V 8   

int G[V][V] = {  
    {0,  8,  2,  4,  0,  0,  0,  0},   
    {8,  0,  0,  0,  6,  3,  0,  0},   
    {2,  0,  0,  6,  0,  7,  0,  4},   
    {4,  0,  6,  0,  1,  0,  0,  0},   
    {0,  6,  0,  1,  0,  0,  4,  0},   
    {0,  3,  7,  0,  0,  0,  3,  0},   
    {0,  0,  0,  0,  4,  3,  0,  5},   
    {0,  0,  4,  0,  0,  0,  5,  0}    
};  

void prim_mst() {  
    int no_edge = 0;   
    int total_cost = 0;  
    bool selected[V];   
    
  memset(selected, false, sizeof(selected));  
    selected[0] = true; // Починаємо з вершини 1 (індекс 0)  

  cout << "Edge : Weight" << endl;  

  while (no_edge < V - 1) {   
        int min_weight = INF;  
        int u = 0;   
        int v = 0;   

  // Пошук найдешевшого ребра, що з'єднує обрані з необраними  
        for (int i = 0; i < V; i++) {  
            if (selected[i]) {   
                for (int j = 0; j < V; j++) {  
                    // Якщо j не обрана, і G[i][j] > 0 (ребро існує)  
                    if (!selected[j] && G[i][j] > 0) {   
                        if (min_weight > G[i][j]) {  
                            min_weight = G[i][j];  
                            u = i;  
                            v = j;  
                        }  
                    }  
                }  
            }  
        }  
        
  // Додавання обраного ребра  
        cout << u + 1 << " - " << v + 1 << " : " << G[u][v] << endl;  
        total_cost += G[u][v];  
        selected[v] = true;  
        no_edge++;  
    }  
    cout << "Total Cost: " << total_cost << endl;  
}  

int main() {  
    prim_mst();  
    return 0;  
}  
