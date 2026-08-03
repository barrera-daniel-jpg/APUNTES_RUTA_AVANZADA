# 02 · Python para el análisis de datos

> **Paso 1 de 3 de la ruta.** Antes de usar librerías como pandas, hay que entender cómo Python maneja los datos por sí solo. Este apunte cubre desde la preparación del entorno hasta el procesamiento eficiente con Python puro.

### 🗺️ Ruta de estudio

|Paso|Archivo|Qué cubre|
|---|---|---|
|**1**|**`02` ← estás aquí**|Entorno de trabajo · Python puro para datos|
|2|`03A`|Qué son las librerías · NumPy · Pandas|
|3|`03B`|Visualización con Matplotlib|

> 📌 **Este archivo es la referencia única sobre el entorno de trabajo.** Todo lo relacionado con entornos virtuales, instalación de librerías y variables de entorno vive en la sección 1 de aquí. Los archivos 03A y 03B asumen que ya lo tienes montado y activo.

---

## 📑 Índice

1. [Entorno de trabajo](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#1-entorno-de-trabajo)
2. [Manipulación de estructuras de datos](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#2-manipulaci%C3%B3n-de-estructuras-de-datos)
3. [Comprensiones y operaciones avanzadas](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#3-comprensiones-y-operaciones-avanzadas)
4. [Manejo de archivos](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#4-manejo-de-archivos)
5. [Procesamiento de datos](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#5-procesamiento-de-datos)
6. [Manejo de errores y calidad de datos](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#6-manejo-de-errores-y-calidad-de-datos)
7. [Eficiencia en el procesamiento](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#7-eficiencia-en-el-procesamiento)
8. [Cierre: el puente hacia pandas](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#8-cierre-el-puente-hacia-pandas)

---

## 1. Entorno de trabajo

Antes de analizar un DataFrame, lo más recomendable es empezar con un buen entorno de trabajo.

En proyectos reales no se trabaja directamente sobre el Python del sistema. Cada proyecto tiene sus propias dependencias, versiones de librerías y configuraciones. Si eso no se controla, se generan conflictos.

### ¿Por qué usar un environment?

En el mundo real no trabajas con un solo proyecto. Imagina esto:

- El proyecto A necesita `pandas 1.3`
- El proyecto B necesita `pandas 2.4`

Si todo se instala de forma global, es inevitable que esos proyectos entren en conflicto: cambian los métodos, cambian los parámetros y el código deja de funcionar sin que hayas tocado nada.

Un environment resuelve esto porque:

- Cada proyecto es independiente
- No hay conflictos de versiones
- Puedes replicar el entorno fácilmente en otra máquina

### ¿Qué es un environment en Python?

Un **environment** (entorno virtual) es un espacio aislado donde instalas y gestionas las librerías de un proyecto sin afectar el resto del sistema.

Es como crear una "caja independiente" para cada proyecto, donde defines:

- Qué versión de Python usar
- Qué librerías instalar
- Qué dependencias manejar

Esto es fundamental en analítica de datos, porque cada proyecto puede necesitar configuraciones diferentes.

### Opción A: uv (recomendada)

`uv` es un gestor de paquetes y proyectos de Python extremadamente rápido, escrito en Rust y desarrollado por Astral (la misma empresa del linter Ruff). Está diseñado como alternativa unificada a varias herramientas existentes: `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine` y `virtualenv`.

```bash
# Crear el entorno (carpeta .venv por defecto)
uv venv

# O con un nombre específico
uv venv mi-entorno

# Activarlo
source .venv/bin/activate      # Linux / macOS
.venv\Scripts\activate         # Windows (PowerShell / CMD)

# Instalar librerías
uv pip install pandas numpy

# Ejecutar sin activar el entorno
uv run python analisis.py
```

Si trabajas el proyecto completo con uv:

```bash
uv init mi-proyecto     # crea pyproject.toml
uv add pandas numpy     # instala Y registra la dependencia
uv sync                 # replica el entorno exacto desde el lockfile
```

> **Nota:** siempre conviene revisar la guía oficial, porque uv está en desarrollo activo y los comandos evolucionan.

### Opción B: venv + pip (sin uv)

Es la forma estándar que trae Python. Más lenta, pero funciona en cualquier máquina sin instalar nada extra.

```bash
# 1. Crear el entorno
python -m venv .venv

# 2. Activarlo
source .venv/bin/activate      # Linux / macOS
.venv\Scripts\activate         # Windows

# 3. Instalar librerías
pip install pandas numpy

# 4. Congelar las versiones exactas
pip freeze > requirements.txt

# 5. En otra máquina: replicar el entorno
pip install -r requirements.txt

# 6. Salir del entorno
deactivate
```

**Cómo saber si está activo:** el nombre del entorno aparece al inicio del prompt → `(.venv) usuario@equipo:~/proyecto$`

### Variables de entorno

Las credenciales (contraseñas de base de datos, API keys) **nunca** van escritas dentro del código. Van en un archivo `.env`.

```bash
# archivo .env
DB_HOST=localhost
DB_USER=admin
DB_PASSWORD=secreto123
```

```python
# archivo config.py
import os
from dotenv import load_dotenv

load_dotenv()  # lee el archivo .env y carga las variables

DB_HOST = os.getenv("DB_HOST")
DB_USER = os.getenv("DB_USER")
DB_PASSWORD = os.getenv("DB_PASSWORD")
```

```bash
pip install python-dotenv   # o: uv add python-dotenv
```

> ⚠️ El archivo `.env` **siempre** va en el `.gitignore`. Lo que se sube al repo es un `.env.example` con las llaves pero sin los valores.

### Instalar y gestionar librerías dentro del entorno

Ya con el entorno **creado y activo**, aquí es donde se instalan las librerías. Este es el orden que nunca hay que invertir:

```
crear entorno  →  ACTIVARLO  →  instalar librerías  →  congelar versiones
```

> ⚠️ Si instalas sin haber activado el entorno, las librerías se van al Python global y vuelves al problema del principio. **Verifica siempre que el prompt muestre `(.venv)` antes de instalar.**

```bash
# Con uv (recomendado)
uv add pandas                    # instala Y registra en pyproject.toml
uv add pandas==2.2.0             # versión exacta
uv add "pandas>=2.0,<3.0"        # rango de versiones
uv remove pandas                 # desinstalar

# Con pip
pip install pandas
pip install pandas==2.2.0
pip uninstall pandas
```

**Ver qué tienes instalado:**

```bash
uv pip list                # todas las librerías del entorno
uv pip show pandas         # versión, dependencias y ubicación
```

```python
import pandas as pd
print(pd.__version__)      # verificar la versión desde el código
```

> Verificar la versión **no es opcional**. Muchos errores al seguir un tutorial vienen de que el tutorial usa una versión distinta a la tuya.

### Versionado semántico (SemVer)

Las versiones tienen la forma `MAYOR.MENOR.PARCHE` → `2.2.3`

|Parte|Cuándo cambia|¿Rompe tu código?|
|---|---|---|
|**MAYOR** (2.x.x)|Cambios incompatibles|✅ Sí, muy probablemente|
|**MENOR** (x.2.x)|Funcionalidad nueva, compatible|❌ No debería|
|**PARCHE** (x.x.3)|Corrección de bugs|❌ No|

Ejemplo real: al pasar de pandas 1.x a 2.x se eliminó `df.append()`. Código que funcionaba dejó de hacerlo de un día para otro.

### Reproducibilidad

El objetivo: que otra persona clone tu repositorio y obtenga **exactamente** tu mismo entorno.

```bash
# Con uv → pyproject.toml + uv.lock
uv sync                          # replica el entorno exacto

# Con pip → requirements.txt
pip freeze > requirements.txt
pip install -r requirements.txt
```

> El archivo `uv.lock` (o `requirements.txt`) **sí va al repositorio**. La carpeta `.venv/` **no**: va en el `.gitignore`.

### Diagnóstico rápido cuando "no funciona"

El 90 % de los errores de tipo `ModuleNotFoundError` se resuelven con estas tres líneas:

```python
import sys
print(sys.executable)      # ¿qué Python se está usando?
print(sys.path)            # ¿dónde busca las librerías?
```

Si `sys.executable` **no** apunta a tu carpeta `.venv`, ahí está el problema: el entorno no está activo o el kernel de Jupyter apunta a otro intérprete.

|Síntoma|Causa probable|Solución|
|---|---|---|
|`ModuleNotFoundError`|Entorno no activo o librería no instalada|Activa el `.venv` y reinstala|
|Funciona en terminal, falla en Jupyter|El kernel usa otro intérprete|Selecciona el kernel del `.venv`|
|Funciona en tu equipo, falla en el de otro|Falta `requirements.txt` / `uv.lock`|Congela y sube el archivo|
|El ejemplo del tutorial no corre|Versión distinta|Compara `__version__`|

### ✅ Checklist del entorno

- [ ] Creé el entorno (`uv venv` o `python -m venv .venv`)
- [ ] Lo activé y veo `(.venv)` en el prompt
- [ ] Instalé las librerías **después** de activar
- [ ] Congelé las versiones (`uv.lock` o `requirements.txt`)
- [ ] `.venv/` y `.env` están en el `.gitignore`
- [ ] `sys.executable` apunta a mi `.venv`

---

## 2. Manipulación de estructuras de datos

Una vez tienes el entorno listo, el siguiente paso es entender cómo viven los datos dentro de Python.

Los datos no existen en abstracto dentro del código: siempre están contenidos dentro de estructuras. Las más comunes son **listas** y **diccionarios**.

### Lista → una colección de valores

Una lista representa un conjunto de elementos del mismo tipo. En analítica, se parece a una **columna** de datos.

```python
edades = [23, 31, 19, 45, 28]

edades[0]        # 23  → acceso por índice
edades[-1]       # 28  → último elemento
len(edades)      # 5   → cantidad de elementos
edades.append(37)  # agrega al final
```

### Diccionario → un registro

Un diccionario representa datos estructurados mediante **claves y valores**. En analítica, se parece a una **fila** de datos con múltiples atributos.

```python
atleta = {
    "nombre": "Laura",
    "edad": 17,
    "categoria": "Juvenil",
    "asistencias": 22
}

atleta["nombre"]              # 'Laura'
atleta.get("posicion")        # None → no revienta si la clave no existe
atleta.get("posicion", "N/A") # 'N/A' → valor por defecto
atleta["posicion"] = "Central"  # agrega o actualiza
```

> **Clave:** usar `.get()` en lugar de `[]` evita un `KeyError` cuando el dato viene incompleto. En datos reales, esto pasa todo el tiempo.

### Lista de diccionarios → un dataset

Aquí es donde empieza a parecerse a datos reales: una lista de diccionarios es básicamente una **tabla**.

```python
equipo = [
    {"nombre": "Laura",  "edad": 17, "categoria": "Juvenil", "asistencias": 22},
    {"nombre": "Sofía",  "edad": 15, "categoria": "Infantil", "asistencias": 18},
    {"nombre": "Camila", "edad": 16, "categoria": "Infantil", "asistencias": 25},
    {"nombre": "Valeria","edad": 17, "categoria": "Juvenil", "asistencias": 12},
]

# Recorrer el dataset
for jugadora in equipo:
    print(jugadora["nombre"], "-", jugadora["asistencias"])

# Acceder a un valor puntual
equipo[0]["nombre"]   # 'Laura'
```

**Equivalencia mental:**

|Python|Analítica|pandas|
|---|---|---|
|Lista|Columna|`Series`|
|Diccionario|Fila / registro|Fila de un DataFrame|
|Lista de diccionarios|Tabla / dataset|`DataFrame`|

---

## 3. Comprensiones y operaciones avanzadas

Al inicio es común usar loops para procesar datos. Sin embargo, Python permite una forma más directa mediante **comprensiones**.

Esto cambia la forma de trabajar: pasas de _recorrer_ datos a _transformarlos_ directamente.

### Forma tradicional vs. comprensión

```python
# ❌ Forma tradicional
nombres = []
for jugadora in equipo:
    nombres.append(jugadora["nombre"])

# ✅ Comprensión
nombres = [jugadora["nombre"] for jugadora in equipo]
```

Ambas dan el mismo resultado: `['Laura', 'Sofía', 'Camila', 'Valeria']`

### Con filtro (condición)

La estructura es: `[expresión for elemento in colección if condición]`

```python
# Solo las juveniles
juveniles = [j for j in equipo if j["categoria"] == "Juvenil"]

# Solo los nombres de quienes tienen más de 20 asistencias
constantes = [j["nombre"] for j in equipo if j["asistencias"] > 20]
# ['Laura', 'Camila']
```

### Transformando valores

```python
# Convertir asistencias a porcentaje sobre 30 sesiones
porcentajes = [round(j["asistencias"] / 30 * 100, 1) for j in equipo]
# [73.3, 60.0, 83.3, 40.0]
```

### Comprensión de diccionarios

```python
# {clave: valor for ...}
asistencia_por_nombre = {j["nombre"]: j["asistencias"] for j in equipo}
# {'Laura': 22, 'Sofía': 18, 'Camila': 25, 'Valeria': 12}
```

### ¿Cuándo NO usar comprensiones?

- Cuando la lógica requiere varios pasos o condiciones anidadas → el loop normal se lee mejor
- Cuando necesitas manejar errores (`try/except`) dentro del proceso
- Regla práctica: **si no cabe cómodamente en una línea, usa un loop**

```python
# ❌ Ilegible
r = [x["a"]/x["b"] if x["b"] != 0 else 0 for x in datos if x.get("a") and x.get("activo")]
```

---

## 4. Manejo de archivos

Hasta ahora trabajaste con datos definidos dentro del código, pero en la realidad los datos vienen de fuentes externas.

La forma más común de recibir datos es a través de archivos. Los formatos más usados en analítica son **CSV**, **Excel** y **archivos de texto**.

Aprender a leer y escribir estos archivos es fundamental: es el primer punto de contacto con datos reales. Este paso conecta directamente con el mundo empresarial, donde la mayoría de los datos iniciales vienen en estos formatos.

### Lectura de TXT

```python
# 'with' cierra el archivo automáticamente, incluso si ocurre un error
with open("notas.txt", "r", encoding="utf-8") as archivo:
    contenido = archivo.read()        # todo el archivo como un string
    
with open("notas.txt", "r", encoding="utf-8") as archivo:
    lineas = archivo.readlines()      # lista de líneas (incluye el \n)

# Recomendado para archivos grandes: recorre línea por línea
with open("notas.txt", "r", encoding="utf-8") as archivo:
    for linea in archivo:
        print(linea.strip())          # .strip() quita espacios y saltos de línea
```

**Escribir:**

```python
with open("salida.txt", "w", encoding="utf-8") as archivo:   # "w" sobrescribe
    archivo.write("Reporte de asistencias\n")

with open("salida.txt", "a", encoding="utf-8") as archivo:   # "a" agrega al final
    archivo.write("Laura: 22\n")
```

|Modo|Significado|
|---|---|
|`"r"`|Lectura (por defecto). Falla si el archivo no existe|
|`"w"`|Escritura. **Borra** el contenido previo|
|`"a"`|Agregar al final|
|`"x"`|Crear. Falla si ya existe|

### Lectura de CSV

Un CSV es un archivo de texto donde los valores están separados por comas. Se puede leer manualmente, pero el módulo `csv` (viene con Python) maneja los casos difíciles: comas dentro de un valor, comillas, saltos de línea.

```python
import csv

# csv.reader → cada fila es una LISTA
with open("equipo.csv", "r", encoding="utf-8") as archivo:
    lector = csv.reader(archivo)
    encabezados = next(lector)     # consume la primera fila
    for fila in lector:
        print(fila)                # ['Laura', '17', 'Juvenil', '22']
```

```python
# csv.DictReader → cada fila es un DICCIONARIO (usa los encabezados como claves)
with open("equipo.csv", "r", encoding="utf-8") as archivo:
    lector = csv.DictReader(archivo)
    equipo = list(lector)

equipo[0]["nombre"]    # 'Laura'
```

> `DictReader` es casi siempre la mejor opción: el código queda legible y no depende del orden de las columnas.

### Escritura de CSV

```python
import csv

campos = ["nombre", "edad", "categoria", "asistencias"]

with open("reporte.csv", "w", encoding="utf-8", newline="") as archivo:
    escritor = csv.DictWriter(archivo, fieldnames=campos)
    escritor.writeheader()
    escritor.writerows(equipo)
```

> ⚠️ **Dos detalles que causan bugs:**
> 
> - `encoding="utf-8"` → sin esto, las tildes y la ñ se rompen
> - `newline=""` al escribir → sin esto, en Windows aparecen filas vacías entre cada registro

### ⚠️ Todo lo que se lee de un CSV es texto

```python
fila = {"nombre": "Laura", "edad": "17"}   # '17' es un STRING, no un número

fila["edad"] + 1        # ❌ TypeError
int(fila["edad"]) + 1   # ✅ 18
```

La conversión de tipos es uno de los primeros pasos de cualquier limpieza de datos.

---

## 5. Procesamiento de datos

Este es uno de los puntos más importantes de toda la clase.

Antes de introducir herramientas avanzadas, es necesario entender qué significa **procesar datos**: recorrer estructuras, aplicar condiciones, transformar valores y calcular métricas. Todo esto se puede hacer con Python puro.

Aquí se experimenta algo clave: trabajar con datos de esta forma puede volverse complejo y poco eficiente a medida que el volumen crece. **Y ese es precisamente el objetivo.** Porque cuando más adelante veas pandas, entenderás no solo cómo usarlo, sino por qué existe.

### Cálculo total y métricas básicas

```python
asistencias = [j["asistencias"] for j in equipo]

total    = sum(asistencias)                  # 77
promedio = sum(asistencias) / len(asistencias)  # 19.25
maximo   = max(asistencias)                  # 25
minimo   = min(asistencias)                  # 12
cantidad = len(asistencias)                  # 4

print(f"Promedio de asistencias: {promedio:.2f}")
```

### Filtrado

```python
# Filtrar con comprensión
juveniles = [j for j in equipo if j["categoria"] == "Juvenil"]

# Filtro con múltiples condiciones
destacadas = [
    j for j in equipo
    if j["asistencias"] >= 20 and j["edad"] >= 16
]

# Contar sin construir la lista completa (más eficiente)
total_juveniles = sum(1 for j in equipo if j["categoria"] == "Juvenil")
```

### Ordenamiento

```python
# Ordenar por asistencias, de mayor a menor
ranking = sorted(equipo, key=lambda j: j["asistencias"], reverse=True)

for i, j in enumerate(ranking, start=1):
    print(f"{i}. {j['nombre']}: {j['asistencias']}")
```

### Agrupación (el equivalente manual de un `GROUP BY`)

Este es el patrón más importante de esta sección: acumular resultados dentro de un diccionario.

```python
# Sumar asistencias por categoría
por_categoria = {}

for jugadora in equipo:
    cat = jugadora["categoria"]
    # setdefault crea la clave con 0 si no existe todavía
    por_categoria.setdefault(cat, 0)
    por_categoria[cat] += jugadora["asistencias"]

# {'Juvenil': 34, 'Infantil': 43}
```

Y para calcular un **promedio por grupo**, hay que acumular suma y conteo:

```python
acumulado = {}

for jugadora in equipo:
    cat = jugadora["categoria"]
    if cat not in acumulado:
        acumulado[cat] = {"suma": 0, "conteo": 0}
    acumulado[cat]["suma"] += jugadora["asistencias"]
    acumulado[cat]["conteo"] += 1

promedios = {
    cat: datos["suma"] / datos["conteo"]
    for cat, datos in acumulado.items()
}
# {'Juvenil': 17.0, 'Infantil': 21.5}
```

> 👉 Retén este bloque. En pandas, todo esto se reduce a una línea: `df.groupby("categoria")["asistencias"].mean()` Esa es exactamente la razón por la que existe la librería.

---

## 6. Manejo de errores y calidad de datos

En la teoría, los datos son perfectos. En la práctica, no lo son.

Los datos reales pueden contener errores, valores inesperados, formatos incorrectos o información incompleta. Por eso el manejo de errores no es opcional, es fundamental.

Aquí se introducen mecanismos como `try` y `except`, que permiten controlar fallos en el código. Pero más allá de la técnica, lo importante es el concepto: **el analista debe asumir que los datos pueden fallar y prepararse para manejar esos casos.**

### Estructura básica

```python
try:
    # código que puede fallar
    edad = int(fila["edad"])
except ValueError:
    # qué hacer si falla
    edad = None
else:
    # se ejecuta solo si NO hubo error
    print("Conversión exitosa")
finally:
    # se ejecuta siempre, haya error o no
    print("Registro procesado")
```

### Errores más comunes en analítica

|Error|Cuándo ocurre|Ejemplo|
|---|---|---|
|`ValueError`|El valor no se puede convertir|`int("dieciocho")`|
|`KeyError`|La clave no existe en el diccionario|`fila["telefono"]`|
|`IndexError`|El índice está fuera de rango|`lista[99]`|
|`FileNotFoundError`|La ruta del archivo no existe|`open("datos.csv")`|
|`TypeError`|Operación entre tipos incompatibles|`"17" + 1`|
|`ZeroDivisionError`|División entre cero|`total / 0`|

### Patrón: conversión segura

```python
def a_entero(valor, por_defecto=None):
    """Convierte a int sin romper el flujo si el dato viene sucio."""
    try:
        return int(valor)
    except (ValueError, TypeError):
        return por_defecto

a_entero("17")      # 17
a_entero("")        # None
a_entero("N/A")     # None
a_entero(None)      # None
a_entero("abc", 0)  # 0
```

### Patrón: no detener el pipeline por una fila mala

Esta es la mentalidad clave. Si el archivo tiene 10.000 filas y la fila 3.457 está corrupta, el proceso **no debe morir**: debe registrar el problema y continuar.

```python
import csv

validos = []
rechazados = []

with open("equipo.csv", encoding="utf-8") as archivo:
    for numero, fila in enumerate(csv.DictReader(archivo), start=2):
        try:
            registro = {
                "nombre": fila["nombre"].strip(),
                "edad": int(fila["edad"]),
                "asistencias": int(fila["asistencias"]),
            }
            if not registro["nombre"]:
                raise ValueError("nombre vacío")
            validos.append(registro)

        except (ValueError, KeyError) as error:
            rechazados.append({"fila": numero, "motivo": str(error), "datos": fila})

print(f"Procesados: {len(validos)} | Rechazados: {len(rechazados)}")
```

> ❌ **Nunca hagas esto:**
> 
> ```python
> try:
>     ...
> except:      # captura TODO, incluso errores de tipeo tuyos
>     pass     # y los oculta sin dejar rastro
> ```
> 
> Captura errores **específicos** y siempre deja registro de lo que falló.

Este punto conecta directamente con el tema de la siguiente semana: **limpieza de datos**.

---

## 7. Eficiencia en el procesamiento

Finalmente aparece un concepto más avanzado: la **eficiencia**.

No basta con que el código funcione. En analítica, los volúmenes de datos pueden ser grandes, y una mala implementación puede afectar el rendimiento. Aquí entran ideas como evitar operaciones innecesarias, elegir estructuras adecuadas y pensar en cómo escalar el procesamiento.

Este es el primer acercamiento a un pensamiento más técnico, donde no solo importa el resultado, sino **cómo se obtiene**.

### 1. Evitar recorrer los datos varias veces

```python
# ❌ Ineficiente: 3 recorridos completos
total    = sum(j["asistencias"] for j in equipo)
maximo   = max(j["asistencias"] for j in equipo)
cantidad = len([j for j in equipo if j["asistencias"] > 0])

# ✅ Eficiente: 1 solo recorrido
total = 0
maximo = float("-inf")
cantidad = 0

for j in equipo:
    a = j["asistencias"]
    total += a
    if a > maximo:
        maximo = a
    if a > 0:
        cantidad += 1
```

### 2. Elegir la estructura correcta: lista vs. diccionario/set

Buscar en una **lista** obliga a revisar elemento por elemento → _O(n)_. Buscar en un **diccionario** o **set** es prácticamente instantáneo → _O(1)_.

```python
# ❌ Ineficiente: por cada búsqueda recorre toda la lista
ids_activos = ["A01", "A02", "A03", ...]   # 10.000 elementos
for registro in datos:                      # 10.000 registros
    if registro["id"] in ids_activos:       # → 100.000.000 comparaciones
        ...

# ✅ Eficiente: la búsqueda es directa
ids_activos = {"A01", "A02", "A03", ...}   # set
for registro in datos:
    if registro["id"] in ids_activos:       # → 10.000 comparaciones
        ...
```

**Solo cambió un carácter (`[]` → `{}`) y el proceso pasa de minutos a segundos.**

### 3. Generadores para archivos grandes

`list(csv.DictReader(f))` carga **todo** el archivo en memoria. Con un archivo de 2 GB, eso revienta.

```python
import csv

def leer_registros(ruta):
    """Devuelve las filas una por una, sin cargar todo en memoria."""
    with open(ruta, encoding="utf-8") as archivo:
        for fila in csv.DictReader(archivo):
            yield fila

# Procesa 10 millones de filas usando memoria constante
total = sum(int(fila["asistencias"]) for fila in leer_registros("datos.csv"))
```

|Enfoque|Memoria|¿Se puede recorrer 2 veces?|
|---|---|---|
|`[x for x in ...]` (lista)|Todo el dataset|Sí|
|`(x for x in ...)` (generador)|Un elemento a la vez|No|

### 4. No construir strings ni listas dentro de un loop con `+`

```python
# ❌ Crea un string nuevo en cada iteración
texto = ""
for nombre in nombres:
    texto = texto + nombre + ", "

# ✅ Una sola operación
texto = ", ".join(nombres)
```

### 5. Sacar del loop lo que no cambia

```python
# ❌ Recalcula el promedio en cada vuelta
for j in equipo:
    if j["asistencias"] > sum(a["asistencias"] for a in equipo) / len(equipo):
        ...

# ✅ Se calcula una sola vez
promedio = sum(a["asistencias"] for a in equipo) / len(equipo)
for j in equipo:
    if j["asistencias"] > promedio:
        ...
```

### Cómo medir en lugar de suponer

```python
import time

inicio = time.perf_counter()
# ... el código a medir ...
print(f"Tiempo: {time.perf_counter() - inicio:.4f} s")
```

> **Regla:** primero haz que funcione, después que sea legible, y solo optimiza cuando midas que hay un problema real.

---

## 8. Cierre: el puente hacia pandas

Todo lo que se hizo en este apunte con Python puro es exactamente lo que pandas resume en pocas líneas:

|Con Python puro|Con pandas|
|---|---|
|`csv.DictReader` + `list()`|`pd.read_csv("datos.csv")`|
|`[j["nombre"] for j in equipo]`|`df["nombre"]`|
|`[j for j in equipo if j["edad"] > 16]`|`df[df["edad"] > 16]`|
|Loop acumulador con diccionario|`df.groupby("categoria").mean()`|
|`sum(...) / len(...)`|`df["asistencias"].mean()`|
|`sorted(equipo, key=lambda j: ...)`|`df.sort_values("asistencias")`|
|Loops manuales sobre cada fila|Operaciones vectorizadas en C|

Pandas no es magia: es una capa optimizada sobre estas mismas ideas. Entender la versión manual es lo que permite saber **qué está pasando por debajo** cuando algo no funciona como esperabas.

---

## ✅ Checklist de repaso

- [ ] Sé crear y activar un entorno virtual con `uv` y con `venv`
- [ ] Entiendo por qué una lista de diccionarios equivale a una tabla
- [ ] Puedo reescribir un loop `for` como comprensión (y sé cuándo no debo)
- [ ] Sé leer y escribir CSV con `DictReader` / `DictWriter`
- [ ] Recuerdo que todo lo que sale de un CSV es `str`
- [ ] Puedo agrupar y calcular promedios por categoría sin pandas
- [ ] Uso `try/except` con errores específicos, nunca `except:` vacío
- [ ] Sé cuándo usar un `set` en lugar de una lista para búsquedas
- [ ] Entiendo la diferencia entre una lista y un generador

## 🧾 Glosario

|Término|Definición|
|---|---|
|**Environment**|Espacio aislado con sus propias librerías y versiones|
|**Dependencia**|Librería externa que el proyecto necesita para funcionar|
|**Comprensión**|Sintaxis compacta para construir listas/diccionarios en una expresión|
|**Encoding**|Forma de codificar caracteres a bytes (`utf-8` soporta tildes y ñ)|
|**Excepción**|Error controlable en tiempo de ejecución|
|**Generador**|Objeto que produce valores uno a uno sin guardarlos todos en memoria|
|**Vectorización**|Aplicar una operación a toda una colección de golpe, sin loop en Python|