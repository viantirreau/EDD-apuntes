# Funciones de hash

Recordemos que debíamos convertir la clave $k \in D$ en un valor numérico para usarlo como índice. Habíamos definido dicha función como:

$$ h: D \rightarrow \mathbb{N}_0 $$

## Características deseables

- Rápida de computar
- Distribuir uniformemente en el recorrido
- Baja tasa de colisiones
- Funciona como un compresor (contracción de dominio)
- Usa la información relevante del dato

## Hasheando distintos dominios

### RUT

El DV del RUT es una función de hash sobre el rut, con recorrido $\{0, \dots, 9\} \cup \{k\}$

### ADN

La secuencia AGTTGTCAGACT... se puede hashear usando base 4 y la representación numérica de base 4, algo como $x_m 4^m + x_{m-1} 4^{m-1} + \dots$

### Tableros (de damas, 4 fichas)

Es un tablero de 8x8 con cuatro clases de fichas: rojas, negras y reyes de ambas. Podemos asignar a cada celda del tablero cuatro vectores binarios de 8 bits. Cada ficha que esté en el tablero de entrada determinará cuál de los cuatro vectores aleatorios de 8 bits se usa en el proceso (si es vacía se ignora). Luego, si $n$ es la cantidad de piezas en el tablero, haremos un gran XOR de los $n$ vectores elegidos y el resultado es el hash del tablero.

El algoritmo se llama $\texttt{zobristHash}$.
