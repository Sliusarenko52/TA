INF = float('inf')

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

def print_matrix(matrix, title):
    """Функція для виводу матриці у форматі таблиці"""
    n = len(matrix)
    print(title)
    
  # Заголовок стовпців
  print("     ", end="")
    for i in range(n):
        print(f"{i+1:4}", end="")
    print("\n    " + "-" * (4 * n + 2))
    
  # Рядки
  for i in range(n):
        print(f"{i+1:<2} |", end="")
        for j in range(n):
            val = matrix[i][j]
            val_str = str(val) if val != INF else "∞"
            print(f"{val_str:>4}", end="")
        print()
    print("-" * (4 * n + 6))

def val_to_str(val):
    """Допоміжна функція для красивого виводу нескінченності"""
    return "∞" if val == INF else str(val)

def floyd_trace_detailed(w_matrix):
    n = len(w_matrix)
    
  # Ініціалізація матриці D^0
  D = [[INF] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if i == j:
                D[i][j] = 0
            elif w_matrix[i][j] != 0:
                D[i][j] = w_matrix[i][j]
    
  print("=== АЛГОРИТМ ФЛОЙДА-УОРШЕЛЛА ===")
    print_matrix(D, "### 0. Ініціалізація (Матриця D^0)")
    
  # Основний цикл по k (проміжним вершинам)
  for k in range(n):
        print(f"\n###### Крок {k+1}: Ітерація k = {k+1} (Проміжна вершина {k+1}) ######")
        print("Розрахунки змін:")
        
  changes_found_in_step = False
        
  for i in range(n):
            row_updates = []
            
   for j in range(n):
                # Якщо існує шлях через k
                if D[i][k] != INF and D[k][j] != INF:
                    current_val = D[i][j]
                    path_through_k = D[i][k] + D[k][j]
                    
  # Якщо шлях через k коротший
  if current_val > path_through_k:
                        calc_str = (
                            f"d_{i+1},{j+1}^{k+1} = "
                            f"min{{{val_to_str(current_val)}; {D[i][k]} + {D[k][j]}}} = {path_through_k}"
                        )
                        row_updates.append(calc_str)
                        
   D[i][j] = path_through_k
            
  if row_updates:
                changes_found_in_step = True
                print(f"  i={i+1}:")
                for update in row_updates:
                    print(f"    {update}")
        
  if not changes_found_in_step:
            print("  (Змін немає, поточні шляхи оптимальні)")
            
  print_matrix(D, f"Матриця D^{k+1}")

return D

# Запуск алгоритму
floyd_trace_detailed(input_matrix)
