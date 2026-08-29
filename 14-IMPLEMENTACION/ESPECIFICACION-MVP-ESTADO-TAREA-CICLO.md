ESPECIFICACIÓN MVP — ESTADO, TAREA Y CICLO

1. PROPÓSITO

Este documento convierte la arquitectura conceptual del Motor Autónomo de IA
en una especificación mínima que pueda implementarse realmente.

El MVP debe demostrar una capacidad concreta:

«David da una orden y el motor es capaz de recuperar el proyecto, seleccionar
trabajo válido, ejecutarlo, validar el resultado, guardar el estado y
continuar con la siguiente tarea sin necesitar una nueva orden de David.»

El MVP no pretende implementar todavía todas las capacidades futuras.

Su finalidad es demostrar el bucle autónomo básico.

---

2. PRINCIPIO DEL MVP

La unidad fundamental es:

PROYECTO
↓
ESTADO
↓
TAREA
↓
EJECUCIÓN
↓
RESULTADO
↓
VALIDACIÓN
↓
CHECKPOINT
↓
SIGUIENTE TAREA

Este ciclo debe funcionar antes de añadir complejidad.

---

3. ALCANCE

El MVP inicial debe incluir:

✓ Un proyecto
✓ Persistencia de estado
✓ Cola de tareas
✓ Selección de siguiente tarea
✓ Ejecución mediante IA
✓ Resultado estructurado
✓ Validación básica
✓ Checkpoint
✓ Continuación automática
✓ Pausa
✓ Detención
✓ Recuperación después de reinicio
✓ Intervención humana básica
✓ Telegram como canal inicial
✓ Registro de errores

---

4. FUERA DEL MVP INICIAL

No son necesarios para demostrar el primer ciclo:

✗ WhatsApp
✗ Multiusuario
✗ Dashboard avanzado
✗ Alta disponibilidad
✗ Clúster
✗ Escalado horizontal
✗ Sistema multiagente complejo
✗ Optimización avanzada de costes
✗ Rotación automática de múltiples proveedores
✗ VPS obligatorio
✗ Automatización destructiva de GitHub

Podrán añadirse posteriormente.

---

5. ENTIDADES MÍNIMAS

El MVP utilizará cinco entidades principales:

PROJECT
SESSION
TASK
EXECUTION
CHECKPOINT

Y dos entidades auxiliares:

RESULT
ERROR

---

6. PROJECT

Representa un proyecto sobre el que trabaja el motor.

Campos mínimos:

project_id
name
repository
status
objective
created_at
updated_at

Estados:

ACTIVE
PAUSED
STOPPED
COMPLETED
BLOCKED

---

7. SESSION

Representa una ejecución autónoma del proyecto.

Campos:

session_id
project_id
status
started_at
updated_at
finished_at

Estados:

RUNNING
PAUSED
WAITING_HUMAN
BLOCKED
COMPLETED
STOPPED

---

8. TASK

Es la unidad mínima de trabajo.

Campos mínimos:

task_id
project_id
session_id
parent_task_id
title
description
status
priority
depends_on
attempts
max_attempts
created_at
updated_at

---

9. ESTADOS DE TASK

Estados mínimos:

READY
RUNNING
WAITING_VALIDATION
COMPLETED
FAILED
BLOCKED
CANCELLED

Flujo normal:

READY
↓
RUNNING
↓
WAITING_VALIDATION
↓
COMPLETED

---

10. TRANSICIONES DE TASK

READY → RUNNING

Cuando el planificador selecciona una tarea válida.

RUNNING → WAITING_VALIDATION

Cuando la IA termina y existe un resultado.

WAITING_VALIDATION → COMPLETED

Cuando el resultado supera la validación.

WAITING_VALIDATION → FAILED

Cuando el resultado no supera la validación y puede reintentarse.

FAILED → READY

Cuando todavía quedan intentos disponibles.

FAILED → BLOCKED

Cuando se alcanza "max_attempts" o existe un bloqueo no recuperable.

RUNNING → BLOCKED

Cuando la tarea requiere una acción externa o humana.

---

11. PRIORIDAD

El MVP utilizará inicialmente:

HIGH
NORMAL
LOW

Regla:

Una tarea de prioridad superior puede seleccionarse antes que otra de menor
prioridad siempre que sus dependencias estén satisfechas.

---

12. DEPENDENCIAS

Una tarea puede depender de otras.

Ejemplo:

TASK-003
depends_on:
TASK-001
TASK-002

TASK-003 no puede ejecutarse hasta que:

TASK-001 = COMPLETED
TASK-002 = COMPLETED

---

