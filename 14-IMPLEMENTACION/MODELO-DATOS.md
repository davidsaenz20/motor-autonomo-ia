MODELO DE DATOS

1. OBJETIVO

Definir la estructura mínima de datos que necesita el Motor Autónomo de IA
para trabajar de forma persistente, recuperar su estado y continuar sin
intervención humana innecesaria.

n8n será el orquestador. La información persistente no dependerá de una
ejecución concreta de n8n.

---

2. PRINCIPIO FUNDAMENTAL

El sistema debe poder detener una ejecución y posteriormente recuperar
exactamente el punto de trabajo.

Por tanto:

EJECUCIÓN
↓
RESULTADO
↓
CHECKPOINT
↓
NUEVA EJECUCIÓN
↓
RECUPERACIÓN
↓
CONTINUACIÓN

---

3. JERARQUÍA

La estructura lógica será:

PROJECT
  ↓
SESSION
  ↓
TASK
  ↓
EXECUTION
  ↓
RESULT
  ↓
CHECKPOINT

Las acciones humanas y los errores estarán vinculados al elemento que los
genere.

---

4. PROJECT

Representa un proyecto completo.

Campos:

PROJECT_ID
NAME
DESCRIPTION
REPOSITORY
REPOSITORY_PATH
STATUS
OBJECTIVE
CREATED_AT
UPDATED_AT
CURRENT_SESSION_ID
CURRENT_TASK_ID

---

5. PROJECT_ID

Identificador único del proyecto.

Nunca depender exclusivamente del nombre visible.

Ejemplo:

project_001

---

6. REPOSITORY

Identifica el repositorio GitHub asociado.

Ejemplo:

davidsaenz20/base-proyectos

El motor no estará limitado a un repositorio concreto.

---

7. REPOSITORY_PATH

Permite indicar una carpeta concreta dentro del repositorio.

Ejemplo:

06-PROYECTOS/proyecto-x

Esto permite trabajar simultáneamente con distintos proyectos dentro de
un mismo repositorio.

---

8. PROJECT_STATUS

Estados mínimos:

ACTIVE
PAUSED
BLOCKED
COMPLETED
ARCHIVED

---

9. SESSION

Representa una sesión de trabajo autónomo.

Campos:

SESSION_ID
PROJECT_ID
STATUS
STARTED_AT
ENDED_AT
TASKS_COMPLETED
TASKS_FAILED
LAST_CHECKPOINT_ID

---

10. SESSION_STATUS

Estados:

RUNNING
PAUSED
WAITING_HUMAN
COMPLETED
ERROR
STOPPED

---

11. TASK

Representa una unidad concreta de trabajo.

Campos:

TASK_ID
PROJECT_ID
SESSION_ID
PARENT_TASK_ID
TITLE
DESCRIPTION
TYPE
PRIORITY
STATUS
DEPENDENCIES
CREATED_AT
STARTED_AT
COMPLETED_AT
RESULT_ID

---

12. PARENT_TASK_ID

Permite crear jerarquías:

TRABAJO PRINCIPAL
 ├── SUBTRABAJO A
 ├── SUBTRABAJO B
 └── SUBTRABAJO C

Esto permitirá mostrar a David el trabajo principal y sus subtrabajos.

---

13. TASK_TYPE

Ejemplos:

RESEARCH
ANALYSIS
KEYWORD_RESEARCH
WEB_RESEARCH
CONTENT
TECHNICAL
VALIDATION
DECISION
HUMAN_ACTION
OTHER

La lista podrá ampliarse.

---

14. TASK_STATUS

Estados mínimos:

PENDING
READY
RUNNING
COMPLETED
FAILED
BLOCKED
WAITING_HUMAN
CANCELLED

---

15. DEPENDENCIES

Una tarea puede depender de otras.

Ejemplo:

TASK-003
depends_on:
  TASK-001
  TASK-002

El planificador no ejecutará TASK-003 hasta que sus dependencias válidas
estén completadas.

---

16. EXECUTION

Representa una ejecución concreta de una tarea.

Campos:

EXECUTION_ID
TASK_ID
SESSION_ID
STARTED_AT
ENDED_AT
STATUS
AI_PROVIDER
AI_MODEL
INPUT_REFERENCE
OUTPUT_REFERENCE
ERROR
RETRY_COUNT

---

17. DIFERENCIA TASK / EXECUTION

Una tarea puede ejecutarse varias veces.

Ejemplo:

TASK
 ↓
EXECUTION 1 → ERROR
 ↓
