# Estrategias algorítmicas

- Dividir para conquistar
  - Merge y quick sort
- Backtracking
- Algoritmos codiciosos (_greedy_)
- Programación dinámica
- Branch and bound
  - Optimización

# Backtracking


## 3-coloring

Dado un grafo no dirigido $G(V,E)$, y 3 
colores, queremos determinar si es 
3-coloreable, es decir, que ningún par
de nodos adyacentes tenga el mismo color.

Si se nos presenta una solución, es muy
fácil chequear que es válida. Sin 
embargo, probar que un grafo arbitrario
no es 3-coloreable es más complejo.
Intuitivamente, deberíamos ver todas las
posibles asignaciones de colores, y
si ninguna de ellas es válida, entonces
el grafo no es coloreable.

Este problema se categoriza dentro de la
**satisfacción de restricciones**.

Tiene **variables** en un **dominio**, y
las **restricciones** prohíben ciertas
combinaciones de valores.

- Una variable para cada nodo (color)
- El dominio de la cada variable es 
  $\{1,2,3 \}$. Si alguna vale 0, no 
  tiene un color asociado.

Si existe una solución, queremos una 
prueba, si no existe, queremos una 
garantía.

Podemos generar todas las $3^{|V|}$ 
permutaciones (fuerza bruta), pero
resulta que no requerimos probar
una cantidad exponencial de 
configuraciones.

Podemos probar coloreando los hijos,
si en algún momento encontramos dos 
nodos conectados por una arista con el 
mismo color, el resto del problema se
puede ignorar y se "poda" esa rama del
espacio de búsqueda.

Explora el grafo usando DFS:

```
3-col(G(V,E)):

    if todos los nodos tienen color:
        return true
    v = nodo no coloreado en V
    for c in [1,2,3]:
        if is_valid_assignment(v,c,E):
            v.color = c
            if 3-col(G(V,E)):
                return True
            v.color = 0 # backtrack
    return False  # si salimos del for, no es 3-coloreable
``` 

Como generamos los cambios de forma
incremental, descartando inmediatamente 
los estados que no tienen sentido.

## Estrategia general del _Backtracking_
```
is_solvable(X,R):
    if is_solution(X,R):
        return true
    x = choose_unassigned(X) # ojalá O(1)
    for v in X.domain:
        if is_valid_assignment(v, X, R):
            assign(x,v)
            if is_solvable(X,R):
                return true
            unassign(x,v)
    return false
```

## Problema de las 8 reinas

- Variables: una para cada posición del 
  tablero. Es binaria: tiene reina o no.
  La cardinalidad del espacio de variables
  es $2^{64} \approx 10^{19}$.

En lugar de revisar todo el resto de la 
fila y columna (sabiendo que no puede
haber otra reina), mejor tenemos 1 
variable para cada fila y asumimos que 
cada una tiene una reina en cualquiera
de las 8 columnas. De esta forma, el nuevo
espacio tiene $8^8$ estados posibles.


## Slitherlink

Tiene la restricción de que la cantidad de 
aristas que toca un cuadrado con un número
debe ser igual a su valor. Además, debemos
hacer un ciclo cerrado conectando la grilla
de puntos del tablero.

Una estrategia para reducir la complejidad
del chequeo de "is_solvable", es agregar
restricciones adicionales al espacio de
búsqueda. Denominamos esto como **poda**.
Se deducen de las restricciones originales.
Si son algo local, $\mathcal{O}(1)$, 
conviene bastante. Pero si es algo a nivel 
del espacio completo, tal vez no es buena 
idea.


## Heurísticas

Nos indican qué opciones nos convienen más.
Conviene usarlas si son fáciles de 
calcular, incluso pudiendo contener
información imperfecta.