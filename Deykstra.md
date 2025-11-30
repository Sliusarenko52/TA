import heapq

INF = float('inf')

# Ваша матриця суміжності (Варіант 18)
input_matrix = [
    # 1  2  3  4  5  6  7  8
    [0, 8, 2, 4, 0, 0, 0, 0], # Вершина 1
    [8, 0, 0, 0, 6, 3, 0, 0], # Вершина 2
    [2, 0, 0, 0, 0, 7, 0, 4], # Вершина 3
    [4, 0, 0, 0, 1, 0, 0, 0], # Вершина 4
    [0, 6, 0, 1, 0, 0, 4, 0], # Вершина 5
    [0, 3, 7, 0, 0, 0, 3, 0], # Вершина 6
    [0, 0, 0, 0, 4, 3, 0, 5], # Вершина 7
    [0, 0, 4, 0, 0, 0, 5, 0]  # Вершина 8
]

def print_state(step, u, dist, pred, pq):
    """Функція для красивого виводу поточного стану"""
    print(f"\nПоточний стан після кроку {step}:")
    
  d_str = ", ".join([f"d[{i+1}]={d if d != INF else '∞'}" for i, d in enumerate(dist)])
    print(f"  dist: {{{d_str}}}")
    
   p_str = ", ".join([f"p[{i+1}]={p+1 if p != -1 else '-'}" for i, p in enumerate(pred)])
    print(f"  pred: {{{p_str}}}")
    
  pq_sorted = sorted(pq)
    q_str = ", ".join([f"({d}, v{v+1})" for d, v in pq_sorted])
    print(f"  Черга PQ: {{{q_str}}}")
    print("-" * 60)

def dijkstra_trace(matrix, start_node):
    n = len(matrix)
    
  adj = {i: [] for i in range(n)}
    for i in range(n):
        for j in range(n):
            if matrix[i][j] != 0:
                adj[i].append((j, matrix[i][j]))

  dist = [INF] * n
    pred = [-1] * n
    dist[start_node] = 0
    pq = [(0, start_node)]
    
  print(f"=== АЛГОРИТМ ДЕЙКСТРИ (Старт: Вершина {start_node + 1}) ===")
    print_state(0, start_node, dist, pred, pq)
    
  step_counter = 1
    
  while pq:
        d, u = heapq.heappop(pq)
        
  if d > dist[u]:
            continue
        
  print(f"### Крок {step_counter}: Обробка вершини {u + 1} (dist={d})")
        print(f"Суміжні вершини Adj({u + 1}):")
        
  updated = False
        for v, weight in adj[u]:
            new_dist = dist[u] + weight
            old_dist_str = str(dist[v]) if dist[v] != INF else "∞"
            
  msg = f"  Релаксація {u+1} -> {v+1}: {dist[u]} + {weight} = {new_dist}. "
            
  if new_dist < dist[v]:
                msg += f"Оновлення! (було {old_dist_str})"
                dist[v] = new_dist
                pred[v] = u
                heapq.heappush(pq, (new_dist, v))
                print(msg)
                updated = True
            else:
                msg += f"Оновлення не потрібне (поточне {old_dist_str})."
                print(msg)
        
  if not updated:
            print("  (Змін відстаней не відбулося)")

  print_state(step_counter, u, dist, pred, pq)
        step_counter += 1

return dist, pred

# Запуск алгоритму від вершини 1 (індекс 0)
dijkstra_trace(input_matrix, 0)