13. TAREA EJECUTABLE

Una tarea es ejecutable si:

status = READY

y:

todas sus dependencias = COMPLETED

y:

project.status = ACTIVE

y:

session.status = RUNNING

---

14. SELECCIÓN DE TAREA

El planificador realizará:

OBTENER TASKS
↓
FILTRAR READY
↓
ELIMINAR DEPENDENCIAS NO CUMPLIDAS
↓
ORDENAR POR PRIORIDAD
↓
SELECCIONAR

Si no existe ninguna tarea ejecutable:

NO_WORK

---

15. EJECUCIÓN

Cada ejecución debe identificarse mediante:

execution_id

Campos mínimos:

execution_id
task_id
session_id
started_at
finished_at
status
provider
model
input_reference
output_reference
error_id

---

16. ESTADOS DE EXECUTION

RUNNING
SUCCESS
FAILED
TIMEOUT
CANCELLED

---

17. RESULTADO

La IA no debe devolver únicamente texto libre cuando el resultado vaya a
utilizarse para continuar el proceso.

El motor deberá transformar el resultado en una estructura mínima:

result_id
execution_id
summary
output
confidence
validation_status
created_at

---

18. VALIDACIÓN

El MVP tendrá una validación básica.

Como mínimo comprobará:

¿Existe resultado?
¿Tiene contenido?
¿Responde al objetivo de la tarea?
¿Tiene el formato esperado?
¿Existe algún error evidente?

Resultado:

VALID
INVALID
PARTIAL

---

19. REGLA CRÍTICA

Nunca:

IA RESPONDE
↓
TASK COMPLETED

Debe existir:

IA RESPONDE
↓
RESULTADO
↓
VALIDACIÓN
↓
TASK COMPLETED

---

20. CHECKPOINT

El checkpoint representa el último estado seguro conocido.

Campos mínimos:

checkpoint_id
project_id
session_id
current_task_id
state_snapshot
created_at

---

21. CUÁNDO CREAR CHECKPOINT

Como mínimo después de:

TASK COMPLETED
TASK FAILED
TASK BLOCKED
PAUSE
STOP
WAITING_HUMAN

También podrá crearse antes de operaciones críticas.

---

22. STATE SNAPSHOT

El snapshot debe permitir reconstruir:

project
session
current_task
pending_tasks
completed_tasks
blocked_tasks
important_results
pending_human_action

No es necesario guardar absolutamente todo en el snapshot si existe una base
de datos persistente.

El objetivo es poder reconstruir el estado.

---

23. RECUPERACIÓN

Después de un reinicio:

LOAD PROJECT
↓
LOAD SESSION
↓
LOAD LAST CHECKPOINT
↓
RECONSTRUCT STATE
↓
CHECK CURRENT TASK
↓
CONTINUE

---

24. REGLA DE NO DUPLICACIÓN

Si una tarea ya está:

COMPLETED

el motor no debe volver a ejecutarla simplemente porque n8n haya sido
reiniciado.

La recuperación debe continuar desde el último estado válido.

---

25. SESIÓN AUTÓNOMA

El ciclo principal será:

START SESSION
↓
LOAD STATE
↓
SELECT TASK
↓
EXECUTE
↓
VALIDATE
↓
SAVE RESULT
↓
CHECKPOINT
↓
SELECT NEXT TASK
↓
...

---

26. CONDICIONES DE CONTINUACIÓN

Después de completar una tarea:

¿MOTOR ACTIVO?
├── NO → STOP
└── SÍ
    ↓
¿NECESITA HUMANO?
├── SÍ → WAITING_HUMAN
└── NO
    ↓
¿HAY SIGUIENTE TAREA?
├── SÍ → CONTINUE
└── NO → OBJECTIVE CHECK

---

27. COMPROBACIÓN DE OBJETIVO

Cuando no queden tareas ejecutables:

NO_WORK
↓
OBJECTIVE_CHECK

La IA/validador determinará:

OBJECTIVE_COMPLETED

o:

OBJECTIVE_NOT_COMPLETED

Si no está completado y existe una razón válida para continuar:

GENERATE NEXT TASK

Si no existe trabajo útil:

SESSION_COMPLETED

---

28. REGLA CONTRA TRABAJO ARTIFICIAL

El motor no puede generar tareas simplemente para continuar ejecutándose.

Toda nueva tarea deberá tener:

objective
reason
expected_output

como mínimo.

---

29. GENERACIÓN DE TAREAS

En el MVP se permitirá generar una nueva tarea cuando:

1. el objetivo todavía no esté cumplido;
2. exista una necesidad identificable;
3. la nueva tarea tenga un resultado esperado;
4. no sea un duplicado;
5. no provoque un ciclo conocido.

