# Aplicaciones del algoritmo BFS

Para DFS vimos que podíamos recorrer
**grafos acíclicos** para encontrar un
**orden topológico**, y también usarlo
para encontrar las **componentes
fuertemente conexas** en
**grafos cíclicos.**

En ambos casos, requeríamos _leer_
los grafos nuevamente para encontrar
el resultado buscado. Esta vez, con
BFS podemos encontrar la solución
instantáneamente al terminar la primera
búsqueda.

## Búsqueda de la ruta más corta

En un grafo direccional con costos,
podemos encontrar el camino más
corto entre 2 nodos conectados (deben
ser alcanzables, sino no tiene sentido)
usando una generalización de BFS.

Es importante que los costos sean
"sumables linealmente" para que
la solución tenga sentido.

Las soluciones a esta clase de
problemas cumplen la
propiedad de tener una sub-estructura
óptima. Es decir, en una ruta más corta
(óptima) entre dos nodos, todos los
nodos intermedios tienen el mismo camino óptimo entre ellos.

El problema de minimización consiste
en encontrar la menor suma de costos
entre el origen y el destino.

BFS encuentra el camino más corto en
**cantidad de tramos**, pero no
directamente la suma de los costos.

### Propiedades del problema de rutas más cortas

- Rutas direccionales
- Costos representan distancias,
  tiempos de viaje, etc.
- Puede haber nodos no alcanzables
  desde el vértice de partida
- Si hay costos negativos, es más
  complicado resolver el problema (en particular si hay ciclos)
- Puede haber múltiples caminos óptimos

## BFS++

Puedo recorrer el tramo más corto
primero, dentro de los nodos
adyacentes. Siempre que no haya
costos negativos, esto implica que la
primera vez que veamos un nodo,
encontramos la ruta menos costosa
a partir del nodo inicial.

1. Explora todos los vecinos  
   inmediatos y marca el nodo actual
   como "cerrado". En ese momento, su
   costo queda congelado, no podrá
   haber otro menor.
2. Actualiza los costos de cada hijo
   según el costo del último tramo +
   el del camino hasta ahora
   (almacenado en el nodo padre), pero solo si son menores a los ya
   almacenados.
   Si eso ocurre, ctualiza también en
   cada nodo la referencia al padre
   (para luego hacer _trace-back_)
3. En cada iteración, expande el nodo
   con el menor costo (que incluye
   camino hasta ahí + último tramo)

En la ejecución recorreremos cada una
de las $|V|+|E|$ vértices y aristas.
Por cada una, debemos hacer una
extracción de la _open_. Una mala
implementación deberá recorrer los |V|
nodos en cada iteración, haciendo el
algoritmo de complejidad $\mathcal{O}(|
V|^2)$. Sin embargo, si usamos un
_approach_ con cola de prioridad
(_Binary Heap_), cuyo tiempo de
extración es $\mathcal{O}
(1)$ y percolate_up_down e inserción son $\mathcal{O}
(\log |V|)$, deja al algoritmo de
Dijkstra con una complejidad
$\mathcal{O}
\left((|V|+|E|)\log |V|\right)$.
