# Propiedades del MST (_Minimum Spanning Tree_)
- La arista de costo mínimo 
  siempre está en el MST
- Una arista que es única para 
  un vértice dado (única forma 
  de llegar al vértice), 
  siempre está en el MST

# Algoritmo de Kruskal

Se basa en la observación de
que el árbol de costo mínimo
siempre estará constituido
por las aristas de menor costo,
al menos aquellas que no formen 
un ciclo.

También es un algoritmo 
codicioso: no se arrepiente
de las decisiones tomadas.


```
kruskal(G(V,E)):
    E.sortby(cost) # from lowest to highest cost
    T = empty
    for e in E:
        if e does not form a cycle:
            T.append(e)
    return T
```

¿Revisar si forma un ciclo es 
caro?

Notemos que el algoritmo de 
Prim siempre forma un árbol
contiguo en cualquier punto
de la iteración. En contraste,
Kruskal va armando subárboles
que pueden ser disconexos.

Al momento de insertar una 
arista que va a formar un ciclo,
sabemos que es la más costosa 
del ciclo, ya que fue la última
en agregarse.

Si tenemos $|V| -1$ aristas en 
el árbol, nos detenemos. 
Siempre que agreguemos una más, 
formaremos un ciclo.

Notemos que ordenar E por costo
tarda, como mínimo
$\mathcal{O}(|E| \log |E|)$.
Prim por completo tardaba
$\mathcal{O}((|V| + |E|) \log V)$, 
así que más vale que detectar
ciclos sea barato para que sea
competitivo.

## Encontrar ciclos

Nota que agregar $(u,v)$ forma
un ciclo si y solo si $u$ y $v$
están en el mismo subárbol. 
Cada vértice es un subárbol por sí
solo. Cada vez que comenzamos el 
algoritmo, uniremos el vértice $u$ 
(un subárbol) con el $v$ (otro).
El subárbol es más fácil entenderlo
como el subconjunto de vértices
conectados por caminos.
De esta forma, **dos subárboles
siempre corresponden a conjuntos
disjuntos de vértices**.
Una vez que una arista los une, 
pasan a ser el mismo conjunto.

Luego, esto es simple. Solo
revisamos si $u$ y $v$ pertenecen 
al mismo conjunto. En ese caso,
la arista $(u,v)$ formaría un 
ciclo, así que la omitimos.

```
kruskal(G(V,E)):
    E.sortby(cost)
    Considerar cada nodo como un conjunto
    T = None
    for (u,v) in E:
        si u y v no están en el mismo conjunto:
            agregar (u,v) a T
            unir los conjuntos de u y v
    return T
```

## Operaciones sobre conjuntos

Necesitamos:
- Identificar en qué conjunto está
  un elemento
- Unir dos conjuntos y borrar las
  referencias a los anteriores

### Representación

Cada conjunto tiene un vértice 
representante. 

Cada nodo tiene una referencia a su
representante, incluso el
"embajador".

Dos vértices están en el mismo
conjunto si ambos apuntan al 
mismo representante.

Podemos usar árboles en que todos
los elementos del conjunto tienen
un puntero a su representante.
Cuando un vértice se apunta a sí mismo, es el representante del set
al que pertenece.


`make_set(x)`: inicializa $x$ como
su propio representante (loop)

`find_set(x)` retorna el 
representante de $x$. Es posible 
llamarlo recursivamente con el 
padre como argumento, hasta que 
haya un loop. Como consecuencia
de la búsqueda, podemos aprovechar
de actualizar el puntero al 
representante.

`union(x,y)` une los conjuntos
a los que pertenecen $x$ e $y$, 
quedando solo la unión y 
desapareciendo los conjuntos 
originales. Corre en tiempo 
constante.

La operación `find_set(x)` se 
llamará $2 |E|$  veces, una por
cada extremo de las aristas.

La operación de unión se hará
$|V|-1$ veces, ya que inicialmente
hay un conjunto por vértice y 
debemos terminar en un solo 
conjunto.

Podemos lograr la unión cambiando
el puntero del representante de
un conjunto a apuntar al 
representante del otro. Nos 
conviene unir el set menos profundo
hacia el más profundo (se habla
del _rank_ de un subárbol).
Si los rangos son distintos y
uno el de menor rango hacia el 
mayor, el rango resultante se 
mantiene. Por otra parte, si eran
iguales, la unión aumenta el rango
resultante en uno.


```
kruskal(G(V,E)):
    E.sortby(cost)
    for v in V:
        make_set(v)
    T = None
    for (u,v) in E:
        if find_set(u) != find_set(v):
            T.append((u,v))
            union(u, v)
    return T
```


## Correctitud

Para facilitar la demostración, 
conviene mostrar que la solución 
es:
- Un árbol
- Una cobertura
- De costo total mínimo (entre 
  todos los _spanning trees_)
