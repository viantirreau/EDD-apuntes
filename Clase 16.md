# Recorriendo grafos

## DFS: Depth First Search

Las aristas se exploran a partir del vértice recién descubierto que aún
tiene aristas sin explorar.

Los vértices se pintan para evitar recorrer cíclicamente.
Inicialmente, todos son **blancos**. Apenas encontramos un nodo,
lo pintamos de **gris**. Si la lista de adyacencias se
exploró exhaustivamente, el nodo aludido se pinta de **negro**.

Este algoritmo tiene una complejidad en $\mathcal{O}(|V| + |E|)$.
Si bien el número de aristas está acotado por $\mathcal{O}(|V|^2)$,
ya que, si se conectan todos con todos,
hay $\frac{(|V|)\cdot(|V|-1)}{2}$ aristas.

```
dfs(D, s, goal):
    if s in D: return false
    D.insert(s)
    if s == goal: return true
    foreach valid_state, op in operations(s):
        t <- valid_state
        t.parent <- s
        t.operation <- op
        if dfs(D, t, goal):
            return True
    return false
```

**Usa un stack para la lista open**.

### ¿Cuánto nos demoraremos en encontrar una solución usando DFS?

Sabemos que el puzle de 15 tiene un espacio de estados
cuya cardinalidad es del orden de 16! (sí, factorial).

Nada impide que debamos recorrer todo el espacio para
saber si existe una solución y cuál es. Además, no sabemos nada
sobre la optimalidad de la solución.

## BFS: Breadth first search

Recorre **por niveles** y encuentra soluciones óptimas.
No obstante, requiere una cantidad de memoria
exponencial en la profundidad recorrida.
**Usa una cola para la lista open**.

En pseudocódigo:

1. set $i=1$.
2. generar estados a distancia $i$ del origen
3. si es el _goal_, return
4. $i$++, goto 2.

```
bfs(D, s, goal):
    open = queue()
    D = dict()
    open.insert(s)
    D.insert(s)
    while open is not empty:
        s = open.next()
        foreach operation op:
            t = op(s)
            t.parent = s
            t.operation = op
            if t==goal:
                return true
            if t not in D:
                D.insert(t)
                open.insert(t)
    return false
```
