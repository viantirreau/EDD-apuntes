# Algoritmos codiciosos

- En grafos no direccionales:
  - MSP: _minimum spanning tree_
- En grafos direccionales:
  - Rutas más cortas de un vértice a 
  todos los otros, y entre todos los 
  pares de vértices

# _Minimum Spanning Tree_

### Motivación

Conexión mínima posible entre todas
las ciudades de una región. Podemos
ahorrar "cable" si eliminamos los 
ciclos, por lo que lo mínimo para 
asegurar conectividad entre nodos es
construir múltiples caminos.

¿Cómo hacemos para minimizar el costo
total de una instalación que conecta
todos con todos?

**Asumimos grafos no dirigidos.**

La respuesta es, efectivamente, un 
árbol. Evitamos los ciclos, que 
generan aristas redundantes y no 
agregan conectividad.

Puede no ser único si todas las 
aristas tienen igual costos. Si todas 
son distintas, la solución es única y 
siempre existe para grafos conexos.

La parte "codiciosa" del algoritmo nace
de que agrega una arista a la vez a la
solución, con la mejor decisión 
disponible hasta el momento. Tiene la 
ventaja que no requiere deshacer una 
decisión ya tomada (no se arrepiente).

Explotaremos propiedades del problema
para tomar una decisión de la que no 
nos arrepentiremos.

En cada paso de la resolución, tenemos
un conjunto de nodos y aristas ya 
incluidos ($V_1, E_1$), otro conjunto 
de nodos y aristas no incluidos ($V_2, 
E_2$) , y aristas entre 
estos dos conjuntos $E_{inter}$. 
Si aún no termina 
la ejecución, aún quedan nodos sin 
incluir. Luego, necesariamente, alguna 
de las aristas entre ambos conjuntos 
debe estar en la solución óptima. 
Como, además, estamos minimizando 
costos, en principio elegiremos la más 
barata de $E_{inter}$.

# Algoritmo de Prim
Para un grafo $G(V,E)$, y un nodo 
inicial $x$ (arbitrario, da igual):

1. Dado $x$, sabemos que está a 
   distancia cero de sí mismo. La 
   distancia de los otros vértices a 
   la solución que llevamos hasta 
   ahora es infinita. A medida que 
   vayamos construyendo la solución, 
   el árbol se irá acercando a los 
   otros vértices.

2. Incorpora a la solución el vértice
   a menor distancia de la solución.

3. Por cada vecino del vértice recién 
   incorporado que aún no esté en la 
   solución, recalcula la distancia 
   del vecino al vértice incorporado. 
   Si es menor a la actual, 
   actualízala. Nota que incorporar un 
   vértice a la solución es la única 
   forma que otros vértices que no son 
   parte de ella puedan "acercarse al 
   árbol", por lo que solo necesito 
   actualizar sus vecinos.

4. Encuentra la arista con menor costo
   entre que conectan a nodos aún 
   no presentes en la solución. Vuelve a 2.


Usaremos una cola de prioridades para
mantener un acceso eficiente al mejor
vecino por explorar. En este sentido 
se parece bastante a Dijkstra. 

```
prim(G(V,E), x):
    T = empty
    H = heap()
    H.insert(x)
    x.key = 0
    x.parent = None
    while H is not empty:
        u = H.extract() # O(log n)
        u.painted = True
        if u.parent is not None:
            foreach neighbor not painted:
                if v not in H:
                    H.insert(v) # O(log n)
                if w(u,v) < v.key:
                    v.key = w(u,v) # O(log n)
                    v.parent = u
    return T
```

Este algoritmo mira todas las aristas y 
vértices del grafo, y las operaciones del 
Heap son logarítmicas en el número de 
elementos (vértices). Por tanto, el 
algoritmo
es $\mathcal{O}((|V| + |E|) \log V)$.

Como sabemos que el grafo es conexo, además
tenemos que $|E|\geq|V|$, por lo que la 
complejidad se puede simplificar a 
$\mathcal{O}(|E| \log V)$.

No todos los algoritmos codiciosos 
encuentran soluciones globalmente óptimas.