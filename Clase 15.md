# Motivación

Bajo qué condiciones se puede hacer un proyecto por completo, si este involucra muchas tareas con relaciones de precedencia (requisito) entre ellas. ¿Cómo sé que ese proyecto es viable?

## Requisitos inconsistentes

Escribiremos las relaciones de prerrequisito como
$$ A \rightarrow B$$
si $A$ es prerrequisito de $B$.

Si existe una secuencia cíclica de requisitos:

$$X\rightarrow R \rightarrow \cdots \rightarrow K \rightarrow X$$

entonces no se puede realizar el proyecto.

## Verificación

¿Cómo hacemos un programa que, dada una lista de tareas y requisitos, pueda determinar si el proyecto es viable? ¿Cómo se puede lograr eficientemente?

Las **tablas de hash** nos servirán para revisar eficientemente a qué proyetos ya le hemos revisado sus prerrequisitos, para detectar ciclos.

Lo interpretaremos como **grafos**.

# Grafos

Son un conjunto de vértices o nodos, conectados mediante arcos, almacenados como un conjunto de aristas.

- Direccionales: las aristas tienen una dirección de conexión
- No direccionales: la relación de `está conectado con` es simétrica

## Representación de grafos

- Listas de adyacencia: el grafo direccional se representa como $|V|$ listas ligadas, cada una contiene los prerrequisitos como nodos de la lista.
- Matriz de incidencia

## Detección de ciclos

Algunas definiciones:

- Posterior: es la clausura transitiva de la conexión de "prerrequisito". Un nodo $Z$ es posterior a $X$ si existe una tarea $Y$ tal que $X \rightarrow Y$ o $Y$ es posterior a $X$, y también $Y\rightarrow Z$.
- Un nodo es posterior a sí mismo si existe un ciclo que contiene al nodo.

```
posteriores(X):
    P = vacio
    for Y tal que X -> Y:
        P = P union {Y}
        P = P union posteriores(Y)
    return P
```

Pero este algoritmo tiene un problema: si efectivamente hay un ciclo, no terminará nunca la recursión. Modificamos el algoritmo para _marcar_ los nodos visitados:

```
posteriores(X):
    if X está pintado: return vacío
    pintar(X)
    P = vacio
    for Y tal que X -> Y:
        P = P union {Y}
        P = P union posteriores(Y)
    return P
```

Notemos que al momento de verificar un nodo ya pintado, tenemos dos opciones para haberlo alcanzado:

- Se alcanzó desde un nodo anterior: no hay ciclo (aún)
- Se alcanzó desde un nodo posterior: hay un ciclo.

Mientras la llamada a `posteriores(X)` siga avanzando, estamos explorando nodos posteriores a $X$. Podemos utilizar el algoritmo de búsqueda de posteriores para considerar la distinción de cómo se alcanza un nodo:

```
hay_ciclo(X):
    if X está pintado de gris: return True
    if X está pintado de negro: return False
    pintar X de gris
    for Y tal que X->Y:
        if hay_ciclo(Y): return True
    pintar X de negro
    return False
```

En este caso, los nodos pintados de gris son todos los visitados normales, mientras que los negros son aquellos a los que se expandieron todos los posteriores. De esta forma, si llego a visitar un negro, no es un ciclo, sino una nueva ruta para alcanzarlo. En contraste, si llego a un gris, tengo seguridad de que es un ciclo.

Y ahora, si queremos saber si hay ciclos en el grafo completo, basta con llamar a la función `hay_ciclo(X)` para cada $X \in V$.

Este algoritmo es bastante eficiente, ya que tiene que revisar a lo más una vez cada arista y cada vértice. Si $|V|$ es la cantidad de vértices y $|E|$ es la cantidad de aristas, el algoritmo tiene complejidad en $\mathcal{O}(|V| + |E|)$.

Este algoritmo se clasifica como búsqueda en profundidad (DFS).
