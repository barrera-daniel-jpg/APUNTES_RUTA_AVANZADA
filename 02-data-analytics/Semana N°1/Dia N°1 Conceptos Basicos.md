# Apuntes — Iniciación a Ciencia, Ingeniería y Análisis de Datos

> Clase de iniciación · Modelado de datos · OLTP y OLAP · Python · pandas
> Formato: conceptos → relaciones → ejemplos → glosario → preguntas de repaso

---

## 0. Mapa general: ¿de qué va todo esto?

Antes de entrar en detalle, hay que ubicar las tres disciplinas que suelen mezclarse. Comparten herramientas, pero **responden preguntas distintas**.

| Disciplina | Pregunta que responde | Qué produce | Herramientas típicas |
|---|---|---|---|
| **Ingeniería de datos** | ¿Cómo llevo los datos desde donde nacen hasta donde se consumen, de forma confiable? | Pipelines, bases de datos, data warehouses | SQL, Python, Airflow, Spark, PostgreSQL |
| **Análisis de datos** | ¿Qué pasó y por qué? | Reportes, dashboards, KPIs | SQL, Excel, pandas, Power BI, Tableau |
| **Ciencia de datos** | ¿Qué va a pasar y qué debería hacer? | Modelos predictivos, experimentos | Python, scikit-learn, estadística, ML |

**La cadena de valor del dato:**

```
Fuente → Ingesta → Almacenamiento → Modelado → Transformación → Análisis → Decisión
  (app)   (ETL)      (BD / DWH)      (esquema)    (pandas/SQL)    (BI/ML)   (negocio)
   |________________|                 |___________________________|
     Ingeniería de datos                Análisis / Ciencia de datos
```

> **Idea clave:** el dato no vale por existir. Vale cuando **reduce la incertidumbre de una decisión**. Todo lo demás (modelado, pipelines, pandas) es infraestructura para llegar ahí.

**Jerarquía DIKW** — la escalera que sube el valor:

```
Dato       →  "38"                          (hecho crudo, sin contexto)
Información→  "38 atletas activos en julio"  (dato + contexto)
Conocimiento→ "los activos suben 20% tras el torneo" (patrón)
Sabiduría  →  "abro inscripciones después de cada torneo" (acción)
```

---

## 1. Modelado de datos

### 1.1 Definición

**Modelado de datos** es el proceso de definir *qué información se guarda, con qué estructura, con qué reglas y cómo se relacionan las piezas entre sí*, antes de escribir una sola línea de código de base de datos.

Es el equivalente al plano de un edificio: puedes construir sin plano, pero pagarás el error tres veces más caro después.

### 1.2 Los tres niveles del modelado

Este es el esquema que casi siempre se pregunta en evaluación:

| Nivel | Pregunta que responde | Detalle | Audiencia | Ejemplo |
|---|---|---|---|---|
| **Conceptual** | ¿Qué entidades existen en el negocio? | Bajo | Negocio / cliente | "Un Entrenador tiene muchos Atletas" |
| **Lógico** | ¿Qué atributos, llaves y relaciones tiene cada entidad? | Medio | Analista / arquitecto | `atleta(id, nombre, fecha_nac, entrenador_id)` |
| **Físico** | ¿Cómo se implementa en un motor concreto? | Alto | Desarrollador / DBA | `CREATE TABLE atleta (id SERIAL PRIMARY KEY, ...)` con tipos, índices, constraints |

```
CONCEPTUAL  →  LÓGICO  →  FÍSICO
 (qué)          (cómo)     (con qué motor)
 agnóstico      agnóstico   PostgreSQL / MySQL / Mongo
```

### 1.3 Piezas del modelo

- **Entidad**: cosa del mundo real sobre la que guardo datos → se vuelve una **tabla**.
- **Atributo**: propiedad de la entidad → se vuelve una **columna**.
- **Registro / tupla**: una ocurrencia concreta → una **fila**.
- **Llave primaria (PK)**: identifica de forma única cada fila. No se repite, no es nula.
- **Llave foránea (FK)**: columna que apunta a la PK de otra tabla. Es lo que **crea la relación**.
- **Cardinalidad**: cuántas instancias de A se relacionan con cuántas de B.

