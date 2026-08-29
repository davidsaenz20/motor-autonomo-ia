CONFIGURACIÓN DE IA Y HERRAMIENTAS

1. OBJETIVO

Definir la capa de inteligencia y herramientas del Motor Autónomo de IA.

La arquitectura debe permitir cambiar de proveedor de IA o de herramienta sin
reconstruir el motor.

El motor debe priorizar:

1. fiabilidad;
2. coste bajo;
3. capacidad de razonamiento;
4. disponibilidad;
5. facilidad de integración;
6. continuidad del trabajo.

---

2. PRINCIPIO FUNDAMENTAL

La IA no es el motor completo.

n8n = ORQUESTACIÓN
IA = RAZONAMIENTO
MEMORIA = PERSISTENCIA
HERRAMIENTAS = ACCESO / EJECUCIÓN
VALIDADOR = CONTROL DE CALIDAD
TELEGRAM = COMUNICACIÓN

Ningún proveedor de IA debe convertirse en una dependencia estructural del
sistema.

---

3. PROVEEDOR DE IA

El sistema utilizará una abstracción:

AI_PROVIDER
AI_MODEL
AI_REQUEST
AI_RESPONSE
AI_STATUS

Esto permitirá sustituir un proveedor por otro.

---

4. PROVEEDORES

Inicialmente se investigarán proveedores con API disponible y opciones
gratuitas o de bajo coste.

Categorías:

PRIMARY_AI
SECONDARY_AI
TERTIARY_AI

No se asumirá que un servicio es gratuito permanentemente.

Los límites y precios deberán comprobarse antes de incorporarlo al sistema.

---

5. IA PRINCIPAL

La IA principal será seleccionada según:

- calidad de razonamiento;
- contexto disponible;
- velocidad;
- coste;
- límites de API;
- capacidad para tareas de investigación;
- disponibilidad.

La selección será revisable.

---

6. IA SECUNDARIA

La IA secundaria se utilizará como respaldo cuando la principal:

- alcance un límite;
- no esté disponible;
- produzca un error técnico;
- no pueda realizar una tarea compatible.

---

7. IA TERCIARIA

Será opcional.

Solo se incorporará si aporta una ventaja real.

No añadir proveedores simplemente para aumentar la complejidad.

---

8. FALLBACK

Arquitectura:

PRIMARY
  ↓
ERROR / LIMIT
  ↓
SECONDARY
  ↓
ERROR / LIMIT
  ↓
TERTIARY
  ↓
ERROR
  ↓
ACTION_REQUIRED

---

9. REGLA DE FALLBACK

Un cambio de proveedor no debe provocar pérdida del contexto.

El ejecutor deberá conservar:

TASK
PROJECT
RELEVANT_MEMORY
INPUT
PREVIOUS_RESULTS

---

10. COSTE

El motor deberá registrar:

AI_PROVIDER
AI_MODEL
TOKENS / USAGE
ESTIMATED_COST

cuando el proveedor permita obtener esos datos.

---

11. CUOTAS

Cada proveedor tendrá una configuración conceptual:

DAILY_LIMIT
MONTHLY_LIMIT
REQUEST_LIMIT
TOKEN_LIMIT

No se debe asumir que una API gratuita tiene capacidad ilimitada.

---

12. SELECCIÓN DE IA

El ejecutor decidirá el proveedor mediante reglas.

Ejemplo:

TASK TYPE
↓
REQUIRED CAPABILITY
↓
AVAILABLE PROVIDERS
↓
QUOTA
↓
COST
↓
SELECT PROVIDER

---

13. TAREAS SIMPLES

Para tareas sencillas podrá utilizarse una IA económica o gratuita.

Ejemplos:

CLASSIFICATION
SUMMARIZATION
EXTRACTION
FORMATTING

---

14. TAREAS COMPLEJAS

Para:

STRATEGY
RESEARCH
BUSINESS ANALYSIS
VALIDATION
MULTI-STEP REASONING

