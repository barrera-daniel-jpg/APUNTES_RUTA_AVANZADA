---

title: Guía práctica de análisis de datos con Python tags:

- python
- pandas
- numpy
- sqlalchemy
- visualizacion
- riwi cssclasses:
- wide-page

---

# Guía práctica de análisis de datos con Python

**NumPy · pandas · SQLAlchemy · Matplotlib · Seaborn**

> Material de estudio autocontenido y documento de consulta rápida. Todos los ejemplos corren sobre **un solo dataset** generado dentro del propio script: no hay que descargar nada.
> 
> Verificado con Python 3.12, NumPy 2.x, pandas 2.2, SQLAlchemy 2.0, Matplotlib 3.10, Seaborn 0.13.

---

## Tabla de contenidos

- [[#0. Cómo usar esta guía|0. Cómo usar esta guía]]
- [[#1. Prerrequisitos de Python|1. Prerrequisitos de Python]]
- [[#2. El dataset del documento|2. El dataset del documento]]
- [[#3. NumPy|3. NumPy]]
    - [[#3.1 Por qué NumPy y no listas|3.1 Por qué NumPy y no listas]]
    - [[#3.2 Creación de arrays|3.2 Creación de arrays]]
    - [[#3.3 Atributos y tipos (dtype)|3.3 Atributos y tipos (dtype)]]
    - [[#3.4 Indexación y slicing|3.4 Indexación y slicing]]
    - [[#3.5 Máscaras booleanas|3.5 Máscaras booleanas]]
    - [[#3.6 Broadcasting y operaciones vectorizadas|3.6 Broadcasting y operaciones vectorizadas]]
    - [[#3.7 Agregaciones por eje|3.7 Agregaciones por eje]]
    - [[#3.8 Manejo de NaN|3.8 Manejo de NaN]]
    - [[#3.9 Reshape y concatenación|3.9 Reshape y concatenación]]
    - [[#3.10 Tabla resumen NumPy|3.10 Tabla resumen NumPy]]
- [[#4. pandas|4. pandas]]
    - [[#4.1 Series y DataFrame|4.1 Series y DataFrame]]
    - [[#4.2 Lectura y escritura|4.2 Lectura y escritura]]
    - [[#4.3 Inspección|4.3 Inspección]]
    - [[#4.4 Selección: loc e iloc|4.4 Selección: loc e iloc]]
    - [[#4.5 Filtrado booleano|4.5 Filtrado booleano]]
    - [[#4.6 Valores nulos|4.6 Valores nulos]]
    - [[#4.7 Tipos de datos y conversión|4.7 Tipos de datos y conversión]]
    - [[#4.8 Columnas derivadas|4.8 Columnas derivadas]]
    - [[#4.9 groupby y agregaciones|4.9 groupby y agregaciones]]
    - [[#4.10 Ordenamiento|4.10 Ordenamiento]]
    - [[#4.11 merge, join y concat|4.11 merge, join y concat]]
    - [[#4.12 Fechas|4.12 Fechas]]
    - [[#4.13 pivot_table y melt|4.13 pivot_table y melt]]
    - [[#4.14 Duplicados|4.14 Duplicados]]
    - [[#4.15 Texto con el accesor .str|4.15 Texto con el accesor .str]]
    - [[#4.16 apply frente a operaciones vectorizadas|4.16 apply frente a operaciones vectorizadas]]
    - [[#4.17 Tabla resumen pandas|4.17 Tabla resumen pandas]]
- [[#5. SQLAlchemy|5. SQLAlchemy]]
    - [[#5.1 Core vs ORM|5.1 Core vs ORM]]
    - [[#5.2 Engine y conexión a PostgreSQL|5.2 Engine y conexión a PostgreSQL]]
    - [[#5.3 Ejecutar SQL|5.3 Ejecutar SQL]]
    - [[#5.4 Integración con pandas|5.4 Integración con pandas]]
    - [[#5.5 Sesiones y ORM mínimo|5.5 Sesiones y ORM mínimo]]
    - [[#5.6 Tabla resumen SQLAlchemy|5.6 Tabla resumen SQLAlchemy]]
- [[#6. Matplotlib|6. Matplotlib]]
    - [[#6.1 Figura y ejes|6.1 Figura y ejes]]
    - [[#6.2 Los cinco gráficos|6.2 Los cinco gráficos]]
    - [[#6.3 Subplots|6.3 Subplots]]
    - [[#6.4 Anotación, estilo y guardado|6.4 Anotación, estilo y guardado]]
    - [[#6.5 Tabla resumen Matplotlib|6.5 Tabla resumen Matplotlib]]
- [[#7. Seaborn|7. Seaborn]]
    - [[#7.1 Tabla resumen Seaborn|7.1 Tabla resumen Seaborn]]
- [[#8. Errores frecuentes|8. Errores frecuentes]]
- [[#9. Glosario|9. Glosario]]
- [[#10. Menciones honoríficas|10. Menciones honoríficas]]
- [[#11. 15 ejercicios progresivos|11. 15 ejercicios progresivos]]
- [[#12. Soluciones comentadas|12. Soluciones comentadas]]
- [[#13. Ruta de estudio|13. Ruta de estudio]]

---

## 0. Cómo usar esta guía

Esta guía **no** es la documentación completa de cada librería. Es el subconjunto que un analista usa todas las semanas: aproximadamente el 20% de la API que resuelve el 80% del trabajo. Lo demás está listado, sin desarrollar, en [[#10. Menciones honoríficas|Menciones honoríficas]].

Cada método se presenta con la misma plantilla:

|Campo|Qué contiene|
|---|---|
|**Sintaxis**|La forma mínima de invocarlo|
|**Qué hace**|Una frase|
|**Ejemplo**|Código ejecutable sobre el dataset de la guía|
|**Salida**|Escrita como comentario `#` dentro del bloque|
|**Cuándo usarlo**|El caso real de uso|

Cuando hay varias formas de hacer lo mismo, una está marcada **`[RECOMENDADA]`** y las demás **`[LA VERÁS EN CÓDIGO AJENO]`**. No son equivalentes: hay una que conviene y las otras se documentan solo para que las reconozcas cuando leas código de otros.

**Instalación (una sola vez):**

```bash
pip install numpy pandas sqlalchemy psycopg2-binary matplotlib seaborn openpyxl
```

---

## 1. Prerrequisitos de Python

Checklist de autoevaluación. Lee cada ejemplo. **Si no puedes predecir la salida sin ejecutarlo, repasa ese tema antes de seguir.** Estos ocho conceptos aparecen en prácticamente cada línea de pandas.

- [ ] **Indexación y slicing de listas**

```python
xs = [10, 20, 30, 40, 50]
xs[1:4], xs[-1], xs[::2]   # ([20, 30, 40], 50, [10, 30, 50])
```

- [ ] **Comprensiones de lista**

```python
[x * 2 for x in [1, 2, 3] if x != 2]   # [2, 6]
```

- [ ] **Funciones lambda**

```python
f = lambda x: x * 1.19
f(1000)   # 1190.0
```

- [ ] **Desempaquetado de tuplas** — es lo que hace funcionar `for nombre, grupo in df.groupby(...)`

```python
ciudad, total = ("Barranquilla", 20804205)
ciudad   # 'Barranquilla'
```

- [ ] **Argumentos con nombre y valores por defecto** — pandas vive de esto (`axis=`, `how=`, `inplace=`)

```python
def saludar(nombre, saludo="Hola"):
    return f"{saludo}, {nombre}"
saludar("Ana", saludo="Buenas")   # 'Buenas, Ana'
```

- [ ] **Mutable vs inmutable** — explica por qué un DataFrame puede cambiar "solo"

```python
a = [1, 2]; b = a; b.append(3); a   # [1, 2, 3]  <- la lista es mutable, b y a son el MISMO objeto
s = "ab";   t = s; t += "c";    s   # 'ab'       <- el string es inmutable, t es un objeto nuevo
```

- [ ] **Encadenamiento de métodos** — el estilo dominante en pandas

```python
"  Ana Gomez  ".strip().lower().replace(" ", "_")   # 'ana_gomez'
```

- [ ] **Rutas de archivos y context managers**

```python
from pathlib import Path
ruta = Path("datos") / "ventas.csv"      # datos/ventas.csv, portable Windows/Linux
with open(ruta, "w", encoding="utf-8") as f:
    f.write("hola")                       # el archivo se cierra solo al salir del with
```

- [ ] **Diccionarios y `.items()`** — la estructura con la que se construye un DataFrame

```python
d = {"city": ["Cali", "Bogota"], "units": [10, 20]}
list(d.keys())   # ['city', 'units']
```

- [ ] **Manejo de excepciones**

```python
try:
    int("x")
except ValueError as e:
    print("no convertible")   # no convertible
```

---

## 2. El dataset del documento

Un solo dataset recorre toda la guía: **ventas de una distribuidora colombiana**, 2024–2025. Tiene a propósito columnas numéricas, de texto, categóricas, de fecha, **valores nulos y filas duplicadas**, porque los datos reales vienen así.

Copia este bloque a un archivo `ventas.py`. Todos los ejemplos posteriores asumen que ya lo ejecutaste.

```python
import numpy as np
import pandas as pd

rng = np.random.default_rng(42)   # semilla fija -> resultados reproducibles
N = 500

cities     = ["Barranquilla", "Bogota", "Medellin", "Cali", "Cartagena"]
channels   = ["Online", "Tienda", "Mayorista"]
categories = ["Bebidas", "Snacks", "Aseo", "Lacteos"]
products = {
    "Bebidas": ["Agua 600ml", "Gaseosa 1.5L", "Jugo Mango"],
    "Snacks":  ["Papas Fritas", "Mani Salado", "Galletas"],
    "Aseo":    ["Jabon Barra", "Detergente 1kg", "Shampoo"],
    "Lacteos": ["Leche 1L", "Yogurt 200g", "Queso Costeno"],
}
# nombres sucios a propósito: espacios sobrantes y mayúsculas inconsistentes
reps = ["  ana gomez ", "CARLOS DIAZ", "luisa Perez",
        "  Jorge Ramirez", "maria  lopez ", "PEDRO TORRES"]

category = rng.choice(categories, N, p=[0.35, 0.25, 0.20, 0.20])
product  = np.array([rng.choice(products[c]) for c in category])

ventas = pd.DataFrame({
    "order_id":   np.arange(1001, 1001 + N),
    "order_date": pd.to_datetime("2024-01-01") + pd.to_timedelta(rng.integers(0, 545, N), unit="D"),
    "city":       rng.choice(cities, N, p=[0.30, 0.25, 0.20, 0.15, 0.10]),
    "channel":    rng.choice(channels, N, p=[0.45, 0.35, 0.20]),
    "category":   category,
    "product":    product,
    "sales_rep":  rng.choice(reps, N),
    "units":      rng.integers(1, 40, N),
    "unit_price": np.round(rng.gamma(shape=4.0, scale=2200, size=N), -1),  # COP
    "discount":   np.round(rng.choice([0.0, .05, .10, .15, .20], N, p=[.5, .2, .15, .1, .05]), 2),
})

# nulos intencionales
ventas.loc[rng.choice(N, 35, replace=False), "units"]      = np.nan
ventas.loc[rng.choice(N, 20, replace=False), "unit_price"] = np.nan
ventas.loc[rng.choice(N, 15, replace=False), "discount"]   = np.nan
ventas.loc[rng.choice(N, 10, replace=False), "city"]       = None

# 8 filas duplicadas intencionales
ventas = pd.concat([ventas, ventas.sample(8, random_state=7)], ignore_index=True)

pd.set_option("display.width", 110)
pd.set_option("display.max_columns", 20)

print(ventas.shape)
# (508, 10)
```

**Diccionario de datos:**

|Columna|Tipo|Descripción|Nulos|
|---|---|---|---|
|`order_id`|int64|Identificador del pedido|0|
|`order_date`|datetime64|Fecha del pedido|0|
|`city`|object|Ciudad de destino|10|
|`channel`|object|Online / Tienda / Mayorista|0|
|`category`|object|Categoría del producto|0|
|`product`|object|Nombre del producto|0|
|`sales_rep`|object|Vendedor (texto sucio)|0|
|`units`|float64|Unidades vendidas|35|
|`unit_price`|float64|Precio unitario en COP|20|
|`discount`|float64|Descuento (0.0 – 0.20)|15|

**Métrica derivada** que usaremos en toda la guía:

```python
revenue = units * unit_price * (1 - discount)
```

---

## 3. NumPy

NumPy es la capa de abajo. pandas está construido sobre él: cada columna numérica de un DataFrame **es** un array de NumPy. Aprender NumPy no es opcional aunque trabajes solo con pandas.

```python
import numpy as np
```

### 3.1 Por qué NumPy y no listas

Tres razones, en orden de importancia:

1. **Memoria contigua y tipo homogéneo.** Una lista de Python guarda punteros a objetos dispersos en memoria; cada `int` de Python pesa ~28 bytes. Un array `int64` guarda 8 bytes por elemento, uno pegado al otro.
2. **Operaciones vectorizadas.** El bucle ocurre en C, no en Python. No hay overhead de intérprete por elemento.
3. **Sintaxis.** `arr * 2` en vez de `[x * 2 for x in lista]`.

```python
import timeit
pyf = [float(x) for x in range(100_000)]
arr = np.arange(100_000, dtype=np.float64)

a = timeit.timeit(lambda: sum(x * x for x in pyf), number=50) / 50
b = timeit.timeit(lambda: (arr * arr).sum(),       number=50) / 50
print(f"lista: {a*1000:.2f} ms | numpy: {b*1000:.3f} ms | {a/b:.0f}x")
# lista: 2.62 ms | numpy: 0.092 ms | 28x

# y en memoria:
arr.nbytes                    # 800000  (100k * 8 bytes)
# la lista equivalente ronda los 3.6 MB entre punteros y objetos int
```

La regla práctica: **si escribes un `for` sobre un array o una columna, casi siempre hay una forma vectorizada más rápida y más corta.**

---

### 3.2 Creación de arrays

**Sintaxis:** `np.array(secuencia)` · `np.zeros(n)` · `np.arange(inicio, fin, paso)` · `np.linspace(inicio, fin, n)`

**Qué hace:** construye un array de tipo homogéneo, ya sea a partir de datos existentes o generado.

```python
np.array([12, 5, 30, 8])
# array([12,  5, 30,  8])

np.zeros(3)                  # relleno inicial
# array([0., 0., 0.])

np.ones((2, 3))              # forma como tupla -> matriz 2x3
# array([[1., 1., 1.],
#        [1., 1., 1.]])

np.full((2, 2), 7)
# array([[7, 7],
#        [7, 7]])

np.arange(0, 10, 2)          # como range(), pero array. FIN EXCLUIDO
# array([0, 2, 4, 6, 8])

np.linspace(0, 1, 5)         # n puntos equiespaciados. FIN INCLUIDO
# array([0.  , 0.25, 0.5 , 0.75, 1.  ])

rng = np.random.default_rng(42)
rng.integers(1, 40, 5)       # enteros aleatorios reproducibles
# array([28, 21, 39, 31,  1])
```

**Cuándo usarlo:** `np.array` para convertir datos que ya tienes; `zeros`/`full` para preasignar un resultado; `arange` para índices; `linspace` para ejes de gráficos.

> **Aleatoriedad.** `np.random.default_rng(semilla)` **`[RECOMENDADA]`** — API moderna, generador aislado, reproducible. `np.random.seed(42)` + `np.random.rand(...)` **`[LA VERÁS EN CÓDIGO AJENO]`** — API legacy con estado global compartido; si otra librería la toca, tus resultados cambian.

---

### 3.3 Atributos y tipos (dtype)

**Sintaxis:** `arr.shape` · `arr.dtype` · `arr.astype(tipo)`

**Qué hace:** describe la forma y el tipo del array, y permite convertirlo.

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
a.shape       # (2, 3)   filas, columnas
a.ndim        # 2        número de dimensiones
a.size        # 6        total de elementos
a.dtype       # dtype('int64')
a.itemsize    # 8        bytes por elemento

np.array([1, 2, 3], dtype="float64")
# array([1., 2., 3.])

np.array([1.7, 2.3]).astype("int64")   # TRUNCA, no redondea
# array([1, 2])

np.array([1, 2, "x"]).dtype            # un solo str contamina todo el array
# dtype('<U21')   -> texto Unicode
```

**Cuándo usarlo:** `shape` es lo primero que se revisa cuando algo falla. `astype` al cargar datos que vienen como texto. Si un cálculo numérico da error, revisa `dtype` antes que nada.

Tipos que importan: `int64`, `float64`, `bool`, `datetime64[ns]`, `<U…` (texto). **NaN solo existe en `float`**: un array de enteros no puede tener nulos.

---

### 3.4 Indexación y slicing

**Sintaxis:** `arr[i]` · `arr[inicio:fin:paso]` · `arr[fila, columna]`

**Qué hace:** extrae elementos o subconjuntos. En 2D, la coma separa las dimensiones.

```python
units = np.array([28, 21, 39, 31, 1, 14, 35, 9])

units[0]        # 28
units[-1]       # 9      último
units[1:4]      # array([21, 39, 31])   fin excluido
units[:3]       # array([28, 21, 39])
units[::2]      # array([28, 39,  1, 35])   uno de cada dos
units[::-1]     # array([ 9, 35, 14,  1, 31, 39, 21, 28])   invertido

m = np.array([[10, 20, 30],
              [40, 50, 60],
              [70, 80, 90]])

m[1, 2]         # 60                      fila 1, columna 2
m[:, 1]         # array([20, 50, 80])     TODA la columna 1
m[0:2, 1:3]     # array([[20, 30],
                #        [50, 60]])       submatriz
```

**Cuándo usarlo:** constantemente. `m[:, i]` para quedarte con una columna es el patrón más frecuente.

> **Cuidado:** un slice de NumPy es una **vista**, no una copia. Ver [[#8. Errores frecuentes|Errores frecuentes → mutación por referencia]].

---

### 3.5 Máscaras booleanas

**Sintaxis:** `arr[condicion]` · `np.where(condicion, valor_si, valor_no)`

**Qué hace:** una comparación sobre un array devuelve un array de `True`/`False`; usarlo como índice filtra.

```python
units = np.array([28, 21, 39, 31, 1, 14, 35, 9])

units > 20
# array([ True,  True,  True,  True, False, False,  True, False])

units[units > 20]                        # filtrar
# array([28, 21, 39, 31, 35])

units[(units > 10) & (units < 35)]       # AND -> &   OR -> |   NOT -> ~
# array([28, 21, 31, 14])

(units > 20).sum()                       # contar True (True vale 1)
# 5
(units > 20).mean()                      # proporción
# 0.625

np.where(units > 20, "alto", "bajo")     # if/else vectorizado
# array(['alto', 'alto', 'alto', 'alto', 'bajo', 'bajo', 'alto', 'bajo'], dtype='<U4')

np.where(units > 20)                     # con un solo argumento: POSICIONES
# (array([0, 1, 2, 3, 6]),)
```

**Cuándo usarlo:** filtrar es la operación número uno del análisis de datos. `np.where` reemplaza cualquier `if/else` dentro de un bucle.

> **Los paréntesis son obligatorios:** `units > 10 & units < 35` falla, porque `&` tiene mayor precedencia que `>`. Siempre `(a) & (b)`. **Y nunca uses `and`/`or`/`not`** con arrays: `and` intenta evaluar el array completo como un único booleano y lanza `ValueError: The truth value of an array ... is ambiguous`.

---

### 3.6 Broadcasting y operaciones vectorizadas

**Sintaxis:** `arr1 * arr2` · `arr * escalar` · `np.sqrt(arr)`

**Qué hace:** aplica la operación elemento a elemento. Si las formas no coinciden, NumPy "estira" la más pequeña — eso es _broadcasting_.

```python
price = np.array([6580., 11760., 6020., 10570.])
u     = np.array([28., 21., 39., 31.])
disc  = np.array([0.05, 0.10, 0.05, 0.15])

price * u                        # array-array, elemento a elemento
# array([184240., 246960., 234780., 327670.])

price * 1.19                     # array-escalar: el escalar se "estira"
# array([ 7830.2, 13994.4,  7163.8, 12578.3])

np.round(price * u * (1 - disc), 2)      # la fórmula de revenue, vectorizada
# array([175028. , 222264. , 223041. , 278519.5])

np.sqrt(np.array([4., 9., 16.]))   # array([2., 3., 4.])
np.abs(np.array([-3., 2.]))        # array([3., 2.])
np.log(np.array([1., np.e]))       # array([0., 1.])
```

**Regla de broadcasting:** dos formas son compatibles si, comparadas de derecha a izquierda, cada par de dimensiones es igual o una de ellas vale 1.

```python
m = np.array([[10., 20., 30.],
              [40., 50., 60.]])          # (2, 3)
m + np.array([1., 2., 3.])               # (3,) se estira a (2, 3)
# array([[11., 22., 33.],
#        [41., 52., 63.]])

m + np.array([[100.], [200.]])           # (2, 1) se estira a (2, 3)
# array([[110., 120., 130.],
#        [240., 250., 260.]])
```

**Cuándo usarlo:** siempre que ibas a escribir un `for`. Normalizar, aplicar IVA, calcular márgenes, restar la media.

---

### 3.7 Agregaciones por eje

**Sintaxis:** `arr.sum()` · `arr.sum(axis=0)` · `arr.mean(axis=1)`

**Qué hace:** reduce el array a un resumen. `axis` indica **qué eje se colapsa**.

```python
v = np.array([[10., 20., 30.],
              [40., 50., 60.]])

v.sum()          # 210.0     todo
v.mean()         # 35.0
v.std()          # 17.0783
v.min(), v.max() # (10.0, 60.0)
np.median(v)     # 35.0
v.argmax()       # 5         POSICIÓN del máximo (aplanado)

v.sum(axis=0)    # colapsa filas -> un resultado POR COLUMNA
# array([50., 70., 90.])

v.sum(axis=1)    # colapsa columnas -> un resultado POR FILA
# array([ 60., 150.])
```

**La regla mnemotécnica del `axis`:** `axis=0` recorre hacia abajo (a lo largo de las filas) y deja un valor por columna. `axis=1` recorre hacia el lado y deja un valor por fila. Piensa "el eje que desaparece".

**Cuándo usarlo:** totales, promedios, detección de outliers vía `std`, `argmax` para saber _cuál_ es el máximo y no solo cuánto vale.

---

### 3.8 Manejo de NaN

**Sintaxis:** `np.isnan(arr)` · `np.nanmean(arr)` · `np.nansum(arr)`

**Qué hace:** NaN es contagioso: cualquier operación aritmética con un NaN da NaN. Las funciones `nan*` lo ignoran.

```python
x = np.array([28., np.nan, 39., 31.])

x.sum()          # nan     <- contagio
x.mean()         # nan

np.nansum(x)     # 98.0
np.nanmean(x)    # 32.666666666666664
np.nanmax(x)     # 39.0

np.isnan(x)      # array([False,  True, False, False])
x[~np.isnan(x)]  # array([28., 39., 31.])   quitar nulos

np.nan == np.nan # False  <- NUNCA compares con ==, usa np.isnan()
```

**Cuándo usarlo:** en cuanto un cálculo da `nan` inesperadamente. En pandas esto se maneja distinto (las agregaciones ignoran NaN por defecto), pero en NumPy crudo hay que ser explícito.

---

### 3.9 Reshape y concatenación

**Sintaxis:** `arr.reshape(filas, cols)` · `np.concatenate([a, b])` · `arr.T`

**Qué hace:** cambia la forma sin cambiar los datos, o pega arrays.

```python
b = np.arange(12)

b.reshape(3, 4)
# array([[ 0,  1,  2,  3],
#        [ 4,  5,  6,  7],
#        [ 8,  9, 10, 11]])

b.reshape(3, -1).shape    # -1 = "calcula tú esta dimensión"
# (3, 4)

b.reshape(3, 4).T         # transpuesta: filas <-> columnas
# array([[ 0,  4,  8],
#        [ 1,  5,  9],
#        [ 2,  6, 10],
#        [ 3,  7, 11]])

b.reshape(3, 4).ravel()   # aplanar a 1D
# array([ 0,  1, ..., 11])
```

```python
p = np.array([1, 2, 3]); q = np.array([4, 5, 6])

np.concatenate([p, q])    # array([1, 2, 3, 4, 5, 6])       [RECOMENDADA] - genérica, controla el axis
np.vstack([p, q])         # apila como filas
# array([[1, 2, 3],
#        [4, 5, 6]])
np.column_stack([p, q])   # apila como columnas
# array([[1, 4],
#        [2, 5],
#        [3, 6]])
np.hstack([p, q])         # array([1, 2, 3, 4, 5, 6])       [LA VERÁS EN CÓDIGO AJENO]
```

> `np.concatenate([a, b], axis=0)` **`[RECOMENDADA]`**: es explícita sobre el eje y es la que funciona igual en 1D y 2D. `vstack` / `hstack` / `column_stack` **`[LA VERÁS EN CÓDIGO AJENO]`**: son atajos legibles, pero `hstack` cambia de significado según las dimensiones y es fuente de bugs silenciosos.

**Otras dos que vale la pena tener a mano:**

```python
c = np.array(["Online", "Tienda", "Online", "Mayorista"])
np.unique(c)                     # array(['Mayorista', 'Online', 'Tienda'], dtype='<U9')
np.unique(c, return_counts=True) # (array([...]), array([1, 2, 1]))

units = np.array([28, 21, 39, 31, 1, 14, 35, 9])
np.sort(units)     # array([ 1,  9, 14, 21, 28, 31, 35, 39])   copia ordenada
np.argsort(units)  # array([4, 7, 5, 1, 0, 3, 6, 2])           posiciones que ordenarían
```

---

### 3.10 Tabla resumen NumPy

|Método|Qué hace|Ejemplo|
|---|---|---|
|`np.array(x)`|Crea array desde secuencia|`np.array([1, 2, 3])`|
|`np.arange(a, b, p)`|Rango como array (fin excluido)|`np.arange(0, 10, 2)`|
|`np.linspace(a, b, n)`|n puntos equiespaciados (fin incluido)|`np.linspace(0, 1, 5)`|
|`np.zeros/ones/full`|Array preasignado|`np.zeros((2, 3))`|
|`.shape`|Forma (filas, columnas)|`a.shape`|
|`.dtype`|Tipo de los elementos|`a.dtype`|
|`.astype(t)`|Convierte de tipo|`a.astype("int64")`|
|`a[i:j]`|Slicing (devuelve **vista**)|`units[1:4]`|
|`a[cond]`|Filtro booleano|`units[units > 20]`|
|`np.where(c, x, y)`|if/else vectorizado|`np.where(u > 20, "alto", "bajo")`|
|`a * b`, `a + 1`|Operación elemento a elemento|`price * units`|
|`.sum(axis=0/1)`|Suma colapsando un eje|`v.sum(axis=0)`|
|`.mean/.std/.min/.max`|Agregaciones|`v.mean()`|
|`.argmax()/.argmin()`|Posición del extremo|`v.argmax()`|
|`np.isnan(a)`|Máscara de nulos|`a[~np.isnan(a)]`|
|`np.nanmean/nansum`|Agrega ignorando NaN|`np.nanmean(x)`|
|`.reshape(f, c)`|Cambia forma (`-1` = auto)|`b.reshape(3, -1)`|
|`.T`|Transpuesta|`m.T`|
|`.ravel()`|Aplana a 1D|`m.ravel()`|
|`np.concatenate([a, b])`|Pega arrays|`np.concatenate([p, q])`|
|`np.unique(a)`|Valores únicos (+ conteos)|`np.unique(c, return_counts=True)`|
|`np.sort/argsort`|Ordena / posiciones de orden|`np.argsort(units)`|
|`.copy()`|Copia real, rompe la vista|`a[:2].copy()`|

---

## 4. pandas

```python
import pandas as pd
```

### 4.1 Series y DataFrame

Dos estructuras, y la primera es un caso particular de la segunda.

- **Series**: un array 1D **con índice**. Es una columna.
- **DataFrame**: un diccionario de Series que comparten el mismo índice. Es una tabla.

```python
ser = pd.Series([28, 21, 39], index=["a", "b", "c"], name="units")
print(ser)
# a    28
# b    21
# c    39
# Name: units, dtype: int64

ser.values          # array([28, 21, 39])   <- el array de NumPy que hay debajo
ser.index.tolist()  # ['a', 'b', 'c']
ser.dtype           # dtype('int64')

ventas["units"]     # seleccionar una columna devuelve una Series
type(ventas["units"])       # <class 'pandas.core.series.Series'>
type(ventas[["units"]])     # <class 'pandas.core.frame.DataFrame'>  <- doble corchete
```

**El índice no es una columna.** Es la etiqueta de cada fila y es lo que pandas usa para alinear operaciones entre objetos. Ese alineamiento automático es la diferencia de fondo con NumPy: `serie_a + serie_b` suma por etiqueta coincidente, no por posición.

**Crear un DataFrame desde cero:**

```python
metas = pd.DataFrame({                       # [RECOMENDADA] dict de columnas
    "city": ["Barranquilla", "Bogota"],
    "meta": [20_000_000, 18_000_000],
})

pd.DataFrame([                               # [LA VERÁS EN CÓDIGO AJENO] lista de dicts
    {"city": "Barranquilla", "meta": 20_000_000},
    {"city": "Bogota",       "meta": 18_000_000},
])
```

> El dict de columnas es más rápido y deja explícito el tipo de cada columna. La lista de dicts aparece cuando los datos vienen de una API JSON — en ese caso es lo natural.

---

### 4.2 Lectura y escritura

**Sintaxis:** `pd.read_csv(ruta, **opciones)` · `df.to_csv(ruta, index=False)`

**Qué hace:** carga y persiste tablas desde y hacia archivos o bases de datos.

```python
# --- CSV: el 80% de los casos ---
ventas.to_csv("ventas.csv", index=False)      # index=False evita una columna basura "Unnamed: 0"

df = pd.read_csv(
    "ventas.csv",
    parse_dates=["order_date"],   # sin esto, las fechas llegan como texto
    dtype={"order_id": "int64"},
    sep=",",                      # en exportaciones de Excel en español suele ser ";"
    encoding="utf-8",             # si falla: "latin-1"
    na_values=["", "NA", "N/A", "null", "-"],  # qué texto considerar nulo
)
df.shape
# (508, 10)

# --- Excel ---
ventas.to_excel("ventas.xlsx", index=False, sheet_name="ventas")
pd.read_excel("ventas.xlsx", sheet_name="ventas")     # requiere: pip install openpyxl

# --- JSON ---
ventas.to_json("ventas.json", orient="records", date_format="iso")
pd.read_json("ventas.json")

# --- SQL (detalle en la sección 5) ---
from sqlalchemy import create_engine
engine = create_engine("postgresql+psycopg2://user:pass@localhost:5432/midb")
ventas.to_sql("ventas", engine, if_exists="replace", index=False)
pd.read_sql("SELECT * FROM ventas", engine)

# --- Parquet: para datasets grandes ---
ventas.to_parquet("ventas.parquet")     # conserva dtypes, pesa ~5x menos que CSV
pd.read_parquet("ventas.parquet")
```

**Cuándo usarlo:** `read_csv` con `parse_dates` es el arranque de casi todo pipeline. Guarda resultados intermedios en Parquet, no en CSV: el CSV pierde los tipos y hay que re-parsear fechas cada vez.

> **Argumento crítico:** `index=False` al escribir. Sin él, cada round-trip CSV agrega una columna de índice y a la tercera vuelta tienes `Unnamed: 0`, `Unnamed: 0.1`, `Unnamed: 0.2`.

---

### 4.3 Inspección

**Sintaxis:** `df.head()` · `df.info()` · `df.describe()` · `df.shape`

**Qué hace:** el ritual obligatorio antes de tocar cualquier dataset.

```python
ventas.shape
# (508, 10)

ventas.head(3)
#    order_id order_date          city    channel category      product     sales_rep  units  unit_price  discount
# 0      1001 2024-10-15  Barranquilla     Tienda     Aseo      Shampoo   CARLOS DIAZ   28.0      6580.0      0.05
# 1      1002 2024-05-21        Bogota  Mayorista   Snacks     Galletas  PEDRO TORRES   21.0     11760.0      0.10
# 2      1003 2024-05-04  Barranquilla     Online  Lacteos  Yogurt 200g    ana gomez    39.0      6020.0      0.05

ventas.info()
# <class 'pandas.core.frame.DataFrame'>
# RangeIndex: 508 entries, 0 to 507
# Data columns (total 10 columns):
#  #   Column      Non-Null Count  Dtype
# ---  ------      --------------  -----
#  0   order_id    508 non-null    int64
#  1   order_date  508 non-null    datetime64[ns]
#  2   city        498 non-null    object
#  3   channel     508 non-null    object
#  4   category    508 non-null    object
#  5   product     508 non-null    object
#  6   sales_rep   508 non-null    object
#  7   units       473 non-null    float64
#  8   unit_price  488 non-null    float64
#  9   discount    493 non-null    float64
# dtypes: datetime64[ns](1), float64(3), int64(1), object(5)
# memory usage: 39.8+ KB
```

`info()` es la vista más densa que existe: en una pantalla te da número de filas, nombres, **nulos** y **tipos**. Es lo segundo que se ejecuta, siempre.

```python
ventas.describe().round(2)          # solo columnas numéricas
#        order_id                     order_date   units  unit_price  discount
# count    508.00                            508  473.00      488.00    493.00
# mean    1250.21  2024-09-20 22:57:38.267716608   20.10     8919.32      0.05
# std      144.62                            NaN   11.43     4440.13      0.06
# min     1001.00            2024-01-01 00:00:00    1.00     1020.00      0.00
# 25%     1125.75            2024-05-05 18:00:00   10.00     5695.00      0.00
# 50%     1250.00            2024-09-15 00:00:00   21.00     8250.00      0.00
# 75%     1375.25            2025-02-08 00:00:00   30.00    11412.50      0.10
# max     1500.00            2025-06-26 00:00:00   39.00    29040.00      0.20

ventas.describe(include="object")   # columnas de texto
#                 city channel category       product    sales_rep
# count            498     508      508           508          508
# unique             5       3        4            12            6
# top     Barranquilla  Online  Bebidas  Gaseosa 1.5L  luisa Perez
# freq             144     225      180            74          102
```

```python
ventas.columns.tolist()
# ['order_id', 'order_date', 'city', 'channel', 'category', 'product', 'sales_rep', 'units', 'unit_price', 'discount']

ventas.nunique()          # cardinalidad de cada columna
# order_id      500        <- ojo: 508 filas pero 500 ids -> hay duplicados
# order_date    328
# city            5
# channel         3
# category        4
# product        12
# sales_rep       6
# units          39
# unit_price    410
# discount        5

ventas["city"].value_counts()
# city
# Barranquilla    144
# Bogota          133
# Medellin         89
# Cali             75
# Cartagena        57

ventas["city"].value_counts(dropna=False)   # incluye los nulos
# Barranquilla    144
# ...
# None             10

ventas["channel"].value_counts(normalize=True).round(3)   # proporciones
# Online       0.443
# Tienda       0.348
# Mayorista    0.209
```

**Cuándo usarlo:** el ritual completo, en este orden, para cada dataset nuevo:

```python
df.shape          # ¿cuántas filas y columnas?
df.head()         # ¿cómo se ven los datos?
df.info()         # ¿tipos correctos? ¿cuántos nulos?
df.describe()     # ¿rangos razonables? ¿outliers evidentes?
df.nunique()      # ¿qué es categórico y qué es identificador?
```

---

### 4.4 Selección: loc e iloc

**Sintaxis:** `df.loc[filas, columnas]` (por **etiqueta**) · `df.iloc[filas, columnas]` (por **posición**)

**Qué hace:** extrae subconjuntos. Es la distinción que más confusión causa en pandas.

||`loc`|`iloc`|
|---|---|---|
|Índice de filas|Etiqueta del índice|Posición entera (0, 1, 2…)|
|Columnas|Nombre de la columna|Posición entera|
|Rango `a:b`|**b incluido**|**b excluido**|
|Acepta booleanos|Sí|No (usa arrays de numpy)|

```python
ventas["city"].head(3)             # una columna -> Series
# 0    Barranquilla
# 1          Bogota
# 2    Barranquilla

ventas[["city", "units"]].head(3)  # varias columnas -> DataFrame
#            city  units
# 0  Barranquilla   28.0
# 1        Bogota   21.0
# 2  Barranquilla   39.0

# --- loc: etiquetas ---
ventas.loc[0, "city"]                   # 'Barranquilla'
ventas.loc[0:2, ["city", "units"]]      # 0, 1 Y 2  <- el fin SÍ se incluye
#            city  units
# 0  Barranquilla   28.0
# 1        Bogota   21.0
# 2  Barranquilla   39.0

# --- iloc: posiciones ---
ventas.iloc[0, 2]        # 'Barranquilla'   fila 0, columna 2
ventas.iloc[0:2, 2:4]    # 0 y 1  <- el fin NO se incluye
#            city    channel
# 0  Barranquilla     Tienda
# 1        Bogota  Mayorista

ventas.iloc[-1]["order_id"]   # 1218   última fila
```

**Cuándo usarlo:**

- **`.loc` en el 95% de los casos.** Referirse a columnas por nombre es legible y no se rompe si cambia el orden de las columnas.
- **`.iloc`** solo cuando la posición es lo que importa: "las primeras 10 filas", "la última columna", muestreo por posición.

> **Nunca uses el índice implícito para asignar.** `ventas["revenue"][0] = 5` es _chained indexing_ y puede escribir sobre una copia temporal. Usa siempre `ventas.loc[0, "revenue"] = 5`. Ver [[#8. Errores frecuentes|Errores frecuentes]].

---

### 4.5 Filtrado booleano

**Sintaxis:** `df[condicion]` · `df[(cond1) & (cond2)]` · `df["col"].isin(lista)`

**Qué hace:** conserva las filas donde la condición es `True`.

```python
ventas[ventas["units"] > 35].shape
# (47, 10)

# AND -> &     OR -> |     NOT -> ~     paréntesis OBLIGATORIOS
ventas[(ventas["city"] == "Barranquilla") & (ventas["units"] > 35)].shape
# (12, 10)

ventas[ventas["city"].isin(["Cali", "Cartagena"])].shape     # pertenencia a un conjunto
# (132, 10)

ventas[~ventas["city"].isin(["Cali", "Cartagena"])].shape    # negación
# (376, 10)

ventas[ventas["units"].between(10, 20)].shape                # rango, ambos extremos incluidos
# (122, 10)

ventas[ventas["units"].notna()].shape                        # sin nulos en esa columna
# (473, 10)
```

**Dos formas de escribir lo mismo:**

```python
# [RECOMENDADA] máscaras booleanas
ventas[(ventas["units"] > 35) & (ventas["city"] == "Barranquilla")]

# [LA VERÁS EN CÓDIGO AJENO] query()
ventas.query("units > 35 and city == 'Barranquilla'")
# (12, 10)   mismo resultado
```

> `query()` es más corta y legible con condiciones largas, pero es un mini-lenguaje aparte: no autocompleta, falla con nombres de columna que tengan espacios, y no permite expresiones arbitrarias de Python sin el prefijo `@`. Las máscaras booleanas son Python normal y componen mejor. Usa `query()` para exploración interactiva, máscaras para código de producción.

---

### 4.6 Valores nulos

**Sintaxis:** `df.isna()` · `df.dropna()` · `df.fillna(valor)`

**Qué hace:** detecta, elimina o rellena datos faltantes.

```python
ventas.isna().sum()                 # nulos POR COLUMNA  <- el diagnóstico estándar
# order_id       0
# order_date     0
# city          10
# channel        0
# category       0
# product        0
# sales_rep      0
# units         35
# unit_price    20
# discount      15

ventas.isna().sum().sum()           # total de celdas nulas
# 80

ventas["units"].isna().mean().round(4)   # PROPORCIÓN de nulos -> lo que realmente decide
# 0.0689     (6.9%)

ventas[ventas["units"].isna()].head(2)   # inspeccionar las filas con nulos
```

```python
# --- eliminar ---
ventas.dropna().shape                     # cualquier nulo en cualquier columna
# (434, 10)     <- se perdieron 74 filas, 15% del dataset
ventas.dropna(subset=["units"]).shape     # [RECOMENDADA] solo donde importa
# (473, 10)
ventas.dropna(axis=1, thresh=500).shape   # columnas con al menos 500 no-nulos

# --- rellenar ---
ventas["units"].fillna(0)                          # constante
ventas["units"].fillna(ventas["units"].median())   # mediana: robusta a outliers
ventas["units"].fillna(ventas["units"].mean())     # media: la sesga cualquier outlier
ventas["city"].fillna("Desconocida")               # categórica -> etiqueta explícita
ventas["discount"].fillna(0)                       # aquí el nulo SIGNIFICA "sin descuento"

# rellenar con la mediana del grupo: mucho mejor que la mediana global
ventas["units"].fillna(ventas.groupby("category")["units"].transform("median"))
```

**Árbol de decisión práctico:**

|Situación|Qué hacer|
|---|---|
|El nulo tiene un significado (`discount` vacío = 0%)|`fillna` con ese significado|
|Columna numérica, pocos nulos (<5%)|`fillna` con la mediana|
|Columna categórica|`fillna("Desconocida")` — no inventes una categoría real|
|La fila no sirve sin ese dato (`revenue` sin `units`)|`dropna(subset=[...])`|
|Más del 40% de la columna es nula|Considera descartar la columna|

> **Nunca `df.dropna()` a secas** como primer paso. Borra filas por nulos en columnas que quizá ni ibas a usar. Aquí cuesta 74 filas de 508.
> 
> **`inplace=True`** (`df.dropna(inplace=True)`) **`[LA VERÁS EN CÓDIGO AJENO]`**: no ahorra memoria, rompe el encadenamiento y está en camino a desaparecer. Escribe `df = df.dropna(...)` **`[RECOMENDADA]`**.

---

### 4.7 Tipos de datos y conversión

**Sintaxis:** `df.dtypes` · `df["c"].astype(t)` · `pd.to_numeric(s, errors="coerce")`

**Qué hace:** consulta y corrige los tipos. Un tipo mal inferido es la causa raíz de la mitad de los errores en pandas.

```python
ventas.dtypes
# order_id               int64
# order_date    datetime64[ns]
# city                  object      <- "object" = casi siempre texto
# units                float64      <- float, no int, PORQUE tiene nulos
# ...

# --- a entero (solo si no hay nulos) ---
tmp = ventas.copy()
tmp["units"] = tmp["units"].fillna(0).astype("int64")
tmp["units"].dtype
# dtype('int64')

# entero que SÍ admite nulos (nullable integer, con I mayúscula)
ventas["units"].astype("Int64").head(2)
# 0    28
# 1    21
# dtype: Int64

# --- texto a número, tolerante a basura ---
pd.to_numeric(pd.Series(["10", "20", "x"]), errors="coerce")
# 0    10.0
# 1    20.0
# 2     NaN      <- "coerce" convierte lo no convertible en NaN en vez de reventar

# --- texto a fecha ---
pd.to_datetime(ventas["order_date"], errors="coerce")
pd.to_datetime(pd.Series(["15/10/2024"]), format="%d/%m/%Y")   # formato colombiano

# --- categórica: ahorro de memoria brutal en columnas de baja cardinalidad ---
ventas["category"].memory_usage(deep=True)                      # 28130 bytes
ventas["category"].astype("category").memory_usage(deep=True)   #  1032 bytes  (27x menos)
```

**Cuándo usarlo:** después de cada `read_csv`, revisa `dtypes`. Si una columna que debería ser numérica aparece como `object`, hay texto escondido en ella (`"1.234,50"`, `"N/D"`, un espacio).

```python
# diagnóstico: ¿qué valores impiden la conversión?
mal = pd.to_numeric(ventas["units"], errors="coerce").isna() & ventas["units"].notna()
ventas.loc[mal, "units"].unique()
```

---

### 4.8 Columnas derivadas

**Sintaxis:** `df["nueva"] = expresion` · `df.assign(nueva=lambda d: ...)`

**Qué hace:** agrega columnas calculadas.

```python
v = ventas.copy()
v["revenue"] = v["units"] * v["unit_price"] * (1 - v["discount"].fillna(0))
v["revenue"].describe().round(2)
# count        455.00
# mean      166531.84
# std       135174.33
# min         4411.50
# 25%        66322.50
# 50%       140301.00
# 75%       228307.50
# max      1035360.00
```

> Nota que `count` es 455 y no 508: NaN en `units` o `unit_price` propaga NaN a `revenue`. Eso es correcto y deseable — no inventes un revenue que no existe.

**Dos formas:**

```python
# [RECOMENDADA] asignación directa: legible, permite pasos intermedios
v["revenue"] = v["units"] * v["unit_price"] * (1 - v["discount"].fillna(0))
v["margen"]  = v["revenue"] * 0.28

# [LA VERÁS EN CÓDIGO AJENO] assign: encadenable, no muta el original
(ventas
   .assign(revenue=lambda d: d["units"] * d["unit_price"] * (1 - d["discount"].fillna(0)))
   .assign(margen=lambda d: d["revenue"] * 0.28)
   .head())
```

> `assign` brilla dentro de una cadena larga de transformaciones y evita el `SettingWithCopyWarning` por construcción. La asignación directa es más simple para scripts lineales y permite depurar paso a paso. Empieza con asignación directa; adopta `assign` cuando ya estés encadenando.

---

### 4.9 groupby y agregaciones

**Sintaxis:** `df.groupby("col")["valor"].agg(funcion)`

**Qué hace:** parte el DataFrame en grupos, aplica una función a cada uno y combina los resultados (_split-apply-combine_). Es la operación central del análisis de datos.

```python
v = ventas.copy()
v["revenue"] = v["units"] * v["unit_price"] * (1 - v["discount"].fillna(0))
v = v.dropna(subset=["revenue", "city"])

# --- una columna, una función ---
v.groupby("city")["revenue"].sum().round(0)
# city
# Barranquilla    20804205.0
# Bogota          18250792.0
# Cali            11505868.0
# Cartagena        9746924.0
# Medellin        14329514.0

# --- una columna, varias funciones ---
v.groupby("city")["revenue"].agg(["sum", "mean", "count"]).round(1)
#                      sum      mean  count
# city
# Barranquilla  20804205.0  162532.9    128
# Bogota        18250792.0  148380.4    123
# Cali          11505867.5  174331.3     66
# Cartagena      9746923.5  180498.6     54
# Medellin      14329514.5  191060.2     75

# --- [RECOMENDADA] named aggregation: nombres de salida controlados ---
v.groupby("city").agg(
    total   = ("revenue", "sum"),
    pedidos = ("order_id", "count"),
    ticket  = ("revenue", "mean"),
).round(1)
#                    total  pedidos    ticket
# city
# Barranquilla  20804205.0      128  162532.9
# Bogota        18250792.0      123  148380.4
# Cali          11505867.5       66  174331.3
# Cartagena      9746923.5       54  180498.6
# Medellin      14329514.5       75  191060.2

# --- agrupar por varias columnas -> índice jerárquico ---
v.groupby(["city", "channel"])["revenue"].sum().round(0).head(8)
# city          channel
# Barranquilla  Mayorista    4253796.0
#               Online       9307343.0
#               Tienda       7243066.0
# Bogota        Mayorista    3421936.0
#               Online       7140872.0
#               Tienda       7687983.0
# Cali          Mayorista    2896862.0
#               Online       4953978.0
```

> **Named aggregation** `agg(nombre=("columna", "funcion"))` **`[RECOMENDADA]`**: las columnas de salida salen con el nombre que tú decides y planas, listas para graficar o exportar. `agg(["sum", "mean"])` **`[LA VERÁS EN CÓDIGO AJENO]`**: produce columnas MultiIndex que después toca aplanar a mano. `agg({"revenue": "sum", "units": "mean"})` **`[LA VERÁS EN CÓDIGO AJENO]`**: dict de columna→función; funciona, pero no puedes aplicar dos funciones distintas a la misma columna con nombres claros.

**`as_index=False`** — para que el resultado sea una tabla plana en vez de tener la clave en el índice:

```python
v.groupby("city", as_index=False)["revenue"].sum().round(0)
#            city     revenue
# 0  Barranquilla  20804205.0
# 1        Bogota  18250792.0
# 2          Cali  11505868.0
# 3     Cartagena   9746924.0
# 4      Medellin  14329514.0
```

Úsalo siempre que vayas a hacer `merge` o graficar el resultado. Alternativa equivalente: `.reset_index()` al final.

**`transform`** — devuelve un resultado del **mismo tamaño** que el original, no uno por grupo. Es lo que se usa para comparar cada fila contra su grupo:

```python
v["ticket_ciudad"] = v.groupby("city")["revenue"].transform("mean")
v["vs_ciudad"]     = (v["revenue"] / v["ticket_ciudad"]).round(2)
v[["city", "revenue", "ticket_ciudad"]].head(3).round(1)
#            city   revenue  ticket_ciudad
# 0  Barranquilla  175028.0       162532.9
# 1        Bogota  222264.0       148380.4
# 2  Barranquilla  223041.0       162532.9
```

|Necesito…|Uso|
|---|---|
|Una fila por grupo|`agg`|
|Una columna nueva alineada al DataFrame original|`transform`|
|Quedarme con las filas que cumplen algo a nivel de grupo|`filter`|

**Cuándo usarlo:** casi cualquier pregunta de negocio ("ventas por ciudad", "ticket promedio por canal", "top vendedores del trimestre") es un `groupby`.

---

### 4.10 Ordenamiento

**Sintaxis:** `df.sort_values("col", ascending=False)` · `df.nlargest(n, "col")`

```python
v.sort_values("revenue", ascending=False)[["order_id", "city", "revenue"]].head(3).round(1)
#      order_id          city    revenue
# 349      1350  Barranquilla  1035360.0
# 193      1194     Cartagena   727974.0
# 103      1104          Cali   687900.0

v.nlargest(3, "revenue")[["order_id", "revenue"]].round(1)   # más corto y más rápido para top-N
#      order_id    revenue
# 349      1350  1035360.0
# 193      1194   727974.0
# 103      1104   687900.0

# multi-clave con direcciones distintas
v.sort_values(["city", "revenue"], ascending=[True, False])[["city", "revenue"]].head(3).round(1)
#              city    revenue
# 349  Barranquilla  1035360.0
# 234  Barranquilla   624360.0
# 96   Barranquilla   489060.0

v.sort_index()                                # ordenar por índice
v.sort_values("revenue", na_position="first") # dónde van los NaN (por defecto: al final)
```

> Para un top-N, **`nlargest(n, col)`** **`[RECOMENDADA]`** — expresa la intención y no ordena todo el DataFrame. `sort_values(...).head(n)` **`[LA VERÁS EN CÓDIGO AJENO]`** — mismo resultado, ordena las 500 filas para quedarse con 3.

---

### 4.11 merge, join y concat

**Sintaxis:** `pd.merge(izq, der, on="clave", how="left")` · `df.merge(...)` · `pd.concat([a, b])`

**Qué hace:** `merge` cruza tablas por una clave (equivale al `JOIN` de SQL). `concat` apila tablas.

```python
metas = pd.DataFrame({
    "city":   ["Barranquilla", "Bogota", "Medellin", "Cali", "Santa Marta"],
    "meta":   [20_000_000, 18_000_000, 14_000_000, 11_000_000, 5_000_000],
    "region": ["Caribe", "Andina", "Andina", "Pacifico", "Caribe"],
})

res = v.groupby("city", as_index=False)["revenue"].sum().round(0)
#            city     revenue
# 0  Barranquilla  20804205.0
# 1        Bogota  18250792.0
# 2          Cali  11505868.0
# 3     Cartagena   9746924.0      <- no tiene meta
# 4      Medellin  14329514.0
#                                  <- Santa Marta tiene meta pero no ventas
```

**Los cuatro `how`:**

```python
res.merge(metas, on="city", how="inner")    # solo lo que está en AMBAS
#            city     revenue      meta    region
# 0  Barranquilla  20804205.0  20000000    Caribe
# 1        Bogota  18250792.0  18000000    Andina
# 2          Cali  11505868.0  11000000  Pacifico
# 3      Medellin  14329514.0  14000000    Andina
# (4 filas: se pierden Cartagena y Santa Marta)

res.merge(metas, on="city", how="left")     # [RECOMENDADA] todo lo de la izquierda
#            city     revenue        meta    region
# 0  Barranquilla  20804205.0  20000000.0    Caribe
# 1        Bogota  18250792.0  18000000.0    Andina
# 2          Cali  11505868.0  11000000.0  Pacifico
# 3     Cartagena   9746924.0         NaN       NaN    <- sin meta -> NaN
# 4      Medellin  14329514.0  14000000.0    Andina
# (5 filas: se conservan TODAS las ventas)

res.merge(metas, on="city", how="outer")    # todo de ambos lados
#            city     revenue        meta    region
# ...
# 5   Santa Marta         NaN   5000000.0    Caribe   <- meta sin ventas

res.merge(metas, on="city", how="right")    # todo lo de la derecha (raro; invierte el orden y usa left)
```

**Regla:** en un pipeline usa **`how="left"`** por defecto, con el hecho a la izquierda y la dimensión a la derecha. Es el único `how` que garantiza que **no pierdes filas del dataset principal**, y los NaN que aparecen te avisan de claves sin correspondencia.

```python
m = res.merge(metas, on="city", how="left")
m["cumplimiento"] = (m["revenue"] / m["meta"]).round(2)
m
#            city     revenue        meta    region  cumplimiento
# 0  Barranquilla  20804205.0  20000000.0    Caribe          1.04
# 1        Bogota  18250792.0  18000000.0    Andina          1.01
# 2          Cali  11505868.0  11000000.0  Pacifico          1.05
# 3     Cartagena   9746924.0         NaN       NaN           NaN
# 4      Medellin  14329514.0  14000000.0    Andina          1.02
```

**`indicator=True`** — auditoría del merge, tu mejor amigo cuando algo no cuadra:

```python
res.merge(metas, on="city", how="left", indicator=True)["_merge"].value_counts()
# _merge
# both          4
# left_only     1      <- Cartagena no encontró meta
# right_only    0
```

**`validate`** — falla explícitamente si la cardinalidad no es la que esperas:

```python
res.merge(metas, on="city", how="left", validate="one_to_one")
# opciones: "one_to_one", "one_to_many", "many_to_one", "many_to_many"
# si hay duplicados en la clave, lanza MergeError en vez de multiplicar filas en silencio
```

**Nombres de clave distintos y sufijos:**

```python
res.merge(metas, left_on="city", right_on="city", how="left")
res.merge(metas, on="city", suffixes=("_real", "_meta"))   # para columnas homónimas
```

**`join` — merge por índice `[LA VERÁS EN CÓDIGO AJENO]`:**

```python
res.set_index("city").join(metas.set_index("city"), how="left").head(2)
#                  revenue        meta  region
# city
# Barranquilla  20804205.0  20000000.0  Caribe
# Bogota        18250792.0  18000000.0  Andina
```

> `merge` **`[RECOMENDADA]`** siempre. `join` solo ahorra escribir si ambos DataFrames ya tienen el índice puesto en la clave, cosa que rara vez ocurre; y su `how` por defecto es `"left"`, distinto del `"inner"` de `merge`, lo cual confunde.

**`concat` — apilar:**

```python
pd.concat([res.head(2), res.tail(2)], ignore_index=True)   # apilar filas (axis=0, por defecto)
#            city     revenue
# 0  Barranquilla  20804205.0
# 1        Bogota  18250792.0
# 2     Cartagena   9746924.0
# 3      Medellin  14329514.0

pd.concat([df_a, df_b], axis=1)   # pegar columnas: alinea por ÍNDICE, no por posición
```

> `ignore_index=True` al apilar filas: sin él arrastras índices repetidos (0,1,0,1) que rompen `.loc`.

|Operación|Herramienta|
|---|---|
|Cruzar por clave (JOIN de SQL)|`merge`|
|Apilar filas del mismo esquema (UNION)|`concat(axis=0)`|
|Pegar columnas alineando por índice|`concat(axis=1)`|

---

### 4.12 Fechas

**Sintaxis:** `pd.to_datetime(s)` · `s.dt.year` · `df.resample("ME")`

**Qué hace:** el accesor `.dt` da acceso a los componentes de una columna `datetime64`.

```python
v["order_date"] = pd.to_datetime(v["order_date"])   # obligatorio antes de usar .dt

v["year"]       = v["order_date"].dt.year
v["month"]      = v["order_date"].dt.month
v["month_name"] = v["order_date"].dt.month_name()
v["dow"]        = v["order_date"].dt.day_name()
v["quarter"]    = v["order_date"].dt.quarter

v[["order_date", "year", "month", "dow"]].head(3)
#   order_date  year  month       dow
# 0 2024-10-15  2024     10   Tuesday
# 1 2024-05-21  2024      5   Tuesday
# 2 2024-05-04  2024      5  Saturday
```

```python
# --- filtrar por rango de fechas ---
v[v["order_date"].between("2024-01-01", "2024-01-31")].shape
# (25, 15)

# --- serie mensual: dos caminos ---

# [RECOMENDADA] resample: requiere la fecha como índice, produce un DatetimeIndex real
v.set_index("order_date").resample("ME")["revenue"].sum().head(4).round(0)
# order_date
# 2024-01-31    3642415.0
# 2024-02-29    3708786.0
# 2024-03-31    6305190.0
# 2024-04-30    3425668.0
# Freq: ME

# [LA VERÁS EN CÓDIGO AJENO] groupby + to_period
v.groupby(v["order_date"].dt.to_period("M"))["revenue"].sum().head(4).round(0)
# 2024-01    3642415.0
# 2024-02    3708786.0
# ...
```

> `resample` **`[RECOMENDADA]`** para series temporales: rellena los periodos sin datos (`asfreq`), permite ventanas móviles y produce fechas reales que Matplotlib grafica bien. `to_period` **`[LA VERÁS EN CÓDIGO AJENO]`**: devuelve un `Period`, cómodo para etiquetas de texto pero incómodo para graficar y para volver a unir con otras tablas.

**Frecuencias de `resample`:** `"D"` día · `"W"` semana · `"ME"` fin de mes · `"QE"` fin de trimestre · `"YE"` fin de año.

```python
# --- ventana móvil ---
mensual = v.set_index("order_date").resample("ME")["revenue"].sum()
mensual.rolling(3).mean()            # media móvil de 3 meses: suaviza la tendencia
mensual.pct_change().round(3)        # variación porcentual mes a mes
mensual.shift(1)                     # valor del mes anterior (para comparaciones)
```

**Cuándo usarlo:** cualquier análisis con eje temporal — estacionalidad, tendencia, comparación año contra año.

---

### 4.13 pivot_table y melt

**Sintaxis:** `df.pivot_table(index=, columns=, values=, aggfunc=)` · `df.melt(id_vars=)`

**Qué hace:** convierten entre formato **largo** (una fila por observación) y **ancho** (una columna por categoría). Son inversos.

```python
pt = v.pivot_table(index="city", columns="channel", values="revenue", aggfunc="sum").round(0)
pt
# channel       Mayorista     Online     Tienda
# city
# Barranquilla  4253796.0  9307343.0  7243066.0
# Bogota        3421936.0  7140872.0  7687983.0
# Cali          2896862.0  4953978.0  3655028.0
# Cartagena     2701049.0  4478526.0  2567349.0
# Medellin      3286004.0  5559222.0  5484288.0

# con totales y relleno de vacíos
v.pivot_table(index="city", columns="channel", values="revenue",
              aggfunc="sum", margins=True, fill_value=0)
```

```python
# melt: de ancho a largo (lo que necesitan Seaborn y las bases de datos)
pt.reset_index().melt(id_vars="city", var_name="channel", value_name="revenue").head(4).round(0)
#            city    channel    revenue
# 0  Barranquilla  Mayorista  4253796.0
# 1        Bogota  Mayorista  3421936.0
# 2          Cali  Mayorista  2896862.0
# 3     Cartagena  Mayorista  2701049.0
```

```python
# crosstab: conteo de frecuencias cruzadas
pd.crosstab(v["city"], v["channel"])
# channel       Mayorista  Online  Tienda
# city
# Barranquilla         29      60      39
# Bogota               24      51      48
# Cali                 13      31      22
# Cartagena            15      27      12
# Medellin             15      28      32
```

**Cuándo usarlo:** `pivot_table` para reportes que va a leer un humano (Excel-style). `melt` antes de graficar con Seaborn o antes de cargar a una base de datos relacional, que siempre prefiere formato largo.

> **`pivot_table` `[RECOMENDADA]`** frente a **`pivot` `[LA VERÁS EN CÓDIGO AJENO]`**: `pivot` falla con `ValueError: Index contains duplicate entries` si hay más de una fila por combinación, que es lo normal. `pivot_table` agrega. Usa `pivot` solo si estás seguro de que la combinación índice-columna es única.
> 
> **`crosstab` vs `pivot_table`**: `crosstab` es un atajo para contar frecuencias; `pivot_table(aggfunc="count")` hace lo mismo y además permite otras agregaciones.

---

### 4.14 Duplicados

**Sintaxis:** `df.duplicated()` · `df.drop_duplicates(subset=, keep=)`

```python
ventas.duplicated().sum()                        # filas idénticas en TODAS las columnas
# 8

ventas.duplicated(subset=["order_id"]).sum()     # duplicados por clave de negocio
# 8

ventas[ventas.duplicated(subset=["order_id"], keep=False)].sort_values("order_id").head(4)
# keep=False marca TODAS las apariciones, no solo las repetidas -> para inspeccionar

ventas.drop_duplicates().shape
# (500, 10)

ventas.drop_duplicates(subset=["order_id"], keep="last").shape   # se queda con la última
# (500, 10)
```

**Cuándo usarlo:** siempre, después de cualquier `concat` o `merge`, y como parte del checklist de limpieza. `keep="last"` cuando las filas nuevas corrigen a las viejas; `keep="first"` (por defecto) cuando la primera es la fuente de verdad.

> **Chequeo de integridad recomendado** al inicio de cualquier pipeline:
> 
> ```python
> assert ventas["order_id"].is_unique, "order_id duplicado: revisar la extracción"
> ```

---

### 4.15 Texto con el accesor .str

**Sintaxis:** `df["col"].str.metodo()`

**Qué hace:** aplica métodos de string a toda una columna, de forma vectorizada y saltándose los NaN.

```python
ventas["sales_rep"].head(3).tolist()
# ['CARLOS DIAZ', 'PEDRO TORRES', '  ana gomez ']

# --- limpieza estándar, encadenada ---
clean = (ventas["sales_rep"]
           .str.strip()                              # quita espacios de los extremos
           .str.title()                              # Capitaliza Cada Palabra
           .str.replace(r"\s+", " ", regex=True))    # colapsa espacios internos

clean.value_counts()
# sales_rep
# Luisa Perez      102
# Carlos Diaz       96
# Ana Gomez         88
# Pedro Torres      80
# Maria Lopez       73
# Jorge Ramirez     69
```

> Sin la limpieza, `" ana gomez "` y `"ana gomez"` cuentan como dos vendedores distintos. **Normalizar texto antes de agrupar no es opcional.**

```python
ventas["product"].str.contains("ml").sum()          # 53      ¿contiene?
ventas["product"].str.startswith("Agua").sum()      # 41
ventas["product"].str.split().str[0].head(3).tolist()   # primera palabra
# ['Shampoo', 'Galletas', 'Yogurt']

ventas["city"].str.len().head(3).tolist()           # [12.0, 6.0, 12.0]  (float por los NaN)
ventas["city"].str.upper()
ventas["city"].str.lower()

# extraer con regex: el número de un producto tipo "Gaseosa 1.5L"
ventas["product"].str.extract(r"(\d+\.?\d*)")[0].head(3).tolist()
# [nan, nan, '200']

# reemplazo literal vs regex
ventas["product"].str.replace(" ", "_", regex=False)
```

**Cuándo usarlo:** limpieza de nombres, categorización por palabra clave, extracción de códigos, normalización antes de `groupby` o `merge`.

> **Un `.str.strip().str.lower()` sobre las claves antes de un `merge`** evita la mitad de los "el merge no encontró nada".

---

### 4.16 apply frente a operaciones vectorizadas

**Sintaxis:** `df["col"].apply(func)` · `df.apply(func, axis=1)`

**Qué hace:** `apply` ejecuta una función Python fila a fila. Es la salida de emergencia, no la herramienta por defecto.

```python
import timeit
t1 = timeit.timeit(lambda: v["units"] * v["unit_price"], number=100) / 100
t2 = timeit.timeit(lambda: v.apply(lambda r: r["units"] * r["unit_price"], axis=1), number=5) / 5
print(f"vectorizado {t1*1000:.3f} ms | apply axis=1 {t2*1000:.1f} ms | {t2/t1:.0f}x")
# vectorizado 0.041 ms | apply axis=1 2.6 ms | 64x
```

**Jerarquía, de mejor a peor:**

```python
# 1. [RECOMENDADA] operación vectorizada
v["revenue"] = v["units"] * v["unit_price"] * (1 - v["discount"].fillna(0))

# 2. [RECOMENDADA] np.where para condiciones binarias
v["tamano"] = np.where(v["units"] > 20, "alto", "bajo")

# 3. [RECOMENDADA] np.select o pd.cut para múltiples condiciones
v["segmento"] = pd.cut(v["units"], bins=[0, 10, 25, 40], labels=["bajo", "medio", "alto"])
v["segmento"].value_counts()
# units
# medio    174
# alto     166
# bajo     115

condiciones = [v["revenue"] > 300_000, v["revenue"] > 100_000]
opciones    = ["A", "B"]
v["clase"]  = np.select(condiciones, opciones, default="C")

# 4. map: sustitución por diccionario
v["region"] = v["city"].map({"Barranquilla": "Caribe", "Cartagena": "Caribe",
                             "Bogota": "Andina", "Medellin": "Andina", "Cali": "Pacifico"})

# 5. [LA VERÁS EN CÓDIGO AJENO] apply sobre una Series - lento pero tolerable
v["tamano"] = v["units"].apply(lambda x: "alto" if x > 20 else "bajo")

# 6. [ÚLTIMO RECURSO] apply(axis=1) - decenas de veces más lento
v["algo"] = v.apply(lambda r: complejo(r["a"], r["b"]), axis=1)
```

**Cuándo usar `apply(axis=1)` de verdad:** cuando la lógica combina varias columnas de una forma que no se puede expresar vectorizadamente (llamar a una API por fila, parsear un JSON anidado, aplicar reglas de negocio muy ramificadas). En esos casos, tolera el costo — pero primero pregúntate si `np.select` no lo resuelve.

---

### 4.17 Tabla resumen pandas

|Método|Qué hace|Ejemplo|
|---|---|---|
|**Lectura / escritura**|||
|`pd.read_csv(f)`|Carga CSV|`pd.read_csv("v.csv", parse_dates=["order_date"])`|
|`df.to_csv(f)`|Escribe CSV|`df.to_csv("out.csv", index=False)`|
|`pd.read_sql(q, eng)`|Consulta SQL a DataFrame|`pd.read_sql("SELECT * FROM ventas", eng)`|
|`df.to_sql(t, eng)`|DataFrame a tabla SQL|`df.to_sql("ventas", eng, if_exists="replace", index=False)`|
|**Inspección**|||
|`df.head(n)`|Primeras n filas|`ventas.head(3)`|
|`df.shape`|(filas, columnas)|`ventas.shape`|
|`df.info()`|Tipos + nulos + memoria|`ventas.info()`|
|`df.describe()`|Estadísticos|`ventas.describe()`|
|`df.dtypes`|Tipo de cada columna|`ventas.dtypes`|
|`df.nunique()`|Cardinalidad|`ventas.nunique()`|
|`s.value_counts()`|Frecuencias|`ventas["city"].value_counts()`|
|**Selección**|||
|`df["c"]` / `df[["a","b"]]`|Columna(s)|`ventas[["city", "units"]]`|
|`df.loc[f, c]`|Por etiqueta (fin incluido)|`ventas.loc[0:2, "city"]`|
|`df.iloc[f, c]`|Por posición (fin excluido)|`ventas.iloc[0:2, 2:4]`|
|`df[cond]`|Filtro booleano|`ventas[ventas["units"] > 35]`|
|`s.isin(lista)`|Pertenencia|`ventas["city"].isin(["Cali"])`|
|`s.between(a, b)`|Rango inclusivo|`ventas["units"].between(10, 20)`|
|**Nulos**|||
|`df.isna().sum()`|Nulos por columna|`ventas.isna().sum()`|
|`df.dropna(subset=)`|Elimina filas con nulos|`ventas.dropna(subset=["units"])`|
|`s.fillna(v)`|Rellena|`ventas["units"].fillna(0)`|
|**Tipos**|||
|`s.astype(t)`|Convierte|`s.astype("Int64")`|
|`pd.to_numeric(s, errors=)`|Texto a número tolerante|`pd.to_numeric(s, errors="coerce")`|
|`pd.to_datetime(s)`|Texto a fecha|`pd.to_datetime(df["fecha"])`|
|**Transformación**|||
|`df["n"] = expr`|Columna derivada|`v["revenue"] = v["units"] * v["unit_price"]`|
|`df.assign(n=lambda d: ...)`|Columna encadenable|`.assign(rev=lambda d: d.u * d.p)`|
|`s.map(dict)`|Sustitución por diccionario|`v["city"].map(regiones)`|
|`pd.cut(s, bins)`|Discretiza en rangos|`pd.cut(v["units"], [0,10,25,40])`|
|`np.select(conds, opts)`|if/elif/else vectorizado|`np.select([c1, c2], ["A","B"], "C")`|
|**Agregación**|||
|`df.groupby(k)[c].agg(f)`|Split-apply-combine|`v.groupby("city")["revenue"].sum()`|
|`.agg(n=("c","f"))`|Named aggregation|`.agg(total=("revenue","sum"))`|
|`.transform("mean")`|Agregado alineado a las filas|`v.groupby("city")["revenue"].transform("mean")`|
|`df.pivot_table(...)`|Tabla cruzada|`v.pivot_table(index="city", columns="channel", values="revenue", aggfunc="sum")`|
|`df.melt(id_vars=)`|De ancho a largo|`pt.melt(id_vars="city")`|
|`pd.crosstab(a, b)`|Frecuencias cruzadas|`pd.crosstab(v["city"], v["channel"])`|
|**Combinación**|||
|`df.merge(o, on=, how=)`|JOIN|`res.merge(metas, on="city", how="left")`|
|`pd.concat([a, b])`|Apila filas|`pd.concat([a, b], ignore_index=True)`|
|**Orden y duplicados**|||
|`df.sort_values(c)`|Ordena|`v.sort_values("revenue", ascending=False)`|
|`df.nlargest(n, c)`|Top-N|`v.nlargest(3, "revenue")`|
|`df.duplicated()`|Marca duplicados|`ventas.duplicated().sum()`|
|`df.drop_duplicates()`|Elimina duplicados|`ventas.drop_duplicates(subset=["order_id"])`|
|**Fechas y texto**|||
|`s.dt.year/month/day_name()`|Componentes de fecha|`v["order_date"].dt.month`|
|`df.resample("ME")`|Reagrupa por periodo|`v.set_index("order_date").resample("ME")["revenue"].sum()`|
|`s.str.strip/lower/contains`|Texto vectorizado|`v["sales_rep"].str.strip().str.title()`|

---

## 5. SQLAlchemy

```python
from sqlalchemy import create_engine, text
```

SQLAlchemy es la capa entre Python y la base de datos. En análisis de datos cumple un papel muy concreto: **es lo que `pandas` usa por debajo para hablar con PostgreSQL**. Para el 80% del trabajo de un analista, con `create_engine` + `read_sql` + `to_sql` es suficiente.

### 5.1 Core vs ORM

SQLAlchemy son dos librerías en una:

||**Core**|**ORM**|
|---|---|---|
|Piensas en|Tablas y SQL|Clases y objetos|
|Escribes|`text("SELECT ...")`|`select(Venta).where(...)`|
|Devuelve|Filas / DataFrames|Instancias de tus clases|
|Ideal para|Análisis, ETL, reportes|Aplicaciones web (CRUD)|

**Para análisis de datos, usa Core.** El ORM está diseñado para aplicaciones donde cada fila es una entidad de negocio con comportamiento; en analítica tú quieres el conjunto completo en un DataFrame, y mapear 500.000 filas a objetos Python es puro desperdicio.

El ORM aparece en esta guía en una sección mínima porque lo vas a encontrar en el backend de las aplicaciones cuyos datos te toque analizar.

---

### 5.2 Engine y conexión a PostgreSQL

**Sintaxis:** `create_engine("postgresql+psycopg2://usuario:clave@host:puerto/basedatos")`

**Qué hace:** crea el objeto que administra el pool de conexiones. **Se crea una sola vez por aplicación**, no una por consulta.

```python
from sqlalchemy import create_engine

engine = create_engine("postgresql+psycopg2://analista:secreto@localhost:5432/ventas_db")
# requiere: pip install psycopg2-binary

# verificar que conecta
with engine.connect() as conn:
    print(conn.execute(text("SELECT version()")).scalar())
# PostgreSQL 16.x on x86_64-pc-linux-gnu ...
```

**Nunca escribas credenciales en el código.** Usa variables de entorno:

```python
import os
from sqlalchemy import create_engine
from urllib.parse import quote_plus

user = os.environ["PGUSER"]
pwd  = quote_plus(os.environ["PGPASSWORD"])   # quote_plus escapa @ / : en la clave
host = os.environ.get("PGHOST", "localhost")
db   = os.environ["PGDATABASE"]

engine = create_engine(f"postgresql+psycopg2://{user}:{pwd}@{host}:5432/{db}", pool_pre_ping=True)
```

`pool_pre_ping=True` verifica que la conexión siga viva antes de usarla — evita el clásico "server closed the connection unexpectedly" en procesos largos.

**Otras URLs:**

```python
create_engine("sqlite:///local.db")                       # archivo local, cero configuración
create_engine("sqlite:///:memory:")                       # en RAM, ideal para pruebas
create_engine("mysql+pymysql://u:p@localhost:3306/db")
```

> Para practicar sin instalar PostgreSQL, usa **SQLite**: la API de SQLAlchemy y de pandas es idéntica, solo cambia la URL.

---

### 5.3 Ejecutar SQL

**Sintaxis:** `with engine.connect() as conn: conn.execute(text(sql), params)`

**Qué hace:** ejecuta SQL crudo. `text()` es obligatorio en SQLAlchemy 2.0.

```python
from sqlalchemy import text

with engine.connect() as conn:
    total = conn.execute(text("SELECT COUNT(*) FROM ventas")).scalar()
    print(total)
    # 446

    filas = conn.execute(
        text("SELECT city, units FROM ventas WHERE units > :u LIMIT 3"),
        {"u": 35},                       # parámetros con nombre -> a prueba de SQL injection
    ).fetchall()
    print(filas)
    # [('Barranquilla', 39.0), ('Bogota', 38.0), ('Cartagena', 36.0)]
```

**Escritura: hace falta `commit()`**

```python
with engine.begin() as conn:      # [RECOMENDADA] begin() hace commit automático al salir sin error
    conn.execute(text("UPDATE ventas SET discount = 0 WHERE discount IS NULL"))
    conn.execute(text("DELETE FROM ventas WHERE units IS NULL"))
# si algo lanza excepción, rollback automático
```

```python
with engine.connect() as conn:    # [LA VERÁS EN CÓDIGO AJENO] commit manual
    conn.execute(text("UPDATE ventas SET discount = 0 WHERE discount IS NULL"))
    conn.commit()                 # si lo olvidas, no se guarda NADA y no hay error
```

> **`engine.begin()` `[RECOMENDADA]`**: transacción explícita, commit/rollback automáticos. El olvido del `commit()` es el bug silencioso número uno de SQLAlchemy 2.0.

**Métodos del resultado:**

|Método|Devuelve|
|---|---|
|`.scalar()`|Un solo valor (primera columna de la primera fila)|
|`.fetchone()`|Una fila como tupla|
|`.fetchall()`|Lista de tuplas|
|`.mappings().all()`|Lista de dicts (acceso por nombre de columna)|

> **Nunca construyas SQL con f-strings:** `text(f"SELECT * FROM t WHERE city = '{ciudad}'")` es una vulnerabilidad de inyección SQL. Usa siempre `:parametro` + dict.

---

### 5.4 Integración con pandas

Esto es el 90% de lo que realmente usarás.

**`pd.read_sql` — extraer:**

```python
import pandas as pd

df = pd.read_sql("SELECT city, COUNT(*) AS n FROM ventas GROUP BY city", engine)
df
#            city    n
# 0  Barranquilla  128
# 1        Bogota  123
# 2          Cali   66
# 3     Cartagena   54
# 4      Medellin   75

# con parámetros
df = pd.read_sql(
    text("SELECT * FROM ventas WHERE city = :c AND order_date >= :d"),
    engine,
    params={"c": "Cali", "d": "2024-06-01"},
)

# con fechas ya parseadas y el índice puesto
df = pd.read_sql("SELECT * FROM ventas", engine,
                 parse_dates=["order_date"], index_col="order_id")
```

**`df.to_sql` — cargar:**

```python
v.to_sql(
    "ventas_agregadas",
    engine,
    if_exists="replace",   # "fail" (default) | "replace" (DROP + CREATE) | "append"
    index=False,           # casi siempre False
    chunksize=10_000,      # inserta por lotes: obligatorio con datasets grandes
    method="multi",        # un INSERT con muchos VALUES: mucho más rápido
)
```

|`if_exists`|Comportamiento|Cuándo|
|---|---|---|
|`"fail"`|Error si la tabla existe|Cargas únicas donde no quieres pisar nada|
|`"replace"`|**DROP TABLE** + CREATE|Tablas de resultados que se recalculan completas|
|`"append"`|INSERT sobre lo existente|Cargas incrementales|

> **Cuidado con `"replace"`**: hace `DROP TABLE`, lo que destruye índices, constraints y permisos que hubieras definido a mano. Para tablas de producción, prefiere `TRUNCATE` + `"append"`:
> 
> ```python
> with engine.begin() as conn:
>     conn.execute(text("TRUNCATE TABLE ventas_agregadas"))
> v.to_sql("ventas_agregadas", engine, if_exists="append", index=False)
> ```

**Dónde agregar: ¿en SQL o en pandas?**

```python
# [RECOMENDADA] agregar en SQL cuando la tabla es grande
q = """
SELECT city, channel, SUM(units * unit_price * (1 - COALESCE(discount, 0))) AS revenue
FROM ventas
GROUP BY city, channel
"""
resumen = pd.read_sql(q, engine)   # la base de datos hace el trabajo pesado; viajan 15 filas

# [LA VERÁS EN CÓDIGO AJENO] traer todo y agregar en pandas
todo = pd.read_sql("SELECT * FROM ventas", engine)   # viajan 500.000 filas por la red
resumen = todo.groupby(["city", "channel"])["revenue"].sum()
```

> **Regla:** filtra y agrega lo más cerca posible de los datos. Trae a pandas solo lo que vas a transformar con lógica que SQL no expresa cómodamente.

---

### 5.5 Sesiones y ORM mínimo

Lo suficiente para leer código ajeno.

```python
from sqlalchemy import create_engine, select, Column, Integer, String, Float
from sqlalchemy.orm import declarative_base, Session

Base = declarative_base()

class Venta(Base):                       # una clase = una tabla
    __tablename__ = "ventas_orm"
    order_id = Column(Integer, primary_key=True)
    city     = Column(String(50))
    revenue  = Column(Float)

engine = create_engine("sqlite:///orm.db")
Base.metadata.create_all(engine)         # CREATE TABLE IF NOT EXISTS

# --- escribir ---
with Session(engine) as s:
    s.add(Venta(order_id=1, city="Cali", revenue=100.0))
    s.add_all([Venta(order_id=2, city="Bogota", revenue=250.0)])
    s.commit()                           # sin commit no se guarda nada

# --- leer ---
with Session(engine) as s:
    for v in s.scalars(select(Venta).where(Venta.revenue > 50).order_by(Venta.revenue.desc())):
        print(v.order_id, v.city, v.revenue)
    # 2 Bogota 250.0
    # 1 Cali 100.0

    print(s.get(Venta, 1).city)          # búsqueda por clave primaria
    # Cali
```

**Session vs Connection:**

- `engine.connect()` → Core. Ejecuta SQL, devuelve filas. Es lo que usa pandas.
- `Session(engine)` → ORM. Además rastrea qué objetos cambiaron y genera los `UPDATE` correspondientes al hacer `commit()`.

**Cuándo usar ORM en análisis de datos:** prácticamente nunca. La excepción es cuando ya existe un modelo ORM en la aplicación y quieres reutilizar sus definiciones para no duplicar el esquema.

---

### 5.6 Tabla resumen SQLAlchemy

|Método|Qué hace|Ejemplo|
|---|---|---|
|`create_engine(url)`|Crea el pool de conexiones (una vez)|`create_engine("postgresql+psycopg2://u:p@host:5432/db")`|
|`engine.connect()`|Conexión de solo lectura|`with engine.connect() as c: ...`|
|`engine.begin()`|Conexión con transacción y commit automático|`with engine.begin() as c: ...`|
|`text(sql)`|Envuelve SQL crudo (obligatorio en 2.0)|`text("SELECT :x")`|
|`.execute(stmt, params)`|Ejecuta|`c.execute(text("... :u"), {"u": 35})`|
|`.scalar()`|Un solo valor|`c.execute(text("SELECT COUNT(*) FROM t")).scalar()`|
|`.fetchall()`|Lista de tuplas|`.fetchall()`|
|`.mappings().all()`|Lista de dicts|`.mappings().all()`|
|`pd.read_sql(q, eng)`|SQL a DataFrame|`pd.read_sql("SELECT * FROM ventas", engine)`|
|`df.to_sql(t, eng)`|DataFrame a tabla|`df.to_sql("t", engine, if_exists="append", index=False)`|
|`Session(engine)`|Sesión ORM|`with Session(engine) as s: ...`|
|`select(Modelo)`|Consulta ORM|`select(Venta).where(Venta.revenue > 50)`|

---

## 6. Matplotlib

```python
import matplotlib.pyplot as plt
```

### 6.1 Figura y ejes

Todo gráfico en Matplotlib tiene dos objetos:

- **`Figure`**: el lienzo completo. Controla tamaño, resolución y guardado.
- **`Axes`**: un sistema de coordenadas dentro del lienzo. Es _donde se dibuja_. Una figura puede tener varios.

```python
# [RECOMENDADA] API orientada a objetos
fig, ax = plt.subplots(figsize=(9, 4))
ax.plot([1, 2, 3], [10, 20, 15])
ax.set_title("Título")
plt.show()

# [LA VERÁS EN CÓDIGO AJENO] API pyplot con estado global
plt.figure(figsize=(9, 4))
plt.plot([1, 2, 3], [10, 20, 15])
plt.title("Título")
plt.show()
```

> **Usa siempre `fig, ax = plt.subplots()`.** La API `plt.*` opera sobre "el gráfico actual", un estado global implícito que se rompe en cuanto tienes más de un gráfico, o cuando el código está dentro de una función, o en un notebook con celdas ejecutadas fuera de orden. Con `ax` explícito siempre sabes dónde estás dibujando.
> 
> Nota la asimetría de nombres: en `plt` es `plt.title()`, en `ax` es `ax.set_title()`. Casi todos los métodos de `Axes` llevan el prefijo `set_`.

---

### 6.2 Los cinco gráficos

Con estos cinco cubres casi todo. Cada uno responde a una pregunta distinta.

```python
import matplotlib.pyplot as plt

v = ventas.copy()
v["revenue"] = v["units"] * v["unit_price"] * (1 - v["discount"].fillna(0))
v = v.dropna(subset=["revenue", "city"])
```

**1. Línea — evolución en el tiempo**

```python
mensual = v.set_index("order_date").resample("ME")["revenue"].sum()

fig, ax = plt.subplots(figsize=(10, 4))
ax.plot(mensual.index, mensual.values, marker="o", linewidth=2, color="#1f77b4")
ax.set_title("Ingresos mensuales 2024-2025")
ax.set_xlabel("Mes")
ax.set_ylabel("Ingresos (COP)")
ax.grid(alpha=0.3)
fig.autofmt_xdate()          # rota las fechas para que no se encimen
plt.show()
```

**2. Barras — comparar categorías**

```python
por_ciudad = v.groupby("city")["revenue"].sum().sort_values(ascending=False)

fig, ax = plt.subplots(figsize=(8, 4))
ax.bar(por_ciudad.index, por_ciudad.values, color="#2ca02c")
ax.set_title("Ingresos por ciudad")
ax.set_ylabel("Ingresos (COP)")
ax.tick_params(axis="x", rotation=45)
plt.show()

# barh: horizontal. Mejor cuando las etiquetas son largas o hay muchas categorías
fig, ax = plt.subplots(figsize=(8, 5))
ax.barh(por_ciudad.index[::-1], por_ciudad.values[::-1])   # [::-1] pone el mayor arriba
plt.show()
```

**3. Dispersión — relación entre dos variables numéricas**

```python
fig, ax = plt.subplots(figsize=(7, 5))
ax.scatter(v["units"], v["revenue"], alpha=0.4, s=25)
ax.set_xlabel("Unidades")
ax.set_ylabel("Ingresos (COP)")
ax.set_title("Relación unidades vs ingresos")
plt.show()
```

> `alpha=0.4` es casi obligatorio: con cientos de puntos, la transparencia revela dónde se acumulan.

**4. Histograma — distribución de una variable**

```python
fig, ax = plt.subplots(figsize=(8, 4))
ax.hist(v["unit_price"].dropna(), bins=25, edgecolor="white")
ax.set_xlabel("Precio unitario (COP)")
ax.set_ylabel("Frecuencia")
ax.set_title("Distribución de precios")
plt.show()
```

> El número de `bins` cambia por completo la historia que cuenta el gráfico. Prueba 10, 25 y 50 antes de decidir.

**5. Boxplot — comparar distribuciones y ver outliers**

```python
grupos     = [g["revenue"].values for _, g in v.groupby("channel")]
etiquetas  = sorted(v["channel"].unique())

fig, ax = plt.subplots(figsize=(7, 5))
ax.boxplot(grupos, tick_labels=etiquetas)
ax.set_ylabel("Ingresos (COP)")
ax.set_title("Distribución de ingresos por canal")
plt.show()
```

> El boxplot en Matplotlib crudo es incómodo (hay que armar la lista de arrays a mano). Este es exactamente el caso donde **Seaborn gana**: `sns.boxplot(v, x="channel", y="revenue")` y listo.

**Qué gráfico para qué pregunta:**

|Pregunta|Gráfico|
|---|---|
|¿Cómo evoluciona X en el tiempo?|Línea|
|¿Cuál categoría es mayor?|Barras|
|¿X se relaciona con Y?|Dispersión|
|¿Cómo se distribuye X?|Histograma|
|¿La distribución de X cambia según el grupo?|Boxplot|

---

### 6.3 Subplots

**Sintaxis:** `fig, axes = plt.subplots(filas, columnas)`

```python
fig, axes = plt.subplots(2, 2, figsize=(12, 8))

axes[0, 0].bar(por_ciudad.index, por_ciudad.values)
axes[0, 0].set_title("Ingresos por ciudad")
axes[0, 0].tick_params(axis="x", rotation=45)

axes[0, 1].scatter(v["units"], v["revenue"], alpha=0.4)
axes[0, 1].set_title("Unidades vs ingresos")

axes[1, 0].hist(v["unit_price"].dropna(), bins=20)
axes[1, 0].set_title("Distribución de precios")

axes[1, 1].plot(mensual.index, mensual.values, marker="o")
axes[1, 1].set_title("Evolución mensual")
axes[1, 1].tick_params(axis="x", rotation=45)

fig.suptitle("Dashboard de ventas", fontsize=14)
fig.tight_layout()               # evita que los títulos se encimen. Casi siempre necesario
plt.show()
```

`axes` es un array de NumPy: con `subplots(1, 3)` se indexa `axes[0]`, `axes[1]`, `axes[2]`; con `subplots(2, 2)` es `axes[fila, columna]`. Para recorrerlos todos: `for ax in axes.ravel(): ...`.

```python
# ejes compartidos: misma escala para comparar de verdad
fig, axes = plt.subplots(1, 3, figsize=(14, 4), sharey=True)
```

---

### 6.4 Anotación, estilo y guardado

```python
fig, ax = plt.subplots(figsize=(10, 5))
ax.plot(mensual.index, mensual.values, marker="o", label="2024-2025")

# --- etiquetas ---
ax.set_title("Ingresos mensuales", fontsize=14, fontweight="bold")
ax.set_xlabel("Mes")
ax.set_ylabel("Ingresos (COP)")
ax.legend(loc="upper left")             # requiere label= en el plot
ax.grid(alpha=0.3, linestyle="--")

# --- formato de eje: millones en vez de notación científica ---
ax.yaxis.set_major_formatter(lambda x, p: f"{x/1e6:.1f}M")

# --- límites y línea de referencia ---
ax.set_ylim(0, mensual.max() * 1.1)
ax.axhline(mensual.mean(), color="red", linestyle="--", label="Promedio")

# --- anotación puntual ---
pico = mensual.idxmax()
ax.annotate(f"Pico: {mensual.max()/1e6:.1f}M",
            xy=(pico, mensual.max()),
            xytext=(10, 15), textcoords="offset points",
            arrowprops=dict(arrowstyle="->"))

fig.tight_layout()
fig.savefig("ingresos.png", dpi=150, bbox_inches="tight")   # bbox_inches evita recortes
plt.close(fig)                                              # libera memoria en bucles
```

**Estilos:**

```python
plt.style.available          # ['ggplot', 'seaborn-v0_8', 'fivethirtyeight', ...]
plt.style.use("seaborn-v0_8-whitegrid")   # aplica a todos los gráficos siguientes
```

**Guardado:**

|Formato|Cuándo|
|---|---|
|`.png` con `dpi=150`|Presentaciones, informes, correos|
|`.svg`|Documentos donde se pueda hacer zoom|
|`.pdf`|Impresión|

> **En scripts (no notebooks):** `plt.show()` bloquea la ejecución. Si solo quieres archivos, usa `fig.savefig(...)` + `plt.close(fig)`. Y si el script corre en un servidor sin pantalla, pon `matplotlib.use("Agg")` antes de importar `pyplot`.

---

### 6.5 Tabla resumen Matplotlib

|Método|Qué hace|Ejemplo|
|---|---|---|
|`plt.subplots(f, c)`|Crea figura + ejes|`fig, ax = plt.subplots(figsize=(9,4))`|
|`ax.plot(x, y)`|Línea|`ax.plot(fechas, valores, marker="o")`|
|`ax.bar(x, h)` / `ax.barh`|Barras vertical / horizontal|`ax.bar(ciudades, totales)`|
|`ax.scatter(x, y)`|Dispersión|`ax.scatter(u, r, alpha=0.4)`|
|`ax.hist(x, bins=n)`|Histograma|`ax.hist(precios, bins=25)`|
|`ax.boxplot(datos)`|Boxplot|`ax.boxplot(grupos, tick_labels=etq)`|
|`ax.set_title/xlabel/ylabel`|Textos|`ax.set_title("Ventas")`|
|`ax.legend()`|Leyenda (requiere `label=`)|`ax.legend(loc="upper left")`|
|`ax.grid(alpha=)`|Rejilla|`ax.grid(alpha=0.3)`|
|`ax.tick_params(rotation=)`|Rota etiquetas|`ax.tick_params(axis="x", rotation=45)`|
|`ax.axhline/axvline`|Línea de referencia|`ax.axhline(promedio, color="red")`|
|`ax.annotate(txt, xy=)`|Anotación con flecha|`ax.annotate("Pico", xy=(x, y))`|
|`fig.suptitle(t)`|Título de la figura completa|`fig.suptitle("Dashboard")`|
|`fig.tight_layout()`|Ajusta espaciados|`fig.tight_layout()`|
|`fig.savefig(f, dpi=)`|Guarda a archivo|`fig.savefig("g.png", dpi=150, bbox_inches="tight")`|
|`plt.close(fig)`|Libera memoria|`plt.close(fig)`|
|`plt.style.use(s)`|Aplica estilo global|`plt.style.use("ggplot")`|

---

## 7. Seaborn

```python
import seaborn as sns
```

Seaborn está construido sobre Matplotlib. **No lo reemplaza: lo complementa.** Aquí van solo las cinco cosas que Seaborn hace claramente mejor y con menos código; para todo lo demás, Matplotlib directo.

Todas las funciones siguen la misma firma: `sns.funcion(data, x=, y=, hue=)`, donde `hue` es la variable que separa por color. Y todas devuelven un `Axes` de Matplotlib, así que puedes seguir configurándolo con `ax.set_title(...)`.

**1. Distribución con densidad — `histplot`**

```python
sns.set_theme(style="whitegrid")          # estilo global agradable, aplica también a Matplotlib

fig, ax = plt.subplots(figsize=(8, 4))
sns.histplot(data=v, x="unit_price", bins=25, kde=True, ax=ax)
ax.set_title("Distribución de precios")
plt.show()
```

> Ventaja sobre `ax.hist`: `kde=True` superpone la curva de densidad estimada en una sola palabra.

**2. Distribución por categoría — `boxplot` / `violinplot`**

```python
fig, ax = plt.subplots(figsize=(8, 5))
sns.boxplot(data=v, x="channel", y="revenue", ax=ax)
ax.set_title("Ingresos por canal")
plt.show()
```

> Ventaja clara: en Matplotlib tenías que construir a mano la lista de arrays por grupo. Aquí basta con nombrar las columnas.

```python
sns.violinplot(data=v, x="channel", y="revenue")   # boxplot + forma de la distribución
sns.stripplot(data=v, x="channel", y="revenue", alpha=0.3)   # puntos individuales
```

**3. Matriz de correlación — `heatmap`**

```python
corr = v[["units", "unit_price", "discount", "revenue"]].corr(numeric_only=True)
corr.round(2)
#             units  unit_price  discount  revenue
# units        1.00       -0.05      0.07     0.69
# unit_price  -0.05        1.00      0.01     0.60
# discount     0.07        0.01      1.00    -0.02
# revenue      0.69        0.60     -0.02     1.00

fig, ax = plt.subplots(figsize=(6, 5))
sns.heatmap(corr, annot=True, fmt=".2f", cmap="RdBu_r", center=0, vmin=-1, vmax=1, ax=ax)
ax.set_title("Matriz de correlación")
plt.show()
```

> Ventaja: hacer esto en Matplotlib requiere `imshow` + anotar cada celda a mano en un doble bucle. `center=0` con una paleta divergente es lo correcto para correlaciones: el blanco marca la ausencia de relación.

**4. Comparación por categorías con intervalos — `barplot`**

```python
fig, ax = plt.subplots(figsize=(8, 4))
sns.barplot(data=v, x="city", y="revenue", estimator="sum", errorbar=None, ax=ax)
ax.set_title("Ingresos por ciudad")
ax.tick_params(axis="x", rotation=45)
plt.show()
```

> Ventaja: agrega él mismo (`estimator="mean"` por defecto), no hace falta el `groupby` previo. Con `errorbar="ci"` dibuja el intervalo de confianza. **Ojo:** cuando el `estimator` es `"mean"` sin `errorbar=None`, Seaborn hace bootstrap y es lento con datasets grandes.

**5. Dispersión con una tercera variable — `scatterplot` y `pairplot`**

```python
fig, ax = plt.subplots(figsize=(8, 5))
sns.scatterplot(data=v, x="units", y="revenue", hue="channel", size="unit_price",
                alpha=0.6, ax=ax)
ax.set_title("Unidades vs ingresos por canal")
plt.show()

# pairplot: todas las combinaciones de a pares, en una línea. Ideal para exploración inicial
sns.pairplot(v[["units", "unit_price", "revenue", "channel"]], hue="channel")
```

> Ventaja: `hue=` y `size=` codifican variables extra y generan la leyenda automáticamente. En Matplotlib eso son varios `scatter` y armar la leyenda a mano.

**Cuándo NO usar Seaborn:** cuando necesitas control fino sobre el resultado (posiciones exactas, ejes secundarios, anotaciones específicas, formatos de eje personalizados). Ahí Matplotlib directo es más corto que pelear con las abstracciones de Seaborn.

---

### 7.1 Tabla resumen Seaborn

|Método|Qué hace|Ejemplo|
|---|---|---|
|`sns.set_theme(style=)`|Estilo global|`sns.set_theme(style="whitegrid")`|
|`sns.histplot(d, x=, kde=)`|Histograma + densidad|`sns.histplot(v, x="unit_price", kde=True)`|
|`sns.boxplot(d, x=, y=)`|Distribución por categoría|`sns.boxplot(v, x="channel", y="revenue")`|
|`sns.violinplot(d, x=, y=)`|Boxplot + forma|`sns.violinplot(v, x="channel", y="revenue")`|
|`sns.heatmap(m, annot=)`|Matriz coloreada|`sns.heatmap(corr, annot=True, cmap="RdBu_r")`|
|`sns.barplot(d, x=, y=, estimator=)`|Barras con agregación|`sns.barplot(v, x="city", y="revenue", estimator="sum")`|
|`sns.countplot(d, x=)`|Frecuencias por categoría|`sns.countplot(v, x="channel")`|
|`sns.scatterplot(d, x=, y=, hue=)`|Dispersión multivariable|`sns.scatterplot(v, x="units", y="revenue", hue="channel")`|
|`sns.lineplot(d, x=, y=)`|Serie temporal con IC|`sns.lineplot(v, x="order_date", y="revenue")`|
|`sns.pairplot(d, hue=)`|Matriz de dispersiones|`sns.pairplot(v[cols], hue="channel")`|

---

## 8. Errores frecuentes

Seis errores que vas a cometer. Para cada uno: **causa**, **síntoma** y **solución**.

---

### 8.1 SettingWithCopyWarning

```
SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.
Try using .loc[row_indexer, col_indexer] = value instead
```

**Causa.** Filtraste un DataFrame y pandas no sabe si el resultado es una **vista** (comparte memoria con el original) o una **copia**. Al escribir sobre él, no puede garantizar dónde va a caer el cambio.

**Síntoma.** Aparece la advertencia y —lo peligroso— **la asignación puede no tener efecto**. El código "funciona", no lanza error, y los datos no cambian.

```python
# MAL
barranquilla = ventas[ventas["city"] == "Barranquilla"]   # ¿vista o copia?
barranquilla["bono"] = 100                                 # SettingWithCopyWarning
```

**Solución.** Declara la intención de forma explícita:

```python
# BIEN, opción 1: copia explícita
barranquilla = ventas[ventas["city"] == "Barranquilla"].copy()
barranquilla["bono"] = 100        # sin advertencia, y el original no se toca

# BIEN, opción 2: escribir directo sobre el original con .loc
ventas.loc[ventas["city"] == "Barranquilla", "bono"] = 100

# BIEN, opción 3: assign dentro de una cadena
resultado = (ventas
             .loc[ventas["city"] == "Barranquilla"]
             .assign(bono=100))
```

> **Regla mecánica:** si filtras y después vas a modificar, pon `.copy()` al final del filtro. Cuesta memoria y ahorra horas.

---

### 8.2 Confusion entre loc e iloc

**Causa.** `loc` usa **etiquetas** del índice, `iloc` usa **posiciones**. Cuando el índice es el `RangeIndex` por defecto (0, 1, 2…), etiqueta y posición coinciden y todo parece funcionar — hasta que filtras u ordenas.

**Síntoma.** `KeyError: 3` o, peor, la fila equivocada sin ningún error.

```python
filtrado = ventas[ventas["units"] > 35]
filtrado.index[:5].tolist()
# [6, 15, 22, 37, 44]   <- el índice conserva las etiquetas ORIGINALES

filtrado.loc[0]    # KeyError: 0    -> la etiqueta 0 ya no existe
filtrado.iloc[0]   # OK             -> primera fila del resultado

# y la trampa silenciosa:
filtrado.loc[6]    # devuelve la fila etiquetada 6 (la PRIMERA del filtro)
filtrado.iloc[6]   # devuelve la SÉPTIMA fila del filtro. Ambas funcionan. Solo una es la que querías.
```

**Solución.** Después de filtrar, si vas a trabajar por posición, resetea el índice:

```python
filtrado = ventas[ventas["units"] > 35].reset_index(drop=True)
filtrado.loc[0]    # ahora sí funciona
```

`drop=True` descarta el índice viejo. Sin él, se guarda como una columna nueva llamada `index`.

> **Regla mnemotécnica:** la **i** de `iloc` es de **integer position**. Y el rango: `loc[0:2]` da **3** filas (fin incluido, como una etiqueta), `iloc[0:2]` da **2** (fin excluido, como un slice de Python).

---

### 8.3 Índices duplicados y filas multiplicadas tras un merge

**Causa.** La clave del merge no es única en la tabla de la derecha. Un `merge` es un producto cartesiano por clave: si `city` aparece 2 veces a la derecha, cada fila de la izquierda se duplica.

**Síntoma.** El DataFrame **crece** después del merge. Los totales se inflan y nadie entiende por qué.

```python
metas_malas = pd.DataFrame({
    "city": ["Barranquilla", "Barranquilla", "Bogota"],   # duplicada!
    "meta": [20_000_000, 21_000_000, 18_000_000],
})

antes = len(res)                                   # 5
despues = len(res.merge(metas_malas, on="city"))   # 6   <- Barranquilla se duplicó
```

**Solución.** Tres capas de defensa:

```python
# 1. verifica la unicidad ANTES
assert metas["city"].is_unique, "clave duplicada en metas"

# 2. deja que pandas verifique la cardinalidad
res.merge(metas, on="city", how="left", validate="many_to_one")
# MergeError si la derecha tiene claves repetidas

# 3. verifica el conteo DESPUÉS
antes = len(res)
res = res.merge(metas, on="city", how="left")
assert len(res) == antes, f"el merge multiplicó filas: {antes} -> {len(res)}"
```

**Índice duplicado tras `concat`:**

```python
pd.concat([a, b]).index.is_unique                       # False
pd.concat([a, b], ignore_index=True).index.is_unique    # True   <- casi siempre lo que quieres
```

---

### 8.4 Columnas de tipo object inesperadas

**Causa.** Un solo valor no numérico contamina toda la columna: un espacio, `"N/D"`, un guion, un número con separador de miles a la colombiana (`"1.234,50"`).

**Síntoma.** `TypeError: unsupported operand type(s) for -: 'str' and 'str'`, o `describe()` que ignora la columna, o una suma que concatena texto.

```python
sucia = pd.Series(["1000", "2000", "N/D", "3.500"])
sucia.dtype       # dtype('O')
sucia.sum()       # '10002000N/D3.500'   <- concatenó en vez de sumar
```

**Solución.** Diagnostica primero, convierte después:

```python
# 1. ¿qué valores estorban?
convertidos = pd.to_numeric(sucia, errors="coerce")
sucia[convertidos.isna()].unique()
# array(['N/D'], dtype=object)     -> 3.500 sí se convirtió (a 3.5, ojo)

# 2. limpia el formato colombiano ANTES de convertir
limpia = (sucia.str.replace(".", "", regex=False)     # quita separador de miles
                .str.replace(",", ".", regex=False))  # coma decimal -> punto
pd.to_numeric(limpia, errors="coerce")
# 0    1000.0
# 1    2000.0
# 2       NaN
# 3    3500.0

# 3. previene en el origen
pd.read_csv("v.csv", na_values=["N/D", "-", "", "NA", "null"], thousands=".", decimal=",")
```

> **`"3.500"` se convierte silenciosamente en `3.5`.** Este es el bug más caro de la lista: no lanza error, no da warning, y tus ventas quedan divididas por mil. Siempre revisa `describe()` después de convertir.

---

### 8.5 Mutación de arrays por referencia

**Causa.** En NumPy, el slicing devuelve una **vista** que comparte memoria con el original. Modificar la vista modifica el original.

**Síntoma.** Un array cambia sin que nadie lo haya tocado aparentemente.

```python
orig = np.array([1, 2, 3, 4])
vista = orig[:2]
vista[0] = 999
orig
# array([999,   2,   3,   4])    <- el original cambió
```

**Solución.**

```python
orig = np.array([1, 2, 3, 4])
copia = orig[:2].copy()
copia[0] = 999
orig
# array([1, 2, 3, 4])            <- intacto

vista.base is orig     # True  -> es una vista
copia.base is None     # True  -> es dueña de sus datos
```

> **Nota:** el filtrado booleano (`arr[arr > 2]`) y la _fancy indexing_ (`arr[[0, 2]]`) **sí** devuelven copias. Solo el slicing (`arr[a:b]`) devuelve vista. Cuando dudes, `.copy()`.
> 
> El mismo problema con listas de Python: `b = a` no copia, `b = a.copy()` sí. Y con DataFrames: `df2 = df` no copia, `df2 = df.copy()` sí.

---

### 8.6 Encadenamiento de indexación (_chained indexing_)

**Causa.** `df["col"][fila]` son **dos** operaciones: primero se extrae la Series, después se indexa. Para leer funciona; para escribir, la segunda operación puede caer sobre un objeto temporal.

**Síntoma.** La asignación no tiene efecto, con o sin `SettingWithCopyWarning`.

```python
# MAL
ventas["units"][0] = 999                              # puede no hacer nada
ventas[ventas["city"] == "Cali"]["units"] = 0         # casi seguro no hace nada

# BIEN: una sola operación con .loc
ventas.loc[0, "units"] = 999
ventas.loc[ventas["city"] == "Cali", "units"] = 0
```

**Solución.** **Nunca dos pares de corchetes seguidos a la izquierda de un `=`.** Si ves `][`, reescríbelo con `.loc[filas, columnas]`.

> A partir de pandas 3.0 el modelo _Copy-on-Write_ es el predeterminado y el chained assignment **falla en silencio siempre** (nunca escribe sobre el original). Es una mejora: el comportamiento se vuelve predecible. Pero el código escrito con `][` deja de funcionar. Escribe `.loc` desde ahora.

---

### 8.7 Errores rápidos

|Error|Causa|Solución|
|---|---|---|
|`ValueError: The truth value of a Series is ambiguous`|Usaste `and`/`or`/`not` con Series|Usa `&`, `\|`, `~` con paréntesis|
|`KeyError: 'columna'`|Nombre mal escrito o con espacios|`df.columns.tolist()`; limpia con `df.columns = df.columns.str.strip()`|
|`TypeError: unhashable type: 'list'`|`df[["a", "b"]]` escrito como `df["a", "b"]`|Doble corchete|
|`df.mean()` da error o ignora columnas|Hay columnas de texto|`df.mean(numeric_only=True)`|
|`AttributeError: Can only use .str accessor with string values`|La columna no es texto|`df["c"].astype(str).str...`|
|`AttributeError: Can only use .dt accessor with datetimelike values`|La fecha llegó como texto|`pd.to_datetime(df["c"])` primero|
|`MemoryError` al leer CSV|El archivo no cabe en RAM|`chunksize=100_000` o `usecols=[...]`|
|Fechas parseadas al revés (día/mes)|Formato ambiguo|`pd.to_datetime(s, format="%d/%m/%Y")` explícito|

---

## 9. Glosario

**DataFrame** — Tabla bidimensional con columnas etiquetadas y de tipos potencialmente distintos. La estructura central de pandas. Conceptualmente: un diccionario de Series que comparten índice.

**Series** — Array unidimensional con índice. Una columna de un DataFrame es una Series. Tiene `name`, `index`, `values` y `dtype`.

**Índice (index)** — Las etiquetas de las filas. No es una columna: es el mecanismo con el que pandas alinea operaciones entre objetos. Puede ser numérico, de texto, de fechas (`DatetimeIndex`) o jerárquico (`MultiIndex`).

**Eje (axis)** — La dirección de una operación. `axis=0` recorre las filas y produce un resultado por columna ("hacia abajo"). `axis=1` recorre las columnas y produce un resultado por fila ("hacia el lado"). El eje indicado es el que **desaparece**.

**dtype** — Tipo de dato de una columna o array: `int64`, `float64`, `bool`, `object` (casi siempre texto), `datetime64[ns]`, `category`. Determina qué operaciones son válidas y cuánta memoria se usa. `NaN` solo existe en `float` (y en los tipos _nullable_ como `Int64`).

**Agregación** — Operación que reduce muchos valores a uno: `sum`, `mean`, `count`, `min`, `max`, `median`, `std`, `nunique`.

**Granularidad** — El nivel de detalle de una fila. `ventas` tiene granularidad _pedido_; el resultado de `groupby("city")` tiene granularidad _ciudad_. Agregar sube el nivel de granularidad (menos detalle, menos filas). Un merge entre tablas de distinta granularidad es la causa típica de filas duplicadas.

**Formato ancho (wide)** — Una columna por categoría. `city` en las filas, `Online`/`Tienda`/`Mayorista` como columnas. Legible para humanos, ideal para reportes tipo Excel. Se obtiene con `pivot_table`.

**Formato largo (long / tidy)** — Una fila por observación, con las categorías como valores de una columna: `city | channel | revenue`. Es el que quieren Seaborn, las bases de datos relacionales y los modelos estadísticos. Se obtiene con `melt`.

**Join interno (inner)** — Conserva solo las filas cuya clave existe en **ambas** tablas. Puede perder datos de los dos lados.

**Join externo (outer / left / right)** — `left` conserva todas las filas de la tabla izquierda y rellena con `NaN` lo que no encuentre a la derecha. `outer` conserva todo de ambos lados. `right` es el espejo de `left`. En un pipeline, `left` es el predeterminado sensato: garantiza que no pierdes filas del dataset principal.

**Valor nulo (NaN / None / NaT)** — Ausencia de dato. `np.nan` para números, `None` para objetos, `NaT` para fechas. Se detecta con `isna()`, nunca con `== None` ni `== np.nan` (`np.nan == np.nan` es `False`). Distinguir siempre "no hay dato" de "el dato es cero".

**Cardinalidad** — Número de valores distintos de una columna (`df["c"].nunique()`). Alta cardinalidad (`order_id`, 500 valores) sugiere un identificador; baja cardinalidad (`channel`, 3 valores) sugiere una categórica que conviene convertir a `category`.

**Vectorización** — Aplicar una operación a un array completo de una vez, ejecutando el bucle en C. Es lo contrario de iterar en Python. Típicamente entre 10x y 100x más rápido.

**Broadcasting** — La regla por la que NumPy "estira" automáticamente arrays de formas distintas para operar entre ellos: `precios * 1.19` funciona aunque `1.19` sea un escalar.

**Máscara booleana** — Array o Series de `True`/`False` del mismo tamaño que los datos, usado para filtrar (`df[df["units"] > 20]`).

**ETL** — _Extract, Transform, Load_. Extraer datos de una fuente, transformarlos (limpiar, derivar, agregar) y cargarlos en un destino, típicamente una base de datos.

**Engine (SQLAlchemy)** — Objeto que administra el pool de conexiones a una base de datos. Se crea una vez por aplicación y se reutiliza.

**Split-apply-combine** — El modelo mental de `groupby`: dividir en grupos, aplicar una función a cada uno, combinar los resultados en una tabla.

---

## 10. Menciones honoríficas

Métodos reales y útiles que **no** se usan cada semana. Una línea cada uno; búscalos en la documentación cuando los necesites.

**NumPy**

|Método|Qué hace|
|---|---|
|`np.dot` / `@`|Producto matricial; base del álgebra lineal y del machine learning.|
|`np.linalg.inv/solve`|Inversa de matriz y resolución de sistemas lineales.|
|`np.random.choice`|Muestreo aleatorio con o sin reemplazo.|
|`np.clip(a, lo, hi)`|Recorta valores fuera de un rango (winsorización de outliers).|
|`np.cumsum/cumprod`|Sumas y productos acumulados.|
|`np.diff`|Diferencias entre elementos consecutivos.|
|`np.percentile/quantile`|Percentiles de un array.|
|`np.histogram`|Conteos por bin, sin dibujar nada.|
|`np.einsum`|Operaciones tensoriales arbitrarias con notación de Einstein.|
|`np.save/load`|Serialización binaria `.npy`, mucho más rápida que CSV.|
|`np.vectorize`|Envuelve una función escalar; **no acelera nada**, solo maquilla el bucle.|
|`np.memmap`|Arrays mapeados a disco, para datos que no caben en RAM.|

**pandas**

|Método|Qué hace|
|---|---|
|`df.pipe(func)`|Inserta una función propia en una cadena de métodos.|
|`df.explode(col)`|Una fila por elemento de una columna que contiene listas.|
|`df.stack/unstack`|Mueve niveles entre índice y columnas en un MultiIndex.|
|`df.set_index/reset_index`|Convierte columnas en índice y viceversa.|
|`s.rank()`|Ranking de valores, con estrategias para los empates.|
|`s.clip(lo, hi)`|Recorta valores fuera de un rango.|
|`s.rolling(n)` / `s.expanding()`|Ventanas móviles y acumuladas.|
|`s.shift(n)` / `s.diff()`|Desplaza valores; base de los cálculos período contra período.|
|`s.qcut(n)`|Discretiza en cuantiles (a diferencia de `cut`, que usa rangos fijos).|
|`df.style`|Formato condicional para exportar a HTML o Excel.|
|`pd.get_dummies`|Codificación one-hot de variables categóricas.|
|`df.sample(n, frac=)`|Muestreo aleatorio de filas.|
|`df.memory_usage(deep=True)`|Consumo real de memoria por columna.|
|`df.eval("a + b")`|Expresiones sobre columnas evaluadas eficientemente.|
|`pd.MultiIndex`|Índices jerárquicos; potentes y una fuente inagotable de confusión.|
|`df.at/.iat`|Acceso a un solo valor; más rápido que `.loc/.iloc` en bucles.|
|`pd.read_csv(chunksize=)`|Lectura por lotes para archivos que no caben en memoria.|
|`df.combine_first(other)`|Rellena nulos con los valores de otro DataFrame.|
|`pd.merge_asof`|Merge por proximidad temporal (la clave más cercana, no la exacta).|
|`df.corrwith(s)`|Correlación de todas las columnas contra una Series.|

**SQLAlchemy**

|Método|Qué hace|
|---|---|
|`MetaData` / `Table` / `reflect()`|Descubre el esquema de una base existente sin declararlo.|
|`inspect(engine)`|Lista tablas, columnas, índices y claves foráneas.|
|`Index` / `UniqueConstraint`|Definición declarativa de índices y restricciones.|
|`sessionmaker`|Fábrica de sesiones ORM configuradas.|
|`relationship` / `ForeignKey`|Relaciones entre modelos ORM (uno a muchos, muchos a muchos).|
|`alembic`|Migraciones de esquema versionadas (librería aparte).|
|`pool_size` / `max_overflow`|Ajuste del pool de conexiones para cargas concurrentes.|
|`event.listen`|Hooks para interceptar eventos de conexión y consulta.|

**Matplotlib / Seaborn**

|Método|Qué hace|
|---|---|
|`ax.twinx()`|Segundo eje Y con escala distinta.|
|`ax.fill_between`|Rellena el área entre dos curvas (bandas de confianza).|
|`ax.stackplot`|Áreas apiladas.|
|`ax.pie`|Gráfico de torta; casi siempre peor que barras.|
|`plt.rcParams`|Configuración global de estilo.|
|`mpl.dates.DateFormatter`|Control fino del formato de fechas en el eje.|
|`sns.FacetGrid` / `catplot` / `relplot`|Rejillas de gráficos por combinación de categorías.|
|`sns.jointplot`|Dispersión con distribuciones marginales.|
|`sns.clustermap`|Heatmap con dendrogramas de agrupamiento.|
|`sns.regplot` / `lmplot`|Dispersión con recta de regresión ajustada.|
|`sns.color_palette`|Construcción de paletas de color personalizadas.|

---

## 11. 15 ejercicios progresivos

Todos operan sobre el `ventas` de la [[#2. El dataset del documento|sección 2]]. Intenta resolverlos antes de mirar las [[#12. Soluciones comentadas|soluciones]].

**Bloque 1 — Inspección y filtrado (día 1)**

1. **Radiografía del dataset.** Reporta: número de filas y columnas, cuántas columnas hay de cada tipo, qué columnas tienen nulos y cuántos, y el rango de fechas cubierto.
    
2. **Filtro compuesto.** ¿Cuántos pedidos son de Barranquilla **y** superan las 30 unidades? ¿Cuál es el promedio de unidades de ese subconjunto?
    
3. **Columna derivada.** Crea la columna `revenue = units * unit_price * (1 - discount)`, tratando el descuento nulo como 0. ¿Cuántos `revenue` quedan nulos y por qué? Reporta el total y el promedio.
    

**Bloque 2 — Limpieza (día 2)**

4. **Normalización de texto.** La columna `sales_rep` tiene espacios sobrantes y mayúsculas inconsistentes. Límpiala y calcula el ingreso total por vendedor, ordenado de mayor a menor.
    
5. **Estrategia de nulos.** Deja el dataset sin ningún nulo, con una decisión justificada por columna: `discount` nulo significa "sin descuento"; `city` nula es "Desconocida"; `units` y `unit_price` se imputan con la mediana de su grupo natural (categoría y producto, respectivamente). Verifica que no quede ningún nulo.
    

**Bloque 3 — Agregación (día 3–4)**

6. **Resumen por ciudad.** Para cada ciudad: ingreso total, número de pedidos, ticket promedio y unidades totales. Ordenado por ingreso descendente.
    
7. **Top 5 productos.** Los 5 productos con mayor ingreso, y qué porcentaje del ingreso total representa cada uno.
    
8. **Duplicados.** ¿Cuántas filas están completamente duplicadas? ¿Cuántos `order_id` se repiten? Elimínalos conservando la primera aparición y verifica que `order_id` quede único.
    

**Bloque 4 — Tiempo y estructura (semana 2)**

9. **Serie mensual.** Ingreso por mes. ¿Cuál fue el mes pico? Calcula la variación porcentual mes a mes.
    
10. **Tabla cruzada.** Tabla de ingresos con ciudades en filas y canales en columnas, con totales. Después, la misma tabla expresada como porcentaje del total de cada ciudad.
    
11. **Cruce con metas.** Cruza el ingreso por ciudad con la tabla `metas` (defínela con Barranquilla 20M, Bogotá 18M, Medellín 14M, Cali 11M y Santa Marta 5M, con sus regiones). Calcula el porcentaje de cumplimiento, identifica las ciudades sin meta y agrega el ingreso por región.
    

**Bloque 5 — Análisis y pipeline (semana 2)**

12. **NumPy puro.** Convierte `revenue` a array de NumPy. Calcula media y desviación estándar, el z-score de cada valor, cuántos outliers hay con |z| > 3 y cuáles son. Reporta también los percentiles 25, 50, 75 y 95.
    
13. **Segmentación.** Clasifica cada pedido en Alto (≥ 300.000), Medio (≥ 100.000) o Bajo, usando `np.select`. Reporta, por segmento, el número de pedidos, el ingreso total y qué porcentaje del ingreso aporta.
    
14. **Dashboard.** Una figura 2×2 con: evolución mensual del ingreso (línea), ingreso por ciudad (barras), distribución de `unit_price` (histograma) e ingreso por canal (boxplot). Con títulos, etiquetas y guardada como PNG.
    
15. **ETL completo.** Un script de principio a fin: extrae el CSV, limpia (nulos, texto, duplicados), transforma (`revenue`, segmento, columnas de fecha), agrega por ciudad y canal, carga a PostgreSQL con SQLAlchemy, vuelve a leer desde la base para validar, y genera un gráfico del resultado. Con logging y validaciones.
    

---

## 12. Soluciones comentadas

> Los números de salida corresponden al dataset generado con `default_rng(42)` tal como está en la [[#2. El dataset del documento|sección 2]].

### Solución 1 — Radiografía del dataset

```python
ventas.shape
# (508, 10)

ventas.dtypes.value_counts()
# object            5
# float64           3
# datetime64[ns]    1
# int64             1

ventas.isna().sum()[lambda x: x > 0]     # solo las columnas CON nulos
# city          10
# units         35
# unit_price    20
# discount      15

print(ventas["order_date"].min(), ventas["order_date"].max())
# 2024-01-01 00:00:00 2025-06-26 00:00:00
```

> El `[lambda x: x > 0]` filtra la Series resultante y evita leer una lista de ceros. Fíjate ya en la pista: 508 filas pero `nunique()` de `order_id` da 500 — hay duplicados esperando.

---

### Solución 2 — Filtro compuesto

```python
f = ventas[(ventas["city"] == "Barranquilla") & (ventas["units"] > 30)]

f.shape
# (30, 10)

f["units"].mean().round(2)
# 34.27
```

> Los paréntesis alrededor de cada condición son obligatorios. Nota además que `units > 30` descarta automáticamente los NaN: cualquier comparación con NaN da `False`.

---

### Solución 3 — Columna derivada

```python
v = ventas.copy()                               # .copy() para no tocar el original
v["revenue"] = v["units"] * v["unit_price"] * (1 - v["discount"].fillna(0))

v["revenue"].isna().sum()
# 53

round(v["revenue"].sum(), 0), round(v["revenue"].mean(), 2)
# (75771988.0, 166531.84)
```

> **Por qué 53 nulos:** `units` tiene 35 nulos y `unit_price` tiene 20; hay 2 filas donde coinciden ambos, así que la unión son 53. NaN se propaga: si falta cualquier factor, el producto es NaN. Eso es correcto — inventar un revenue sería peor.
> 
> `discount` sí se rellena con 0 porque ahí el nulo **significa** "sin descuento", no "dato faltante". La decisión depende del significado de negocio, no de la técnica.

---

### Solución 4 — Normalización de texto

```python
v["sales_rep"] = (v["sales_rep"]
                    .str.strip()                            # espacios de los extremos
                    .str.title()                            # Formato Título
                    .str.replace(r"\s+", " ", regex=True))  # espacios internos dobles

v["sales_rep"].nunique()
# 6      <- sin limpiar habrían aparecido como 6 variantes distintas mal escritas

v.groupby("sales_rep")["revenue"].sum().sort_values(ascending=False).round(0)
# sales_rep
# Luisa Perez      15745980.0
# Carlos Diaz      15320445.0
# Ana Gomez        14821224.0
# Pedro Torres     11191475.0
# Jorge Ramirez     9450206.0
# Maria Lopez       9242658.0
```

> El orden importa: `strip()` antes de `title()`, y el `replace` de espacios internos al final. `"maria lopez "` sin la última línea quedaría como `"Maria Lopez"`, con doble espacio, y sería un vendedor distinto de `"Maria Lopez"`.

---

### Solución 5 — Estrategia de nulos

```python
lim = v.copy()

# 1. el nulo TIENE significado -> constante
lim["discount"] = lim["discount"].fillna(0)

# 2. categórica -> etiqueta explícita (no inventar una ciudad real)
lim["city"] = lim["city"].fillna("Desconocida")

# 3. numéricas -> mediana DEL GRUPO, no la global
lim["units"] = lim["units"].fillna(lim.groupby("category")["units"].transform("median"))
lim["unit_price"] = lim["unit_price"].fillna(
    lim.groupby("product")["unit_price"].transform("median"))

# 4. recalcular la métrica derivada con los datos ya completos
lim["revenue"] = lim["units"] * lim["unit_price"] * (1 - lim["discount"])

lim.isna().sum().sum()
# 0
lim["revenue"].isna().sum()
# 0
```

> **La clave es `transform("median")`**: devuelve una Series alineada al DataFrame original con la mediana del grupo de cada fila. Imputar con la mediana global de `unit_price` mezclaría el precio de un shampoo con el de una bolsa de maní. Imputar por producto es mucho más defendible.
> 
> `"Desconocida"` como categoría explícita permite después medir cuánto ingreso viene de datos incompletos, en vez de esconderlo.

---

### Solución 6 — Resumen por ciudad

```python
r = (lim.groupby("city")
        .agg(total    = ("revenue", "sum"),
             pedidos  = ("order_id", "count"),
             ticket   = ("revenue", "mean"),
             unidades = ("units", "sum"))
        .round(1))

r.sort_values("total", ascending=False)
#                    total  pedidos    ticket  unidades
# city
# Barranquilla  23519693.5      144  163331.2    2838.0
# Bogota        20435352.0      133  153649.3    2604.0
# Medellin      16449647.0       89  184827.5    1908.0
# Cali          13206524.0       75  176087.0    1579.0
# Cartagena     10357220.5       57  181705.6    1135.0
# Desconocida    1514405.0       10  151440.5     143.0
```

> **Named aggregation** en acción: cuatro métricas, nombres controlados, resultado plano. La lectura interesante es que Barranquilla lidera en volumen pero Medellín tiene el mejor **ticket promedio**: son dos historias distintas y hacen falta las dos métricas para verlo.

---

### Solución 7 — Top 5 productos

```python
top = lim.groupby("product")["revenue"].sum().nlargest(5).round(0)
top
# product
# Gaseosa 1.5L     14710243.0
# Galletas          8864582.0
# Agua 600ml        8774971.0
# Jugo Mango        8466122.0
# Queso Costeno     6348389.0

(top / lim["revenue"].sum() * 100).round(1)
# Gaseosa 1.5L     17.2
# Galletas         10.4
# Agua 600ml       10.3
# Jugo Mango        9.9
# Queso Costeno     7.4
```

> `nlargest(5)` en vez de `sort_values(ascending=False).head(5)`: misma salida, expresa la intención y no ordena la tabla completa. El 5 productos concentran ~55% del ingreso.

---

### Solución 8 — Duplicados

```python
ventas.duplicated().sum()                    # filas idénticas en todas las columnas
# 8
ventas["order_id"].duplicated().sum()        # duplicados de la clave de negocio
# 8

dedup = ventas.drop_duplicates(subset=["order_id"], keep="first")
dedup.shape
# (500, 10)
dedup["order_id"].is_unique
# True
```

> Que los dos conteos coincidan confirma que son duplicados exactos, no dos versiones distintas del mismo pedido. Si `order_id` tuviera más duplicados que las filas completas, habría que decidir cuál versión conservar (`keep="last"` si lo nuevo corrige lo viejo).
> 
> **De aquí en adelante trabajamos sobre `lim2 = lim.drop_duplicates(subset=["order_id"])`**, por eso los totales de los ejercicios siguientes son algo menores que los del 6 y el 7. En un pipeline real, la deduplicación va **antes** de cualquier agregación.

---

### Solución 9 — Serie mensual

```python
lim2 = lim.drop_duplicates(subset=["order_id"])

mens = lim2.set_index("order_date").resample("ME")["revenue"].sum()
mens.head(3).round(0)
# order_date
# 2024-01-31    4836916.0
# 2024-02-29    4428806.0
# 2024-03-31    6936370.0
# Freq: ME

mens.idxmax().date(), round(mens.max(), 0)
# (datetime.date(2024, 3, 31), 6936370.0)

mens.pct_change().round(3).head(4)
# 2024-01-31      NaN      <- no hay mes anterior
# 2024-02-29   -0.084      -8.4%
# 2024-03-31    0.566     +56.6%
# 2024-04-30   -0.471     -47.1%
```

> `resample("ME")` exige la fecha como índice: de ahí el `set_index`. `idxmax()` devuelve la **etiqueta** del máximo (la fecha), no el valor — es lo que necesitas para responder "cuándo".
> 
> `pct_change()` en un dataset de 500 pedidos repartidos en 18 meses es muy volátil: con ~28 pedidos por mes, un solo pedido grande mueve la aguja. Antes de sacar conclusiones de una variación, revisa el `count`.

---

### Solución 10 — Tabla cruzada

```python
pt = lim2.pivot_table(index="city", columns="channel", values="revenue",
                      aggfunc="sum", margins=True)
pt.round(0)
# channel        Mayorista      Online      Tienda         All
# city
# Barranquilla   4949725.0   9931580.0   8311048.0  23192353.0
# Bogota         3636676.0   7807738.0   8693200.0  20137614.0
# Cali           3541678.0   5722072.0   3825324.0  13089074.0
# Cartagena      2701049.0   4333550.0   2759641.0   9794240.0
# Desconocida      16170.0   1344214.0    154021.0   1514405.0
# Medellin       3374404.0   6763555.0   6124176.0  16262135.0
# All           18219703.0  35902710.0  29867409.0  83989822.0

# porcentaje por FILA: cada ciudad suma 100%
pct = lim2.pivot_table(index="city", columns="channel", values="revenue", aggfunc="sum")
(pct.div(pct.sum(axis=1), axis=0) * 100).round(1)
# channel       Mayorista  Online  Tienda
# city
# Barranquilla       21.3    42.8    35.8
# Bogota             18.1    38.8    43.2
# Cali               27.1    43.7    29.2
# Cartagena          27.6    44.2    28.2
# Desconocida         1.1    88.8    10.2
# Medellin           20.8    41.6    37.7
```

> `div(..., axis=0)` divide cada fila por su total. Ese `axis=0` significa "alinea el divisor con el índice de las filas" — es el uso de `axis` que más cuesta interiorizar; si te sale al revés, prueba el otro.
> 
> Lectura: Bogotá es la única ciudad donde Tienda supera a Online. Los porcentajes por fila revelan diferencias de mezcla que los valores absolutos esconden.

---

### Solución 11 — Cruce con metas

```python
metas = pd.DataFrame({
    "city":   ["Barranquilla", "Bogota", "Medellin", "Cali", "Santa Marta"],
    "meta":   [20_000_000, 18_000_000, 14_000_000, 11_000_000, 5_000_000],
    "region": ["Caribe", "Andina", "Andina", "Pacifico", "Caribe"],
})

res = lim2.groupby("city", as_index=False)["revenue"].sum()
m = res.merge(metas, on="city", how="left", indicator=True)
m["cumplimiento_pct"] = (m["revenue"] / m["meta"] * 100).round(1)

m.round(0)
#            city     revenue        meta    region     _merge  cumplimiento_pct
# 0  Barranquilla  23192353.0  20000000.0    Caribe       both             116.0
# 1        Bogota  20137614.0  18000000.0    Andina       both             112.0
# 2          Cali  13089074.0  11000000.0  Pacifico       both             119.0
# 3     Cartagena   9794240.0         NaN       NaN  left_only               NaN
# 4   Desconocida   1514405.0         NaN       NaN  left_only               NaN
# 5      Medellin  16262135.0  14000000.0    Andina       both             116.0

m["_merge"].value_counts()
# both          4
# left_only     2      <- Cartagena y Desconocida no tienen meta
# right_only    0      <- Santa Marta se perdió por usar how="left"

m.groupby("region")["revenue"].sum().round(0)
# region
# Andina      36399749.0
# Caribe      23192353.0
# Pacifico    13089074.0
```

> Tres decisiones deliberadas:
> 
> - **`how="left"`** conserva todas las ciudades con ventas. Con `inner` habrías perdido silenciosamente los 11.3M de Cartagena y Desconocida.
> - **`indicator=True`** documenta qué no cruzó. Es el hábito que convierte un merge en una auditoría.
> - **Santa Marta desaparece** porque tiene meta pero cero ventas. Si el reporte debe mostrar metas incumplidas al 0%, usa `how="outer"`. Es una decisión de negocio, no técnica.
> 
> Ojo con `groupby("region")`: solo suma las 4 ciudades con región asignada. Los 11.3M sin región no aparecen en ningún lado. Un `assert` sobre el total evita que ese hueco pase inadvertido.

---

### Solución 12 — NumPy puro

```python
arr = lim2["revenue"].to_numpy()

round(arr.mean(), 2), round(arr.std(), 2)
# (167979.64, 131969.75)

z = (arr - arr.mean()) / arr.std()          # z-score, vectorizado y sin bucles

(np.abs(z) > 3).sum()                        # cuántos outliers
# 6

np.round(arr[np.abs(z) > 3], 0)              # cuáles
# array([ 687900.,  636148.,  727974.,  624360.,  653640., 1035360.])

np.round(np.percentile(arr, [25, 50, 75, 95]), 0)
# array([ 69197., 146458., 229028., 410022.])
```

> `(arr - arr.mean()) / arr.std()` es broadcasting puro: dos escalares se estiran sobre 500 elementos. En Python nativo serían dos bucles.
> 
> **El z-score asume una distribución aproximadamente normal**, y `revenue` está sesgada a la derecha (la media 168k está muy por encima de la mediana 146k). Por eso "solo" detecta 6 outliers extremos. El criterio del rango intercuartílico (`Q3 + 1.5·IQR`) es más apropiado para distribuciones asimétricas:
> 
> ```python
> q1, q3 = np.percentile(arr, [25, 75])
> iqr = q3 - q1
> (arr > q3 + 1.5 * iqr).sum()      # muchos más candidatos, criterio más sensible
> ```

---

### Solución 13 — Segmentación

```python
lim2 = lim2.copy()

condiciones = [lim2["revenue"] >= 300_000, lim2["revenue"] >= 100_000]
lim2["segmento"] = np.select(condiciones, ["Alto", "Medio"], default="Bajo")

lim2["segmento"].value_counts()
# segmento
# Medio    251
# Bajo     180
# Alto      69

(lim2.groupby("segmento")
     .agg(pedidos = ("order_id", "count"),
          total   = ("revenue", "sum"),
          pct     = ("revenue", lambda x: x.sum() / lim2["revenue"].sum() * 100))
     .round(1))
#           pedidos       total   pct
# segmento
# Alto           69  28550469.5  34.0
# Bajo          180   9026993.0  10.7
# Medio         251  46412359.0  55.3
```

> **`np.select` evalúa las condiciones en orden y se queda con la primera que sea `True`.** Por eso el orden importa: si pusieras la condición de 100.000 primero, nada caería nunca en "Alto". Es el equivalente vectorizado de un `if / elif / else`.
> 
> Resultado de negocio: el 13,8% de los pedidos (69 de 500) genera el 34% del ingreso. Un Pareto clásico, y una razón concreta para tratar distinto a ese segmento.

---

### Solución 14 — Dashboard

```python
import matplotlib.pyplot as plt

mens       = lim2.set_index("order_date").resample("ME")["revenue"].sum()
por_ciudad = lim2.groupby("city")["revenue"].sum().sort_values(ascending=False)
canales    = sorted(lim2["channel"].unique())
grupos     = [lim2.loc[lim2["channel"] == c, "revenue"].values for c in canales]

fig, axes = plt.subplots(2, 2, figsize=(13, 8))

# (0,0) evolución mensual
axes[0, 0].plot(mens.index, mens.values, marker="o", linewidth=2)
axes[0, 0].set_title("Evolución mensual del ingreso")
axes[0, 0].set_ylabel("Ingresos (COP)")
axes[0, 0].yaxis.set_major_formatter(lambda x, p: f"{x/1e6:.0f}M")
axes[0, 0].grid(alpha=0.3)
axes[0, 0].tick_params(axis="x", rotation=45)

# (0,1) ingreso por ciudad
axes[0, 1].bar(por_ciudad.index, por_ciudad.values, color="#2ca02c")
axes[0, 1].set_title("Ingreso por ciudad")
axes[0, 1].yaxis.set_major_formatter(lambda x, p: f"{x/1e6:.0f}M")
axes[0, 1].tick_params(axis="x", rotation=45)

# (1,0) distribución de precios
axes[1, 0].hist(lim2["unit_price"], bins=25, edgecolor="white", color="#ff7f0e")
axes[1, 0].set_title("Distribución de precio unitario")
axes[1, 0].set_xlabel("COP")
axes[1, 0].set_ylabel("Frecuencia")

# (1,1) ingreso por canal
axes[1, 1].boxplot(grupos, tick_labels=canales)
axes[1, 1].set_title("Ingreso por canal")
axes[1, 1].yaxis.set_major_formatter(lambda x, p: f"{x/1e6:.1f}M")

fig.suptitle("Dashboard de ventas 2024-2025", fontsize=15, fontweight="bold")
fig.tight_layout()
fig.savefig("dashboard_ventas.png", dpi=150, bbox_inches="tight")
plt.close(fig)
```

> Tres detalles que separan un gráfico presentable de uno improvisado:
> 
> - **`yaxis.set_major_formatter`** evita la notación científica (`1.0e7`), ilegible para cualquier persona de negocio.
> - **`fig.tight_layout()`** después de todos los `set_title`, o los títulos se encimarán.
> - **`plt.close(fig)`** libera memoria: sin él, un bucle que genera 200 gráficos se come toda la RAM.

---

### Solución 15 — ETL completo

```python
"""ETL de ventas: CSV -> limpieza -> transformación -> PostgreSQL -> gráfico."""
import logging
import os
from pathlib import Path

import matplotlib
matplotlib.use("Agg")            # backend sin pantalla: obligatorio en servidores
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from sqlalchemy import create_engine, text

logging.basicConfig(level=logging.INFO, format="%(asctime)s | %(levelname)s | %(message)s")
log = logging.getLogger("etl")

CSV_IN  = Path("ventas_raw.csv")
PNG_OUT = Path("resumen_ventas.png")
TABLA   = "resumen_ventas"


def get_engine():
    """Credenciales por variable de entorno, nunca en el código."""
    url = os.environ.get("DB_URL", "postgresql+psycopg2://analista:secreto@localhost:5432/ventas_db")
    return create_engine(url, pool_pre_ping=True)


# ---------- EXTRACT ----------
def extract(path: Path) -> pd.DataFrame:
    df = pd.read_csv(path, parse_dates=["order_date"],
                     na_values=["", "NA", "N/D", "-", "null"])
    log.info("EXTRACT | %d filas, %d columnas", *df.shape)
    return df


# ---------- TRANSFORM: limpieza ----------
def clean(df: pd.DataFrame) -> pd.DataFrame:
    n0 = len(df)
    df = df.drop_duplicates(subset=["order_id"], keep="first").copy()   # .copy() -> sin SettingWithCopyWarning
    log.info("CLEAN   | duplicados eliminados: %d", n0 - len(df))

    df["sales_rep"] = (df["sales_rep"].str.strip().str.title()
                         .str.replace(r"\s+", " ", regex=True))
    df["city"]       = df["city"].fillna("Desconocida")
    df["discount"]   = df["discount"].fillna(0)
    df["units"]      = df["units"].fillna(df.groupby("category")["units"].transform("median"))
    df["unit_price"] = df["unit_price"].fillna(
        df.groupby("product")["unit_price"].transform("median"))

    assert df["order_id"].is_unique,      "order_id duplicado tras la limpieza"
    assert df.isna().sum().sum() == 0,    "quedaron nulos tras la limpieza"
    log.info("CLEAN   | %d filas limpias, 0 nulos", len(df))
    return df


# ---------- TRANSFORM: derivadas ----------
def transform(df: pd.DataFrame) -> pd.DataFrame:
    df = df.assign(
        revenue = lambda d: (d["units"] * d["unit_price"] * (1 - d["discount"])).round(2),
        year    = lambda d: d["order_date"].dt.year,
        month   = lambda d: d["order_date"].dt.to_period("M").astype(str),
    )
    df["segmento"] = np.select(
        [df["revenue"] >= 300_000, df["revenue"] >= 100_000],
        ["Alto", "Medio"], default="Bajo")

    assert (df["revenue"] >= 0).all(), "revenue negativo"
    log.info("TRANSFORM | revenue total: %.0f", df["revenue"].sum())
    return df


def aggregate(df: pd.DataFrame) -> pd.DataFrame:
    agg = (df.groupby(["city", "channel"], as_index=False)
             .agg(revenue  = ("revenue", "sum"),
                  pedidos  = ("order_id", "count"),
                  ticket   = ("revenue", "mean"),
                  unidades = ("units", "sum")))
    agg[["revenue", "ticket"]] = agg[["revenue", "ticket"]].round(2)
    log.info("AGGREGATE | %d filas agregadas", len(agg))
    return agg


# ---------- LOAD ----------
def load(agg: pd.DataFrame, engine, tabla: str = TABLA) -> None:
    agg.to_sql(tabla, engine, if_exists="replace", index=False,
               chunksize=10_000, method="multi")
    with engine.connect() as conn:
        n = conn.execute(text(f"SELECT COUNT(*) FROM {tabla}")).scalar()
    assert n == len(agg), f"cargadas {n} de {len(agg)} filas"
    log.info("LOAD    | %d filas en la tabla %s", n, tabla)


# ---------- VALIDATE + PLOT ----------
def validate_and_plot(engine, png: Path, tabla: str = TABLA) -> pd.DataFrame:
    df = pd.read_sql(f"SELECT * FROM {tabla} ORDER BY revenue DESC", engine)   # releer desde la BD

    pt = df.pivot_table(index="city", columns="channel", values="revenue", aggfunc="sum")
    pt = pt.loc[pt.sum(axis=1).sort_values(ascending=False).index]

    fig, ax = plt.subplots(figsize=(10, 5))
    pt.plot(kind="barh", stacked=True, ax=ax)
    ax.set_title("Ingresos por ciudad y canal")
    ax.set_xlabel("Ingresos (COP)")
    ax.set_ylabel("")
    ax.xaxis.set_major_formatter(lambda x, p: f"{x/1e6:.0f}M")
    ax.legend(title="Canal")
    fig.tight_layout()
    fig.savefig(png, dpi=150, bbox_inches="tight")
    plt.close(fig)
    log.info("VALIDATE | gráfico guardado en %s", png)
    return df


def main():
    engine = get_engine()
    df  = extract(CSV_IN)
    df  = clean(df)
    df  = transform(df)
    agg = aggregate(df)
    load(agg, engine)
    final = validate_and_plot(engine, PNG_OUT)
    print(final.head(3).round(0))
    log.info("ETL completado")


if __name__ == "__main__":
    main()
```

**Salida de la ejecución:**

```
2026-08-02 18:30:23 | INFO | EXTRACT | 508 filas, 10 columnas
2026-08-02 18:30:23 | INFO | CLEAN   | duplicados eliminados: 8
2026-08-02 18:30:23 | INFO | CLEAN   | 500 filas limpias, 0 nulos
2026-08-02 18:30:23 | INFO | TRANSFORM | revenue total: 84195024
2026-08-02 18:30:23 | INFO | AGGREGATE | 18 filas agregadas
2026-08-02 18:30:23 | INFO | LOAD    | 18 filas en la tabla resumen_ventas
2026-08-02 18:30:24 | INFO | VALIDATE | gráfico guardado en resumen_ventas.png
           city channel    revenue  pedidos    ticket  unidades
0  Barranquilla  Online  9958776.0       63  158076.0    1263.0
1        Bogota  Tienda  8731127.0       53  164738.0    1059.0
2  Barranquilla  Tienda  8311048.0       45  184690.0     931.0
2026-08-02 18:30:24 | INFO | ETL completado
```

**Los siete principios que hacen que este script sea de producción y no un notebook disfrazado:**

1. **Una función por etapa.** Cada una recibe un DataFrame y devuelve un DataFrame. Se pueden probar por separado.
2. **`logging` en vez de `print`.** Con marca de tiempo, nivel y destino configurable. En un `cron` a las 3 a.m. es la única forma de saber qué pasó.
3. **`assert` en las fronteras.** Fallar temprano y ruidosamente vale más que cargar datos corruptos en silencio.
4. **Credenciales por variable de entorno.** Nunca en el código, nunca en Git.
5. **Deduplicar primero.** Antes de imputar y antes de agregar; si no, los duplicados se propagan a todos los totales.
6. **Releer desde la base para validar.** Confirma que lo que quedó guardado es lo que creías haber guardado.
7. **`matplotlib.use("Agg")`.** Sin él, el script muere en un servidor sin entorno gráfico.

> Para probarlo sin PostgreSQL instalado: `export DB_URL="sqlite:///etl_local.db"` y el mismo código funciona sin cambiar una línea. Ese es todo el valor de SQLAlchemy.

---

## 13. Ruta de estudio

No intentes leer esto de corrido. Está diseñado para tres pasadas.

### Día 1 — Poder mínimo viable (3–4 horas)

**Objetivo:** cargar un CSV, mirarlo y filtrarlo. Con esto ya resuelves preguntas reales.

|Leer|Practicar|
|---|---|
|[[#1. Prerrequisitos de Python\|1. Prerrequisitos]] — el checklist, honestamente||
|[[#2. El dataset del documento\|2. El dataset]] — ejecútalo, no lo leas||
|[[#3.1 Por qué NumPy y no listas\|3.1–3.6]] NumPy: creación, indexación, máscaras, broadcasting||
|[[#4.1 Series y DataFrame\|4.1–4.5]] pandas: estructuras, lectura, inspección, `loc`/`iloc`, filtros|**Ejercicios 1, 2, 3**|

**Regla del día 1:** memoriza el ritual `shape → head → info → describe → nunique`. Ejecútalo con cada dataset nuevo, siempre, sin excepción.

---

### Semana 1 — Análisis real (4–5 sesiones de 2 horas)

**Objetivo:** limpiar datos sucios, agregar y responder preguntas de negocio.

|Sesión|Leer|Practicar|
|---|---|---|
|2|[[#4.6 Valores nulos\|4.6–4.8]] nulos, tipos, columnas derivadas · [[#8. Errores frecuentes\|8.1, 8.2, 8.4]]|**Ejercicios 4, 5**|
|3|[[#4.9 groupby y agregaciones\|4.9]] groupby — la sección más importante de la guía · [[#4.10 Ordenamiento\|4.10]]|**Ejercicios 6, 7**|
|4|[[#4.14 Duplicados\|4.14]] duplicados · [[#4.15 Texto con el accesor .str\|4.15]] texto · [[#3.7 Agregaciones por eje\|3.7–3.9]] NumPy restante|**Ejercicios 8, 12**|
|5|[[#6.1 Figura y ejes\|6.1–6.2]] Matplotlib: figura, ejes y los cinco gráficos|Grafica los resultados de los ejercicios 6 y 7|

**Al final de la semana 1** deberías poder tomar cualquier CSV desconocido y producir, en menos de una hora, un resumen limpio con métricas por categoría y un gráfico.

---

### Semana 2 — Pipelines (4–5 sesiones)

**Objetivo:** cruzar fuentes, trabajar con tiempo y automatizar el proceso completo.

|Sesión|Leer|Practicar|
|---|---|---|
|6|[[#4.11 merge, join y concat\|4.11]] merge · [[#8. Errores frecuentes\|8.3]] filas multiplicadas|**Ejercicio 11**|
|7|[[#4.12 Fechas\|4.12]] fechas · [[#4.13 pivot_table y melt\|4.13]] pivot y melt|**Ejercicios 9, 10**|
|8|[[#4.16 apply frente a operaciones vectorizadas\|4.16]] apply vs vectorización · [[#5.1 Core vs ORM\|5.1–5.4]] SQLAlchemy|**Ejercicio 13**|
|9|[[#6.3 Subplots\|6.3–6.4]] subplots y estilo · [[#7. Seaborn\|7. Seaborn]]|**Ejercicio 14**|
|10|Repaso de [[#8. Errores frecuentes\|8. Errores frecuentes]] completo|**Ejercicio 15** — el ETL completo|

---

### Después

- **[[#10. Menciones honoríficas|10. Menciones honoríficas]]** — no las estudies. Léelas una vez para saber que existen, y vuelve cuando tengas el problema.
- **[[#9. Glosario|9. Glosario]]** — repásalo antes de cualquier entrevista técnica. El vocabulario correcto es la mitad de la impresión que dejas.
- **Consulta rápida** — las tablas resumen de cada librería ([[#3.10 Tabla resumen NumPy|NumPy]], [[#4.17 Tabla resumen pandas|pandas]], [[#5.6 Tabla resumen SQLAlchemy|SQLAlchemy]], [[#6.5 Tabla resumen Matplotlib|Matplotlib]], [[#7.1 Tabla resumen Seaborn|Seaborn]]) están hechas para tenerlas abiertas mientras trabajas.

### Tres advertencias finales

1. **No memorices la API.** Nadie recuerda los argumentos de `pivot_table`. Lo que sí hay que tener interiorizado es _qué operación existe_ para cada problema: cuando sabes que lo tuyo es un `groupby` + `transform`, la sintaxis se busca en treinta segundos.
    
2. **Escribe `assert` desde el primer día.** `assert df["id"].is_unique` cuesta una línea y te ahorra descubrir tres semanas después que un reporte estaba inflado por un merge mal hecho.
    
3. **La limpieza es el 70% del trabajo.** Modelar y graficar es la parte visible; lo que decide si el resultado es correcto son las decisiones sobre nulos, duplicados y tipos. Documenta cada una de esas decisiones — sobre todo por qué imputaste con la mediana del grupo y no con la global.
    

---

_Fin de la guía. Todos los ejemplos y salidas fueron verificados ejecutando el código con Python 3.12, NumPy 2.x, pandas 2.2.3, SQLAlchemy 2.0, Matplotlib 3.10 y Seaborn 0.13._