**Cardinalidades:**

| Tipo | Notación | Ejemplo | Implementación |
|---|---|---|---|
| Uno a uno | 1:1 | Usuario ↔ Perfil | FK con `UNIQUE` en cualquiera de las dos |
| Uno a muchos | 1:N | Entrenador → Atletas | FK en el lado "muchos" |
| Muchos a muchos | N:M | Atletas ↔ Rutinas | **Tabla intermedia** (pivote) |

> La N:M **nunca** se implementa directo. Siempre necesita una tabla puente: `atleta_rutina(atleta_id, rutina_id, fecha_asignacion)`.

### 1.4 Normalización (repaso rápido)

Proceso de organizar tablas para **eliminar redundancia** y evitar anomalías de inserción, actualización y borrado.

| Forma normal | Regla | Ejemplo de violación |
|---|---|---|
| **1FN** | Valores atómicos, sin grupos repetidos | Columna `telefonos = "300111, 300222"` |
| **2FN** | 1FN + todo atributo no clave depende de la PK **completa** | En `(pedido_id, producto_id) → nombre_producto`, el nombre depende solo de producto |
| **3FN** | 2FN + no hay dependencias transitivas entre no-clave | `ciudad → departamento` dentro de la tabla `atleta` |

**Regla mnemotécnica:** *"la clave, toda la clave y nada más que la clave"*.

**Anomalías que evita:**
- *Inserción*: no puedo registrar un entrenador sin atletas.
- *Actualización*: cambio el nombre de una ciudad y queda inconsistente en 300 filas.
- *Eliminación*: borro el último atleta y pierdo los datos del entrenador.

### 1.5 Modelo relacional vs modelo dimensional

Aquí está la bisagra que conecta con OLTP/OLAP:

| | **Relacional (normalizado)** | **Dimensional (desnormalizado)** |
|---|---|---|
| Objetivo | Integridad y escritura rápida | Lectura y agregación rápida |
| Forma | 3FN, muchas tablas pequeñas | Tabla de hechos + dimensiones |
| Redundancia | Mínima | Aceptada a propósito |
| Uso | OLTP (la app) | OLAP (el análisis) |

**Esquema estrella (star schema):**

```
        dim_tiempo        dim_atleta
             \                /
              \              /
            [ hecho_entrenamiento ]      ← métricas: series, repeticiones, duración
              /              \
             /                \
      dim_ejercicio       dim_entrenador
```

- **Tabla de hechos**: eventos medibles, muchas filas, contiene métricas numéricas + FKs.
- **Tablas de dimensión**: contexto descriptivo (quién, qué, cuándo, dónde), pocas filas, muchos atributos.
- **Copo de nieve (snowflake)**: igual, pero las dimensiones se normalizan en sub-tablas. Menos redundancia, más JOINs, más lento.

---

## 2. OLTP vs OLAP

### 2.1 OLTP — *Online Transaction Processing*

Sistemas que **operan el negocio en tiempo real**. Registran transacciones: una venta, un login, una inscripción, una rutina asignada.

- Muchas operaciones pequeñas y concurrentes (INSERT, UPDATE, DELETE).
- Optimizado para **escritura** y para leer pocas filas por consulta.
- Datos **actuales y detallados**.
- Modelo **normalizado** (3FN).
- Ejemplo: la base de datos que está detrás de una app web mientras un usuario la usa.

### 2.2 OLAP — *Online Analytical Processing*

Sistemas que **analizan el negocio**. Responden preguntas agregadas sobre históricos.

- Pocas consultas, pero **enormes**: escanean millones de filas y agregan.
- Optimizado para **lectura**.
- Datos **históricos y agregados**.
- Modelo **dimensional** (estrella).
- Ejemplo: el data warehouse que alimenta un dashboard de Power BI.

### 2.3 Tabla comparativa (memorizar)