se priorizará la IA con mejor capacidad de razonamiento disponible.

---

15. WEB SEARCH

La investigación web será una herramienta independiente.

WEB_SEARCH

La IA no debe inventar resultados de búsqueda.

---

16. WEB FETCH

Cuando sea necesario consultar una fuente:

WEB_SEARCH
↓
URL
↓
WEB_FETCH
↓
CONTENT

---

17. FUENTES

Cuando una investigación dependa de información externa, guardar:

SOURCE_URL
SOURCE_TITLE
SOURCE_DATE
SOURCE_TYPE
SOURCE_RELEVANCE

cuando esté disponible.

---

18. INVESTIGACIÓN

El motor deberá distinguir:

SEARCH
↓
COLLECT
↓
COMPARE
↓
ANALYZE
↓
VALIDATE

No considerar una única página como evidencia suficiente cuando el problema
requiera contraste.

---

19. CONTRASTE

Para afirmaciones importantes:

SOURCE A
+
SOURCE B
+
SOURCE C
↓
CROSS-CHECK

La cantidad necesaria dependerá de la importancia de la afirmación.

---

20. FIABILIDAD DE FUENTES

Prioridad general:

OFFICIAL
↓
PRIMARY
↓
SPECIALIZED
↓
REPUTABLE SECONDARY
↓
COMMUNITY
↓
UNKNOWN

La jerarquía puede cambiar según el tipo de investigación.

---

21. KEYWORD RESEARCH

El motor podrá realizar tareas:

KEYWORD_DISCOVERY
SEARCH_VOLUME
SEARCH_INTENT
COMPETITION
SERP_ANALYSIS
LOCAL_INTENT
COMMERCIAL_INTENT

Los datos dependerán de las herramientas disponibles.

---

22. INTENCIÓN DE BÚSQUEDA

Categorías iniciales:

INFORMATIONAL
NAVIGATIONAL
COMMERCIAL
TRANSACTIONAL
LOCAL

Una consulta puede tener más de una característica.

---

23. INVESTIGACIÓN DE NEGOCIOS

Para analizar una idea:

PROBLEM
↓
TARGET
↓
DEMAND
↓
COMPETITION
↓
SEARCH INTENT
↓
MONETIZATION
↓
ACQUISITION
↓
COST
↓
RISK
↓
OPPORTUNITY

---

24. GITHUB

GitHub será inicialmente una fuente de documentación y contexto.

El motor podrá leer:

REPOSITORIES
FILES
DOCUMENTATION
ISSUES
PULL REQUESTS
COMMITS

según las capacidades disponibles.

---

25. ESCRITURA EN GITHUB

El motor no debe intentar escribir directamente en repositorios cuando la
conexión disponible solo tenga permisos de lectura.

Si necesita modificar un archivo y no dispone de escritura:

ACTION_REQUIRED

y deberá indicar a David:

REPOSITORY
FILE
CHANGE REQUIRED
REASON

---

26. REPOSITORIOS AUTORIZADOS

El motor no estará limitado a un repositorio.

Debe trabajar con los repositorios que David autorice.

Ejemplo:

REPOSITORY A
REPOSITORY B
REPOSITORY C

---

27. AISLAMIENTO

Cada proyecto deberá tener:

PROJECT_ID
REPOSITORY
REPOSITORY_PATH

El contexto de un proyecto no debe mezclarse con otro.

---

28. ARCHIVOS

Las herramientas de archivos permitirán:

READ
SEARCH
ANALYZE
VALIDATE

La escritura dependerá de los permisos y herramientas disponibles.

---

29. PROCESAMIENTO DE DATOS

Cuando sea necesario:

COLLECT
↓
NORMALIZE
↓
STRUCTURE
↓
ANALYZE
↓
STORE

---

30. HERRAMIENTAS COMO CAPACIDADES

El ejecutor no debe depender de nombres concretos de herramientas.

