# 03B · Librerías para el análisis de datos — Parte 2: Visualización con Matplotlib

> **Paso 3 de 3 de la ruta.** Anatomía de una figura, sintaxis de Matplotlib, cuándo usar cada tipo de gráfico, personalización y principios de diseño visual.

### 🗺️ Ruta de estudio

|Paso|Archivo|Qué cubre|
|---|---|---|
|1|[`02`](https://claude.ai/chat/02_Uso_de_librerias_de_Python_para_analisis_de_datos.md)|Entorno de trabajo · Python puro para datos|
|2|[`03A`](https://claude.ai/chat/03A_Librerias_para_analisis_de_datos_Parte1_Fundamentos.md)|Qué son las librerías · NumPy · Pandas|
|**3**|**`03B` ← estás aquí**|Visualización con Matplotlib|

> 📌 **Prerrequisito:** manejar DataFrames (archivo 03A). Los ejemplos parten de datos ya limpios.

---

## 📑 Índice

1. [Qué es Matplotlib](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#1-qu%C3%A9-es-matplotlib)
2. [Anatomía de una figura](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#2-anatom%C3%ADa-de-una-figura)
3. [Las dos interfaces](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#3-las-dos-interfaces)
4. [Estructura estándar de un gráfico](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#4-estructura-est%C3%A1ndar-de-un-gr%C3%A1fico)
5. [Cuándo usar cada tipo de gráfico](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#5-cu%C3%A1ndo-usar-cada-tipo-de-gr%C3%A1fico)
6. [Tabla de decisión rápida](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#6-tabla-de-decisi%C3%B3n-r%C3%A1pida)
7. [Personalización](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#7-personalizaci%C3%B3n)
8. [Múltiples gráficos (subplots)](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#8-m%C3%BAltiples-gr%C3%A1ficos-subplots)
9. [Guardar y exportar](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#9-guardar-y-exportar)
10. [Matplotlib, Pandas y Seaborn](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#10-matplotlib-pandas-y-seaborn)
11. [Principios de diseño visual](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#11-principios-de-dise%C3%B1o-visual)
12. [Errores frecuentes al graficar](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#12-errores-frecuentes-al-graficar)
13. [Checklist y glosario](https://claude.ai/chat/6830f90f-a5e2-4133-84a8-70f836ddcde5#13-checklist-y-glosario)

---

## 1. Qué es Matplotlib

Matplotlib es una biblioteca de código abierto para Python y su extensión numérica NumPy, diseñada para crear visualizaciones de datos de alta calidad. Su objetivo principal es generar gráficos 2D y 3D estáticos, animados e interactivos, produciendo figuras con calidad de publicación en diversos formatos.

Desarrollada originalmente por **John D. Hunter en 2002** para visualizar señales cerebrales e inspirada en la sintaxis de MATLAB, Matplotlib ofrece una interfaz flexible tanto procedural como orientada a objetos. Es una herramienta fundamental en Data Science y Machine Learning para la exploración de datos, la identificación de patrones y la comunicación efectiva de resultados.

### Por qué importa aprenderla

Existen alternativas más cómodas (Seaborn, Plotly), pero **todas las de uso general están construidas sobre Matplotlib o imitan su modelo**. Cuando Seaborn no hace exactamente lo que necesitas, la salida es un objeto de Matplotlib que ajustas a mano. Sin esta base, quedas atrapado en lo que la librería de alto nivel decida por ti.

### Las dos funciones de un gráfico

|Propósito|Audiencia|Prioridad|
|---|---|---|
|**Explorar**|Tú mismo, durante el análisis|Velocidad. No pierdas tiempo en estética|
|**Comunicar**|Otras personas: jefe, cliente, informe|Claridad. Cada detalle cuenta|

Confundir ambos es un error clásico: pasar una hora puliendo un gráfico exploratorio que vas a descartar, o presentar un gráfico crudo sin títulos a un cliente.

---

## 2. Anatomía de una figura

Entender estos elementos evita el 80 % de la frustración inicial:

```
Figure (fig)  → el lienzo completo, la hoja de papel
 └── Axes (ax) → cada gráfico individual dentro del lienzo
      ├── Axis (eje X, eje Y) → escala, límites, ticks
      │    ├── Ticks         → las marcas
      │    └── Tick labels   → los números o textos de las marcas
      ├── Title              → título del gráfico
      ├── Labels             → etiquetas de los ejes
      ├── Legend             → leyenda
      ├── Spines             → los bordes del recuadro
      ├── Grid               → la cuadrícula
      └── Artists            → los datos dibujados (líneas, barras, puntos)
```

> ⚠️ **Ojo con el nombre:** **Axes** (el gráfico completo, con sus dos ejes) no es lo mismo que **Axis** (un solo eje). Es el peor nombre de la librería y confunde a todo el mundo. Piensa en `ax` como "un gráfico".

```python
fig, ax = plt.subplots()

fig    # el lienzo → controla tamaño, resolución, guardado
ax     # el gráfico → controla datos, títulos, ejes, leyenda
```

Regla práctica: **si afecta a toda la imagen, es `fig`. Si afecta a un gráfico concreto, es `ax`.**

---

## 3. Las dos interfaces

```python
import matplotlib.pyplot as plt

# 1. Interfaz pyplot (procedural) — rápida, para explorar
plt.plot(meses, ventas)
plt.title("Ventas mensuales")
plt.xlabel("Mes")
plt.show()

# 2. Interfaz orientada a objetos — ✅ la recomendada
fig, ax = plt.subplots(figsize=(8, 5))
ax.plot(meses, ventas)
ax.set_title("Ventas mensuales")
ax.set_xlabel("Mes")
plt.show()
```

**Usa la segunda.** Es más explícita, escala a múltiples gráficos y es la que verás en cualquier código profesional.

### Tabla de traducción entre interfaces

|pyplot|Orientada a objetos|
|---|---|
|`plt.plot()`|`ax.plot()`|
|`plt.title()`|`ax.set_title()`|
|`plt.xlabel()`|`ax.set_xlabel()`|
|`plt.ylabel()`|`ax.set_ylabel()`|
|`plt.xlim()`|`ax.set_xlim()`|
|`plt.xticks()`|`ax.set_xticks()`|
|`plt.legend()`|`ax.legend()`|
|`plt.grid()`|`ax.grid()`|

> El patrón es casi siempre el mismo: en la versión orientada a objetos se antepone `set_`.

### Por qué `plt.show()`

En un script, la figura se construye en memoria y `plt.show()` abre la ventana. En Jupyter, la figura se muestra automáticamente al final de la celda, pero incluir `plt.show()` evita que aparezca la línea de texto `<matplotlib.axes...>` sobre el gráfico.

---

## 4. Estructura estándar de un gráfico

Memoriza este esqueleto. Sirve para el 90 % de los casos:

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(10, 6))       # 1. crear lienzo y ejes

ax.plot(meses, ventas, marker="o",            # 2. dibujar los datos
        color="#2E86AB", linewidth=2, label="Región Norte")

ax.set_title("Ventas mensuales 2026", fontsize=14, fontweight="bold")  # 3. contexto
ax.set_xlabel("Mes")
ax.set_ylabel("Ventas (millones COP)")
ax.legend()
ax.grid(alpha=0.3)

plt.tight_layout()                             # 4. ajustar márgenes
plt.savefig("ventas.png", dpi=300, bbox_inches="tight")   # 5. guardar
plt.show()                                     # 6. mostrar
```

> ⚠️ `plt.savefig()` **siempre va antes** de `plt.show()`. Después de `show()`, la figura se limpia y guardarías una imagen en blanco.

**Qué hace cada paso:**

|Paso|Función|
|---|---|
|`plt.subplots(figsize=)`|Crea la figura y los ejes. `figsize` va en pulgadas|
|`ax.plot()` / `.bar()` / etc.|Dibuja los datos|
|`set_title` / `set_xlabel`|Da el contexto sin el cual el gráfico no comunica|
|`legend()`|Necesaria si hay más de una serie|
|`tight_layout()`|Evita que las etiquetas se corten|
|`savefig()`|Exporta a archivo|

---

## 5. Cuándo usar cada tipo de gráfico

Esta es la parte que realmente separa a un analista de alguien que solo dibuja. **El gráfico no se elige por gusto: se elige según la pregunta que quieres responder.**

### La regla base

|La pregunta es sobre...|Usa|
|---|---|
|Evolución en el tiempo|Línea, área|
|Comparación entre categorías|Barras|
|Distribución de una variable|Histograma, boxplot, violín|
|Relación entre dos variables|Dispersión|
|Composición de un total|Barras apiladas, torta (con cuidado)|
|Correlación entre muchas variables|Mapa de calor|

---

### 📈 Gráfico de líneas — `ax.plot()`

**Cuándo:** para mostrar cómo cambia una variable **continua a lo largo del tiempo** o de una secuencia ordenada.

**Por qué funciona:** la línea conecta los puntos y comunica _continuidad_. El ojo sigue la pendiente y detecta tendencias sin esfuerzo.

```python
fig, ax = plt.subplots(figsize=(9, 5))

ax.plot(meses, ventas_2025, marker="o", label="2025")
ax.plot(meses, ventas_2026, marker="s", label="2026", linestyle="--")

ax.set_title("Comparación de ventas anuales")
ax.set_ylabel("Ventas (millones COP)")
ax.legend()
ax.grid(alpha=0.3)
plt.show()
```

**Parámetros útiles:**

```python
ax.plot(x, y,
        color="#2E86AB",       # color de la línea
        linewidth=2,           # grosor
        linestyle="--",        # '-', '--', '-.', ':'
        marker="o",            # 'o', 's', '^', 'D', '*'
        markersize=6,
        alpha=0.8,             # transparencia
        label="Serie A")       # texto para la leyenda
```

- ✅ Ventas mensuales, temperatura diaria, usuarios activos por semana, peso de un atleta a lo largo de la temporada
- ❌ **No lo uses con categorías sin orden** (ciudades, productos). Conectar "Bogotá" con "Medellín" con una línea sugiere una transición que no existe
- 💡 Máximo 4-5 líneas por gráfico. Más que eso se vuelve un plato de espagueti
- 💡 Si hay pocos puntos (menos de 15), agrega `marker="o"` para que se vean las mediciones reales
- 💡 Cuando hay muchas series, resalta una en color fuerte y deja las demás en gris claro

---

### 📊 Gráfico de barras — `ax.bar()` / `ax.barh()`

**Cuándo:** para **comparar magnitudes entre categorías** distintas.

**Por qué funciona:** comparar longitudes es una de las tareas visuales que el cerebro humano hace con mayor precisión. Por eso las barras son el gráfico más confiable que existe.

```python
fig, ax = plt.subplots(figsize=(9, 5))

ax.bar(categorias, ventas, color="#2E86AB", edgecolor="white")

ax.set_title("Ventas por categoría de producto")
ax.set_ylabel("Unidades vendidas")
ax.spines[["top", "right"]].set_visible(False)   # quitar bordes innecesarios
plt.show()
```

**Barras horizontales** (mejor cuando los nombres son largos):

```python
fig, ax = plt.subplots(figsize=(8, 6))
ax.barh(categorias, ventas)
ax.invert_yaxis()      # deja el valor más alto arriba
plt.show()
```

**Etiquetar el valor sobre cada barra:**

```python
barras = ax.bar(categorias, ventas)
ax.bar_label(barras, fmt="%.0f", padding=3)
```

- ✅ Ventas por región, asistencias por jugadora, votos por candidato, errores por tipo
- ⚠️ **El eje Y debe empezar en cero.** Si lo cortas, exageras visualmente las diferencias. Este es el error más común y más engañoso en gráficos de barras. (En un gráfico de líneas sí es aceptable cortar el eje, porque ahí lo que se lee es la pendiente, no la longitud)
- 💡 Ordena las barras de mayor a menor, salvo que el orden tenga significado propio (meses, categorías de edad)
- 💡 Con más de 8-10 categorías o nombres largos, usa barras horizontales
- 💡 Un solo color para todas las barras, salvo que quieras destacar una específica

---

### 📊 Barras agrupadas y apiladas

**Agrupadas:** comparar subgrupos entre sí dentro de cada categoría.

```python
import numpy as np

x = np.arange(len(categorias))
ancho = 0.35

fig, ax = plt.subplots(figsize=(9, 5))
ax.bar(x - ancho/2, ventas_2025, ancho, label="2025")
ax.bar(x + ancho/2, ventas_2026, ancho, label="2026")

ax.set_xticks(x)
ax.set_xticklabels(categorias)
ax.legend()
plt.show()
```

**Apiladas:** mostrar la composición del total y cómo cada parte contribuye.

```python
fig, ax = plt.subplots(figsize=(9, 5))
ax.bar(categorias, producto_a, label="Producto A")
ax.bar(categorias, producto_b, bottom=producto_a, label="Producto B")
ax.legend()
plt.show()
```

- ⚠️ En las apiladas, **solo el segmento de abajo es fácil de comparar** entre categorías. Los de arriba "flotan" sobre bases distintas y el ojo no puede medirlos con precisión
- 💡 Si lo que importa es comparar los subgrupos, usa agrupadas. Si importa el total, usa apiladas
- 💡 Máximo 3-4 series apiladas

---

### 📉 Histograma — `ax.hist()`

**Cuándo:** para ver **cómo se distribuye una sola variable numérica**: dónde se concentran los valores, si hay sesgo, si hay más de un pico.

**Por qué funciona:** agrupa los valores en intervalos (_bins_) y muestra la frecuencia de cada uno.

```python
fig, ax = plt.subplots(figsize=(9, 5))

ax.hist(edades, bins=15, color="#2E86AB", edgecolor="white")
ax.axvline(np.mean(edades), color="red", linestyle="--", label="Media")

ax.set_title("Distribución de edades")
ax.set_xlabel("Edad")
ax.set_ylabel("Frecuencia")
ax.legend()
plt.show()
```

**Qué leer en un histograma:**

|Forma|Interpretación|
|---|---|
|Simétrica, campana|Distribución normal|
|Cola larga a la derecha|Sesgo positivo (típico en salarios, ventas)|
|Cola larga a la izquierda|Sesgo negativo|
|Dos picos|Bimodal → probablemente hay dos grupos mezclados|
|Barras aisladas al extremo|Posibles outliers o errores de captura|

- ✅ Distribución de salarios, edades, tiempos de entrega, calificaciones
- ⚠️ **No es lo mismo que un gráfico de barras.** El histograma va sobre una variable **numérica continua** y sus barras se tocan; el de barras va sobre **categorías** y sus barras están separadas
- 💡 El número de `bins` cambia la historia. Prueba varios valores: muy pocos ocultan detalle, demasiados generan ruido. `bins="auto"` da un punto de partida razonable
- 💡 Agregar una línea vertical con la media o la mediana ayuda mucho a la lectura

---

### 📦 Diagrama de caja y bigotes (boxplot) — `ax.boxplot()`

**Cuándo:** para resumir una distribución en cinco números y, sobre todo, para **comparar distribuciones entre grupos** y **detectar valores atípicos**.

**Cómo leerlo:**

```
     │    ← bigote superior (hasta Q3 + 1.5 × RIC)
   ┌─┴─┐  ← Q3 (percentil 75)
   │───│  ← mediana (Q2, percentil 50)
   └─┬─┘  ← Q1 (percentil 25)
     │    ← bigote inferior (hasta Q1 − 1.5 × RIC)
     ●    ← valores atípicos (outliers)
```

La caja contiene el **50 % central** de los datos. Su altura es el **rango intercuartílico (RIC = Q3 − Q1)**, la medida de dispersión más robusta frente a valores extremos.

```python
fig, ax = plt.subplots(figsize=(9, 5))

ax.boxplot([sueldos_junior, sueldos_semi, sueldos_senior],
           tick_labels=["Junior", "Semi", "Senior"])

ax.set_title("Distribución salarial por nivel")
ax.set_ylabel("Salario (COP)")
ax.grid(axis="y", alpha=0.3)
plt.show()
```

> En Matplotlib anterior a la 3.9, el parámetro se llama `labels=` en lugar de `tick_labels=`. Verifica con `matplotlib.__version__`.

**Qué comparar entre cajas:**

- **Posición de la mediana** → ¿qué grupo tiene valores típicamente más altos?
    
- **Altura de la caja** → ¿qué grupo es más disperso?
    
- **Longitud de los bigotes** → ¿hay asimetría?
    
- **Puntos sueltos** → ¿dónde están los casos excepcionales?
    
- ✅ Comparar salarios por nivel, tiempos de respuesta por servidor, notas por curso
    
- ✅ **Es la mejor herramienta para detectar outliers** antes de limpiar datos
    
- ⚠️ Oculta la forma de la distribución: dos conjuntos muy distintos pueden producir el mismo boxplot. Si la forma importa, acompáñalo de un histograma o usa un violín
    
- 💡 Un outlier no siempre es un error. Puede ser el dato más interesante del dataset. Investígalo antes de eliminarlo
    

---

### 🎻 Gráfico de violín — `ax.violinplot()`

**Cuándo:** cuando necesitas lo del boxplot **más** la forma completa de la distribución.

```python
fig, ax = plt.subplots(figsize=(9, 5))
ax.violinplot([grupo_a, grupo_b, grupo_c], showmedians=True)
ax.set_xticks([1, 2, 3])
ax.set_xticklabels(["A", "B", "C"])
plt.show()
```

- ✅ Comparar distribuciones donde sospechas bimodalidad
- ⚠️ Menos conocido: si tu audiencia no es técnica, quizá no lo interprete bien. En una presentación gerencial, un boxplot o un histograma comunican mejor

---

### 🔵 Gráfico de dispersión (scatter) — `ax.scatter()`

**Cuándo:** para explorar la **relación entre dos variables numéricas**. ¿Cuando una sube, la otra también?

```python
fig, ax = plt.subplots(figsize=(8, 6))

ax.scatter(horas_entrenamiento, rendimiento,
           alpha=0.6, s=60, c="#2E86AB", edgecolors="white")

ax.set_title("Relación entre entrenamiento y rendimiento")
ax.set_xlabel("Horas de entrenamiento semanales")
ax.set_ylabel("Puntaje de rendimiento")
plt.show()
```

**Codificar más variables:**

```python
ax.scatter(x, y,
           s=poblacion / 1000,      # tamaño → tercera variable
           c=ingresos,              # color → cuarta variable
           cmap="viridis",
           alpha=0.6)
```

**Qué buscar:**

|Patrón|Interpretación|
|---|---|
|Nube ascendente|Correlación positiva|
|Nube descendente|Correlación negativa|
|Nube sin forma|Sin relación lineal|
|Curva|Relación no lineal (una regresión lineal fallaría)|
|Grupos separados|Posibles subpoblaciones|
|Puntos muy alejados|Outliers|

- ✅ Precio vs. metros cuadrados, horas de estudio vs. nota, edad vs. ingresos
- 💡 Con muchos puntos superpuestos, usa `alpha=0.3` para ver la densidad
- ⚠️ **Correlación no implica causalidad.** Que dos variables se muevan juntas no significa que una cause la otra. Puede haber una tercera variable explicando ambas
- 💡 El scatter es el gráfico donde más se descubren cosas inesperadas. Úsalo temprano en la exploración

---

### 🥧 Gráfico de torta (pie) — `ax.pie()`

**Cuándo:** para mostrar la **composición porcentual de un total**, y solo bajo condiciones estrictas.

```python
fig, ax = plt.subplots(figsize=(7, 7))

ax.pie(valores, labels=categorias, autopct="%1.1f%%",
       startangle=90, counterclock=False)

ax.set_title("Distribución del presupuesto")
plt.show()
```

**Úsalo solo si se cumple todo esto:**

- ✅ Las partes suman el 100 % de un total real
- ✅ Hay **máximo 5 categorías**
- ✅ Las diferencias entre porciones son **evidentes**

**Los problemas:**

- ⚠️ **Es el gráfico más criticado en analítica**, y con razón: el ojo humano compara ángulos y áreas mucho peor que longitudes. Con porciones parecidas (24 %, 26 %, 25 %) es imposible saber cuál es mayor sin leer los números
- ⚠️ No permite comparar dos periodos de forma efectiva
- ⚠️ Con muchas categorías se vuelve ilegible
- ❌ Nunca uses tortas en 3D ni de dona con muchas porciones

> 💡 **En la mayoría de casos, un gráfico de barras ordenado comunica lo mismo mejor.** Si dudas, usa barras. Si el jefe pide una torta, hazla, pero pon los porcentajes visibles.

---

### 🌡️ Mapa de calor (heatmap) — `sns.heatmap()` / `ax.imshow()`

**Cuándo:** para visualizar una **matriz de valores** mediante intensidad de color. El uso más común es la matriz de correlación.

```python
import seaborn as sns

fig, ax = plt.subplots(figsize=(8, 6))
sns.heatmap(df.corr(numeric_only=True),
            annot=True, fmt=".2f",
            cmap="coolwarm", center=0,
            vmin=-1, vmax=1, ax=ax)
ax.set_title("Matriz de correlación")
plt.show()
```

- ✅ Correlaciones entre variables, actividad por día y hora, tablas cruzadas, matrices de confusión
- 💡 Para correlaciones usa una paleta **divergente** (`coolwarm`, `RdBu`) centrada en cero
- 💡 Para magnitudes usa una paleta **secuencial** (`viridis`, `Blues`)
- ⚠️ Elige paletas legibles para daltonismo. `viridis` es la opción segura
- 💡 `annot=True` muestra los números: sin ellos, el lector solo puede estimar

---

### 📐 Gráfico de área — `ax.fill_between()` / `ax.stackplot()`

**Cuándo:** como un gráfico de líneas, pero cuando quieres enfatizar el **volumen acumulado** o la composición del total en el tiempo.

```python
fig, ax = plt.subplots(figsize=(9, 5))

ax.stackplot(meses, producto_a, producto_b, producto_c,
             labels=["A", "B", "C"], alpha=0.8)
ax.legend(loc="upper left")
plt.show()
```

- ✅ Evolución de la cuota de mercado, ingresos acumulados por línea de producto
- ⚠️ Con áreas apiladas, solo la serie inferior se lee con precisión
- 💡 `fill_between()` también sirve para sombrear bandas de incertidumbre alrededor de una línea

---

## 6. Tabla de decisión rápida

|Quiero mostrar...|Gráfico recomendado|Evita|
|---|---|---|
|Tendencia en el tiempo|Línea, área|Torta, barras si hay muchos puntos|
|Comparar categorías|Barras (ordenadas)|Línea, torta|
|Distribución de una variable|Histograma|Barras|
|Comparar distribuciones|Boxplot / violín|Barras de promedios|
|Relación entre dos variables|Dispersión|Barras|
|Composición de un total|Barras apiladas|Torta con >5 partes|
|Composición en el tiempo|Área apilada|Varias tortas|
|Detectar outliers|Boxplot / dispersión|Promedios solos|
|Correlación entre variables|Mapa de calor|Muchos scatter sueltos|
|Ranking (top 10)|Barras horizontales|Torta|
|Parte de un todo, pocas categorías|Barras o torta simple|Torta 3D|

---

## 7. Personalización

### Colores

```python
ax.plot(x, y, color="red")          # nombre
ax.plot(x, y, color="#2E86AB")      # hexadecimal (más control)
ax.plot(x, y, color=(0.2, 0.4, 0.6))  # RGB de 0 a 1
ax.plot(x, y, color="C0")           # color 0 del ciclo por defecto
```

**Una paleta base sobria y funcional:**

```python
COLORES = {
    "principal": "#2E86AB",
    "secundario": "#A23B72",
    "acento":     "#F18F01",
    "neutro":     "#9CA3AF",
    "texto":      "#1F2937",
}
```

### Estilos predefinidos

```python
print(plt.style.available)          # ver todos los disponibles

plt.style.use("seaborn-v0_8-whitegrid")   # limpio, con cuadrícula
plt.style.use("ggplot")
plt.style.use("fivethirtyeight")
plt.style.use("default")                   # volver al original
```

> En Matplotlib 3.6+ los estilos de Seaborn se renombraron con el prefijo `seaborn-v0_8-`.

### Configuración global (rcParams)

```python
plt.rcParams["figure.figsize"] = (10, 6)
plt.rcParams["font.size"] = 11
plt.rcParams["axes.titlesize"] = 14
plt.rcParams["axes.spines.top"] = False
plt.rcParams["axes.spines.right"] = False
plt.rcParams["figure.dpi"] = 100
```

Define esto una vez al inicio del notebook y todos tus gráficos quedan consistentes.

### Ejes, ticks y formato

```python
ax.set_xlim(0, 100)                       # límites
ax.set_ylim(bottom=0)                     # forzar que el eje Y arranque en cero

ax.set_xticks([0, 25, 50, 75, 100])       # posiciones de las marcas
ax.set_xticklabels(["Q1", "Q2", "Q3", "Q4", "Fin"])
ax.tick_params(axis="x", rotation=45)     # rotar etiquetas largas

ax.set_yscale("log")                      # escala logarítmica

# Formatear el eje Y como miles con separador
from matplotlib.ticker import FuncFormatter
ax.yaxis.set_major_formatter(FuncFormatter(lambda v, p: f"{v:,.0f}"))
```

### Anotaciones y líneas de referencia

```python
ax.axhline(y=meta, color="red", linestyle="--", label="Meta")
ax.axvline(x=fecha_lanzamiento, color="gray", linestyle=":")

ax.annotate("Pico histórico",
            xy=(fecha_pico, valor_pico),          # punto señalado
            xytext=(fecha_pico, valor_pico * 1.2), # posición del texto
            arrowprops=dict(arrowstyle="->", color="gray"))

ax.text(0.02, 0.95, "Fuente: DANE 2026",
        transform=ax.transAxes, fontsize=8, color="gray")
```

> `transform=ax.transAxes` usa coordenadas de 0 a 1 relativas al gráfico, en lugar de los valores de los datos. Muy útil para notas al pie.

### Leyenda

```python
ax.legend(loc="upper left",       # 'best', 'upper right', 'lower center'...
          frameon=False,          # sin recuadro
          ncol=2,                 # en dos columnas
          fontsize=9)
```

### Quitar ruido visual

```python
ax.spines[["top", "right"]].set_visible(False)   # quitar bordes superior y derecho
ax.grid(axis="y", alpha=0.3)                     # cuadrícula solo horizontal
ax.set_facecolor("white")
```

---

## 8. Múltiples gráficos (subplots)

```python
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(12, 5))

axes[0].bar(categorias, valores)
axes[0].set_title("Ventas por categoría")

axes[1].hist(edades, bins=15)
axes[1].set_title("Distribución de edades")

plt.tight_layout()
plt.show()
```

**Cuadrícula 2×2:**

```python
fig, axes = plt.subplots(2, 2, figsize=(12, 8))

axes[0, 0].plot(x, y1);    axes[0, 0].set_title("Tendencia")
axes[0, 1].bar(cat, v);    axes[0, 1].set_title("Comparación")
axes[1, 0].hist(datos);    axes[1, 0].set_title("Distribución")
axes[1, 1].scatter(a, b);  axes[1, 1].set_title("Relación")

fig.suptitle("Dashboard de análisis", fontsize=16)
plt.tight_layout()
plt.show()
```

**Recorrer todos los ejes:**

```python
for ax, columna in zip(axes.flat, df.columns):
    ax.hist(df[columna], bins=20)
    ax.set_title(columna)
```

**Compartir ejes** (útil para comparar en la misma escala):

```python
fig, axes = plt.subplots(1, 3, figsize=(14, 4), sharey=True)
```

---

## 9. Guardar y exportar

```python
plt.savefig("grafico.png", dpi=300, bbox_inches="tight")
plt.savefig("grafico.pdf", bbox_inches="tight")       # vectorial, para imprimir
plt.savefig("grafico.svg")                            # vectorial, para web
plt.savefig("grafico.png", transparent=True)          # fondo transparente
```

|Formato|Cuándo usarlo|
|---|---|
|**PNG**|Presentaciones, web, informes digitales|
|**PDF / SVG**|Documentos para imprimir. Vectorial: no pierde calidad al ampliar|
|**JPG**|Casi nunca: comprime con pérdida y ensucia los bordes|

- `dpi=300` es el mínimo para calidad de impresión; `dpi=100` basta para pantalla
- `bbox_inches="tight"` recorta el espacio en blanco sobrante
- Recuerda: **`savefig()` antes de `show()`**

---

## 10. Matplotlib, Pandas y Seaborn

### Graficar directo desde pandas

Pandas tiene un atajo que usa Matplotlib por debajo. Perfecto para exploración rápida:

```python
df["ventas"].plot(kind="line")
df["ventas"].plot(kind="hist", bins=20)
df.plot(kind="scatter", x="precio", y="ventas")
df.groupby("region")["ventas"].sum().plot(kind="bar")

# Devuelve un objeto Axes → se puede personalizar
ax = df.plot(kind="bar", figsize=(9, 5))
ax.set_title("Ventas")
```

Valores de `kind`: `line`, `bar`, `barh`, `hist`, `box`, `kde`, `area`, `pie`, `scatter`, `hexbin`.

### Seaborn: menos código, mejor diseño por defecto

```python
import seaborn as sns

sns.barplot(data=df, x="region", y="ventas")
sns.histplot(data=df, x="edad", bins=20, kde=True)
sns.boxplot(data=df, x="nivel", y="salario")
sns.scatterplot(data=df, x="precio", y="ventas", hue="region", size="cantidad")
sns.heatmap(df.corr(numeric_only=True), annot=True)
sns.pairplot(df)          # todos los scatter cruzados de golpe
```

**Seaborn devuelve objetos de Matplotlib**, así que puedes seguir ajustando:

```python
fig, ax = plt.subplots(figsize=(9, 5))
sns.barplot(data=df, x="region", y="ventas", ax=ax)
ax.set_title("Ventas por región")
ax.set_ylabel("Millones COP")
plt.show()
```

### Cuál usar

|Situación|Herramienta|
|---|---|
|Exploración rápida durante el análisis|`df.plot()` o Seaborn|
|Gráfico estadístico (distribuciones, categorías)|Seaborn|
|Control total, gráfico final para presentar|Matplotlib|
|Gráfico interactivo para un dashboard|Plotly|

---

## 11. Principios de diseño visual

### La jerarquía de la percepción

Cleveland y McGill demostraron experimentalmente qué codificaciones visuales lee el ojo humano con mayor precisión. De mejor a peor:

1. **Posición en una escala común** ← barras, líneas, dispersión
2. Posición en escalas no alineadas
3. **Longitud** ← barras
4. Ángulo / pendiente ← torta
5. Área ← burbujas
6. Volumen ← gráficos 3D
7. Color e intensidad ← mapas de calor

> Esta lista **es** la explicación de por qué las barras funcionan y las tortas 3D no. No es una cuestión de gusto: es percepción medida.

### Data-ink ratio

Todo píxel que no comunica información es ruido. Elimina:

- Bordes innecesarios del recuadro
- Cuadrículas densas y oscuras
- Fondos de color
- Sombras, degradados y efectos 3D
- Leyendas redundantes cuando puedes etiquetar directo sobre el dato

### Contexto mínimo obligatorio

Un gráfico que sale de tu computador hacia otra persona **siempre** debe tener:

- Título que diga **qué** se está viendo
- Etiquetas en ambos ejes, **con unidades**
- Leyenda si hay más de una serie
- Fuente y periodo de los datos

### Color con propósito

- Usa color para **distinguir** categorías o **destacar** un elemento, nunca por decoración
- Un solo color para una sola serie
- Gris para el contexto, color fuerte para lo que importa
- Evita rojo y verde como única distinción: ~8 % de los hombres tiene daltonismo
- Máximo 5-6 colores distintos en un gráfico

### La honestidad del eje

|Situación|Regla|
|---|---|
|Barras|El eje debe empezar en **cero**, siempre|
|Líneas|Puede no empezar en cero, porque se lee la pendiente|
|Escala logarítmica|Válida, pero **indícalo explícitamente**|
|Ejes dobles|Evítalos: permiten manipular la aparente correlación|

---

## 12. Errores frecuentes al graficar

1. **Cortar el eje Y en barras** → exagera diferencias que no existen
2. **Gráficos sin título ni etiquetas** → el lector no sabe qué ve ni en qué unidades
3. **Demasiadas series juntas** → el gráfico deja de comunicar
4. **Efectos 3D** → distorsionan la percepción sin aportar información
5. **Mostrar solo promedios** → el promedio esconde la dispersión y los outliers
6. **Usar torta por costumbre** → casi siempre hay una mejor opción
7. **Paletas ilegibles** → rojo y verde juntos excluyen a parte de tu audiencia
8. **Líneas para categorías sin orden** → sugiere una continuidad falsa
9. **`savefig()` después de `show()`** → guarda una imagen en blanco
10. **Olvidar `tight_layout()`** → las etiquetas salen cortadas
11. **Elegir el gráfico antes que la pregunta** → el orden correcto es al revés
12. **Copiar colores llamativos de una plantilla** → el color debe servir al dato

> **Regla de oro:** un gráfico debe poder entenderse **sin que tú estés al lado explicándolo**.

---

## 13. Checklist y glosario

### ✅ Checklist antes de entregar un gráfico

- [ ] ¿Cuál es la pregunta que responde este gráfico?
- [ ] ¿El tipo de gráfico es el adecuado para esa pregunta?
- [ ] ¿Tiene título descriptivo?
- [ ] ¿Ambos ejes están etiquetados, con unidades?
- [ ] ¿El eje empieza en cero (si son barras)?
- [ ] ¿Hay leyenda cuando hay más de una serie?
- [ ] ¿Se indica la fuente y el periodo de los datos?
- [ ] ¿Eliminé el ruido visual (bordes, sombras, 3D)?
- [ ] ¿Los colores son legibles y tienen propósito?
- [ ] ¿Alguien que no conoce el proyecto lo entendería solo?
- [ ] ¿Guardé con `savefig()` **antes** de `show()`?

### ✅ Checklist de repaso técnico

- [ ] Distingo `Figure` de `Axes` (y `Axes` de `Axis`)
- [ ] Uso la interfaz orientada a objetos (`fig, ax = plt.subplots()`)
- [ ] Sé traducir entre `plt.title()` y `ax.set_title()`
- [ ] Puedo crear una cuadrícula de subplots y recorrerla
- [ ] Sé cuándo usar cada uno de los 9 tipos de gráfico
- [ ] Sé leer un boxplot: mediana, cuartiles, bigotes, outliers
- [ ] Distingo histograma de gráfico de barras
- [ ] Sé cuándo **no** usar un gráfico de torta
- [ ] Puedo personalizar colores, ticks, anotaciones y leyenda
- [ ] Sé exportar en el formato correcto según el destino

### 🧾 Glosario

|Término|Definición|
|---|---|
|**Figure**|El lienzo completo que contiene uno o más gráficos|
|**Axes**|Un gráfico individual dentro de la figura|
|**Axis**|Un eje concreto (X o Y) con su escala y marcas|
|**Artist**|Cualquier elemento dibujable de Matplotlib|
|**Tick**|Marca sobre un eje|
|**Spine**|Cada uno de los cuatro bordes del recuadro del gráfico|
|**Bin**|Intervalo en el que un histograma agrupa los valores|
|**RIC**|Rango intercuartílico: el 50 % central de los datos (Q3 − Q1)|
|**Outlier**|Valor atípico, alejado del resto de la distribución|
|**Cuartil**|Cada uno de los tres valores que dividen los datos en cuatro partes|
|**DPI**|Puntos por pulgada; define la resolución de la imagen exportada|
|**Colormap**|Paleta de colores usada para representar valores numéricos|
|**Paleta divergente**|Con dos extremos y un centro neutro (para valores +/−)|
|**Paleta secuencial**|De claro a oscuro (para magnitudes)|
|**rcParams**|Configuración global de estilo de Matplotlib|
|**Data-ink ratio**|Proporción de tinta que realmente comunica información|

---

**👈 Volver a la [Parte 1: Fundamentos y ecosistema](https://claude.ai/chat/03A_Librerias_para_analisis_de_datos_Parte1_Fundamentos.md)**