| Criterio | **OLTP** | **OLAP** |
|---|---|---|
| Propósito | Operar | Analizar |
| Operación típica | INSERT / UPDATE | SELECT con agregaciones |
| Consulta típica | "Dame el atleta id=42" | "Promedio de asistencia por mes y categoría" |
| Volumen por consulta | Pocas filas | Millones de filas |
| Usuarios | Miles (clientes, empleados) | Decenas (analistas, gerencia) |
| Diseño | Normalizado (3FN) | Desnormalizado (estrella) |
| Tiempo de respuesta | Milisegundos | Segundos a minutos |
| Datos | Actuales, detallados | Históricos, agregados |
| Métrica de éxito | Transacciones/segundo | Tiempo de respuesta de la consulta |
| Almacenamiento | Orientado a **filas** | Orientado a **columnas** |
| Backup | Crítico (pérdida = dinero) | Recuperable desde OLTP |
| Ejemplos | PostgreSQL, MySQL, Oracle | Snowflake, BigQuery, Redshift, ClickHouse |

### 2.4 El puente entre los dos: ETL / ELT

Los datos nacen en OLTP y viajan al OLAP.

```
OLTP  ──Extract──►  Staging  ──Transform──►  Load  ──►  Data Warehouse (OLAP)
```

| | **ETL** | **ELT** |
|---|---|---|
| Orden | Extraer → Transformar → Cargar | Extraer → Cargar → Transformar |
| Dónde transforma | Servidor intermedio | Dentro del warehouse |
| Época | Clásico (on-premise) | Moderno (nube, cómputo barato) |

**Conceptos asociados:**
- **Data Warehouse**: repositorio central, estructurado, para análisis. Datos ya limpios (*schema-on-write*).
- **Data Lake**: repositorio de datos crudos en cualquier formato (*schema-on-read*). Barato, flexible, riesgo de volverse "data swamp".
- **Data Mart**: subconjunto del warehouse orientado a un área (ventas, RRHH).
- **Data Lakehouse**: híbrido moderno (Databricks, Delta Lake).

### 2.5 Por qué el almacenamiento columnar importa

```
Fila (OLTP):     [1, Ana, 25, Bqla][2, Luis, 31, Cali][3, Sara, 19, Bogotá]
                  ↑ leer un registro completo es barato

Columna (OLAP):  [1,2,3][Ana,Luis,Sara][25,31,19][Bqla,Cali,Bogotá]
                                        ↑ AVG(edad) lee SOLO este bloque
```

Por eso una consulta como `SELECT AVG(edad) FROM millones_de_filas` es dramáticamente más rápida en un motor columnar: no toca las columnas que no necesita.

---

## 3. Python

### 3.1 Qué es

**Python** es un lenguaje de programación de **alto nivel**, **interpretado**, de **tipado dinámico** y **propósito general**, creado por Guido van Rossum (1991). Su filosofía prioriza la legibilidad ("*readability counts*").

**Características:**
- **Interpretado**: se ejecuta línea a línea, sin compilación previa a binario.
- **Tipado dinámico**: no declaras el tipo, se infiere en tiempo de ejecución.
- **Multiparadigma**: imperativo, orientado a objetos, funcional.
- **Indentación significativa**: los espacios definen los bloques (no llaves).
- **Baterías incluidas**: librería estándar amplia + ecosistema PyPI gigantesco.

### 3.2 Por qué Python domina el mundo de datos

1. **Curva de entrada baja** → el analista no necesita ser ingeniero de software.
2. **Ecosistema**: NumPy, pandas, scikit-learn, matplotlib, TensorFlow, PyTorch.
3. **Pegamento**: conecta bases de datos, APIs, archivos y modelos con poco código.
4. **Comunidad**: cualquier error que tengas, alguien ya lo tuvo y lo respondió.
5. **Notebooks**: Jupyter permite explorar de forma iterativa, ver resultados al instante.

> Comparación honesta: **SQL** sigue siendo insustituible para consultar bases de datos; **Python** brilla cuando necesitas transformar, modelar o automatizar más allá de lo que SQL permite. No compiten, se complementan.

### 3.3 Sintaxis mínima de supervivencia

