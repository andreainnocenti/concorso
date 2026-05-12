# Agenti Autonomi e Sistemi Multiagente

## 📖 Introduzione

Agenti che percepiscono l'ambiente e agiscono per raggiungere obiettivi.

## 🤖 Agenti Razionali

```python
class Agent:
    def __init__(self):
        self.percepts = []
    
    def perceive(self, environment):
        """Percepisce l'ambiente"""
        return environment.get_state()
    
    def decide(self, percept):
        """Decide l'azione"""
        # Logic here
        return action
    
    def act(self, action, environment):
        """Esegue l'azione"""
        environment.apply(action)
```

## 🗺️ Pianificazione Automatica

### A* Algorithm

```python
import heapq

def a_star(start, goal, graph, heuristic):
    frontier = [(0, start)]
    came_from = {start: None}
    cost_so_far = {start: 0}
    
    while frontier:
        current_cost, current = heapq.heappop(frontier)
        
        if current == goal:
            break
        
        for next_node in graph.neighbors(current):
            new_cost = cost_so_far[current] + graph.cost(current, next_node)
            
            if next_node not in cost_so_far or new_cost < cost_so_far[next_node]:
                cost_so_far[next_node] = new_cost
                priority = new_cost + heuristic(next_node, goal)
                heapq.heappush(frontier, (priority, next_node))
                came_from[next_node] = current
    
    return reconstruct_path(came_from, start, goal)
```

## 👥 Sistemi Multiagente

Più agenti che interagiscono.

**Applicazioni:**
- Traffico autonomo
- Swarm robotics
- Mercati elettronici
- Giochi multiplayer

## 🔗 Collegamenti
- **Precedente:** [Rappresentazione Conoscenza](rappresentazione-conoscenza.md)
- **Prossimo:** [Problemi ML](problemi-ml.md)
