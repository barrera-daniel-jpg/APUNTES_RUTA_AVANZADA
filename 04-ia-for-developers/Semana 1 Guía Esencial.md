# Semana 1
# Guía Esencial de Modelos de Lenguaje (LLMs) e Inteligencia Artificial Generativa

Este repositorio contiene apuntes claros, explicaciones técnicas sin rodeos y recursos clave para entender cómo funcionan los LLMs hoy en día, cómo compararlos y cómo utilizarlos de manera profesional.

---

## Estructura del Repositorio

* `apuntes/`: Conceptos teóricos, arquitecturas y guías paso a paso.
* `recursos/`: Enlaces a documentación oficial, papers clave y herramientas del ecosistema.

---

## Contenido de los Apuntes

### 1. ¿Cómo funcionan los LLMs?

Un **LLM (Large Language Model)** es, en esencia, un sistema estadístico hipercomplejo diseñado para predecir la siguiente palabra (o *token*) más probable en función de un contexto dado.

#### Pilares Fundamentales:
* **Arquitectura Transformer:** Introducida en 2017 (*"Attention Is All You Need"*). Utiliza el mecanismo de **Atención (Self-Attention)**, que permite al modelo procesar todas las palabras de un texto en paralelo y entender la relación/contexto entre palabras lejanas dentro de una oración.
* **Tokenización:** Los modelos no leen palabras como los humanos; convierten el texto en fragmentos numéricos llamados *tokens* (aprox. 1 token = 0.75 palabras en inglés, varía en español).
* **Entrenamiento en 3 Etapas:**
  1. **Preentrenamiento (Pre-training):** El modelo consume terabytes de texto de Internet para aprender patrones del lenguaje, gramática y hechos del mundo.
  2. **Alineación / Fine-Tuning (SFT - Supervised Fine-Tuning):** Se le enseña a responder en formato de asistente (instrucción $\rightarrow$ respuesta).
  3. **RLHF (Reinforcement Learning from Human Feedback):** Ajuste fino basado en preferencias humanas para reducir respuestas tóxicas o inútiles.

---

### 2. Comparativa: GPT-4o vs Claude vs Gemini

Cada modelo top del mercado tiene fortalezas de acuerdo con su diseño y entrenamiento:

<img width="1202" height="845" alt="image" src="https://github.com/user-attachments/assets/b4d29373-fd3b-4ee0-932e-22a7d2c70970" />

| Modelo | Desarrollador | Fortalezas Principales | Caso de Uso Ideal |
| :--- | :--- | :--- | :--- |
| **GPT-4o** | OpenAI | Latencia baja, soporte multimodal nativo (texto, visión, audio fluida) y ecosistema sólido de APIs. | Aplicaciones de propósito general, procesamiento en tiempo real y agentes. |
| **Claude (3.5 Sonnet)** | Anthropic | Lógica de programación excepcional, prosa natural, seguimiento preciso de instrucciones complejas y tono muy humano. | Refactorización de código, redacción técnica y tareas de razonamiento profundo. |
| **Gemini** | Google | Ventanas de contexto masivas (hasta millones de tokens), integración nativa con el ecosistema Google y procesamiento multimodal extremo. | Análisis de repositorios completos de código, libros enteros o horas de video continuo. |

---

### 3. Prompt Engineering Avanzado

El *Prompt Engineering* no se trata de "saber qué pedir", sino de **estructurar el contexto y restringir el espacio de búsqueda de respuestas** del modelo.

#### Técnicas Clave:
* **Zero-Shot Prompting:** Funciona bien para tareas simples y bien definidas. Para tareas complejas o ambiguas, puede no ser suficiente. Consiste en pedirle al modelo que realice una tarea sin darle ningún ejemplo previo.El modelo se basa únicamente en su conocimiento de entrenamiento.
Ejemplo: “Clasifica el siguiente texto como positivo, negativo o neutro: ‘El servicio fue excelente’”
* **Few-Shot Prompting:** Proporcionar al modelo 2 o 3 ejemplos de entrada/salida deseada antes de hacer la pregunta real para guiar el formato exacto.
* **Chain-of-Thought (CoT) / Cadenas de Pensamiento:** Pedirle al modelo que *"piense paso a paso"*. Esto obliga al sistema a generar tokens intermedios de razonamiento antes de dar la respuesta final, reduciendo errores drásticamente en matemática y lógica.
* **Rol y Restricciones:** Asignar una persona o rol claro (*"Actúa como un arquitecto de software senior..."*) y fijar fronteras explícitas (*"Si no sabes la respuesta, di 'no lo sé', no inventes"*).
* **Metodología XML/Markdown:** Estructurar el prompt usando etiquetas claras (p. ej., `<instrucciones>`, `<contexto>`, `<salida_esperada>`) para evitar confusiones en textos largos.

---

### 4. Alucinaciones y Límites Reales

#### ¿Qué es una Alucinación?
Es cuando un LLM genera información de manera plausible, segura y gramaticalmente correcta, pero **completamente falsa o carente de sustento en los datos**. Ocurre porque el modelo prioriza la fluidez estadística sobre la verificación factual.

#### Límites Reales de la Tecnología:
1. **Límite de Ventana de Contexto y Olvido:** Aunque las ventanas crezcan, la atención decae con contexto hiper-extenso (*"Needle in a Haystack problem"*).
2. **Conocimiento Estático:** Los modelos solo conocen lo que estaba presente hasta la fecha de corte de su entrenamiento (a menos que usen herramientas de búsqueda o RAG).
3. **Costo Computacional y Latencia:** A mayor razonamiento interno, mayor es el costo por token y el tiempo de respuesta.

#### Estrategias de Mitigación en Producción:
* **RAG (Retrieval-Augmented Generation):** Conectar el LLM a una base de datos vectorial con documentos reales para que responda **únicamente** con base en esa información.
* **Baja Temperatura:** Configurar la temperatura de muestreo del modelo cerca de `0.0` para hacer respuestas más deterministas.
* **Validación de Salida:** Uso de esquemas estrictos (JSON Schema, herramientas como Pydantic) para forzar salidas formateadas y verificables por código.

---

## Enlaces y Recursos Recomendados (`recursos/`)

* **Paper:** *Attention Is All You Need* (Vaswani et al.)
* **Plataforma de Pruebas:** [OpenRouter](https://openrouter.ai/) o plataformas oficiales para comparar latencia/costos.
* **Documentación:** Guías de Prompt Engineering de Anthropic y OpenAI.