EXECUTION 2 → ERROR
 ↓
EXECUTION 3 → SUCCESS

La tarea sigue siendo la misma.

---

18. RESULT

Representa el resultado validable producido por una ejecución.

Campos:

RESULT_ID
TASK_ID
EXECUTION_ID
STATUS
SUMMARY
FINDINGS
OUTPUT
CONFIDENCE
VALIDATION_STATUS
CREATED_AT

---

19. RESULT_STATUS

Estados:

VALID
PARTIAL
INVALID
NEEDS_HUMAN

---

20. FINDINGS

Contendrá descubrimientos relevantes.

Debe distinguir entre:

CONFIRMED
PROBABLE
HYPOTHESIS
DISCARDED
UNKNOWN

Esto mantiene la filosofía documental del sistema.

---

21. CONFIDENCE

La confianza puede expresarse como:

HIGH
MEDIUM
LOW
UNKNOWN

No inventar precisión matemática cuando no exista una base objetiva.

---

22. CHECKPOINT

Representa el estado recuperable del motor.

Campos:

CHECKPOINT_ID
PROJECT_ID
SESSION_ID
TASK_ID
EXECUTION_ID
MOTOR_STATUS
QUEUE_REFERENCE
MEMORY_REFERENCE
LAST_RESULT_ID
CREATED_AT

---

23. CHECKPOINT PRINCIPAL

Después de cada tarea válida:

TASK
↓
EXECUTION
↓
RESULT
↓
VALIDATION
↓
CHECKPOINT

El checkpoint será el punto oficial de recuperación.

---

24. RECUPERACIÓN

Si n8n se reinicia:

LOAD CHECKPOINT
↓
LOAD PROJECT
↓
LOAD SESSION
↓
LOAD TASK
↓
VERIFY STATUS
↓
CONTINUE

---

25. QUEUE

La cola contiene tareas que todavía pueden ejecutarse.

Cada elemento debe poder identificarse mediante:

TASK_ID
PRIORITY
STATUS
DEPENDENCIES

---

26. REGLA DE COLA

No duplicar tareas equivalentes.

Antes de crear una nueva tarea:

SEARCH EXISTING TASK
↓
DUPLICATE?
 ├── YES → reutilizar
 └── NO → crear

---

27. PRIORITY

Valores iniciales:

CRITICAL
HIGH
NORMAL
LOW

---

28. HUMAN_ACTION

Representa una acción que necesariamente debe realizar David.

Campos:

ACTION_ID
PROJECT_ID
TASK_ID
DESCRIPTION
REASON
STATUS
CREATED_AT
REQUESTED_AT
COMPLETED_AT

---

29. HUMAN_ACTION STATUS

Estados:

PENDING
NOTIFIED
CONFIRMED
CANCELLED

---

30. REGLA DE INTERVENCIÓN

Cuando una tarea requiera acción humana:

TASK
↓
WAITING_HUMAN
↓
CREATE HUMAN_ACTION
↓
NOTIFY DAVID

No continuar con tareas dependientes.

---

31. REANUDACIÓN HUMANA

Cuando David responda:

HECHO

el motor debe:

IDENTIFY ACTION
↓
UPDATE ACTION
↓
VALIDATE TASK
↓
UPDATE TASK
↓
CONTINUE

---

32. ERROR

Los errores deben registrarse independientemente del resultado.

Campos mínimos:

ERROR_ID
PROJECT_ID
SESSION_ID
TASK_ID
EXECUTION_ID
TYPE
MESSAGE
RETRYABLE
RETRY_COUNT
CREATED_AT

---

33. ERROR TYPE

Ejemplos:

TEMPORARY
AUTHENTICATION
PERMISSION
TOOL
AI
VALIDATION
SYSTEM
UNKNOWN

---

34. RETRYABLE

Valores:

TRUE
FALSE

Nunca reintentar indefinidamente un error marcado como no recuperable.

---

35. MEMORIA

La memoria se divide en:

SYSTEM_MEMORY
PROJECT_MEMORY
SESSION_MEMORY
TASK_MEMORY

---

36. SYSTEM_MEMORY

Contiene reglas permanentes del motor.

No debe contener información específica de un proyecto.

---

37. PROJECT_MEMORY

Contiene:

- objetivo;
- decisiones;
- descubrimientos;
- hipótesis;
- resultados;
- problemas;
- contexto permanente.

---

38. SESSION_MEMORY

Contiene información específica de la sesión actual.

---

39. TASK_MEMORY