---

30. LÍMITE DE GENERACIÓN

Debe existir:

max_task_generation_depth

para evitar:

TASK
↓
SUBTASK
↓
SUBTASK
↓
SUBTASK
↓
...

indefinidamente.

Valor inicial recomendado para pruebas:

3

Podrá modificarse después de validar el comportamiento.

---

31. DETECCIÓN DE DUPLICADOS

Antes de crear una nueva tarea:

COMPARE WITH EXISTING TASKS

Si ya existe una tarea equivalente:

REUSE EXISTING TASK

o:

DO NOT CREATE

según el estado de la tarea existente.

---

32. DETECCIÓN DE BUCLES

El motor deberá comprobar que no está ejecutando repetidamente:

misma tarea
mismo objetivo
mismo resultado
mismo error

sin progreso.

Si detecta:

NO_PROGRESS

deberá:

BLOCK

o:

WAITING_HUMAN

según el caso.

---

33. REINTENTOS

Cada tarea tendrá:

attempts
max_attempts

Ejemplo inicial:

max_attempts = 3

Flujo:

FAILED
↓
attempts < max_attempts?
├── SÍ → READY
└── NO → BLOCKED

---

34. ERRORES

Cada error deberá registrar como mínimo:

error_id
execution_id
task_id
type
message
created_at
recoverable

Tipos mínimos:

TEMPORARY
PROVIDER
TOOL
VALIDATION
TIMEOUT
AUTH
PERMISSION
UNKNOWN

---

35. ERROR RECUPERABLE

Ejemplo:

TEMPORARY

Acción:

RETRY

respetando el límite de intentos.

---

36. ERROR NO RECUPERABLE

Ejemplo:

AUTH
PERMISSION

Acción:

BLOCKED

y, cuando proceda:

ACTION_REQUIRED

---

37. INTERVENCIÓN HUMANA

Si la tarea requiere una acción de David:

TASK
↓
BLOCKED / WAITING_HUMAN
↓
CHECKPOINT
↓
NOTIFY

El sistema debe guardar:

human_action_id
task_id
question
reason
status
created_at

---

38. ESPERA HUMANA

Estados:

WAITING
RESOLVED
CANCELLED

Mientras una acción esté:

WAITING

el motor no debe ejecutar tareas que dependan de ella.

---

39. RESPUESTA DE DAVID

Telegram recibirá:

HECHO

o una respuesta contextual.

El motor deberá identificar a qué acción humana corresponde.

Después:

RESOLVE HUMAN ACTION
↓
VALIDATE
↓
UPDATE STATE
↓
CONTINUE

---

40. PAUSA

Comando:

/pausar

Acción:

SAVE CHECKPOINT
↓
SESSION = PAUSED

No deben iniciarse nuevas tareas.

Una tarea que ya esté ejecutándose deberá manejarse de forma segura.

---

41. CONTINUAR

Comando:

/continuar

Acción:

LOAD STATE
↓
SESSION = RUNNING
↓
CONTINUE CYCLE

---

42. DETENER

Comando:

/detener

Acción:

SAVE CHECKPOINT
↓
SESSION = STOPPED

No debe continuar automáticamente.

---

43. ESTADO

Comando:

/estado

Debe devolver información suficiente para conocer:

PROJECT
SESSION
CURRENT TASK
STATUS
COMPLETED TASKS
PENDING TASKS
BLOCKED TASKS
HUMAN ACTIONS
LAST CHECKPOINT

---

44. TELEGRAM

Telegram es inicialmente un canal de control.

No debe contener la lógica del motor.

Arquitectura:

TELEGRAM
↓
N8N
↓
MOTOR
↓
ESTADO

---

45. N8N

n8n será responsable de la orquestación.

El MVP no debe depender de que n8n contenga toda la lógica intelectual.

La lógica deberá estar separada en componentes suficientemente claros para
poder migrarla posteriormente.

---

46. CICLO N8N MÍNIMO

Conceptualmente:

TRIGGER
↓
LOAD STATE
↓
CHECK SESSION
↓
SELECT TASK
↓
EXECUTE TASK
↓
SAVE RESULT
↓
VALIDATE
↓
UPDATE TASK
↓
CREATE CHECKPOINT
↓
DECIDE NEXT ACTION

---

47. CONTINUACIÓN N8N

Si:

NEXT_TASK EXISTS

el workflow deberá volver al ciclo de ejecución.

Conceptualmente:

UPDATE STATE
↓
NEXT ITERATION

No debe depender de que David envíe otro mensaje.