```python
# Variables — sin declaración de tipo
nombre = "Daniel"          # str
edad = 25                  # int
altura = 1.78              # float
activo = True              # bool
nada = None                # NoneType

# Estructuras de datos
lista  = [1, 2, 3]                    # ordenada, mutable
tupla  = (1, 2, 3)                    # ordenada, inmutable
conj   = {1, 2, 3}                    # sin orden, sin duplicados
dicc   = {"nombre": "Ana", "edad": 25}  # clave → valor

# Control de flujo
if edad >= 18:
    print("Mayor de edad")
elif edad >= 13:
    print("Adolescente")
else:
    print("Menor")

# Bucles
for i in range(3):
    print(i)              # 0, 1, 2

for clave, valor in dicc.items():
    print(clave, valor)

# Funciones
def promedio(valores):
    return sum(valores) / len(valores)

promedio([10, 20, 30])    # 20.0

# List comprehension — muy común en datos
cuadrados = [x**2 for x in range(5)]           # [0, 1, 4, 9, 16]
pares     = [x for x in range(10) if x % 2 == 0]  # [0, 2, 4, 6, 8]
```

### 3.4 El ecosistema de datos en Python

| Librería | Para qué sirve | Analogía |
|---|---|---|
| **NumPy** | Arreglos numéricos n-dimensionales, álgebra lineal | El motor matemático |
| **pandas** | Tablas etiquetadas, limpieza, agregación | El Excel programable |
| **Matplotlib** | Gráficos base | El lápiz |
| **Seaborn** | Gráficos estadísticos bonitos sobre matplotlib | El lápiz con estilo |
| **scikit-learn** | Machine learning clásico | La caja de modelos |
| **SQLAlchemy / psycopg2** | Conexión a bases de datos | El cable a PostgreSQL |
| **Jupyter** | Cuadernos interactivos | El laboratorio |
| **Anaconda** | Distribución que trae todo lo anterior preinstalado | El kit completo |

> **Dependencia clave:** pandas está construido *encima* de NumPy. Cuando pandas es rápido, es porque por debajo NumPy está ejecutando código C vectorizado, no bucles de Python.

---

## 4. pandas

### 4.1 Qué es

**pandas** (de *panel data*, término econométrico) es la librería estándar de Python para **manipulación y análisis de datos tabulares**. Creada por Wes McKinney en 2008 en un fondo de inversión, porque necesitaba en Python lo que R ya ofrecía.

Piensa en ella como **una hoja de cálculo controlada por código**: mismas operaciones que harías en Excel (filtrar, ordenar, tabla dinámica, buscarv), pero reproducibles, versionables y escalables a millones de filas.

### 4.2 Las dos estructuras fundamentales

**Series** — un arreglo unidimensional etiquetado (una columna):

```python
import pandas as pd

s = pd.Series([10, 20, 30], index=["a", "b", "c"])
# a    10
# b    20
# c    30
# dtype: int64
```

**DataFrame** — tabla bidimensional (un conjunto de Series que comparten índice):

```python
df = pd.DataFrame({
    "nombre":     ["Ana", "Luis", "Sara", "Diego"],
    "edad":       [25, 31, 19, 28],
    "ciudad":     ["Barranquilla", "Cali", "Bogotá", "Barranquilla"],
    "asistencias":[12, 8, 15, 3]
})
```

```
   nombre  edad        ciudad  asistencias
0     Ana    25  Barranquilla           12
1    Luis    31          Cali            8
2    Sara    19        Bogotá           15
3   Diego    28  Barranquilla            3
   ↑                                     
 índice
```

**Anatomía:**
- **Índice (`df.index`)**: etiquetas de las filas.
- **Columnas (`df.columns`)**: etiquetas de las columnas.
- **Valores (`df.values`)**: el array de NumPy por debajo.
- **dtype**: tipo de cada columna (`int64`, `float64`, `object`, `datetime64`, `bool`, `category`).

### 4.3 Flujo de trabajo típico

```
1. Cargar     → read_csv, read_excel, read_sql, read_json
2. Explorar   → head, info, describe, shape, dtypes
3. Limpiar    → dropna, fillna, drop_duplicates, astype, rename
4. Transformar→ filtros, columnas nuevas, apply, map
5. Agregar    → groupby, pivot_table, merge
6. Exportar   → to_csv, to_excel, to_sql
```

### 4.4 Operaciones esenciales con ejemplos

**Cargar y explorar:**

