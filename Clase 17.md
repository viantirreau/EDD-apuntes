# Repaso DFS

Primero se pintan todos los nodos por visitar de color 
blanco. Cuando recién lo expando lo marco de gris, y
cuando termino de recorrer todos los nodos adjacentes,
lo pinto de negro (al cerrarlo). 

## Agregamos un contador para intervalos de tiempo

Cada vez que pintamos un nodo gris, asignamos un número
único, autoincremental a cada nodo. Hacemos lo mismo
con el nodo cuando lo pintamos de negro. De esta forma,
cada nodo almacena un "tiempo de inicio" y un "tiempo
de término".

Si terminamos el recorrido desde un nodo de inicio, 
seguimos aumentando el contador y verificamos los
otros nodos que sigan en blanco, agregando los
tiempos de inicio y término.

Si desde un nodo que está explorando sus hijos expande
a otro que está pintado de gris, sabemos que hay un 
ciclo. En cualquier caso, si todos los hijos de un nodo
aún gris están en gris o negro, cerramos dicho nodo,
pintamos de negro y agregamos el tiempo de término.

Notemos que el tiempo de término está acotado por
la cantidad de aristas que, a su vez, están en $\mathcal{O}(|V|^2)$.

### Tipos de aristas

- De árbol $\rightarrow$ un conjunto de caminos 
  (acíclico) a partir de un nodo inicial
- Hacia adelante $\rightarrow$ shortcut. Camino más 
  corto a nodo visitado
- Hacia atrás $\rightarrow$ cierran ciclos (gris a 
  gris) 
- Cruzadas $\rightarrow$ entre componentes conexas del 
  grafo (solo direccionales)

### Propiedades de los intervalos de tiempo

- No se traslapan. Están completamente contenidos en  otro intervalo o son disjuntos.
- Si un intervalo está contenido en otro, los nodos
  asociados están en el mismo árbol (componente conexa)
- Si dos intervalos son disjuntos, están en 
  componentes distintas

Dado que los nodos quedan de negro al terminar el 
algoritmo, no los podemos usar para conocer propiedades
del grafo. Sin embargo, quedan los intervalos disponibles.

¿Cuál es la utilidad práctica de tener el tiempo de inicio y término?

# Orden topológico y componentes conexas

Para grafos acíclicos, los tiempos de finalización
nos permiten encontrar un orden topológico para los
vértices. Por ejemplo, si el grafo representa 
las tareas de un proyecto, podemos encontrar 
una secuencia en la cual realizar las actividades,
con tal de garantizar que dada cualquier actividad, 
podemos realizarla al tener sus prerrequisitos 
cumplidos. Otro ejemplo es "estirar" un grafo y ponerlo
como una secuencia de nodos conectados de 
izquierda a derecha.

Para grafos cíclicos, los tiempos de finalización 
nos permiten determinar las componentes fuertemente
conexas del grafo, es decir, aquel subconjunto de $V$ 
que tiene nodos que se alcanzan todos con todos 
dentro de ese conjunto.

## Orden topológico

En grafos acíclicos, el primer nodo en ser asignado un
tiempo de finalización, ese nodo necesariamente el 
último nodo del orden topológico. Todo el resto tiene
flechas de salida, luego el primero en terminar
es el que no tiene nodos de salida. 

Si por casualidad hubiese muchos nodos de partida, 
se puede crear un "nodo virtual" que actúe como 
iniciador de todos. Se puede hacer de forma análoga 
para los de término, si hubiese varios nodos que no 
son prerrequisitos para ningún otro.

Para un grafo, hay varias permutaciones de nodos
que resultan en distintos órdenes topológicos, 
dependiendo de los nodos iniciales que se elijan.

```
topoSort(G):
    l = lista vacía
    dfs_con_tiempos(G):
        cada vez que calculamos el tiempo end:
            l.append(nodo)
    return l
```
Es un algoritmo de complejidad $\mathcal{O}(|V|+|E|)$.

## Componentes fuertemente conexas

#### Algoritmo de Kosaraju

Cuando agrupo las CFC en otro grafo (etiquetando, por 
ejemplo, los nodos como la concatenación de los 
elementos de la componente), el grafo resultante es **acíclico**.
Además, el grafo traspuesto (dar vuelta todas las 
aristas) tiene las mismas componentes conexas, mientras
que el grafo de CFC también es acíclico y con las 
conexiones invertidas. Si este grafo acíclico lo 
ordeno topológicamente, el último nodo es una componente conexa.

Visto de otra forma, debemos mantener los tiempos
de finalización y recorrerlas en sentido descendiente 
respecto al tiempo de finalización.
Busca los conjuntos conexos maximales, en que los
nodos del conjunto son todos alcanzables entre todos.

Debemos hacer 2 DFS, así que es $\mathcal{O}(|V|+|E|)$.

```
kosaraju(G):
    L = empty list
    dfs(G)
    l.insert(G.sortby(final_times, descending=True))
    for u in L:
       assign(u, u)
    return L
```