---

48. LÍMITE DE SEGURIDAD

El workflow debe tener mecanismos para evitar una ejecución infinita.

Como mínimo:

MAX_ITERATIONS
MAX_RUNTIME
MAX_RETRIES
MAX_GENERATION_DEPTH

---

49. MAX_ITERATIONS

Para las primeras pruebas se podrá establecer:

MAX_ITERATIONS = 20

Si se alcanza:

CHECKPOINT
↓
PAUSE
↓
NOTIFY

Este valor es de prueba, no una decisión definitiva de producción.

---

50. MAX_RUNTIME

El motor debe poder detener una sesión si supera el tiempo máximo permitido.

Antes de detener:

CHECKPOINT

Después:

PAUSED

---

51. CONTEXTO DE IA

Cada tarea deberá recibir solamente el contexto necesario.

Como mínimo:

PROJECT OBJECTIVE
TASK
RELEVANT STATE
RELEVANT PREVIOUS RESULTS
RELEVANT RULES

No enviar indiscriminadamente toda la memoria.

---

52. REGLAS DEL REPOSITORIO

Si el proyecto trabaja sobre GitHub:

READ REPOSITORY
↓
DISCOVER RULES
↓
APPLY RELEVANT RULES
↓
EXECUTE

Las reglas encontradas en el repositorio no pueden superar las reglas del
motor.

---

53. RESULTADOS

Los resultados importantes deberán poder ser utilizados por tareas futuras.

No todo texto generado necesita convertirse en memoria permanente.

El motor debe distinguir:

TEMPORARY OUTPUT
IMPORTANT RESULT
DECISION
FACT
HYPOTHESIS

---

54. HECHO VS HIPÓTESIS

Los resultados de investigación deberán poder clasificarse como:

CONFIRMED
PROBABLE
HYPOTHESIS
UNKNOWN

No presentar una hipótesis como hecho.

---

55. AUDITORÍA

Cada ciclo debe permitir reconstruir:

qué tarea se ejecutó;
cuándo;
con qué modelo;
qué resultado produjo;
cómo se validó;
qué estado generó;
qué ocurrió después.

---

56. CRITERIO DE ÉXITO

El MVP será considerado técnicamente demostrado cuando pueda realizar:

/continuar
↓
LOAD STATE
↓
SELECT TASK
↓
EXECUTE
↓
VALIDATE
↓
CHECKPOINT
↓
SELECT NEXT TASK
↓
EXECUTE
↓
VALIDATE
↓
CHECKPOINT
↓
...

sin intervención de David entre las tareas.

---

57. PRUEBA DE REINICIO

Durante una ejecución:

TASK-001 = COMPLETED
TASK-002 = READY

detener n8n.

Reiniciar.

Resultado obligatorio:

TASK-001 NO SE REPITE
TASK-002 PUEDE CONTINUAR

---

58. PRUEBA DE ERROR

Provocar:

TASK-002 → ERROR

Resultado:

ATTEMPT 1
↓
RETRY
↓
ATTEMPT 2

hasta alcanzar el límite.

Nunca:

RETRY INFINITO

---

59. PRUEBA HUMANA

Crear una tarea que requiera:

ACTION_REQUIRED

Resultado:

WAITING_HUMAN
↓
TELEGRAM
↓
DAVID
↓
HECHO
↓
VALIDATE
↓
CONTINUE

---

60. PRUEBA DE PAUSA

Durante una sesión:

/pausar

Resultado:

CHECKPOINT
SESSION = PAUSED

Después:

/continuar

Resultado:

RECOVER
CONTINUE

---

61. PRUEBA DE DETENCIÓN

Durante una sesión:

/detener

Resultado:

CHECKPOINT
SESSION = STOPPED

No debe continuar hasta una nueva orden.

---

62. PRUEBA DE OBJETIVO

Cuando el objetivo se haya cumplido:

OBJECTIVE_COMPLETED

El motor debe terminar.

No debe generar trabajo innecesario.

---

63. ARQUITECTURA MÍNIMA

El MVP puede reducirse conceptualmente a:

                ┌──────────────┐
                │   TELEGRAM   │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │     N8N      │
                │ ORQUESTADOR  │
                └──────┬───────┘
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
      ┌──────────────┐   ┌──────────────┐
      │   ESTADO     │   │     IA       │
      └──────┬───────┘   └──────┬───────┘
             │                  │
             └────────┬─────────┘
                      ▼
                ┌──────────────┐
                │ VALIDACIÓN   │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │ CHECKPOINT   │
                └──────────────┘

---

64. IMPLEMENTACIÓN PROGRESIVA