```python
df = pd.read_csv("atletas.csv")
df = pd.read_excel("datos.xlsx", sheet_name="Hoja1")
df = pd.read_sql("SELECT * FROM atleta", con=conexion)

df.head(5)        # primeras 5 filas
df.tail(3)        # últimas 3
df.shape          # (filas, columnas) → (4, 4)
df.info()         # tipos, nulos, memoria
df.describe()     # estadísticos de columnas numéricas
df.columns        # nombres de columnas
df["ciudad"].unique()        # valores distintos
df["ciudad"].value_counts()  # frecuencia de cada valor
df.isnull().sum()            # nulos por columna
```

**Selección:**

```python
df["edad"]                    # una columna → Series
df[["nombre", "edad"]]        # varias columnas → DataFrame

df.loc[0]                     # fila por ETIQUETA
df.iloc[0]                    # fila por POSICIÓN
df.loc[0:2, ["nombre","edad"]]  # filas y columnas por etiqueta
```

> **Diferencia clave que siempre cae en examen:** `loc` usa **etiquetas** (y el rango es inclusivo en ambos extremos), `iloc` usa **posiciones enteras** (rango excluyente al final, como en Python).

**Filtrado (boolean masking):**

```python
df[df["edad"] > 24]

# Varias condiciones: paréntesis obligatorios, & y | en vez de and/or
df[(df["edad"] > 20) & (df["ciudad"] == "Barranquilla")]

df[df["ciudad"].isin(["Cali", "Bogotá"])]
df[df["nombre"].str.startswith("A")]
```

**Columnas nuevas:**

```python
df["mayor_edad"] = df["edad"] >= 18
df["categoria"]  = df["asistencias"].apply(
    lambda x: "Alta" if x >= 10 else "Baja"
)
```

**Limpieza:**

```python
df.dropna()                          # elimina filas con nulos
df.fillna(0)                         # rellena con 0
df["edad"].fillna(df["edad"].mean()) # imputa con la media
df.drop_duplicates()
df.rename(columns={"nombre": "atleta"})
df["edad"] = df["edad"].astype(int)
```

**Agrupación — el corazón del análisis:**

```python
df.groupby("ciudad")["asistencias"].mean()

# ciudad
# Barranquilla     7.5
# Bogotá          15.0
# Cali             8.0

# Varias métricas a la vez
df.groupby("ciudad").agg(
    total_atletas = ("nombre", "count"),
    edad_promedio = ("edad", "mean"),
    max_asist     = ("asistencias", "max")
)
```

> **Patrón split–apply–combine:** groupby *divide* el DataFrame por los valores de la clave, *aplica* una función a cada grupo y *combina* los resultados. Es el equivalente exacto de `GROUP BY` en SQL.

**Unir tablas:**

```python
pd.merge(atletas, entrenadores, on="entrenador_id", how="left")
# how: 'inner' | 'left' | 'right' | 'outer'  ← igual que los JOIN de SQL

pd.concat([df_enero, df_febrero])   # apilar verticalmente
```

**Tabla dinámica:**

```python
df.pivot_table(
    index="ciudad",
    columns="categoria",
    values="asistencias",
    aggfunc="mean"
)
```

**Ordenar y exportar:**

```python
df.sort_values("edad", ascending=False)
df.to_csv("resultado.csv", index=False)
df.to_excel("resultado.xlsx", index=False)
```

### 4.5 Equivalencias SQL ↔ pandas

| SQL | pandas |
|---|---|
| `SELECT col1, col2` | `df[["col1","col2"]]` |
| `WHERE edad > 20` | `df[df["edad"] > 20]` |
| `ORDER BY edad DESC` | `df.sort_values("edad", ascending=False)` |
| `GROUP BY ciudad` | `df.groupby("ciudad")` |
| `JOIN` | `pd.merge(a, b, on="id", how="inner")` |
| `LIMIT 5` | `df.head(5)` |
| `COUNT(DISTINCT x)` | `df["x"].nunique()` |
| `UNION ALL` | `pd.concat([a, b])` |

### 4.6 Sobre `df.query()` y el uso práctico de pandas

Anoté "quemar pandas" en clase; lo más probable es que se refiriera a **`query()` en pandas** o al hecho de *poner a trabajar* pandas. Cubro ambas cosas por si acaso.

**`df.query()`** — filtrar escribiendo la condición como texto, estilo SQL:

```python
df.query("edad > 24 and ciudad == 'Barranquilla'")

# Equivale exactamente a:
df[(df["edad"] > 24) & (df["ciudad"] == "Barranquilla")]

# Se puede referenciar una variable de Python con @
edad_min = 20
df.query("edad >= @edad_min")
```

Ventajas: más legible con condiciones largas, sin paréntesis anidados. Desventaja: no todos los editores lo autocompletan y no funciona con nombres de columna que tengan espacios (salvo con acentos graves: `` df.query("`nombre completo` == 'Ana'") ``).

**Instalar y arrancar pandas (por si era eso):**

```bash
pip install pandas
# o con Anaconda ya viene instalado
```

```python
import pandas as pd   # el alias "pd" es una convención universal
print(pd.__version__)
```

**Nota de rendimiento — "quemar" máquina con pandas:** pandas carga **todo en memoria RAM**. Con un archivo de 2 GB en un equipo de 6 GB de RAM, se satura. Alternativas: leer por trozos (`pd.read_csv(..., chunksize=100000)`), usar solo las columnas necesarias (`usecols=`), convertir a `category` los strings repetidos, o pasar a **Polars** / **Dask** / hacer la agregación en SQL antes de traer los datos.

---

## 5. Relaciones entre todos los conceptos

```
                        MODELADO DE DATOS
                   (define la estructura)
                          /        \
                         /          \
              relacional/3FN      dimensional/estrella
                     |                    |
                     ▼                    ▼
                  OLTP  ──── ETL/ELT ──►  OLAP
              (opera el negocio)      (analiza el negocio)
                     |                    |
                     └────────┬───────────┘
                              ▼
                     PYTHON (lenguaje)
                              |
                    ┌─────────┴──────────┐
                    ▼                    ▼
                 NumPy  ──base de──►  pandas
                                         |
                              ┌──────────┼──────────┐
                              ▼          ▼          ▼
                          limpieza   groupby     merge
                                         |
                                         ▼
                              ANÁLISIS / VISUALIZACIÓN / ML
                                         |
                                         ▼
                                    DECISIÓN
```

**Cómo se encadenan en la práctica (un ejemplo real):**

1. Una app registra asistencias de atletas → escribe en **PostgreSQL (OLTP)**, con un modelo **normalizado en 3FN**.
2. Cada noche, un proceso **ETL** en **Python** extrae esos datos.
3. Los carga en un **data warehouse** con **modelo estrella**: `hecho_asistencia` + `dim_atleta` + `dim_tiempo`.
4. Un analista abre **Jupyter**, usa **pandas** con `read_sql`, hace `groupby` por mes y categoría.
5. Grafica con **Matplotlib**, arma el dashboard, y el entrenador decide reforzar la categoría con más deserción.

---

## 6. Glosario