Debe trabajar conceptualmente con:

SEARCH
FETCH
READ
WRITE
ANALYZE
VALIDATE
COMMUNICATE

Las implementaciones concretas podrán cambiar.

---

31. TOOL REGISTRY

En una fase posterior se podrá mantener un registro:

TOOL_ID
TOOL_NAME
TYPE
STATUS
COST
LIMITS
CAPABILITIES

---

32. ESTADO DE HERRAMIENTA

AVAILABLE
LIMITED
UNAVAILABLE
ERROR
DISABLED

---

33. SELECCIÓN DE HERRAMIENTA

TASK
↓
REQUIRED CAPABILITY
↓
AVAILABLE TOOLS
↓
LIMITS
↓
COST
↓
SELECT

---

34. HERRAMIENTA FALLIDA

Si una herramienta falla:

TOOL ERROR
↓
RETRY?
├── YES → RETRY
└── NO → ALTERNATIVE TOOL

Si no existe alternativa:

ACTION_REQUIRED

o:

BLOCKED

según el caso.

---

35. VALIDACIÓN DE HERRAMIENTAS

Una herramienta no se considera fiable simplemente porque devuelva datos.

Los resultados importantes deberán ser validados por el workflow correspondiente.

---

36. IA + HERRAMIENTAS

La IA puede determinar:

WHAT

n8n determina:

HOW

Ejemplo:

IA:
"Necesito conocer la demanda local."

n8n:
SEARCH → COLLECT → STORE

---

37. NO PERMITIR ALUCINACIONES

La IA nunca debe presentar como hecho:

- una búsqueda que no realizó;
- una fuente que no consultó;
- un dato que no obtuvo;
- una acción que no ejecutó.

Debe diferenciar:

OBSERVED
INFERRED
ASSUMED
UNKNOWN

---

38. RESULTADOS DE INVESTIGACIÓN

Todo resultado importante deberá poder clasificarse:

CONFIRMED
PROBABLE
HYPOTHESIS
DISCARDED
UNKNOWN

---

39. CONFIANZA

Cada resultado importante podrá tener:

HIGH
MEDIUM
LOW
UNKNOWN

---

40. VALIDACIÓN CRUZADA

Cuando la importancia lo justifique:

AI RESULT
↓
SOURCE CHECK
↓
SECOND SOURCE
↓
VALIDATION

---

41. CONTEXTO MÍNIMO

No enviar a la IA todo el proyecto en cada petición.

Enviar:

TASK
+
RELEVANT PROJECT MEMORY
+
RELEVANT PREVIOUS RESULTS
+
REQUIRED SOURCES

---

42. CONTEXTO EXCESIVO

Si el contexto supera los límites:

SUMMARIZE
↓
COMPRESS
↓
RETAIN IMPORTANT FACTS
↓
EXECUTE

No eliminar hechos importantes.

---

43. SEGURIDAD

Las credenciales de:

- APIs;
- Telegram;
- GitHub;
- servicios externos;

no deben almacenarse dentro de documentos públicos del proyecto.

---

44. SECRETOS

Nunca incluir:

API_KEYS
TOKENS
PASSWORDS
PRIVATE_KEYS
WEBHOOK_SECRETS

en archivos ".md" versionados.

---

45. CREDENCIALES

n8n deberá utilizar su sistema de credenciales o el mecanismo seguro
correspondiente.

Los valores reales se introducirán durante la implementación.

---

46. RATE LIMITING

Cada herramienta deberá respetar sus límites.

Si se detecta:

RATE_LIMIT

el motor deberá:

WAIT
↓
RETRY

o cambiar de proveedor si existe una alternativa válida.

---

47. BACKOFF

Los reintentos deberán evitar peticiones continuas.

Conceptualmente:

RETRY 1
↓
WAIT
↓
RETRY 2
↓
WAIT LONGER
↓
RETRY 3

---