No implementar todo simultáneamente.

Orden:

1. Estado
2. Task
3. Ejecución
4. Resultado
5. Validación
6. Checkpoint
7. Ciclo
8. Continuidad
9. Telegram
10. Intervención
11. Recuperación
12. Pruebas

---

65. PRIMERA VERSIÓN

La primera versión debe ser deliberadamente pequeña.

Debe poder demostrar:

1 proyecto
1 sesión
varias tareas
1 proveedor IA
1 canal Telegram
1 mecanismo persistente
1 ciclo autónomo

Eso es suficiente para validar la arquitectura.

---

66. NO OPTIMIZAR ANTES DE VALIDAR

No dedicar tiempo inicialmente a:

- optimización extrema;
- interfaces complejas;
- múltiples proveedores;
- escalado;
- microservicios;
- alta disponibilidad.

Primero demostrar que:

FUNCIONA

---

67. MIGRABILIDAD

Aunque el MVP sea sencillo, no debe diseñarse de forma que impida crecer.

Las interfaces conceptuales deben permitir posteriormente:

PC
↓
VPS

y:

IA-A
↓
IA-B
↓
IA-C

sin modificar el modelo fundamental:

PROJECT
TASK
EXECUTION
RESULT
CHECKPOINT

---

68. SEGURIDAD MÍNIMA

No almacenar:

API KEYS
TOKENS
PASSWORDS

en los archivos del proyecto.

Las credenciales deben gestionarse mediante el mecanismo seguro disponible
en n8n/infraestructura.

---

69. GITHUB MÍNIMO

En la primera prueba:

READ

antes que:

WRITE

La escritura automática sobre repositorios reales se incorporará después de
validar:

permisos
confirmación
rollback
auditoría

---

70. ESTADO DE IMPLEMENTACIÓN

Este documento NO significa que el MVP esté implementado.

Significa que existe una especificación mínima suficientemente concreta para
comenzar a implementarlo.

Estado:

"READY_FOR_IMPLEMENTATION"

---

71. PUNTO DE ENTRADA

El primer workflow real que debe construirse será:

MVP-001 — CONTINUAR

Entrada:

/continuar

Salida:

motor iniciado
estado recuperado
tarea seleccionada

---

72. SEGUNDO WORKFLOW

Después:

MVP-002 — EJECUTAR TAREA

Debe:

recibir TASK
↓
llamar IA
↓
guardar EXECUTION
↓
guardar RESULT

---

73. TERCER WORKFLOW

Después:

MVP-003 — VALIDAR Y CONTINUAR

Debe:

RESULT
↓
VALIDATE
↓
UPDATE TASK
↓
CHECKPOINT
↓
NEXT TASK

---

74. CUARTO WORKFLOW

Después:

MVP-004 — CONTROL TELEGRAM

Debe gestionar:

/continuar
/pausar
/detener
/estado

---

75. QUINTO WORKFLOW

Después:

MVP-005 — ERROR

Debe centralizar:

ERROR
↓
REGISTER
↓
RETRY / BLOCK / HUMAN

---

76. SEXTO WORKFLOW

Después:

MVP-006 — INTERVENCIÓN HUMANA

Debe gestionar:

ACTION_REQUIRED
↓
WAIT
↓
HECHO
↓
RESOLVE
↓
CONTINUE

---

77. ORDEN DEFINITIVO DEL MVP

MVP-001
ESTADO + CONTINUAR

MVP-002
EJECUCIÓN IA

MVP-003
VALIDACIÓN + CHECKPOINT + CONTINUIDAD

MVP-004
TELEGRAM

MVP-005
ERRORES

MVP-006
INTERVENCIÓN HUMANA

MVP-007
RECUPERACIÓN

MVP-008
PRUEBAS COMPLETAS

---

78. CRITERIO PARA PASAR DE MVP

Solo después de superar:

CONTINUIDAD
PERSISTENCIA
RECUPERACIÓN
VALIDACIÓN
ERRORES
INTERVENCIÓN
AISLAMIENTO

se podrá comenzar a ampliar el sistema.

---

79. SIGUIENTE PUNTO VÁLIDO

El siguiente trabajo ya no es crear documentación conceptual.

Es:

PC
↓
N8N
↓
PERSISTENCIA
↓
MVP-001

La primera implementación deberá ser lo suficientemente pequeña para poder
probarla y corregirla rápidamente.

---

80. ESTADO FINAL DEL DOCUMENTO

"READY_FOR_IMPLEMENTATION"

Este archivo constituye el punto de transición entre:

DISEÑO

y:

IMPLEMENTACIÓN REAL

FIN