| Término | Definición |
|---|---|
| **Agregación** | Operación que resume muchas filas en un valor (SUM, AVG, COUNT, MAX). |
| **Anaconda** | Distribución de Python con librerías de datos preinstaladas y gestor de entornos `conda`. |
| **Atributo** | Propiedad de una entidad; en la tabla, una columna. |
| **Cardinalidad** | Número de instancias que se relacionan entre dos entidades (1:1, 1:N, N:M). |
| **Columnar** | Almacenamiento por columnas, óptimo para consultas analíticas. |
| **Data Lake** | Repositorio de datos crudos en cualquier formato, sin esquema previo. |
| **Data Mart** | Subconjunto de un data warehouse orientado a un área del negocio. |
| **Data Warehouse (DWH)** | Repositorio central de datos históricos, limpios y estructurados para análisis. |
| **DataFrame** | Estructura tabular bidimensional de pandas. |
| **Desnormalización** | Introducir redundancia a propósito para acelerar lecturas. |
| **DIKW** | Dato → Información → Conocimiento → Sabiduría. |
| **dtype** | Tipo de dato de una columna en pandas (`int64`, `object`, `datetime64`...). |
| **Entidad** | Objeto del mundo real modelado como tabla. |
| **ETL / ELT** | Extract-Transform-Load / Extract-Load-Transform. Procesos de movimiento de datos. |
| **Esquema estrella** | Modelo dimensional: una tabla de hechos rodeada de dimensiones. |
| **Esquema copo de nieve** | Estrella con dimensiones normalizadas en sub-tablas. |
| **FK (Foreign Key)** | Columna que referencia la PK de otra tabla; materializa la relación. |
| **Granularidad** | Nivel de detalle de una fila en la tabla de hechos (por venta, por día, por mes). |
| **groupby** | Método de pandas que aplica el patrón split-apply-combine. |
| **Hecho** | Evento medible del negocio; fila de la tabla de hechos. |
| **iloc / loc** | Selección por posición entera / por etiqueta en pandas. |
| **Índice (pandas)** | Etiquetas que identifican las filas de un DataFrame. |
| **Índice (BD)** | Estructura que acelera búsquedas en una tabla. |
| **Jupyter Notebook** | Entorno interactivo que mezcla código, resultados y texto. |
| **JOIN / merge** | Combinar dos tablas por una clave común. |
| **Modelado de datos** | Definir estructura, reglas y relaciones de la información. |
| **Normalización** | Organizar tablas para eliminar redundancia (1FN, 2FN, 3FN). |
| **NumPy** | Librería de arreglos numéricos; base de pandas. |
| **OLAP** | Procesamiento analítico en línea; lectura y agregación de históricos. |
| **OLTP** | Procesamiento transaccional en línea; operaciones del día a día. |
| **pandas** | Librería de Python para manipulación de datos tabulares. |
| **Pipeline** | Secuencia automatizada de pasos que mueve y transforma datos. |
| **PK (Primary Key)** | Identificador único de cada fila. |
| **Pivot table** | Tabla dinámica: reorganiza datos cruzando filas y columnas con agregación. |
| **query()** | Método de pandas para filtrar con una expresión tipo texto. |
| **Series** | Estructura unidimensional etiquetada de pandas. |
| **Schema-on-read / write** | Definir el esquema al leer (lake) o al escribir (warehouse). |
| **Split-apply-combine** | Patrón: dividir por grupos, aplicar función, combinar resultados. |
| **Tabla de dimensión** | Contexto descriptivo (quién, qué, cuándo, dónde). |
| **Vectorización** | Operar sobre arreglos completos en vez de iterar fila por fila. Mucho más rápido. |

---

## 7. Preguntas de repaso

### Nivel básico

**1. ¿Cuál es la diferencia entre modelo conceptual, lógico y físico?**
El conceptual define *qué* entidades existen (lenguaje de negocio); el lógico añade atributos, llaves y relaciones sin comprometerse con un motor; el físico implementa en un DBMS concreto con tipos, índices y constraints.

**2. ¿Por qué normalizamos?**
Para eliminar redundancia y evitar anomalías de inserción, actualización y eliminación. Cada dato vive en un solo lugar.

**3. ¿Cuál es la diferencia central entre OLTP y OLAP?**
OLTP opera el negocio (muchas escrituras pequeñas, datos actuales, normalizado). OLAP analiza el negocio (pocas lecturas enormes, datos históricos, desnormalizado).

**4. ¿Qué es un DataFrame?**
Estructura tabular bidimensional de pandas, con índice de filas y columnas etiquetadas, donde cada columna es una Series con su propio dtype.

**5. ¿Diferencia entre `loc` e `iloc`?**
`loc` selecciona por etiqueta (rango inclusivo); `iloc` por posición entera (rango excluyente al final).

### Nivel intermedio

**6. ¿Por qué un data warehouse se desnormaliza si en clase de bases de datos nos enseñaron a normalizar?**
Porque los objetivos son opuestos. En OLTP la prioridad es integridad y escritura rápida → normalizar. En OLAP la prioridad es lectura rápida sobre millones de filas → cada JOIN cuesta, así que se acepta redundancia para evitarlos. No es contradicción, es optimizar para el patrón de acceso dominante.

**7. ¿Qué hace `groupby` internamente?**
Split-apply-combine: divide el DataFrame según los valores de la clave, aplica la función de agregación a cada grupo, y combina los resultados en un nuevo objeto.

**8. ¿Por qué pandas es rápido si Python es lento?**
Porque delega en NumPy, que ejecuta operaciones vectorizadas en C sobre bloques de memoria contigua. El bucle no ocurre en Python. Por eso `df["a"] * 2` es órdenes de magnitud más rápido que un `for` sobre las filas.