48. NO REINTENTOS INFINITOS

Todos los reintentos tendrán límite.

Cuando se alcance:

MAX_RETRIES

se deberá decidir:

ALTERNATIVE
BLOCKED
ACTION_REQUIRED

---

49. OBSERVABILIDAD

Cada ejecución deberá registrar:

WHAT
WHEN
PROVIDER
TOOL
STATUS
RESULT
ERROR

cuando sea necesario para diagnosticar problemas.

---

50. COST CONTROL

El motor deberá evitar:

RESEARCH DUPLICATION
UNNECESSARY AI CALLS
UNNECESSARY RETRIES
EXCESSIVE CONTEXT

---

51. CACHÉ

En una fase posterior se podrá reutilizar información que siga siendo válida.

Ejemplo:

SEARCH RESULT
↓
CACHE
↓
REUSE

La duración dependerá del tipo de dato.

---

52. DATOS TEMPORALES

Información que cambia rápidamente deberá tener fecha:

COLLECTED_AT

y, cuando sea relevante:

EXPIRES_AT

---

53. DECISIONES

La IA podrá recomendar:

DECISION
REASON
EVIDENCE
CONFIDENCE

Pero las decisiones importantes deberán quedar registradas como parte de la
memoria del proyecto.

---

54. NO CONFUNDIR ANÁLISIS CON HECHO

Ejemplo:

HECHO:
"Se encontraron X resultados."

INFERENCIA:
"Esto podría indicar una oportunidad."

HIPÓTESIS:
"Probablemente exista demanda suficiente."

El motor debe conservar esta diferencia.

---

55. HERRAMIENTAS GRATUITAS

La arquitectura debe permitir utilizar herramientas gratuitas cuando sean
suficientes.

Pero:

FREE
≠
UNLIMITED

Siempre comprobar:

- límites;
- condiciones;
- disponibilidad;
- restricciones;
- cambios de servicio.

---

56. COSTE CERO COMO OBJETIVO DEL MVP

El MVP priorizará:

COSTE ≈ 0

siempre que esto no reduzca la capacidad de demostrar correctamente el
sistema.

No sacrificar la prueba del motor por ahorrar unos céntimos.

---

57. CAMBIO DE PROVEEDOR

Cambiar una IA o herramienta no debe requerir modificar:

PROJECT
TASK
SESSION
CHECKPOINT
MEMORY
CONTROL

Solo la capa de ejecución.

---

58. CONFIGURACIÓN CENTRAL

Los proveedores y herramientas deberán configurarse de forma centralizada.

Conceptualmente:

CONFIG
├── AI
├── SEARCH
├── GITHUB
├── TELEGRAM
├── LIMITS
└── SECURITY

---

59. PRIMER MVP

Para el primer MVP se utilizará:

1 IA
1 herramienta de búsqueda
GitHub READ
Telegram
1 almacenamiento persistente

No se implementará inicialmente un ecosistema complejo de proveedores.

---

60. EVOLUCIÓN

Una vez validado el MVP:

1 IA
↓
2 IA
↓
FALLBACK
↓
OPTIMIZACIÓN COSTE
↓
SELECCIÓN INTELIGENTE

---

61. PRINCIPIO DE SIMPLICIDAD

Si una solución más sencilla consigue el mismo resultado:

UTILIZAR LA SENCILLA

No añadir infraestructura por anticipación.

---

62. ESTADO

Estado del documento:

"READY_FOR_IMPLEMENTATION"

---

63. SIGUIENTE PASO

Con los documentos:

MODELO-DATOS.md
WORKFLOWS-N8N.md
CONFIGURACION-IA-Y-HERRAMIENTAS.md

el diseño funcional principal queda preparado.

El siguiente documento será:

"IMPLEMENTACION-PC.md"

Ese documento contendrá únicamente las acciones que habrá que realizar cuando
David tenga acceso al ordenador.

No requiere ejecución inmediata.


