# 03A · Librerías para el análisis de datos — Parte 1: Fundamentos y ecosistema

> **Paso 2 de 3 de la ruta.** Qué es una librería, cuáles existen, cuáles se usan de verdad, cómo funcionan por dentro NumPy y Pandas, y la diferencia entre métodos, atributos y parámetros.

### 🗺️ Ruta de estudio

|Paso|Archivo|Qué cubre|
|---|---|---|
|1|[`02`](https://claude.ai/chat/02_Uso_de_librerias_de_Python_para_analisis_de_datos.md)|Entorno de trabajo · Python puro para datos|
|**2**|**`03A` ← estás aquí**|Qué son las librerías · NumPy · Pandas|
|3|[`03B`](https://claude.ai/chat/03B_Librerias_para_analisis_de_datos_Parte2_Visualizacion.md)|Visualización con Matplotlib|

> 📌 **Prerrequisito:** este apunte asume un entorno virtual activo con el stack instalado. Si no lo tienes, empieza por la [sección 1 del archivo 02](https://claude.ai/chat/02_Uso_de_librerias_de_Python_para_analisis_de_datos.md#1-entorno-de-trabajo).

---

## 📑 Índice

1. [¿Qué es una librería?](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#1-qu%C3%A9-es-una-librer%C3%ADa)
2. [Prerrequisito: el entorno de trabajo](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#2-prerrequisito-el-entorno-de-trabajo)
3. [¿Qué librerías existen en Python para el análisis de datos?](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#3-qu%C3%A9-librer%C3%ADas-existen-en-python-para-el-an%C3%A1lisis-de-datos)
4. [¿Cuáles son las más usadas?](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#4-cu%C3%A1les-son-las-m%C3%A1s-usadas)
5. [¿Para qué sirven realmente dentro del análisis de datos?](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#5-para-qu%C3%A9-sirven-realmente-dentro-del-an%C3%A1lisis-de-datos)
6. [Métodos, atributos y parámetros](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#6-m%C3%A9todos-atributos-y-par%C3%A1metros)
7. [NumPy: el motor que está debajo](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#7-numpy-el-motor-que-est%C3%A1-debajo)
8. [Pandas en profundidad](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#8-pandas-en-profundidad)
9. [Cómo leer la documentación oficial](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#9-c%C3%B3mo-leer-la-documentaci%C3%B3n-oficial)
10. [Errores frecuentes con librerías](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#10-errores-frecuentes-con-librer%C3%ADas)
11. [Checklist y glosario](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#11-checklist-y-glosario)

---

## 1. ¿Qué es una librería?

Una **librería** (o biblioteca) es un conjunto de código ya escrito, probado y documentado por otras personas, que puedes importar en tu proyecto para resolver un problema sin programarlo desde cero.

La idea de fondo es simple: **alguien más ya resolvió ese problema mejor de lo que lo resolverías tú en una tarde.**

### La analogía

Si programar fuera construir una casa:

- **Python puro** son las herramientas básicas: martillo, serrucho, metro
- Una **librería** es un módulo prefabricado: la puerta ya armada, la ventana con su marco
- No dejas de ser el constructor: sigues decidiendo dónde va cada cosa y por qué

### Módulo vs. paquete vs. librería vs. framework

Se usan de forma intercambiable, pero técnicamente:

|Término|Qué es|Ejemplo|
|---|---|---|
|**Módulo**|Un solo archivo `.py` con funciones y clases|`csv`, `json`, `math`|
|**Paquete**|Una carpeta con varios módulos organizados|`numpy`, `pandas`|
|**Librería**|Término general para código reutilizable de terceros|"la librería pandas"|
|**Framework**|Impone la estructura del proyecto; tú llenas los huecos|Django, FastAPI|
|**API**|El conjunto de funciones públicas que expone una librería|"la API de pandas"|

> **Diferencia clave entre librería y framework:** con una librería, **tú llamas al código**. Con un framework, **el framework llama a tu código**. Es lo que se conoce como _inversión de control_.

### Estándar vs. terceros

```python
# Librería ESTÁNDAR: viene con Python, no se instala
import csv
import json
import datetime
import os

# Librería de TERCEROS: hay que instalarla en el entorno virtual
import pandas as pd     # requiere: uv add pandas
import numpy as np      # requiere: uv add numpy
```

Para saber si algo es estándar: si aparece en la documentación oficial de Python bajo _"The Python Standard Library"_, viene incluido.

### Formas de importar

```python
import pandas                        # acceso completo: pandas.read_csv(...)
import pandas as pd                  # con alias (lo estándar): pd.read_csv(...)
from pandas import DataFrame         # solo un elemento: DataFrame(...)
from datetime import datetime, date  # varios elementos
import matplotlib.pyplot as plt      # un submódulo con alias

from pandas import *                 # ❌ NUNCA: contamina el espacio de nombres
```

**Por qué `import *` es un problema:**

```python
from numpy import *
from math import *

sqrt([1, 4, 9])   # ¿cuál sqrt se está usando? La última importada gana.
                  # math.sqrt no acepta listas → TypeError inesperado
```

### Los alias estándar

Estos cuatro son convenciones que **toda** la comunidad respeta. Úsalos siempre:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

Si escribes `import pandas as panda`, tu código funciona pero nadie lo lee cómodo. Las convenciones existen para que cualquier persona entienda tu código sin traducirlo mentalmente.

### Por qué usar librerías

1. **Menos código** → menos superficie donde meter errores
2. **Están probadas** → miles de personas las usan a diario y reportan bugs
3. **Son rápidas** → pandas y NumPy ejecutan en C, no en Python
4. **Son el estándar** → cualquier equipo entiende tu código
5. **Tienen documentación** → no dependes de tu memoria ni de tus comentarios

### Cuándo NO usar una librería

No todo amerita una dependencia. Instalar una librería tiene costo:

- ❌ Si el problema se resuelve en 5 líneas de Python puro
- ❌ Si la librería no tiene mantenimiento activo (último commit hace 3 años)
- ❌ Si arrastra 20 dependencias para una función que necesitas una vez
- ❌ Si nadie más en el equipo la conoce y no está documentada

> Cada dependencia es algo que puede romperse, quedar obsoleto o tener una vulnerabilidad. Menos dependencias, menos superficie de riesgo.

---

## 2. Prerrequisito: el entorno de trabajo

> ⚠️ **Antes de instalar cualquier librería, necesitas un entorno virtual creado y activo.**

Todo lo relacionado con entornos de trabajo está consolidado en la **sección 1 del archivo 02**, que es la referencia única sobre el tema:

- Por qué usar un entorno virtual
- Crear y activar el entorno con `uv venv` o con `python -m venv`
- Instalar y gestionar librerías (`uv add`, `pip install`)
- Versionado semántico y reproducibilidad (`uv.lock`, `requirements.txt`)
- Variables de entorno con `.env`
- Diagnóstico cuando aparece `ModuleNotFoundError`

📄 **[Ir a `02` · Sección 1: Entorno de trabajo](https://claude.ai/chat/02_Uso_de_librerias_de_Python_para_analisis_de_datos.md#1-entorno-de-trabajo)**

### Verificación en 30 segundos

Antes de seguir con este apunte, confirma que tu entorno está listo:

```bash
# 1. ¿El prompt muestra (.venv)?
# 2. Instala el stack básico
uv add pandas numpy matplotlib seaborn jupyter
```

```python
import sys, pandas as pd
print(sys.executable)      # debe apuntar a tu carpeta .venv
print(pd.__version__)      # debe imprimir una versión, no un error
```

Si ambas líneas responden como se espera, puedes continuar.

---

## 3. ¿Qué librerías existen en Python para el análisis de datos?

El ecosistema es amplio. Lo importante no es memorizarlas, sino saber **qué categoría resuelve qué problema**.

### 🔢 Cálculo numérico y manipulación de datos

|Librería|Para qué sirve|Cuándo elegirla|
|---|---|---|
|**NumPy**|Arrays multidimensionales y operaciones vectorizadas|Base de todo lo demás|
|**Pandas**|Manipulación de datos tabulares (Series y DataFrames)|Por defecto, hasta ~1-5 GB|
|**Polars**|Alternativa moderna escrita en Rust, mucho más rápida|Datos grandes en una sola máquina|
|**Dask**|Operaciones tipo pandas en paralelo y fuera de RAM|Datos que no caben en memoria|
|**PySpark**|Procesamiento distribuido en clústeres|Big Data real, varios nodos|
|**DuckDB**|Base de datos analítica embebida; SQL sobre archivos|Consultas SQL sin montar servidor|

### 📊 Visualización

|Librería|Para qué sirve|Cuándo elegirla|
|---|---|---|
|**Matplotlib**|La base de la visualización en Python|Control total, gráficos para publicar|
|**Seaborn**|Construida sobre Matplotlib, gráficos estadísticos|Exploración rápida y elegante|
|**Plotly**|Gráficos interactivos (zoom, hover, filtros)|Dashboards y entregables web|
|**Bokeh**|Visualización interactiva en navegador|Apps de datos|
|**Altair**|Gráficos declarativos: describes qué ver, no cómo|Prototipado rápido y limpio|

### 📈 Estadística y machine learning

|Librería|Para qué sirve|
|---|---|
|**SciPy**|Estadística, optimización, álgebra lineal, señales|
|**Statsmodels**|Regresión, series temporales, tests de hipótesis con salida estadística formal|
|**Scikit-learn**|ML clásico: clasificación, regresión, clustering, preprocesamiento|
|**TensorFlow / PyTorch**|Deep learning y redes neuronales|
|**XGBoost / LightGBM**|Modelos de árboles con gradient boosting|

### 🔌 Conexión a datos externos

|Librería|Para qué sirve|
|---|---|
|**SQLAlchemy**|ORM y motor de conexión a bases de datos SQL|
|**psycopg / psycopg2**|Driver específico de PostgreSQL|
|**requests**|Consumo de APIs REST|
|**BeautifulSoup / Scrapy**|Web scraping|
|**openpyxl / xlsxwriter**|Lectura y escritura de archivos Excel|
|**pyarrow**|Formato Parquet, intercambio eficiente de datos|

### 🛠️ Entorno y calidad

|Librería|Para qué sirve|
|---|---|
|**Jupyter**|Notebooks: código, resultado y explicación en un documento|
|**python-dotenv**|Carga de variables de entorno desde `.env`|
|**Ruff**|Linter y formateador extremadamente rápido|
|**pytest**|Pruebas automatizadas|
|**Great Expectations / Pandera**|Validación de calidad de datos|

---

## 4. ¿Cuáles son las más usadas?

Si tuvieras que aprender solo cinco, serían estas. Cubren la gran mayoría del trabajo real de un analista.

|#|Librería|Alias|Rol en el flujo de trabajo|
|---|---|---|---|
|1|**NumPy**|`np`|El motor numérico. Casi nunca lo usas directo, pero está debajo de todo|
|2|**Pandas**|`pd`|Cargar, limpiar, transformar y agregar. **La herramienta central**|
|3|**Matplotlib**|`plt`|Visualizar y comunicar resultados|
|4|**Seaborn**|`sns`|Gráficos estadísticos rápidos y con buen diseño por defecto|
|5|**Scikit-learn**|`sklearn`|Cuando el análisis pasa a modelado predictivo|

### El stack mínimo

```bash
uv add pandas numpy matplotlib seaborn jupyter
```

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### Cómo encajan entre sí

```
Fuente de datos (CSV, Excel, SQL, API)
          │
          ▼
      PANDAS  ──── carga, limpia y transforma
          │        (por debajo usa NUMPY)
          ▼
   Datos limpios (DataFrame)
          │
          ├──────────────► MATPLOTLIB / SEABORN  → gráficos y reportes
          │
          ├──────────────► SCIKIT-LEARN          → modelos predictivos
          │
          └──────────────► STATSMODELS           → inferencia estadística
```

> **No compiten entre ellas, se complementan.** Seaborn no reemplaza a Matplotlib: la usa por debajo. Pandas no reemplaza a NumPy: está construida encima.

### ¿Pandas, Polars o Spark?

|Criterio|Pandas|Polars|PySpark|
|---|---|---|---|
|Volumen típico|Hasta ~1-5 GB|Hasta decenas de GB|Terabytes|
|Velocidad|Media|Muy alta|Alta (distribuida)|
|Curva de aprendizaje|Baja|Media|Alta|
|Comunidad y recursos|Enorme|Creciente|Grande|
|Infraestructura|Ninguna|Ninguna|Clúster|

> **Para aprender y para el 95 % de los proyectos: pandas.** El resto son optimizaciones que se justifican cuando el volumen realmente lo exige.

---

## 5. ¿Para qué sirven realmente dentro del análisis de datos?

La respuesta corta: **te devuelven el tiempo.** La respuesta larga tiene cuatro partes.

### a) Reducen código a una fracción

Todo lo que hiciste con Python puro se comprime:

|Con Python puro|Con pandas|
|---|---|
|`csv.DictReader` + loop + conversión de tipos|`pd.read_csv("datos.csv")`|
|Comprensión de lista para filtrar|`df[df["edad"] > 16]`|
|Diccionario acumulador con `setdefault`|`df.groupby("categoria").sum()`|
|`sum(...) / len(...)` con manejo de errores|`df["ventas"].mean()`|
|Loops anidados para cruzar dos tablas|`df1.merge(df2, on="id")`|
|40 líneas de loops y validaciones|4 líneas legibles|

### b) Son mucho más rápidas

Un loop de Python procesa elemento por elemento, interpretando cada instrucción. NumPy y pandas ejecutan la operación completa en **C compilado** sobre bloques de memoria contiguos. A esto se le llama **vectorización**.

```python
# ❌ Loop de Python: lento
resultado = []
for valor in lista_de_un_millon:
    resultado.append(valor * 1.19)

# ✅ Vectorizado: la operación se aplica a todo el array de golpe
resultado = array_de_un_millon * 1.19
```

Con un millón de registros, la diferencia va de segundos a milisegundos. Y esa diferencia se multiplica cuando el pipeline tiene 30 operaciones encadenadas.

### c) Resuelven los problemas que no ves venir

Esto es lo que más ahorra tiempo en el mundo real:

- **Valores faltantes:** `NaN` como concepto de primera clase, con `.fillna()`, `.dropna()`, `.isna()`
- **Tipos de datos:** `read_csv` infiere si una columna es número, texto o fecha
- **Fechas:** zonas horarias, frecuencias, rangos y resampleo sin escribir lógica de calendario
- **Uniones:** combinar tablas con `.merge()` en lugar de loops anidados
- **Formatos raros:** CSV con comillas, separadores distintos, encodings mixtos, filas mal formadas
- **Índices:** alinear automáticamente dos series por su etiqueta antes de operar

> El valor real de una librería no está en lo que hace bien, sino en **los mil casos borde que ya tiene resueltos**.

### d) Hacen tu trabajo verificable por otros

Un pipeline de 200 líneas de loops propios solo lo entiendes tú. Uno de 20 líneas de pandas lo revisa cualquier persona del equipo en cinco minutos. En un entorno profesional, esto vale tanto como la velocidad.

---

## 6. Métodos, atributos y parámetros

Esta confusión es muy común al empezar. La forma más simple de entenderlo:

> **El método es la acción. El parámetro es la instrucción sobre cómo ejecutar esa acción. El atributo es un dato que el objeto ya tiene.**

### Método

Un **método** es una función que pertenece a un objeto. Se escribe con **punto** y siempre lleva **paréntesis**.

```python
df.head()        # método: "muéstrame las primeras filas"
df.mean()        # método: "calcula el promedio"
df.dropna()      # método: "elimina las filas con nulos"
```

### Parámetro

Un **parámetro** es un dato que le pasas al método, **dentro de los paréntesis**, para modificar su comportamiento.

```python
df.head(10)                                  # cuántas filas
df.dropna(subset=["edad"])                   # en qué columna mirar
df.sort_values("ventas", ascending=False)    # dos parámetros
```

El mismo método, con parámetros distintos, produce resultados distintos:

```python
pd.read_csv("datos.csv")                     # con los valores por defecto
pd.read_csv("datos.csv", sep=";")            # separador punto y coma
pd.read_csv("datos.csv", sep=";", encoding="latin-1", nrows=100)
```

### Parámetro vs. argumento

Es un matiz que aparece en la documentación:

- **Parámetro** → el nombre en la definición de la función (`sep`, `encoding`)
- **Argumento** → el valor concreto que pasas (`";"`, `"latin-1"`)

```python
def saludar(nombre, saludo="Hola"):   # nombre y saludo son PARÁMETROS
    return f"{saludo}, {nombre}"

saludar("Daniel", saludo="Buenas")    # "Daniel" y "Buenas" son ARGUMENTOS
```

### Posicional vs. con nombre (keyword)

```python
df.sort_values("ventas", False)              # posicional: depende del orden exacto
df.sort_values(by="ventas", ascending=False) # con nombre: explícito y seguro
```

> **Recomendación:** usa parámetros con nombre. El código se lee solo y no se rompe si cambia el orden en una versión nueva. Muchas librerías modernas incluso **obligan** a pasar ciertos parámetros con nombre.

### Parámetros con valor por defecto

```python
def calcular_iva(monto, tasa=0.19):
    return monto * tasa

calcular_iva(100000)             # usa 0.19 → 19000.0
calcular_iva(100000, tasa=0.05)  # sobreescribe → 5000.0
```

Casi todos los métodos de pandas tienen decenas de parámetros con valores por defecto. Por eso `pd.read_csv("datos.csv")` funciona sin configurar nada: los defaults cubren el caso común.

### `*args` y `**kwargs`

Cuando ves esto en la documentación, significa "acepta un número variable de argumentos":

```python
def resumen(*valores, **opciones):
    print(valores)    # tupla con los argumentos posicionales
    print(opciones)   # diccionario con los argumentos con nombre

resumen(1, 2, 3, titulo="Ventas", decimales=2)
# (1, 2, 3)
# {'titulo': 'Ventas', 'decimales': 2}
```

### ⚠️ Método vs. atributo (la confusión más frecuente)

Un **atributo** es una propiedad del objeto: un dato que ya existe. **No lleva paréntesis.**

```python
# ATRIBUTOS → sin paréntesis
df.shape       # (100, 5)  → dimensiones
df.columns     # nombres de las columnas
df.dtypes      # tipos de dato de cada columna
df.index       # el índice
df.size        # total de celdas
df.empty       # True/False

# MÉTODOS → con paréntesis
df.head()      # ejecuta una acción
df.describe()  # calcula estadísticas
df.copy()      # crea una copia
df.info()      # imprime un resumen
```

Si escribes `df.shape()` obtendrás `TypeError: 'tuple' object is not callable`, porque `shape` **ya es** el dato, no una función que lo produce.

||Atributo|Método|
|---|---|---|
|Sintaxis|`df.shape`|`df.head()`|
|Qué es|Un dato guardado|Una acción a ejecutar|
|Acepta parámetros|No|Sí|
|Analogía|El color de un carro|Arrancar el carro|

### Encadenamiento de métodos (method chaining)

Como la mayoría de métodos de pandas **devuelven un nuevo DataFrame**, se pueden encadenar:

```python
resultado = (
    df
    .dropna(subset=["ventas"])
    .query("region == 'Norte'")
    .groupby("categoria")["ventas"]
    .sum()
    .sort_values(ascending=False)
    .head(5)
)
```

Se lee de arriba hacia abajo como una receta. Los paréntesis externos permiten partir la línea sin usar `\`.

### ⚠️ El parámetro `inplace`

```python
df.dropna(inplace=True)      # ❌ modifica el original, no devuelve nada
df = df.dropna()             # ✅ explícito y encadenable
```

`inplace=True` está desaconsejado por el propio equipo de pandas: no ahorra memoria como se cree, rompe el encadenamiento y hace el código más difícil de depurar. **Prefiere la reasignación.**

---

## 7. NumPy: el motor que está debajo

Aunque casi nunca lo llames directamente, entender NumPy explica **por qué** pandas funciona como funciona.

### Array vs. lista

```python
import numpy as np

lista = [1, 2, 3, 4]
array = np.array([1, 2, 3, 4])

lista * 2    # [1, 2, 3, 4, 1, 2, 3, 4]  → repite la lista
array * 2    # array([2, 4, 6, 8])       → multiplica cada elemento
```

||Lista de Python|Array de NumPy|
|---|---|---|
|Tipos de dato|Mixtos|Uno solo (`dtype`)|
|Memoria|Punteros dispersos|Bloque contiguo|
|Operaciones matemáticas|Con loop|Vectorizadas|
|Velocidad|Base|10–100× más rápido|
|Tamaño|Dinámico|Fijo al crearse|

### El `dtype`

Todo array tiene un tipo único. Ahí está el secreto de la velocidad: NumPy sabe de antemano cuántos bytes ocupa cada elemento.

```python
a = np.array([1, 2, 3])
a.dtype          # dtype('int64')

b = np.array([1.5, 2.5])
b.dtype          # dtype('float64')

c = np.array([1, 2, 3], dtype=np.float32)   # forzar el tipo
```

### Operaciones vectorizadas y broadcasting

```python
precios   = np.array([1000, 2000, 3000])
cantidad  = np.array([2, 1, 4])

total = precios * cantidad          # array([2000, 2000, 12000])
con_iva = total * 1.19              # el escalar se "expande" → broadcasting
promedio = total.mean()             # 5333.33
```

**Broadcasting** es la regla que permite operar arrays de formas distintas sin escribir loops. Un escalar se aplica a todos los elementos; un array de una fila se aplica a todas las filas.

### Funciones útiles

```python
np.array([1, 2, 3])          # crear desde lista
np.zeros(5)                  # [0. 0. 0. 0. 0.]
np.ones((2, 3))              # matriz 2×3 de unos
np.arange(0, 10, 2)          # [0 2 4 6 8]
np.linspace(0, 1, 5)         # [0. 0.25 0.5 0.75 1.]
np.random.seed(42)           # semilla → resultados reproducibles
np.random.normal(0, 1, 100)  # 100 valores de una distribución normal

a.mean(), a.std(), a.sum(), a.min(), a.max()
a.reshape(2, 3)              # cambiar la forma
np.where(a > 2, "alto", "bajo")   # condicional vectorizado
```

### `NaN`: el valor faltante

```python
np.nan              # "Not a Number"
np.nan == np.nan    # False ← ⚠️ un NaN nunca es igual a otro NaN
np.isnan(np.nan)    # True  ← así se verifica correctamente
```

Esta rareza es la razón por la que en pandas se usa `.isna()` y no `== None`.

---

## 8. Pandas en profundidad

### Las dos estructuras

Pandas es una biblioteca de código abierto diseñada específicamente para la manipulación y el análisis de datos estructurados. Su nombre proviene del término econométrico _panel data_ (datos de panel), y se ha convertido en el estándar de la industria para la ciencia de datos por su flexibilidad y rendimiento.

Creada por **Wes McKinney en 2008** y construida sobre NumPy, proporciona dos estructuras fundamentales:

**Series:** estructuras unidimensionales similares a arrays etiquetados.

```python
ventas = pd.Series([1500, 2300, 1800], index=["Ene", "Feb", "Mar"], name="ventas")

ventas["Feb"]      # 2300 → acceso por etiqueta
ventas.mean()      # 1866.67
ventas.index       # Index(['Ene', 'Feb', 'Mar'])
ventas.values      # array([1500, 2300, 1800])
```

**DataFrames:** estructuras bidimensionales en forma de tablas, análogas a hojas de cálculo o bases de datos SQL, que permiten manejar datos heterogéneos (numéricos, texto, fechas).

```python
df = pd.DataFrame({
    "mes": ["Ene", "Feb", "Mar"],
    "ventas": [1500, 2300, 1800],
    "region": ["Norte", "Norte", "Sur"]
})
```

> **Un DataFrame es un diccionario de Series que comparten el mismo índice.** Cada columna es una Series; el índice las alinea.

### El índice: lo que hace distinto a pandas

```python
a = pd.Series([1, 2, 3], index=["x", "y", "z"])
b = pd.Series([10, 20, 30], index=["z", "y", "x"])

a + b    # x: 31, y: 22, z: 13  ← se alinean por ETIQUETA, no por posición
```

Con listas de Python tendrías que ordenar manualmente. Pandas lo hace solo. Esa alineación automática es la fuente de su potencia y también de sus errores más confusos.

### Capacidades principales

- **Lectura y escritura** desde CSV, Excel, JSON y bases de datos SQL
- **Limpieza y transformación**, incluyendo manejo eficiente de valores faltantes (`NaN`)
- **Agregación y agrupación** (GroupBy) para resumir grandes volúmenes
- **Análisis de series temporales**, con datos indexados por fechas y frecuencias

### Carga de datos

```python
df = pd.read_csv("datos.csv")
df = pd.read_csv("datos.csv", sep=";", encoding="latin-1", decimal=",")
df = pd.read_excel("datos.xlsx", sheet_name="Ventas")
df = pd.read_json("datos.json")
df = pd.read_sql("SELECT * FROM ventas", conexion)
df = pd.read_parquet("datos.parquet")

# Parámetros útiles de read_csv
pd.read_csv("datos.csv",
            usecols=["fecha", "ventas"],    # solo estas columnas
            nrows=1000,                     # solo las primeras filas
            parse_dates=["fecha"],          # convertir a datetime
            na_values=["N/A", "-", ""])     # qué considerar como nulo
```

**Escritura:**

```python
df.to_csv("salida.csv", index=False)     # index=False evita una columna extra
df.to_excel("salida.xlsx", index=False)
df.to_parquet("salida.parquet")
```

### Exploración inicial (los primeros 6 comandos, siempre)

```python
df.head()          # primeras 5 filas
df.tail(3)         # últimas 3
df.shape           # (filas, columnas) → atributo
df.info()          # tipos de dato y conteo de no-nulos
df.describe()      # estadísticas de las columnas numéricas
df.isna().sum()    # cuántos nulos hay por columna
```

Complementarios:

```python
df.dtypes                        # tipo de cada columna
df.columns                       # nombres de columnas
df["region"].unique()            # valores únicos
df["region"].nunique()           # cuántos valores únicos
df["region"].value_counts()      # frecuencia de cada valor
df.duplicated().sum()            # cuántas filas duplicadas
df.sample(5)                     # 5 filas al azar
```

### Selección: `[]`, `.loc` y `.iloc`

```python
df["ventas"]                   # una columna → Series
df[["mes", "ventas"]]          # varias columnas → DataFrame

# .loc → por ETIQUETA (el final es INCLUSIVO)
df.loc[0]                      # fila con índice 0
df.loc[0:2]                    # filas 0, 1 y 2
df.loc[0:2, ["mes", "ventas"]] # filas y columnas
df.loc[df["ventas"] > 1600]    # filtro booleano

# .iloc → por POSICIÓN (el final es EXCLUSIVO, como las listas)
df.iloc[0]                     # primera fila
df.iloc[0:2]                   # filas 0 y 1
df.iloc[-1]                    # última fila
df.iloc[0:3, 1:3]              # rango de filas y columnas
```

||`.loc`|`.iloc`|
|---|---|---|
|Se basa en|Etiquetas del índice|Posición numérica|
|`[0:2]` devuelve|3 filas (inclusivo)|2 filas (exclusivo)|
|Acepta booleanos|Sí|No|

### Filtrado

```python
df[df["ventas"] > 2000]                                  # una condición
df[(df["ventas"] > 1500) & (df["region"] == "Norte")]    # AND
df[(df["region"] == "Norte") | (df["region"] == "Sur")]  # OR
df[df["region"].isin(["Norte", "Sur"])]                  # más limpio
df[~df["region"].isin(["Norte"])]                        # NOT
df[df["ventas"].between(1500, 2500)]                     # rango

df.query("ventas > 1500 and region == 'Norte'")          # sintaxis alternativa
```

> ⚠️ En pandas se usa `&`, `|` y `~`, **no** `and`, `or` y `not`. Y cada condición va entre paréntesis, porque `&` tiene mayor precedencia que `>`.

### Limpieza

```python
# Nulos
df.isna().sum()                        # diagnóstico
df.dropna()                            # eliminar filas con algún nulo
df.dropna(subset=["ventas"])           # solo si falta esta columna
df["ventas"].fillna(0)                 # rellenar con un valor
df["ventas"].fillna(df["ventas"].median())   # rellenar con la mediana

# Duplicados
df.drop_duplicates()
df.drop_duplicates(subset=["id"], keep="last")

# Tipos
df["ventas"] = df["ventas"].astype(float)
df["fecha"] = pd.to_datetime(df["fecha"])
df["ventas"] = pd.to_numeric(df["ventas"], errors="coerce")  # lo inválido → NaN

# Texto
df["region"] = df["region"].str.strip().str.title()
df["nombre"] = df["nombre"].str.lower()

# Renombrar
df = df.rename(columns={"ventas": "ventas_cop"})
```

### Columnas nuevas

```python
df["total"] = df["precio"] * df["cantidad"]                 # vectorizado
df["con_iva"] = df["total"] * 1.19

# Condicional vectorizado
df["nivel"] = np.where(df["ventas"] > 2000, "Alto", "Bajo")

# Varias condiciones
df["categoria"] = pd.cut(df["edad"], bins=[0, 12, 18, 100],
                         labels=["Niño", "Adolescente", "Adulto"])

# assign → encadenable
df = df.assign(margen=lambda d: d["ventas"] - d["costo"])
```

### Agrupación (GroupBy)

Este es el corazón del análisis. Sigue el patrón **dividir → aplicar → combinar**:

```python
df.groupby("region")["ventas"].sum()
df.groupby("region")["ventas"].mean()
df.groupby(["region", "categoria"])["ventas"].sum()

# Varias métricas a la vez
df.groupby("region").agg(
    total_ventas=("ventas", "sum"),
    promedio=("ventas", "mean"),
    cantidad=("ventas", "count"),
    maximo=("ventas", "max")
).reset_index()
```

> `reset_index()` convierte el resultado agrupado de vuelta en un DataFrame plano. Úsalo casi siempre después de un `groupby`.

### Uniones

```python
pd.merge(ventas, clientes, on="cliente_id", how="left")
pd.merge(a, b, left_on="id", right_on="codigo", how="inner")

pd.concat([df_enero, df_febrero])              # apilar filas
pd.concat([df_a, df_b], axis=1)                # unir columnas
```

|`how`|Qué conserva|
|---|---|
|`inner`|Solo las coincidencias (por defecto)|
|`left`|Todo lo de la izquierda|
|`right`|Todo lo de la derecha|
|`outer`|Todo de ambas|

Es el mismo concepto que los `JOIN` de SQL.

### Tablas dinámicas

```python
df.pivot_table(index="region",
               columns="mes",
               values="ventas",
               aggfunc="sum",
               fill_value=0)
```

### ⚠️ `SettingWithCopyWarning`

```python
# ❌ Genera el warning: pandas no sabe si modificas una copia o el original
subset = df[df["region"] == "Norte"]
subset["bono"] = 100

# ✅ Correcto
subset = df[df["region"] == "Norte"].copy()
subset["bono"] = 100

# ✅ O directamente sobre el original
df.loc[df["region"] == "Norte", "bono"] = 100
```

### El flujo típico de un análisis (EDA)

```python
# 1. Cargar
df = pd.read_csv("ventas.csv", parse_dates=["fecha"])

# 2. Inspeccionar
df.head(); df.info(); df.describe(); df.isna().sum()

# 3. Limpiar
df = df.drop_duplicates()
df = df.dropna(subset=["ventas"])
df["region"] = df["region"].str.strip().str.title()

# 4. Transformar
df["total"] = df["precio"] * df["cantidad"]
df["mes"] = df["fecha"].dt.month_name()

# 5. Analizar
resumen = df.groupby("region").agg(
    total=("total", "sum"),
    promedio=("total", "mean")
).reset_index()

# 6. Visualizar → ver Parte 2
# 7. Exportar
resumen.to_csv("resumen_por_region.csv", index=False)
```

---

## 9. Cómo leer la documentación oficial

Saber leer la documentación vale más que memorizar métodos. Es la habilidad que te independiza de los tutoriales.

### Anatomía de una firma (signature)

```python
DataFrame.sort_values(by, *, axis=0, ascending=True, inplace=False, kind='quicksort', na_position='last')
```

- `by` → obligatorio, no tiene valor por defecto
- `*` → todo lo que viene después **debe** pasarse con nombre
- `ascending=True` → opcional, con su valor por defecto visible
- Los tipos y el valor de retorno aparecen debajo, en las secciones _Parameters_ y _Returns_

### Desde el propio código

```python
help(pd.read_csv)        # documentación completa en consola
pd.read_csv?             # en Jupyter: docstring resumido
pd.read_csv??            # en Jupyter: incluye el código fuente
dir(df)                  # lista todos los métodos y atributos disponibles
```

### Dónde buscar

1. **Documentación oficial** → siempre primero, y verifica que la versión coincida con la tuya
2. **User Guide** → explicaciones conceptuales con ejemplos
3. **API Reference** → la lista completa de métodos y parámetros
4. **Stack Overflow** → mira la fecha; una respuesta de 2016 puede usar métodos ya eliminados

> Señal de alarma: si el ejemplo que copiaste usa `df.append()` o `df.ix[]`, es de pandas 1.x o anterior. Ambos ya no existen.

---

## 10. Errores frecuentes con librerías

|Error|Causa|Solución|
|---|---|---|
|`ModuleNotFoundError: No module named 'pandas'`|La librería no está instalada, o el entorno no está activo|Activa el `.venv` e instala|
|`ImportError: cannot import name X`|Versión distinta a la del ejemplo|Verifica `pd.__version__`|
|`AttributeError: 'DataFrame' object has no attribute 'append'`|Método eliminado en pandas 2.x|Usa `pd.concat()`|
|`TypeError: 'tuple' object is not callable`|Llamaste un atributo con paréntesis|`df.shape` sin `()`|
|`KeyError: 'ventas'`|La columna no existe o tiene espacios|Revisa `df.columns`|
|`UnicodeDecodeError`|Encoding incorrecto al leer|Prueba `encoding="latin-1"`|
|`SettingWithCopyWarning`|Modificaste una vista, no una copia|Usa `.copy()` o `.loc[]`|
|Funciona en Jupyter, falla en script|Kernel apuntando a otro entorno|Verifica el intérprete seleccionado|

**Diagnóstico rápido cuando "no funciona":**

```python
import sys, pandas as pd
print(sys.executable)      # ¿qué Python se está usando?
print(pd.__version__)      # ¿qué versión de pandas?
print(pd.__file__)         # ¿de dónde se cargó?
```

Si `sys.executable` no apunta a tu `.venv`, ahí está el problema.

---

## 11. Checklist y glosario

### ✅ Checklist de repaso

- [ ] Puedo explicar qué es una librería y en qué se diferencia de un framework
- [ ] Sé instalar, listar y fijar versiones de librerías
- [ ] Entiendo el versionado semántico y por qué un salto de MAYOR rompe código
- [ ] Conozco el stack básico: NumPy, Pandas, Matplotlib, Seaborn, Scikit-learn
- [ ] Uso los alias estándar (`pd`, `np`, `plt`, `sns`)
- [ ] Distingo un método (`df.head()`) de un atributo (`df.shape`)
- [ ] Distingo un parámetro de un argumento
- [ ] Sé por qué `inplace=True` está desaconsejado
- [ ] Entiendo qué es la vectorización y por qué es rápida
- [ ] Sé la diferencia entre `.loc` y `.iloc`
- [ ] Uso `&`, `|`, `~` en los filtros, no `and`, `or`, `not`
- [ ] Puedo hacer un `groupby().agg()` con varias métricas
- [ ] Sé leer la firma de una función en la documentación

### 🧾 Glosario

|Término|Definición|
|---|---|
|**Librería**|Conjunto de código reutilizable escrito por terceros|
|**Módulo**|Un archivo `.py` con código importable|
|**Paquete**|Carpeta con varios módulos organizados|
|**Framework**|Estructura que llama a tu código (inversión de control)|
|**Alias**|Nombre corto asignado al importar (`import pandas as pd`)|
|**Dependencia**|Librería externa que el proyecto necesita para funcionar|
|**SemVer**|Versionado `MAYOR.MENOR.PARCHE`|
|**Vectorización**|Aplicar una operación a toda una colección de golpe, sin loop|
|**Broadcasting**|Regla que permite operar arrays de formas distintas|
|**Método**|Función que pertenece a un objeto; lleva paréntesis|
|**Atributo**|Propiedad o dato del objeto; no lleva paréntesis|
|**Parámetro**|Nombre de la entrada en la definición de una función|
|**Argumento**|Valor concreto que se pasa al llamar la función|
|**DataFrame**|Estructura tabular bidimensional de pandas|
|**Series**|Estructura unidimensional etiquetada de pandas|
|**Índice**|Etiquetas que identifican cada fila y permiten la alineación|
|**dtype**|Tipo de dato de una columna o array|
|**NaN**|Marcador de valor faltante|
|**GroupBy**|Patrón dividir → aplicar → combinar|
|**EDA**|Análisis exploratorio de datos|

---

**➡️ Continúa en la [Parte 2: Visualización con Matplotlib](https://claude.ai/chat/03B_Librerias_para_analisis_de_datos_Parte2_Visualizacion.md)**