**9. ¿Cuándo NO usar pandas?**
Cuando el dataset no cabe en RAM, cuando la agregación se puede hacer más barato en la base de datos con SQL, o cuando necesitas paralelismo real (ahí van Polars, Dask o Spark).

**10. ¿Qué es la granularidad de una tabla de hechos y por qué importa?**
Es qué representa una fila (una venta, un día, un mes). Define el nivel máximo de detalle que se podrá analizar: nunca se puede bajar de la granularidad elegida, solo agregar hacia arriba.

### Nivel de aplicación

**11. Escribe el código para: cargar `ventas.csv`, quedarte con las de 2025, y obtener el total vendido por ciudad ordenado de mayor a menor.**

```python
import pandas as pd

df = pd.read_csv("ventas.csv", parse_dates=["fecha"])

resultado = (
    df[df["fecha"].dt.year == 2025]
      .groupby("ciudad")["monto"]
      .sum()
      .sort_values(ascending=False)
)
print(resultado)
```

**12. Modela en estrella un sistema de entrenamientos deportivos.**
- `hecho_sesion(id, atleta_id, entrenador_id, ejercicio_id, tiempo_id, series, repeticiones, duracion_min, carga_kg)`
- `dim_atleta(id, nombre, categoria, sexo, fecha_nacimiento, ciudad)`
- `dim_entrenador(id, nombre, especialidad)`
- `dim_ejercicio(id, nombre, grupo_muscular, tipo)`
- `dim_tiempo(id, fecha, dia, mes, trimestre, año, dia_semana)`

Granularidad: una fila por atleta-ejercicio-sesión.

---

## 8. Errores comunes de principiante

| Error | Por qué está mal | Forma correcta |
|---|---|---|
| `df[df.edad > 20 and df.ciudad == "X"]` | `and` no funciona con Series | `df[(df.edad > 20) & (df.ciudad == "X")]` |
| Iterar con `for i in range(len(df))` | Lentísimo, anti-patrón | Vectorizar: `df["col"] * 2` |
| `df.dropna()` sin mirar antes | Puedes borrar el 60% del dataset sin darte cuenta | Revisar `df.isnull().sum()` primero |
| Olvidar `index=False` al exportar | Aparece una columna basura `Unnamed: 0` | `df.to_csv("x.csv", index=False)` |
| Confundir `=` con `==` en filtros | Asignación vs comparación | `==` para comparar |
| Modificar sobre una vista | `SettingWithCopyWarning` | Usar `.copy()` o `.loc[]` |
| Normalizar el data warehouse a 3FN | Consultas analíticas lentísimas | Modelo dimensional |

---

## 9. Ruta de estudio sugerida

**Semana 1 — Fundamentos**
Python básico: tipos, listas, diccionarios, bucles, funciones, comprehensions.

**Semana 2 — pandas I**
Series y DataFrame, lectura de archivos, exploración, selección, filtrado, limpieza de nulos.

**Semana 3 — pandas II**
`groupby`, `merge`, `pivot_table`, fechas con `datetime`, exportación.

**Semana 4 — Modelado y arquitectura**
Repasar normalización, diseñar un esquema estrella desde una BD normalizada, entender OLTP→ETL→OLAP con un caso propio.

**Semana 5 — Visualización**
Matplotlib y Seaborn: barras, líneas, histogramas, boxplots, heatmaps. Cuándo usar cada gráfico.

**Semana 6 — Integración**
Proyecto end-to-end: conectar Python a PostgreSQL con `read_sql`, transformar con pandas, graficar, escribir conclusiones.

---

## 10. Recursos

- **pandas — documentación oficial**: *10 minutes to pandas* y el *User Guide* (la mejor fuente, en inglés).
- **Wes McKinney, "Python for Data Analysis"** (3ª ed.) — el libro del creador de pandas.
- **Kimball, "The Data Warehouse Toolkit"** — la biblia del modelado dimensional.
- **Kaggle Learn** — micro-cursos gratuitos de pandas y visualización con ejercicios prácticos.
- **pandas cheat sheet** oficial (PDF de una página, imprimible).
