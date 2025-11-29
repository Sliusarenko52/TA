 #include <iostream>  
#include <vector>  
#include <algorithm>  
#include <numeric>   

using namespace std;  

#define edge pair<int,int>  

class Graph {  
private:  
    vector<pair<int, edge>> G;   
    vector<pair<int, edge>> T;  
    int *parent;  
    int V;   
public:  
    Graph(int V);  
    void AddWeightedEdge(int u, int v, int w);  
    int find_set(int i);  
    void union_set(int u, int v);  
    void kruskal();  
    void print();  
};  

Graph::Graph(int V) {  
    this->V = V;  
    parent = new int[V + 1];  
    // Кожна вершина є представником своєї власної множини  
    for (int i = 1; i <= V; i++)   
        parent[i] = i;   
    G.clear();  
    T.clear();  
}  

void Graph::AddWeightedEdge(int u, int v, int w) {  
    G.push_back(make_pair(w, edge(u, v)));  
}  

int Graph::find_set(int i) {  
    if (i == parent[i])  
        return i;  
    return parent[i] = find_set(parent[i]);   
}  

void Graph::union_set(int u, int v) {  
    // Просто приєднуємо корінь однієї множини до кореня іншої  
    parent[find_set(u)] = find_set(v);   
}  

void Graph::kruskal() {  
    sort(G.begin(), G.end());   

  for (const auto& entry : G) {  
        int u = entry.second.first;  
        int v = entry.second.second;  

  if (find_set(u) != find_set(v)) {   
            T.push_back(entry); // Додати ребро до МКД  
            union_set(u, v); // Об'єднати множини  
        }  
    }  
}  

void Graph::print() {  
    cout << "Edge : Weight" << endl;  
    int totalCost = 0;  
    for (const auto& entry : T) {  
        cout << entry.second.first << " - " << entry.second.second << " : "  
             << entry.first << endl;  
        totalCost += entry.first;  
    }  
    cout << "Total Cost: " << totalCost << endl;  
}  

int main() {  
    Graph g(8); // Ініціалізація графа з 8 вершин (1-8)  

   g.AddWeightedEdge(1, 2, 8);  
    g.AddWeightedEdge(1, 3, 2);  
    g.AddWeightedEdge(1, 4, 4);  
    g.AddWeightedEdge(2, 5, 6);  
    g.AddWeightedEdge(2, 6, 3);  
    g.AddWeightedEdge(3, 4, 6);  
    g.AddWeightedEdge(3, 6, 7);  
    g.AddWeightedEdge(3, 8, 4);  
    g.AddWeightedEdge(4, 5, 1);  
    g.AddWeightedEdge(5, 7, 4);  
    g.AddWeightedEdge(6, 7, 3);  
    g.AddWeightedEdge(7, 8, 5);  
    
  g.kruskal();  
    g.print();  
    return 0;  
}  
