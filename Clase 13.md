# Tablas de hash

- Si **no nos interesa** el orden de los datos, podemos lograr complejidades menores a $\mathcal{O}(n \log n)$. ¡Podemos acceder a ellos en $\mathcal{O}(1)$!
- Quizás podemos utilizar un arreglo y explotar el acceso en tiempo constante. Pero necesitamos una función que nos permita transformar inputs en llaves.
  
## Funciones de hash

Una función de hash se define como 
$$ h: D \rightarrow \mathbb{N}_0 $$
Donde $D$ es el dominio de las claves. Podemos usar esta función para definir en qué celda guardar una llave $\texttt{key} \in D$.

## Tablas de hash

Como diccionario, guarda pares `(key, value)`, con `key` $\in D$. Ahora, `value` se guarda en $T[h(k)]$, con $T$ un arreglo de tamaño $m$.

**Atención**, ¿qué ocurriría si $T[h(k)] > m$? Debemos prepararnos para este caso restringiendo el output de $h(k)$.

### Método de la división

Una forma trivial de lograr esto es usar $h'(k) = h(k) \mod m$, guardando ahora `(k,v)` en $T[h'(k)]$. Se recomienda que $m$ sea número primo para evitar las colisiones.

### Método de la multiplicación

Sea $A \in [0, 1]$

$$h'(x) = \lfloor m \cdot (A \cdot h(x) \mod 1) \rfloor$$

- $h(x)$ va a devolver un número entero positivo.
- Al multiplicar por $A$ quedará probablemente un real entre 0 y el valor más alto que pueda salir de $h$.
- Al sacar $\mod 1$ quedará la parte fraccionaria. 
- Al multiplicarlo por $m$ escalamos al valor de la tabla
- Finalmente con el piso, aseguramos que sea entero entre 0 y $m$.


Por lo tanto, `(k, v)` se guarda en $T[h'(k)]$

Se recomienda utilizar $A = \frac{1}{\phi}$ (golden ratio)


## Colisiones

Hay una colisión en la función de hash si existen valores $a, b \in D$, $a\neq b$, tales que 

$$ h(a) = h(b),$$

mientras que se considera una colisión de tabla si 

$$ h'(a) = h'(b) $$

Colisión de hash $\Rightarrow$ colisión de tabla. No siempre la implicancia contraria se cumple.

## Manejo de colisiones

### Métodos populares de direccionamiento abierto

Se refiere a la búsqueda de **otras** posiciones en la tabla para una llave que sufre una colisión.

- Sondeo lineal: buscar en $H$, $H+1$, $H+2$,...
- Sondeo cuadrático: buscar en $H$, $H+1 c_1 + 1^2 c_2$, $H + 2 c_1 + 2^2 c_2$,...
- _Double hashing_: buscar en $h_1(k), h_1(k) + h_2(k)$, ...

Con estos métodos, basta iterar en el sondeo hasta que la llave almacenada en la celda sea la que estoy buscando o esté vacía (en cuyo caso, no existe).

Estos métodos tienen un grave problema. **No podemos eliminar los nodos!** Por ejemplo, usando el sondeo lineal, si eliminamos una celda del arreglo, todas las llaves que vienen después no podrán ser accedidas si hubo una colisión durante su inserción. Como posible solución, veremos que cada celda de la tabla de hash se puede conectar a la cabeza de una lista ligada o la raíz de un árbol.

## Factor de carga

El factor de carga $\lambda$ se define como la razón entre la cantidad $n$ de claves almacenadas en la tabla, y su tamaño $m$.

$$\lambda = \dfrac{n}{m}$$

Podemos fijar un valor $\lambda_{max}$ para preocuparnos que, en todo momento, $\lambda < \lambda_{max}$.

Si en algún momento $\lambda > \lambda_{max}$, debemos hacer crecer la tabla, aumentando $m$ (no podemos borrar los $n$). Debemos hacer **rehashing**.

### Rehashing

Es un proceso con complejidad al menos $\mathcal{O}(n)$, ya que debemos _rehashear_ todos los elementos.

## Resumen

- Identificar el dominio de las claves
- Diseñar una función de hash para ese dominio
- Definir qué hacer con los valores que escapan del tamaño de la tabla
- Definir un esquema de resolución de colisiones
- Elegir un $\lambda_{max}$ para garantizar eficiencia.

- En la práctica, las tablas de hash bien diseñadas permiten búsquedas, inserciones y eliminaciones son operaciones con complejidad $\mathcal{O}(1)$.
  