Contiene únicamente el contexto necesario para ejecutar o validar una tarea.

---

40. REFERENCIAS

Cuando un resultado sea demasiado grande para almacenarlo directamente en
un registro, se utilizará:

OUTPUT_REFERENCE
MEMORY_REFERENCE
INPUT_REFERENCE
QUEUE_REFERENCE

La referencia apuntará al almacenamiento correspondiente.

---

41. NO DUPLICAR INFORMACIÓN

El sistema no debe copiar indiscriminadamente todo el contexto en cada
ejecución.

Debe utilizar referencias cuando sea posible.

---

42. IDENTIFICADORES

Todos los elementos principales deberán disponer de identificadores únicos:

PROJECT_ID
SESSION_ID
TASK_ID
EXECUTION_ID
RESULT_ID
CHECKPOINT_ID
ACTION_ID
ERROR_ID

---

43. RELACIONES

PROJECT
 ├── SESSION
 │    ├── TASK
 │    │    ├── EXECUTION
 │    │    │    └── RESULT
 │    │    └── HUMAN_ACTION
 │    └── CHECKPOINT
 └── MEMORY

---

44. ESTADO GLOBAL

El motor debe poder obtener rápidamente:

CURRENT_PROJECT
CURRENT_SESSION
CURRENT_TASK
CURRENT_STATUS
CURRENT_CHECKPOINT
PENDING_HUMAN_ACTION

---

45. ESTADO DE AUTONOMÍA

Valores:

AUTONOMOUS
WAITING_HUMAN
PAUSED
STOPPED
ERROR

---

46. REGLA DE CONTINUIDAD

Si:

AUTONOMOUS

y existe una tarea ejecutable:

CONTINUE

---

47. REGLA DE PARADA

Detener únicamente cuando:

- exista acción humana;
- exista error no recuperable;
- exista condición de seguridad;
- se alcance un límite;
- no exista trabajo válido;
- David ordene detener.

---

48. PROGRESO

El sistema deberá calcular progreso mediante:

TASKS_COMPLETED
TASKS_PENDING
TASKS_BLOCKED

Cuando exista un objetivo cuantificable.

---

49. PROGRESO DESCONOCIDO

Si no puede calcularse objetivamente:

PROGRESS = UNKNOWN

No inventar porcentajes.

---

50. AUDITORÍA

Cada modificación relevante de estado deberá poder rastrearse mediante:

TIMESTAMP
PROJECT_ID
TASK_ID
EXECUTION_ID
EVENT_TYPE

---

51. EVENTOS

Ejemplos:

PROJECT_STARTED
SESSION_STARTED
TASK_CREATED
TASK_STARTED
TASK_COMPLETED
TASK_FAILED
RESULT_VALIDATED
CHECKPOINT_CREATED
HUMAN_ACTION_CREATED
HUMAN_ACTION_COMPLETED
SESSION_PAUSED
SESSION_RESUMED
SESSION_STOPPED
ERROR_OCCURRED

---

52. PRINCIPIO DE RECUPERABILIDAD

El sistema debe poder reconstruir:

QUÉ PROYECTO
QUÉ SESIÓN
QUÉ TAREA
QUÉ EJECUCIÓN
QUÉ RESULTADO
QUÉ CHECKPOINT
QUÉ FALTA

sin depender del historial de conversación de ChatGPT.

---

53. IMPLEMENTACIÓN MVP

Para el primer MVP no se debe sobredimensionar la base de datos.

La tecnología concreta de almacenamiento se decidirá posteriormente según:

- simplicidad;
- coste;
- persistencia;
- facilidad de integración con n8n;
- recuperación;
- escalabilidad.

---

54. REGLA DE ARQUITECTURA

n8n:

ORQUESTA

Almacenamiento:

PERSISTE

IA:

RAZONA

Herramientas:

EJECUTAN

Validador:

COMPRUEBA

Telegram:

COMUNICA

---

55. PRINCIPIO FINAL

Ningún componente debe convertirse en el propietario absoluto del estado.

El estado persistente debe permitir sustituir:

- n8n;
- proveedor de IA;
- herramienta;
- canal de comunicación;

sin perder el proyecto.

---

56. ESTADO DEL DOCUMENTO

Estado:

"READY_FOR_IMPLEMENTATION"

Siguiente documento:

"WORKFLOWS-N8N.md"

Ese documento definirá exactamente los workflows, nodos, entradas, salidas,
condiciones y conexiones que posteriormente construiremos en n8n.


