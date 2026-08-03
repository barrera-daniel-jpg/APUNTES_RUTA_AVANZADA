# Apuntes — Analítica de Datos: OLTP, OLAP y Modelado Dimensional

> **Temario de la clase**
> 1. ¿Qué es la analítica de datos?
> 2. OLTP y OLAP
> 3. Flujo de datos en una organización (ETL)
> 4. Modelado de datos para analítica
> 5. Python: qué es e historia
> 6. Ciencia vs. Ingeniería vs. Análisis de datos
>
> Formato: concepto → relaciones → ejemplos → glosario → preguntas de repaso

---

<a id="indice"></a>

## Índice de contenidos

- **[1. ¿Qué es la analítica de datos?](#s1)**
  - [1.1 Definición](#s1-1)
  - [1.2 Los cuatro niveles de analítica](#s1-2)
  - [1.3 Métrica vs. KPI](#s1-3)
  - [1.4 Calidad de datos: las seis dimensiones](#s1-4)
- **[2. Diferencia entre Análisis, Ciencia e Ingeniería de datos](#s2)**
  - [2.1 Comparación directa](#s2-1)
  - [2.2 Cómo se relacionan en el flujo](#s2-2)
  - [2.3 Roles relacionados que suelen aparecer](#s2-3)
  - [2.4 Big Data: las 5 V](#s2-4)
- **[3. OLTP y OLAP](#s3)**
  - [3.1 OLTP — Online Transaction Processing](#s3-1)
  - [3.2 OLAP — Online Analytical Processing](#s3-2)
  - [3.3 Tabla comparativa completa](#s3-3)
  - [3.4 Por qué el almacenamiento columnar cambia todo](#s3-4)
  - [3.5 Por qué no se analiza directo sobre el OLTP](#s3-5)
- **[4. Flujo de datos en una organización (ETL)](#s4)**
  - [4.1 El recorrido completo](#s4-1)
  - [4.2 Las tres fases](#s4-2)
  - [4.3 ETL vs ELT](#s4-3)
  - [4.4 Repositorios: Warehouse, Lake, Mart, Lakehouse](#s4-4)
  - [4.5 Conceptos operativos del pipeline](#s4-5)
- **[5. Modelado de datos para analítica](#s5)**
  - [5.1 Qué es el modelado de datos](#s5-1)
  - [5.2 Los tres niveles](#s5-2)
  - [5.3 Piezas del modelo](#s5-3)
  - [5.4 Modelado transaccional vs modelado analítico](#s5-4)
  - [5.5 Modelado dimensional: hechos y dimensiones](#s5-5)
  - [5.6 Esquema estrella y copo de nieve](#s5-6)
  - [5.7 Conceptos avanzados del modelado dimensional](#s5-7)
  - [5.8 Kimball vs Inmon](#s5-8)
- **[6. Python](#s6)**
  - [6.1 Qué es](#s6-1)
  - [6.2 Historia](#s6-2)
  - [6.3 Por qué Python domina el mundo de datos](#s6-3)
  - [6.4 Python y SQL: no compiten](#s6-4)
  - [6.5 Sintaxis mínima](#s6-5)
  - [6.6 Ecosistema por etapa del flujo de datos](#s6-6)
- **[7. Mapa de relaciones entre todos los conceptos](#s7)**
- **[8. Glosario](#s8)**
- **[9. Preguntas de repaso](#s9)**
  - [Nivel básico](#nivel-basico)
  - [Nivel intermedio](#nivel-intermedio)
  - [Nivel de aplicación y conceptual](#nivel-de-aplicacion-y-conceptual)
- **[10. Errores conceptuales frecuentes](#s10)**
- **[11. Ruta de estudio sugerida](#s11)**
- **[12. Recursos recomendados](#s12)**

### Acceso rápido por concepto

| Concepto | Sección |
|---|---|
| Analítica descriptiva / diagnóstica / predictiva / prescriptiva | [1.2](#s1-2) |
| Métrica vs. KPI | [1.3](#s1-3) |
| Calidad de datos (6 dimensiones) | [1.4](#s1-4) |
| Análisis vs. Ciencia vs. Ingeniería | [2.1](#s2-1) |
| Roles del área de datos | [2.3](#s2-3) |
| Big Data (5 V) | [2.4](#s2-4) |
| OLTP | [3.1](#s3-1) |
| OLAP | [3.2](#s3-2) |
| Tabla comparativa OLTP/OLAP | [3.3](#s3-3) |
| Almacenamiento columnar | [3.4](#s3-4) |
| ETL — las tres fases | [4.2](#s4-2) |
| ETL vs. ELT | [4.3](#s4-3) |
| Warehouse / Lake / Mart / Lakehouse | [4.4](#s4-4) |
| Idempotencia, linaje, CDC | [4.5](#s4-5) |
| Niveles del modelado (conceptual/lógico/físico) | [5.2](#s5-2) |
| PK, FK, cardinalidad | [5.3](#s5-3) |
| Normalización (1FN, 2FN, 3FN) | [5.4](#s5-4) |
| Hechos y dimensiones | [5.5](#s5-5) |
| Esquema estrella / copo de nieve / constelación | [5.6](#s5-6) |
| Granularidad | [5.7](#s5-7) |
| Llaves subrogadas | [5.7](#s5-7) |
| SCD tipos 1, 2 y 3 | [5.7](#s5-7) |
| Aditividad de métricas | [5.7](#s5-7) |
| Kimball vs. Inmon | [5.8](#s5-8) |
| Historia de Python | [6.2](#s6-2) |
| Python vs. SQL | [6.4](#s6-4) |
| Mapa general de relaciones | [7](#s7) |
| Glosario completo | [8](#s8) |
| Preguntas de repaso | [9](#s9) |
| Errores conceptuales frecuentes | [10](#s10) |

---

## <a id="s1"></a>1. ¿Qué es la analítica de datos?

### <a id="s1-1"></a>1.1 Definición

**Analítica de datos** es el proceso de examinar datos con métodos y herramientas para **extraer conclusiones que soporten decisiones**. No es "sacar gráficas": es convertir registros crudos en respuestas a preguntas de negocio.

La diferencia con "reportar" es la intención. Un reporte muestra números. La analítica **responde una pregunta que alguien necesitaba responder para actuar**.

### <a id="s1-2"></a>1.2 Los cuatro niveles de analítica

Esta es la clasificación más citada (viene de Gartner) y la que más aparece en evaluaciones. Cada nivel sube en valor y en dificultad.

| Nivel | Pregunta | Técnica | Ejemplo deportivo | Dificultad | Valor |
|---|---|---|---|---|---|
| **1. Descriptiva** | ¿Qué pasó? | Agregaciones, KPIs, dashboards | "La asistencia bajó 30% en junio" | Baja | Bajo |
| **2. Diagnóstica** | ¿Por qué pasó? | Segmentación, correlación, drill-down | "Bajó solo en la categoría Juvenil, en horario de 6pm" | Media | Medio |
| **3. Predictiva** | ¿Qué va a pasar? | Regresión, clasificación, series de tiempo | "Estas 5 atletas tienen 80% de probabilidad de desertar" | Alta | Alto |
| **4. Prescriptiva** | ¿Qué debo hacer? | Optimización, simulación, reglas | "Cambia el horario a 5pm y contacta a estas 5 atletas" | Muy alta | Muy alto |

```
              valor de negocio
                    ▲
   Prescriptiva     │                              ●  ¿qué hago?
   Predictiva       │                    ●            ¿qué pasará?
   Diagnóstica      │          ●                      ¿por qué pasó?
   Descriptiva      │  ●                              ¿qué pasó?
                    └──────────────────────────────►
                              complejidad
```

> **Error frecuente:** querer saltar directo a lo predictivo. Sin analítica descriptiva sólida (datos limpios, métricas bien definidas, un dashboard que nadie discute), un modelo predictivo se construye sobre arena. El 80% del valor real en una organización que arranca está en los niveles 1 y 2.

### <a id="s1-3"></a>1.3 Métrica vs. KPI

- **Métrica**: cualquier valor medible. *Número de sesiones registradas.*
- **KPI** (*Key Performance Indicator*): la métrica que está **atada a un objetivo** y a la que alguien responde. *Tasa de retención mensual de atletas ≥ 85%.*

Toda organización tiene cientos de métricas y debería tener menos de diez KPIs. Si todo es clave, nada lo es.

### <a id="s1-4"></a>1.4 Calidad de datos: las seis dimensiones

Analítica con datos malos produce decisiones malas con más confianza. Las dimensiones estándar de calidad:

| Dimensión | Pregunta | Ejemplo de falla |
|---|---|---|
| **Exactitud** | ¿El valor refleja la realidad? | Edad = 250 años |
| **Completitud** | ¿Faltan valores? | 40% de atletas sin fecha de nacimiento |
| **Consistencia** | ¿Coincide entre sistemas? | La app dice 38 atletas, el warehouse dice 41 |
| **Unicidad** | ¿Hay duplicados? | "Ana Pérez" y "ana perez" como dos registros |
| **Vigencia** | ¿Está actualizado? | Datos del warehouse de hace 3 semanas |
| **Validez** | ¿Cumple el formato/regla? | Email sin `@`, teléfono con letras |

> **Regla 1-10-100:** prevenir un error de dato cuesta 1, corregirlo después cuesta 10, y no corregirlo (decidir con él) cuesta 100.

[↑ Volver al índice](#indice)

---

## <a id="s2"></a>2. Diferencia entre Análisis, Ciencia e Ingeniería de datos

Esta es la pregunta que más se confunde. Las tres usan datos, pero **el producto que entregan es distinto**.

### <a id="s2-1"></a>2.1 Comparación directa

| | **Ingeniería de Datos** | **Análisis de Datos** | **Ciencia de Datos** |
|---|---|---|---|
| **Pregunta central** | ¿Cómo hago que los datos lleguen limpios y confiables? | ¿Qué pasó y por qué? | ¿Qué pasará y qué conviene hacer? |
| **Producto entregado** | Pipelines, bases de datos, warehouse, APIs | Dashboards, reportes, análisis ad-hoc | Modelos predictivos, experimentos, algoritmos |
| **Nivel de analítica** | Habilita todos | Descriptiva y diagnóstica | Predictiva y prescriptiva |
| **Herramientas** | SQL, Python, Airflow, Spark, dbt, PostgreSQL, cloud | SQL, Excel, Power BI, Tableau, pandas | Python, scikit-learn, estadística, ML, notebooks |
| **Habilidad diferenciadora** | Ingeniería de software y arquitectura | Comunicación y entendimiento del negocio | Estadística y matemáticas |
| **Horizonte temporal** | Pasado (mover lo que ya ocurrió) | Pasado y presente | Futuro |
| **Analogía de cocina** | Trae, limpia y organiza los ingredientes | Sirve el plato del menú y explica qué contiene | Inventa recetas nuevas y predice qué gustará |
| **Analogía de agua** | Construye la tubería | Abre la llave y mide el consumo | Predice la sequía del próximo año |

### <a id="s2-2"></a>2.2 Cómo se relacionan en el flujo

```
   INGENIERÍA DE DATOS          ANÁLISIS               CIENCIA
   ───────────────────      ─────────────         ──────────────
   construye la base    →   explota la base   →   modela sobre la base
        (cómo)                  (qué/por qué)         (qué pasará)

   fuentes → pipeline → warehouse → dashboards → modelos → decisiones
   └──── ingeniería ────────────┘└─ análisis ─┘└─ ciencia ─┘
```

**Dependencia jerárquica:** sin ingeniería no hay datos confiables que analizar; sin análisis descriptivo no se sabe qué vale la pena predecir; sin ciencia de datos se queda todo en mirar el espejo retrovisor.

### <a id="s2-3"></a>2.3 Roles relacionados que suelen aparecer

| Rol | Se enfoca en |
|---|---|
| **Data Engineer** | Ingesta, transformación, orquestación, infraestructura |
| **Data Analyst** | Consultas, visualización, reportes, métricas de negocio |
| **Data Scientist** | Modelado estadístico y machine learning |
| **Analytics Engineer** | Rol puente: modela y transforma dentro del warehouse (dbt, SQL) |
| **BI Developer** | Dashboards y capa semántica |
| **ML Engineer** | Poner modelos en producción y mantenerlos |
| **Data Steward / Governance** | Calidad, definiciones, políticas, cumplimiento |

> **Nota práctica:** en organizaciones pequeñas (una startup, un club deportivo, un proyecto académico) **una sola persona hace los tres papeles**. La distinción importa para saber qué sombrero llevas puesto en cada momento, no para dividir el trabajo entre tres contratos.

### <a id="s2-4"></a>2.4 Big Data: las 5 V

Contexto que suele acompañar esta clasificación:

| V | Significado |
|---|---|
| **Volumen** | Cantidad de datos (TB, PB) |
| **Velocidad** | Rapidez con que se generan y deben procesarse (batch vs streaming) |
| **Variedad** | Estructurados, semiestructurados (JSON, XML), no estructurados (texto, video) |
| **Veracidad** | Confiabilidad e incertidumbre del dato |
| **Valor** | Utilidad real para el negocio (la única V que justifica las otras cuatro) |

[↑ Volver al índice](#indice)

---

## <a id="s3"></a>3. OLTP y OLAP

### <a id="s3-1"></a>3.1 OLTP — *Online Transaction Processing*

Sistemas que **operan el negocio en tiempo real**: registran cada transacción en el momento en que ocurre.

- Muchas operaciones **pequeñas y concurrentes**: INSERT, UPDATE, DELETE.
- Optimizado para **escritura** y para leer pocas filas por consulta.
- Datos **actuales y detallados**; el histórico se archiva o se pierde.
- Modelo **normalizado** (3FN) → cero redundancia, integridad garantizada.
- Requiere **ACID**: Atomicidad, Consistencia, Aislamiento, Durabilidad.
- Ejemplos: la BD detrás de una app web, un POS, un cajero automático, un sistema de inscripciones.

### <a id="s3-2"></a>3.2 OLAP — *Online Analytical Processing*

Sistemas que **analizan el negocio**: responden preguntas agregadas sobre datos históricos.

- Pocas consultas, pero **enormes**: escanean millones de filas y agregan.
- Optimizado para **lectura**.
- Datos **históricos, integrados y agregados** (años de información).
- Modelo **dimensional** (esquema estrella) → redundancia aceptada a propósito.
- Almacenamiento **columnar**.
- Ejemplos: data warehouse que alimenta Power BI, Snowflake, BigQuery, Redshift.

### <a id="s3-3"></a>3.3 Tabla comparativa completa

| Criterio | **OLTP** | **OLAP** |
|---|---|---|
| Propósito | Operar el negocio | Analizar el negocio |
| Operación típica | INSERT / UPDATE / DELETE | SELECT con GROUP BY |
| Consulta ejemplo | "Dame el atleta id=42" | "Asistencia promedio por mes y categoría en 3 años" |
| Filas por consulta | Pocas (1–100) | Muchas (miles a millones) |
| Consultas por segundo | Miles | Pocas |
| Usuarios | Miles (clientes, empleados) | Decenas (analistas, gerencia) |
| Diseño de datos | Normalizado (3FN) | Desnormalizado (estrella / copo de nieve) |
| Redundancia | Mínima | Aceptada |
| Tiempo de respuesta | Milisegundos | Segundos a minutos |
| Alcance temporal | Actual (días/meses) | Histórico (años) |
| Almacenamiento | Orientado a **filas** | Orientado a **columnas** |
| Fuente de datos | Captura directa del usuario | Consolidación de varios OLTP |
| Métrica de éxito | Transacciones/segundo | Tiempo de respuesta de consulta |
| Criticidad del backup | Máxima (pérdida = dinero) | Reconstruible desde OLTP |
| Tamaño típico | GB | TB – PB |
| Ejemplos de motor | PostgreSQL, MySQL, Oracle, SQL Server | Snowflake, BigQuery, Redshift, ClickHouse |

### <a id="s3-4"></a>3.4 Por qué el almacenamiento columnar cambia todo

```
Orientado a FILAS (OLTP):
[1, Ana, 25, Bqla] [2, Luis, 31, Cali] [3, Sara, 19, Bogotá]
 ↑ traer un registro completo = 1 lectura contigua → ideal para "dame el atleta 2"

Orientado a COLUMNAS (OLAP):
[1,2,3] [Ana,Luis,Sara] [25,31,19] [Bqla,Cali,Bogotá]
                          ↑ AVG(edad) lee SOLO este bloque
```

Tres consecuencias:
1. **Menos I/O**: `SELECT AVG(edad)` no toca las otras columnas.
2. **Mejor compresión**: valores del mismo tipo juntos se comprimen mucho más (una columna "ciudad" con 5 valores distintos repetidos millones de veces comprime brutalmente).
3. **Peor para escritura puntual**: actualizar una fila obliga a tocar todos los bloques de columna. De ahí que no sirva para OLTP.

### <a id="s3-5"></a>3.5 Por qué no se analiza directo sobre el OLTP

Es tentador (¿para qué construir un warehouse si los datos ya están en PostgreSQL?), pero:

| Problema | Consecuencia |
|---|---|
| Una consulta analítica bloquea recursos | La app se pone lenta para los usuarios reales |
| El OLTP no guarda histórico | No se puede comparar contra el año pasado |
| Los datos están en varios sistemas | La app, el Excel de pagos y el CRM no se cruzan |
| El esquema normalizado exige 8 JOINs | Consultas lentas y difíciles de escribir |
| El OLTP guarda el estado *actual* | Se pierde cómo era el dato cuando ocurrió el evento |

Esa última es la más sutil e importante: si una atleta cambia de categoría, el OLTP la sobreescribe. El histórico de "cuántas sesiones hizo cuando era Infantil" se pierde para siempre. El warehouse lo preserva (ver [dimensiones lentamente cambiantes → 5.7](#s5-7)).

[↑ Volver al índice](#indice)

---

## <a id="s4"></a>4. Flujo de datos en una organización (ETL)

### <a id="s4-1"></a>4.1 El recorrido completo

```
  FUENTES              INGESTA        ZONA INTERMEDIA      DESTINO         CONSUMO
┌──────────┐
│ App OLTP │──┐
├──────────┤  │      ┌─────────┐    ┌─────────────┐   ┌────────────┐   ┌──────────┐
│  Excel   │──┼─────►│ Extract │───►│   Staging   │──►│ Warehouse  │──►│ Dashboard│
├──────────┤  │      └─────────┘    │ (Transform) │   │   (OLAP)   │   │ Modelos  │
│   APIs   │──┤                     └─────────────┘   └────────────┘   │ Reportes │
├──────────┤  │                                              │         └──────────┘
│ Archivos │──┘                                              ▼
└──────────┘                                          ┌────────────┐
                                                      │ Data Marts │
                                                      └────────────┘
```

### <a id="s4-2"></a>4.2 Las tres fases

**E — Extract (extraer)**
Sacar datos de las fuentes sin afectar su operación.

| Estrategia | Cómo funciona | Cuándo usarla |
|---|---|---|
| **Full extraction** | Trae toda la tabla cada vez | Tablas pequeñas, dimensiones |
| **Incremental** | Solo lo nuevo/modificado desde la última corrida (por `updated_at`) | Tablas grandes, hechos |
| **CDC** (*Change Data Capture*) | Lee el log de transacciones de la BD | Necesidad de casi tiempo real |

**T — Transform (transformar)**
Aquí ocurre el 70% del trabajo real:

- **Limpieza**: nulos, outliers, valores imposibles.
- **Estandarización**: `"BQLA"`, `"Barranquilla"`, `"b/quilla"` → `"Barranquilla"`. Formatos de fecha, unidades, mayúsculas.
- **Deduplicación**: un mismo atleta registrado dos veces.
- **Integración**: unir la app + el Excel de pagos con una llave común.
- **Enriquecimiento**: agregar datos externos (clima, geografía, demografía).
- **Reglas de negocio**: calcular "atleta activo" = con ≥1 sesión en los últimos 30 días.
- **Modelado dimensional**: armar hechos y dimensiones, generar llaves subrogadas.

**L — Load (cargar)**
Escribir en el destino.

| Modo | Descripción |
|---|---|
| **Full refresh** | Borra y recarga todo. Simple, costoso. |
| **Incremental / append** | Solo agrega lo nuevo. |
| **Upsert (merge)** | Actualiza si existe, inserta si no. |

### <a id="s4-3"></a>4.3 ETL vs ELT

| | **ETL** | **ELT** |
|---|---|---|
| Orden | Extraer → Transformar → Cargar | Extraer → Cargar → Transformar |
| Dónde transforma | Servidor intermedio dedicado | Dentro del warehouse |
| Datos crudos | No se conservan | Se conservan (se puede reprocesar) |
| Época y contexto | Clásico, on-premise, cómputo caro | Moderno, nube, cómputo elástico y barato |
| Herramientas | Informatica, Talend, SSIS, Python | dbt + Snowflake/BigQuery |
| Ventaja | Datos llegan ya limpios; menos carga en destino | Flexibilidad total; se transforma varias veces sin re-extraer |

> **Tendencia actual:** ELT gana en la nube. Como almacenar es barato y el warehouse tiene poder de cómputo enorme, conviene cargar crudo y transformar allí — así, si mañana cambia la regla de negocio, se recalcula sin volver a molestar al sistema fuente.

### <a id="s4-4"></a>4.4 Repositorios: Warehouse, Lake, Mart, Lakehouse

| Concepto | Qué es | Esquema | Datos | Usuario |
|---|---|---|---|---|
| **Data Warehouse** | Repositorio central estructurado para análisis | *Schema-on-write* | Limpios, modelados | Analista |
| **Data Lake** | Almacén de datos crudos en cualquier formato | *Schema-on-read* | Crudos, sin procesar | Científico de datos, ingeniero |
| **Data Mart** | Subconjunto del warehouse por área (ventas, RRHH) | Dimensional | Limpios, específicos | Área de negocio |
| **Data Lakehouse** | Híbrido: flexibilidad de lake + gobierno de warehouse | Mixto | Ambos | Todos |

> **Data swamp** (*pantano de datos*): lo que pasa cuando un data lake se llena sin catálogo, sin documentación ni gobierno. Nadie sabe qué hay, de dónde vino, ni si sirve. Es el fracaso más común de proyectos de datos.

### <a id="s4-5"></a>4.5 Conceptos operativos del pipeline

- **Orquestación**: coordinar el orden y las dependencias de las tareas (Airflow, Prefect, Dagster).
- **Idempotencia**: correr el pipeline dos veces produce el mismo resultado, no datos duplicados. **Requisito no negociable.**
- **Batch vs Streaming**: por lotes cada N horas vs continuo evento por evento (Kafka).
- **Linaje (*data lineage*)**: poder rastrear de dónde vino cada número del dashboard.
- **Observabilidad**: monitoreo, alertas cuando el pipeline falla o llegan datos raros.
- **SLA de frescura**: qué tan viejos pueden estar los datos del dashboard (¿24h? ¿5 min?).

[↑ Volver al índice](#indice)

---

## <a id="s5"></a>5. Modelado de datos para analítica

### <a id="s5-1"></a>5.1 Qué es el modelado de datos

**Modelado de datos** es definir *qué información se guarda, con qué estructura, con qué reglas y cómo se relacionan las piezas*, antes de implementar. Es el plano del edificio: se puede construir sin él, pero el error sale tres veces más caro después.

### <a id="s5-2"></a>5.2 Los tres niveles

| Nivel | Pregunta | Detalle | Audiencia | Ejemplo |
|---|---|---|---|---|
| **Conceptual** | ¿Qué entidades existen? | Bajo | Negocio | "Un Entrenador tiene muchos Atletas" |
| **Lógico** | ¿Qué atributos, llaves y relaciones? | Medio | Analista/arquitecto | `atleta(id, nombre, fecha_nac, entrenador_id)` |
| **Físico** | ¿Cómo se implementa en el motor? | Alto | Dev / DBA | `CREATE TABLE atleta (id SERIAL PRIMARY KEY...)` |

### <a id="s5-3"></a>5.3 Piezas del modelo

- **Entidad** → tabla. **Atributo** → columna. **Registro** → fila.
- **PK (llave primaria)**: identifica de forma única cada fila. Única, no nula.
- **FK (llave foránea)**: apunta a la PK de otra tabla; es lo que **materializa la relación**.
- **Cardinalidad**: 1:1, 1:N, N:M.

| Cardinalidad | Ejemplo | Implementación |
|---|---|---|
| 1:1 | Usuario ↔ Perfil | FK con `UNIQUE` |
| 1:N | Entrenador → Atletas | FK en el lado "muchos" |
| N:M | Atletas ↔ Rutinas | **Tabla intermedia obligatoria** |

### <a id="s5-4"></a>5.4 Modelado transaccional vs modelado analítico

Esta es la bisagra de toda la clase:

| | **Transaccional (OLTP)** | **Analítico (OLAP)** |
|---|---|---|
| Técnica | Normalización (3FN) | Modelado dimensional |
| Objetivo | Integridad, escritura eficiente | Comprensión y lectura rápida |
| Tablas | Muchas y pequeñas | Pocas y anchas |
| Redundancia | Eliminada | Aceptada a propósito |
| JOINs por consulta | Muchos | Pocos (1 nivel) |
| Diseñado para | La aplicación | El humano que pregunta |

**Repaso de normalización** (base del lado OLTP):

| Forma | Regla | Violación típica |
|---|---|---|
| **1FN** | Valores atómicos, sin grupos repetidos | `telefonos = "300111, 300222"` |
| **2FN** | 1FN + dependencia de la PK **completa** | `nombre_producto` depende solo de `producto_id`, no del par |
| **3FN** | 2FN + sin dependencias transitivas | `ciudad → departamento` dentro de `atleta` |

Mnemotecnia: *"la clave, toda la clave y nada más que la clave"*.

### <a id="s5-5"></a>5.5 Modelado dimensional: hechos y dimensiones

Propuesto por **Ralph Kimball**. La idea: organizar los datos como los entiende un humano que hace preguntas de negocio, no como los necesita una aplicación.

Toda pregunta analítica tiene la misma forma:

> **"¿Cuánto** de *[algo medible]* **por** *[algún contexto]* **en** *[algún periodo]*?"

- Lo medible = **hecho** (*fact*)
- El contexto = **dimensión** (*dimension*)

**Tabla de hechos:**
- Contiene **métricas numéricas** (las cosas que se suman, promedian, cuentan).
- Contiene **FKs** hacia las dimensiones.
- Muchísimas filas, pocas columnas.
- Crece constantemente.

**Tabla de dimensión:**
- Contiene **atributos descriptivos** (texto: nombre, categoría, ciudad, tipo).
- Responde quién, qué, cuándo, dónde, cómo.
- Pocas filas, muchas columnas.
- Crece poco.

**Truco para distinguirlas:** las dimensiones son las palabras que van después de "**por**" en la pregunta; los hechos son lo que va después de "**cuánto**".
*"Cuántas **sesiones** por **atleta** por **mes** por **categoría**"* → hecho: sesiones; dimensiones: atleta, tiempo, categoría.

### <a id="s5-6"></a>5.6 Esquema estrella y copo de nieve

**Esquema estrella (star schema):** una tabla de hechos al centro, dimensiones desnormalizadas alrededor, a un solo JOIN de distancia.

```
        dim_tiempo              dim_atleta
        ─────────               ──────────
        tiempo_id  ◄──┐      ┌──►  atleta_id
        fecha         │      │     nombre
        mes           │      │     categoria
        trimestre     │      │     sexo
        anio          │      │     ciudad
                      │      │
                 ┌────┴──────┴────────────────┐
                 │   hecho_sesion             │
                 │  ─────────────             │
                 │   sesion_id (PK)           │
                 │   tiempo_id     (FK)       │
                 │   atleta_id     (FK)       │
                 │   ejercicio_id  (FK)       │
                 │   entrenador_id (FK)       │
                 │  ── métricas ──            │
                 │   series                   │
                 │   repeticiones             │
                 │   carga_kg                 │
                 │   duracion_min             │
                 └────┬──────────────┬────────┘
                      │              │
        dim_ejercicio ◄┘              └► dim_entrenador
        ─────────────                    ──────────────
        ejercicio_id                     entrenador_id
        nombre                           nombre
        grupo_muscular                   especialidad
        tipo                             sede
```

**Esquema copo de nieve (snowflake):** igual, pero las dimensiones se normalizan en sub-tablas.

```
dim_atleta ──► dim_ciudad ──► dim_departamento ──► dim_pais
```

| | **Estrella** | **Copo de nieve** |
|---|---|---|
| Dimensiones | Desnormalizadas | Normalizadas |
| JOINs | 1 nivel | Varios niveles |
| Velocidad de consulta | Mayor | Menor |
| Espacio | Más | Menos |
| Facilidad para el analista | Alta | Media |
| Recomendación | **Por defecto** | Solo si la dimensión es enorme o cambia mucho |

**Esquema constelación (*galaxy / fact constellation*):** varias tablas de hechos que **comparten dimensiones**. Esas dimensiones compartidas se llaman **dimensiones conformadas** (*conformed dimensions*) y son lo que permite comparar entre áreas: si `dim_tiempo` y `dim_atleta` son las mismas para `hecho_sesion` y `hecho_pago`, se puede cruzar asistencia con pagos.

### <a id="s5-7"></a>5.7 Conceptos avanzados del modelado dimensional

**Granularidad**
Qué representa **una fila** de la tabla de hechos. Es la decisión más importante del diseño.

```
Granularidad fina  → una fila por serie de ejercicio  (mucho detalle, muchas filas)
Granularidad media → una fila por sesión de atleta
Granularidad gruesa→ una fila por atleta por mes      (poco detalle, pocas filas)
```

> **Regla de oro:** elegir siempre la granularidad **más fina que el negocio pueda necesitar**. Desde el detalle se puede agregar hacia arriba; desde el agregado **nunca** se puede bajar al detalle.

**Llaves subrogadas (*surrogate keys*)**
En el warehouse, las dimensiones no usan la PK del sistema fuente (*llave natural*), sino un entero secuencial propio. Razones: independencia de la fuente, poder manejar historia (§ siguiente), rendimiento, y sobrevivir si dos sistemas fuente usan el mismo ID para cosas distintas.

**Dimensiones lentamente cambiantes (SCD — *Slowly Changing Dimensions*)**
¿Qué hacer cuando un atributo de dimensión cambia (una atleta pasa de Infantil a Juvenil)?

| Tipo | Estrategia | Efecto |
|---|---|---|
| **SCD 1** | Sobreescribir el valor | Se pierde la historia; el pasado se "reescribe" |
| **SCD 2** | Insertar fila nueva con `fecha_inicio`, `fecha_fin`, `es_vigente` | **Preserva la historia** — el estándar |
| **SCD 3** | Columna adicional `categoria_anterior` | Guarda solo un cambio hacia atrás |

Ejemplo SCD 2:

| atleta_sk | atleta_id | nombre | categoria | fecha_inicio | fecha_fin | vigente |
|---|---|---|---|---|---|---|
| 101 | A-42 | Sara | Infantil | 2024-01-01 | 2025-06-30 | No |
| 288 | A-42 | Sara | Juvenil | 2025-07-01 | 9999-12-31 | Sí |

Así, las sesiones de 2024 siguen contando como Infantil. **Esto es exactamente lo que el OLTP no puede hacer.**

**Tipos de tabla de hechos**

| Tipo | Descripción | Ejemplo |
|---|---|---|
| **Transaccional** | Una fila por evento ocurrido | Cada sesión de entrenamiento |
| **Snapshot periódico** | Foto del estado en intervalos fijos | Peso y medidas de cada atleta cada mes |
| **Snapshot acumulado** | Una fila por proceso, se actualiza en cada hito | Proceso de inscripción: contacto → prueba → matrícula |
| **Sin hechos** (*factless*) | Registra que algo ocurrió, sin métrica | Asistencia (ocurrió o no); atleta inscrito en un torneo |

**Aditividad de las métricas**

| Tipo | Se puede sumar | Ejemplo |
|---|---|---|
| **Aditiva** | En todas las dimensiones | Monto vendido, repeticiones totales |
| **Semi-aditiva** | En todas menos el tiempo | Saldo de cuenta, inventario, peso corporal |
| **No aditiva** | En ninguna | Porcentajes, ratios, promedios (se recalculan, no se suman) |

> Sumar promedios da un promedio equivocado. Se guardan el numerador y el denominador como métricas aditivas, y el ratio se calcula al final.

**dim_tiempo (dimensión de calendario)**
Casi todo warehouse tiene una tabla de fechas pre-generada con: fecha, día, mes, nombre del mes, trimestre, año, día de la semana, si es fin de semana, si es festivo, número de semana, periodo escolar/temporada. Permite preguntar "¿cómo rinden los sábados?" o "¿cómo cambia la asistencia en temporada de torneos?" sin calcular nada en la consulta.

### <a id="s5-8"></a>5.8 Kimball vs Inmon

Las dos filosofías clásicas del data warehousing:

| | **Kimball** (bottom-up) | **Inmon** (top-down) |
|---|---|---|
| Punto de partida | Data marts por proceso de negocio | Warehouse corporativo en 3FN |
| Modelo | Dimensional desde el inicio | Normalizado, y de ahí salen marts dimensionales |
| Tiempo a primer resultado | Rápido | Lento |
| Consistencia global | Vía dimensiones conformadas | Garantizada por diseño |
| Recomendación práctica | Proyectos que necesitan valor pronto | Organizaciones grandes con muchas fuentes |

Para un proyecto académico o una organización pequeña: **Kimball**, sin dudarlo.

[↑ Volver al índice](#indice)

---

## <a id="s6"></a>6. Python

### <a id="s6-1"></a>6.1 Qué es

**Python** es un lenguaje de programación de **alto nivel**, **interpretado**, de **tipado dinámico** y **propósito general**, cuya filosofía de diseño prioriza la **legibilidad del código**.

**Características:**
- **Interpretado**: se ejecuta línea a línea, sin compilar a binario previamente.
- **Tipado dinámico**: no se declara el tipo; se infiere en ejecución.
- **Multiparadigma**: imperativo, orientado a objetos y funcional.
- **Indentación significativa**: los espacios definen los bloques (no las llaves).
- **Multiplataforma**: el mismo código corre en Windows, Linux y macOS.
- **Baterías incluidas**: librería estándar amplia + ecosistema PyPI enorme.
- **Open source** y gratuito, gestionado por la Python Software Foundation.

### <a id="s6-2"></a>6.2 Historia

**Origen (1989–1991)**
Guido van Rossum, investigador holandés del CWI (Centro de Matemáticas e Informática, Ámsterdam), empezó el proyecto en **diciembre de 1989** como pasatiempo de vacaciones de Navidad. Quería un lenguaje que superara las limitaciones de ABC (un lenguaje educativo en el que había trabajado) y que sirviera para tareas de administración de sistemas, con la potencia de C pero la facilidad de un lenguaje de scripting.

La primera versión pública, **0.9.0, salió en febrero de 1991**.

**El nombre**
No viene de la serpiente. Van Rossum era fan de **Monty Python's Flying Circus**, el grupo británico de comedia. De ahí también la costumbre de usar `spam` y `eggs` como nombres de variables de ejemplo en la documentación (en lugar de `foo` y `bar`).

**Línea de tiempo del lenguaje**

| Año | Hito |
|---|---|
| 1989 | Van Rossum inicia el proyecto en el CWI |
| 1991 | Primera versión pública (0.9.0) |
| 1994 | Python 1.0: `lambda`, `map`, `filter`, `reduce` |
| 2000 | **Python 2.0**: list comprehensions, recolección de basura, soporte Unicode |
| 2001 | Se crea la Python Software Foundation |
| 2008 | **Python 3.0**: ruptura intencional de compatibilidad (`print()` como función, strings Unicode por defecto, división real) |
| 2018 | Van Rossum renuncia como BDFL (*Benevolent Dictator For Life*); se crea el Steering Council |
| 2020 | **Fin de soporte de Python 2** (1 de enero) — cierra una transición de 12 años |
| 2022 | Python 3.11: mejoras de rendimiento de hasta ~25% |
| 2024+ | Trabajo en el modo *free-threaded* (Python sin GIL) |

**Historia del ecosistema de datos** — esto es lo que convirtió a Python en el lenguaje de datos:

| Año | Librería | Aporte |
|---|---|---|
| 1995 | Numeric | Primer intento de cómputo numérico |
| 2001 | IPython | Consola interactiva (Fernando Pérez) |
| 2003 | Matplotlib | Gráficos (John Hunter) |
| **2006** | **NumPy** | Unifica el cómputo numérico (Travis Oliphant). **Base de todo lo demás** |
| 2007 | scikit-learn | Machine learning accesible |
| **2008** | **pandas** | Estructuras tabulares (Wes McKinney, en un fondo de inversión) |
| 2012 | Anaconda | Distribución con todo preinstalado |
| 2014 | Jupyter | Notebooks (evolución de IPython) |
| 2015–16 | TensorFlow / PyTorch | Deep learning |
| 2020+ | Polars, Dask, DuckDB | Alternativas de alto rendimiento |

**Filosofía**
El *Zen de Python* (PEP 20, escrito por Tim Peters) resume la filosofía en aforismos: preferir lo explícito sobre lo implícito, lo simple sobre lo complejo, y la legibilidad como valor central. Se puede leer ejecutando `import this`. La guía de estilo oficial es **PEP 8**.

### <a id="s6-3"></a>6.3 Por qué Python domina el mundo de datos

1. **Curva de entrada baja** → un analista no necesita ser ingeniero de software.
2. **Ecosistema completo** → desde leer un CSV hasta entrenar una red neuronal, sin cambiar de lenguaje.
3. **Lenguaje pegamento** → conecta bases de datos, APIs, archivos, servicios en la nube y modelos.
4. **Cubre las tres disciplinas** → el ingeniero hace pipelines, el analista explora, el científico modela. Mismo lenguaje, equipos que se entienden.
5. **Comunidad gigantesca** → cualquier error ya lo tuvo alguien y lo respondió.
6. **Reproducibilidad** → un script se versiona en Git y produce el mismo resultado; un Excel manual no.

> **Honestidad técnica:** Python es lento comparado con C o Java. Funciona en datos porque las librerías críticas (NumPy, pandas) están escritas en C/Cython y Python solo las orquesta. Python es el director de orquesta, no los instrumentos.

### <a id="s6-4"></a>6.4 Python y SQL: no compiten

| Tarea | Mejor herramienta |
|---|---|
| Consultar y agregar dentro de una BD | **SQL** |
| Modelar y transformar dentro del warehouse | **SQL** (con dbt) |
| Orquestar pipelines, llamar APIs | **Python** |
| Limpieza compleja, lógica condicional | **Python** |
| Machine learning, estadística | **Python** |
| Visualización interactiva | Ambos + herramienta BI |

**Consejo de la clase:** aprende SQL primero y a fondo. Es el idioma común de los datos y sobrevive a todas las modas de librerías.

### <a id="s6-5"></a>6.5 Sintaxis mínima

```python
# Variables — sin declarar tipo
nombre  = "Daniel"       # str
edad    = 25             # int
altura  = 1.78           # float
activo  = True           # bool
vacio   = None           # NoneType

# Estructuras
lista = [1, 2, 3]                        # ordenada, mutable
tupla = (1, 2, 3)                        # ordenada, inmutable
conj  = {1, 2, 3}                        # sin duplicados
dic   = {"nombre": "Ana", "edad": 25}    # clave → valor

# Control de flujo
if edad >= 18:
    print("Mayor de edad")
else:
    print("Menor")

for i in range(3):
    print(i)                 # 0 1 2

# Función
def promedio(valores):
    return sum(valores) / len(valores)

# List comprehension — muy usada en datos
pares = [x for x in range(10) if x % 2 == 0]   # [0,2,4,6,8]
```

### <a id="s6-6"></a>6.6 Ecosistema por etapa del flujo de datos

| Etapa | Librerías |
|---|---|
| Conexión a BD | `psycopg2`, `SQLAlchemy` |
| Lectura de archivos/APIs | `pandas`, `requests`, `json` |
| Manipulación | `pandas`, `NumPy`, `Polars` |
| Orquestación | `Airflow`, `Prefect`, `Dagster` |
| Visualización | `Matplotlib`, `Seaborn`, `Plotly` |
| Machine learning | `scikit-learn`, `XGBoost` |
| Entorno de trabajo | `Jupyter`, `Anaconda`, `venv` |

[↑ Volver al índice](#indice)

---

## <a id="s7"></a>7. Mapa de relaciones entre todos los conceptos

```
                    ANALÍTICA DE DATOS
              (descriptiva→diagnóstica→predictiva→prescriptiva)
                              │
                    ¿qué necesito para hacerla?
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                      ▼
   MODELADO              INFRAESTRUCTURA          HERRAMIENTAS
        │                     │                      │
  ┌─────┴─────┐         ┌─────┴─────┐          ┌─────┴─────┐
  ▼           ▼         ▼           ▼          ▼           ▼
normalizado dimensional OLTP  ──ETL──► OLAP   SQL       PYTHON
  (3FN)     (estrella)  │              │                  │
    │           │       │              │            ┌─────┴─────┐
    └───►usado en◄──────┘              │            ▼           ▼
        │      └───────►usado en◄──────┘         NumPy ──►  pandas
        │                                                       │
        ▼                                                       ▼
   la aplicación                                        limpieza/análisis
        │                                                       │
        └────────────► genera datos ────────────►───────────────┘
                                                                │
                                                                ▼
                                                    DECISIÓN DE NEGOCIO
```

**Cómo se encadena en un caso real (club de voleibol):**

1. Una app registra asistencias → **PostgreSQL (OLTP)**, modelo **normalizado en 3FN**.
2. Cada noche un pipeline en **Python** hace **extracción incremental** por `updated_at`.
3. **Transforma**: estandariza nombres de categorías, deduplica atletas, calcula "activa", genera llaves subrogadas y aplica **SCD 2** a los cambios de categoría.
4. **Carga** en el **warehouse** con **esquema estrella**: `hecho_sesion` + `dim_atleta` + `dim_tiempo` + `dim_ejercicio`.
5. Una consulta **OLAP** agrega por mes y categoría en segundos, aunque haya millones de filas.
6. El dashboard muestra **analítica descriptiva** (asistencia por mes) y **diagnóstica** (cae en Juvenil, horario 6pm).
7. Con historial suficiente, un modelo predice **deserción** (analítica predictiva).
8. El entrenador cambia el horario y contacta a las atletas en riesgo (**prescriptiva**). El ciclo se cierra: la decisión genera nuevos datos.

[↑ Volver al índice](#indice)

---

## <a id="s8"></a>8. Glosario

| Término | Definición | Ver |
|---|---|---|
| **ACID** | Atomicidad, Consistencia, Aislamiento, Durabilidad. Garantías de una transacción OLTP. | [3.1](#s3-1) |
| **Aditividad** | Si una métrica puede sumarse a través de las dimensiones. | [5.7](#s5-7) |
| **Agregación** | Resumir muchas filas en un valor (SUM, AVG, COUNT, MAX). | [3.2](#s3-2) |
| **Analítica de datos** | Examinar datos para extraer conclusiones que soporten decisiones. | [1.1](#s1-1) |
| **Analítica descriptiva / diagnóstica / predictiva / prescriptiva** | Qué pasó / por qué pasó / qué pasará / qué hacer. | [1.2](#s1-2) |
| **Anaconda** | Distribución de Python con librerías de datos preinstaladas. | [6.6](#s6-6) |
| **Batch / Streaming** | Procesamiento por lotes periódicos vs. continuo evento por evento. | [4.5](#s4-5) |
| **BDFL** | *Benevolent Dictator For Life*; título que tuvo Van Rossum hasta 2018. | [6.2](#s6-2) |
| **Big Data (5 V)** | Volumen, Velocidad, Variedad, Veracidad, Valor. | [2.4](#s2-4) |
| **Cardinalidad** | Número de instancias relacionadas entre dos entidades (1:1, 1:N, N:M). | [5.3](#s5-3) |
| **CDC** | *Change Data Capture*: capturar cambios leyendo el log de la BD. | [4.2](#s4-2) |
| **Columnar** | Almacenamiento por columnas; óptimo para consultas analíticas. | [3.4](#s3-4) |
| **Data Lake** | Repositorio de datos crudos en cualquier formato (*schema-on-read*). | [4.4](#s4-4) |
| **Data Lakehouse** | Híbrido entre lake y warehouse. | [4.4](#s4-4) |
| **Data Mart** | Subconjunto del warehouse orientado a un área de negocio. | [4.4](#s4-4) |
| **Data Swamp** | Data lake sin gobierno ni catálogo; inutilizable. | [4.4](#s4-4) |
| **Data Warehouse** | Repositorio central de datos históricos, limpios y modelados (*schema-on-write*). | [4.4](#s4-4) |
| **Desnormalización** | Introducir redundancia a propósito para acelerar lecturas. | [5.4](#s5-4) |
| **DIKW** | Dato → Información → Conocimiento → Sabiduría. | [1.1](#s1-1) |
| **Dimensión** | Tabla con atributos descriptivos: quién, qué, cuándo, dónde. | [5.5](#s5-5) |
| **Dimensión conformada** | Dimensión compartida por varias tablas de hechos; permite comparar áreas. | [5.6](#s5-6) |
| **Entidad** | Objeto del mundo real modelado como tabla. | [5.3](#s5-3) |
| **ETL / ELT** | Extract-Transform-Load / Extract-Load-Transform. | [4.3](#s4-3) |
| **Esquema estrella** | Hechos al centro, dimensiones desnormalizadas alrededor. | [5.6](#s5-6) |
| **Esquema copo de nieve** | Estrella con dimensiones normalizadas en sub-tablas. | [5.6](#s5-6) |
| **Esquema constelación** | Varias tablas de hechos que comparten dimensiones. | [5.6](#s5-6) |
| **Factless fact table** | Tabla de hechos sin métricas; registra que un evento ocurrió. | [5.7](#s5-7) |
| **FK** | *Foreign Key*: columna que referencia la PK de otra tabla. | [5.3](#s5-3) |
| **Granularidad** | Qué representa una fila de la tabla de hechos. | [5.7](#s5-7) |
| **Hecho** | Evento medible del negocio; fila de la tabla de hechos. | [5.5](#s5-5) |
| **Idempotencia** | Ejecutar un proceso dos veces produce el mismo resultado. | [4.5](#s4-5) |
| **Inmon** | Enfoque *top-down*: warehouse corporativo en 3FN primero. | [5.8](#s5-8) |
| **Kimball** | Enfoque *bottom-up*: data marts dimensionales por proceso de negocio. | [5.8](#s5-8) |
| **KPI** | Métrica clave atada a un objetivo y a un responsable. | [1.3](#s1-3) |
| **Linaje de datos** | Trazabilidad del origen y las transformaciones de cada dato. | [4.5](#s4-5) |
| **Llave natural** | Identificador que viene del sistema fuente. | [5.7](#s5-7) |
| **Llave subrogada** | Entero secuencial generado en el warehouse como PK de dimensión. | [5.7](#s5-7) |
| **Modelado dimensional** | Técnica de modelado analítico basada en hechos y dimensiones. | [5.5](#s5-5) |
| **Modelado de datos** | Definir estructura, reglas y relaciones de la información. | [5.1](#s5-1) |
| **Normalización** | Organizar tablas para eliminar redundancia (1FN, 2FN, 3FN). | [5.4](#s5-4) |
| **NumPy** | Librería base de cómputo numérico en Python. | [6.6](#s6-6) |
| **OLAP** | *Online Analytical Processing*: lectura y agregación de históricos. | [3.2](#s3-2) |
| **OLTP** | *Online Transaction Processing*: operaciones del día a día. | [3.1](#s3-1) |
| **Orquestación** | Coordinar el orden y las dependencias de las tareas del pipeline. | [4.5](#s4-5) |
| **pandas** | Librería de Python para datos tabulares. | [6.6](#s6-6) |
| **PEP 8 / PEP 20** | Guía de estilo de Python / Zen de Python. | [6.2](#s6-2) |
| **Pipeline** | Secuencia automatizada que mueve y transforma datos. | [4.1](#s4-1) |
| **PK** | *Primary Key*: identificador único de cada fila. | [5.3](#s5-3) |
| **SCD** | *Slowly Changing Dimension*: estrategias para manejar cambios históricos (tipos 1, 2, 3). | [5.7](#s5-7) |
| **Schema-on-read / write** | Definir el esquema al leer (lake) o al escribir (warehouse). | [4.4](#s4-4) |
| **Snapshot** | Foto del estado en un momento dado. | [5.7](#s5-7) |
| **Staging** | Zona intermedia donde se preparan los datos antes de cargarlos. | [4.1](#s4-1) |
| **Vectorización** | Operar sobre arreglos completos en lugar de iterar fila por fila. | [6.3](#s6-3) |

[↑ Volver al índice](#indice)

---

## <a id="s9"></a>9. Preguntas de repaso

### <a id="nivel-basico"></a>Nivel básico

**1. ¿Qué es la analítica de datos y en qué se diferencia de hacer reportes?**
Es examinar datos para extraer conclusiones que soporten decisiones. Un reporte muestra números; la analítica responde una pregunta que alguien necesitaba responder para actuar.

**2. Nombra los cuatro niveles de analítica.**
Descriptiva (¿qué pasó?), diagnóstica (¿por qué?), predictiva (¿qué pasará?), prescriptiva (¿qué hago?).

**3. Diferencia central entre OLTP y OLAP.**
OLTP opera el negocio: muchas escrituras pequeñas, datos actuales, modelo normalizado. OLAP analiza el negocio: pocas lecturas enormes, datos históricos, modelo dimensional.

**4. ¿Qué significa ETL?**
Extract, Transform, Load: extraer de las fuentes, transformar (limpiar, estandarizar, integrar) y cargar en el destino analítico.

**5. Diferencia entre tabla de hechos y tabla de dimensión.**
Hechos: métricas numéricas y FKs; muchas filas, crece siempre. Dimensiones: atributos descriptivos de contexto; pocas filas, muchas columnas.

**6. ¿De dónde viene el nombre "Python"?**
Del grupo de comedia británico Monty Python, del que Guido van Rossum era fan. No de la serpiente.

### <a id="nivel-intermedio"></a>Nivel intermedio

**7. Si en bases de datos nos enseñaron a normalizar, ¿por qué el warehouse se desnormaliza?**
Porque los objetivos son opuestos. OLTP prioriza integridad y escritura → normalizar. OLAP prioriza lectura rápida sobre millones de filas, y cada JOIN cuesta → se acepta redundancia para evitarlos. No es contradicción: es optimizar para el patrón de acceso dominante.

**8. ¿Por qué no analizar directamente sobre la base de producción?**
Porque las consultas analíticas degradan el rendimiento de la app, el OLTP no guarda histórico ni datos de otros sistemas, el esquema normalizado exige demasiados JOINs, y el OLTP solo conserva el estado actual (pierde cómo era el dato cuando ocurrió el evento).

**9. Explica ETL vs ELT y por qué ELT ganó terreno.**
ETL transforma antes de cargar, en un servidor intermedio; ELT carga crudo y transforma dentro del warehouse. ELT ganó porque en la nube almacenar es barato y el warehouse tiene cómputo elástico: se conservan los datos crudos y se puede reprocesar sin volver a extraer de la fuente.

**10. ¿Qué es la granularidad y por qué es la decisión más crítica?**
Es lo que representa una fila de la tabla de hechos. Define el detalle máximo analizable: desde el detalle siempre se puede agregar, pero desde el agregado nunca se puede bajar al detalle.

**11. ¿Qué es una SCD tipo 2 y qué problema resuelve?**
Una dimensión que, ante un cambio de atributo, inserta una fila nueva con vigencia (`fecha_inicio`, `fecha_fin`, `es_vigente`) en lugar de sobreescribir. Resuelve la preservación del histórico: los hechos antiguos siguen asociados al valor que tenía el atributo en ese momento.

**12. ¿Por qué no se pueden sumar promedios ni porcentajes en un modelo dimensional?**
Porque son métricas no aditivas: el promedio de promedios no equivale al promedio real. Se almacenan numerador y denominador como métricas aditivas y el ratio se calcula al final de la agregación.

**13. Diferencia entre estrella y copo de nieve. ¿Cuál usarías?**
La estrella tiene dimensiones desnormalizadas a un JOIN de distancia; el copo de nieve las normaliza en jerarquías. Estrella por defecto: más rápida y más fácil para el analista. Copo de nieve solo si la dimensión es enorme o su jerarquía cambia mucho.

### <a id="nivel-de-aplicacion-y-conceptual"></a>Nivel de aplicación y conceptual

**14. Explica la diferencia entre ingeniería, análisis y ciencia de datos con una analogía.**
Ingeniería construye la tubería y garantiza que el agua llegue limpia; análisis abre la llave, mide el consumo y explica por qué subió; ciencia predice la sequía del próximo año y recomienda cómo racionar. Los productos son pipeline, dashboard y modelo respectivamente.

**15. Diseña un modelo estrella para un club deportivo.**
- `hecho_sesion(sesion_sk, tiempo_id, atleta_sk, entrenador_sk, ejercicio_sk, series, repeticiones, carga_kg, duracion_min)` — granularidad: una fila por atleta-ejercicio-sesión.
- `dim_atleta(atleta_sk, atleta_id, nombre, categoria, sexo, fecha_nac, ciudad, fecha_inicio, fecha_fin, vigente)` — SCD 2 por categoría.
- `dim_entrenador(entrenador_sk, nombre, especialidad, sede)`
- `dim_ejercicio(ejercicio_sk, nombre, grupo_muscular, tipo)`
- `dim_tiempo(tiempo_id, fecha, dia, mes, trimestre, anio, dia_semana, es_fin_semana, temporada)`

**16. Una organización quiere "hacer machine learning" pero sus datos están en tres Excel distintos y nadie coincide en cuántos clientes tiene. ¿Qué recomiendas y por qué?**
Empezar por ingeniería y analítica descriptiva: integrar las fuentes, definir métricas únicas y acordadas, y montar un dashboard confiable. Sin datos consistentes ni definiciones comunes, un modelo predictivo produce salidas precisas sobre premisas falsas. El 80% del valor inicial está en los niveles 1 y 2 de [analítica](#s1-2).

**17. ¿Qué significa que un pipeline sea idempotente y por qué es obligatorio?**
Que ejecutarlo dos veces produce el mismo resultado, sin duplicar datos. Es obligatorio porque los pipelines fallan a mitad de camino y se reintentan; sin idempotencia, cada reintento corrompe el warehouse.

[↑ Volver al índice](#indice)

---

## <a id="s10"></a>10. Errores conceptuales frecuentes

| Error | Corrección |
|---|---|
| "OLAP es una herramienta" | Es un **tipo de procesamiento/carga de trabajo**, no un producto. |
| "El data lake reemplaza al warehouse" | Son complementarios: crudo y flexible vs. modelado y gobernado. |
| "Normalizar siempre es mejor" | Depende del patrón de acceso: 3FN para OLTP, dimensional para OLAP. |
| "Más datos = mejores decisiones" | Solo si son de calidad y responden a una pregunta concreta. |
| "Ciencia de datos es lo mismo que análisis" | Distinto producto: modelo predictivo vs. explicación de lo ocurrido. |
| "Primero el machine learning" | Primero datos confiables y analítica descriptiva. |
| "Python reemplaza a SQL" | Se complementan; SQL sigue siendo el idioma base de los datos. |
| "Puedo bajar de granularidad después" | No. Solo se agrega hacia arriba, nunca hacia abajo. |
| "La dimensión guarda el valor actual" | Con SCD 2 guarda la historia; ese es el punto del warehouse. |

[↑ Volver al índice](#indice)

---

## <a id="s11"></a>11. Ruta de estudio sugerida

| Semana | Foco |
|---|---|
| 1 | Analítica: los 4 niveles, métricas vs KPIs, calidad de datos. Definir 3 KPIs de un caso propio. |
| 2 | Repasar normalización (1FN→3FN) sobre un esquema real propio. |
| 3 | OLTP vs OLAP: entender el costo de cada JOIN y el almacenamiento columnar. |
| 4 | Modelado dimensional: convertir un esquema normalizado propio en esquema estrella. |
| 5 | ETL: diseñar el flujo completo desde la BD de la app hasta el warehouse. Definir granularidad y SCD. |
| 6 | Python + SQL: implementar la extracción y la transformación; consultar el modelo estrella. |
| 7 | Visualización: dashboard con los KPIs definidos en la semana 1. Cerrar el ciclo. |

[↑ Volver al índice](#indice)

---

## <a id="s12"></a>12. Recursos recomendados

- **Ralph Kimball — *The Data Warehouse Toolkit* (3ª ed.)**: la referencia definitiva del modelado dimensional. Los capítulos 1–3 cubren el 80% de lo que se usa.
- **Bill Inmon — *Building the Data Warehouse***: el enfoque top-down, para contrastar.
- **Joe Reis & Matt Housley — *Fundamentals of Data Engineering***: el mejor panorama moderno del ciclo de vida del dato.
- **Documentación de dbt**: sus guías de modelado analítico son excelentes y gratuitas.
- **Wes McKinney — *Python for Data Analysis* (3ª ed.)**: escrito por el creador de pandas.
- **Documentación oficial de Python** (`docs.python.org`) y **PEP 8** para estilo.
- **Kaggle Learn**: micro-cursos gratuitos con ejercicios prácticos.

[↑ Volver al índice](#indice)

---

*Apuntes de clase — Iniciación a ciencia, ingeniería y analítica de datos*
