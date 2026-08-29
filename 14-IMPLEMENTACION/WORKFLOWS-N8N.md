WORKFLOWS N8N

1. OBJETIVO

Definir la arquitectura funcional de los workflows que compondrán el Motor
Autónomo de IA.

El objetivo es que n8n actúe como orquestador y permita que el motor continúe
ejecutando trabajo sin requerir que David envíe un mensaje después de cada
tarea.

La ejecución debe detenerse únicamente cuando exista una razón real para
hacerlo.

---

2. PRINCIPIO DE ARQUITECTURA

No se utilizará un único workflow gigantesco.

El sistema estará dividido en workflows especializados.

WF-00 CONTROL
      ↓
WF-01 ESTADO
      ↓
WF-02 PLANIFICADOR
      ↓
WF-03 EJECUTOR
      ↓
WF-04 VALIDADOR
      ↓
WF-05 CHECKPOINT
      ↓
WF-00 CONTROL

Cada workflow debe tener una responsabilidad concreta.

---

3. WORKFLOWS DEL MVP

Inicialmente:

WF-00 CONTROL
WF-01 ESTADO
WF-02 PLANIFICADOR
WF-03 EJECUTOR
WF-04 VALIDADOR
WF-05 CHECKPOINT
WF-06 ENTRADA TELEGRAM
WF-07 NOTIFICACIONES

Los workflows podrán ampliarse posteriormente.

---

4. WF-06 — ENTRADA TELEGRAM

Responsabilidad

Recibir órdenes de David y convertirlas en comandos normalizados.

Entrada

Mensaje recibido desde Telegram.

Comandos iniciales

/continuar
/pausar
/detener
/estado

También podrá aceptar posteriormente lenguaje natural.

Salida

COMMAND
USER_ID
TIMESTAMP
PROJECT_ID
PARAMETERS

---

5. WF-00 — CONTROL

Responsabilidad

Ser el controlador central del motor.

Debe decidir qué debe ocurrir después de cada ejecución.

Entrada

PROJECT_ID
SESSION_ID
CURRENT_STATE
LAST_RESULT
LAST_ERROR

Decisiones

¿PAUSADO?
    ↓ SÍ → detener

¿DETENIDO?
    ↓ SÍ → detener

¿ACCIÓN HUMANA?
    ↓ SÍ → esperar

¿ERROR NO RECUPERABLE?
    ↓ SÍ → detener

¿TAREA DISPONIBLE?
    ↓ SÍ → ejecutar

¿NO HAY TRABAJO?
    ↓ → finalizar / esperar

Regla principal

Si existe una tarea válida y el motor está en estado "AUTONOMOUS", debe
continuar.

---

6. WF-01 — ESTADO

Responsabilidad

Recuperar y actualizar el estado persistente.

Entrada

PROJECT_ID
SESSION_ID

Debe recuperar

PROJECT
SESSION
CURRENT_TASK
CURRENT_CHECKPOINT
PENDING_ACTION
QUEUE
MOTOR_STATUS

Salida

Estado completo necesario para tomar la siguiente decisión.

---

7. WF-02 — PLANIFICADOR

Responsabilidad

Determinar cuál debe ser el siguiente trabajo.

No ejecuta el trabajo.

Entrada

PROJECT
CURRENT_STATE
CURRENT_TASK
QUEUE
LAST_RESULT
PROJECT_MEMORY

Proceso

ANALIZAR OBJETIVO
↓
ANALIZAR ESTADO
↓
ANALIZAR TRABAJO REALIZADO
↓
IDENTIFICAR FALTANTES
↓
GENERAR / SELECCIONAR TAREAS
↓
ORDENAR PRIORIDADES
↓
SELECCIONAR SIGUIENTE TAREA

Salida

TASK_ID
TASK_TITLE
TASK_DESCRIPTION
TASK_TYPE
PRIORITY
DEPENDENCIES
EXECUTION_REQUIREMENTS

---

8. REGLA DEL PLANIFICADOR

El planificador no debe generar trabajo artificial para mantener el sistema
ocupado.

Debe generar trabajo únicamente cuando exista una necesidad real relacionada
con el objetivo del proyecto.

---

9. SUBTAREAS

Una tarea puede generar subtareas.

Ejemplo:

INVESTIGAR MERCADO
│
├── analizar competidores
├── analizar demanda
├── analizar keywords
├── analizar intención
└── validar oportunidad

Cada subtarea tendrá su propio "TASK_ID".

---

10. WF-03 — EJECUTOR

Responsabilidad

Realizar la tarea seleccionada.

Entrada

TASK
PROJECT
RELEVANT_MEMORY
TOOLS
AI_PROVIDER

Proceso

CARGAR CONTEXTO
↓
SELECCIONAR HERRAMIENTAS
↓
EJECUTAR IA
↓
UTILIZAR HERRAMIENTAS
↓
PRODUCIR RESULTADO

Salida

EXECUTION_ID
RESULT
AI_PROVIDER
AI_MODEL
TOOLS_USED
ERROR

---

11. IA DESACOPLADA

El ejecutor no debe depender de un proveedor concreto.

Debe trabajar conceptualmente con:

AI_PROVIDER
AI_MODEL
AI_REQUEST
AI_RESPONSE

Esto permitirá posteriormente utilizar:

Proveedor A
↓
Proveedor B
↓
Proveedor C

sin modificar el resto del motor.

---

12. FALLBACK DE IA

El fallback será una capacidad posterior.

No debe implementarse en el primer MVP hasta comprobar que el núcleo funciona.

Arquitectura prevista:

IA PRINCIPAL
   ↓ ERROR / LÍMITE
IA SECUNDARIA
   ↓ ERROR / LÍMITE
IA TERCIARIA
   ↓
ACTION_REQUIRED

---

13. HERRAMIENTAS

Las herramientas estarán desacopladas de la IA.

Ejemplos:

WEB_SEARCH
WEB_FETCH
GITHUB_READ
FILE_READ
DATA_PROCESSING
KEYWORD_ANALYSIS
OTHER

La IA decide qué necesita.

n8n ejecuta la herramienta.

---

14. WF-04 — VALIDADOR

Responsabilidad

Determinar si el resultado obtenido es suficientemente válido.

No debe limitarse a comprobar que la IA respondió.

Debe comprobar que la tarea realmente se realizó.

Entrada

TASK
EXECUTION
RESULT
EXPECTED_OUTPUT

Salida

VALIDATION_STATUS
VALIDATION_REASON
CONFIDENCE
FINDINGS
NEXT_ACTION

---

15. RESULTADOS DEL VALIDADOR

VALID
PARTIAL
INVALID
NEEDS_HUMAN

---

16. RESULTADO INVALID

Si el resultado es inválido:

RESULT
↓
INVALID
↓
RETRY?
├── SÍ → nueva ejecución
└── NO → BLOCKED / HUMAN

Nunca reintentar indefinidamente.

---

17. RESULTADO PARTIAL

Si el resultado es parcial:

PARTIAL
↓
¿FALTA INFORMACIÓN?
├── SÍ → crear tarea complementaria
└── NO → aceptar resultado

---

18. RESULTADO NEEDS_HUMAN

Si requiere criterio o acción humana:

NEEDS_HUMAN
↓
CREATE HUMAN_ACTION
↓
WAITING_HUMAN
↓
NOTIFY

---

19. WF-05 — CHECKPOINT

Responsabilidad

Guardar el estado recuperable después de cada paso importante.

Entrada

PROJECT
SESSION
TASK
EXECUTION
RESULT
CURRENT_STATE

Proceso

VALIDAR ESTADO
↓
GUARDAR CHECKPOINT
↓
ACTUALIZAR TASK
↓
ACTUALIZAR SESSION
↓
ACTUALIZAR PROJECT

Salida

CHECKPOINT_ID
CURRENT_STATE
NEXT_TASK_ID

---

20. REGLA DEL CHECKPOINT

Debe existir un checkpoint después de una ejecución que modifique
significativamente el estado del proyecto.

Como mínimo:

TASK_COMPLETED
TASK_FAILED
HUMAN_ACTION_CREATED
HUMAN_ACTION_COMPLETED
SESSION_PAUSED
SESSION_STOPPED

---

21. WF-07 — NOTIFICACIONES

Responsabilidad

Informar a David únicamente cuando exista información que requiera su
atención o sea relevante.

No debe hacer

No enviar un mensaje después de cada tarea simplemente para decir que una
tarea terminó.

Eso rompería el objetivo del motor autónomo.

---

22. NOTIFICACIÓN NORMAL

Cuando el sistema continúe trabajando:

NO NOTIFICAR

---

23. NOTIFICACIÓN HUMAN_ACTION

Cuando sea necesaria una acción:

ACTION_REQUIRED
↓
NOTIFICAR DAVID

Ejemplo conceptual:

⚠️ ACCIÓN NECESARIA

Proyecto: X

Necesito que:
[acción concreta]

Cuando termines responde:
HECHO

---

24. NOTIFICACIÓN ERROR

Solo notificar automáticamente cuando:

- el error sea no recuperable;
- se hayan agotado los reintentos;
- exista riesgo de pérdida de información;
- sea necesaria intervención humana.

---

25. CICLO COMPLETO

El ciclo autónomo será:

START
↓
LOAD STATE
↓
CONTROL
↓
PLAN
↓
EXECUTE
↓
VALIDATE
↓
CHECKPOINT
↓
CONTROL

No existe una instrucción humana entre estos pasos.

---

26. CONTINUIDAD

Si el resultado es válido:

VALID
↓
CHECKPOINT
↓
CONTROL
↓
PLAN
↓
NEXT TASK

---

27. CONTINUIDAD TRAS ERROR RECUPERABLE

ERROR
↓
RETRY
↓
EXECUTE
↓
VALIDATE

El número máximo de reintentos será configurable.

---

28. CONTINUIDAD TRAS ACCIÓN HUMANA

WAITING_HUMAN
↓
DAVID → HECHO
↓
VALIDATE ACTION
↓
CHECKPOINT
↓
CONTROL
↓
CONTINUE

---

29. PAUSA

Cuando David envía:

/pausar

el sistema:

CONTROL
↓
MOTOR_STATUS = PAUSED
↓
CHECKPOINT
↓
STOP

No pierde el trabajo.

---

30. REANUDACIÓN

Cuando David envía:

/continuar

el sistema:

LOAD STATE
↓
MOTOR_STATUS = AUTONOMOUS
↓
CONTROL
↓
NEXT TASK

---

31. DETENER

Cuando David envía:

/detener

el sistema:

CHECKPOINT
↓
MOTOR_STATUS = STOPPED
↓
STOP

No debe continuar automáticamente.

---

32. ESTADO

Cuando David envía:

/estado

el sistema debe devolver un resumen.

Debe incluir:

PROJECT
CURRENT TASK
PROGRESS
TASKS COMPLETED
TASKS PENDING
BLOCKED
HUMAN ACTION
MOTOR STATUS

---

33. PROGRESO

El progreso debe calcularse a partir de trabajo real.

Nunca inventar porcentajes.

Cuando exista un conjunto cerrado de tareas:

COMPLETED / TOTAL

Cuando el trabajo sea abierto:

PROGRESS = ESTIMATED

o:

PROGRESS = UNKNOWN

---

34. PROTECCIÓN CONTRA BUCLES

El motor debe detectar:

MISMA TAREA REPETIDA
MISMO ERROR REPETIDO
RESULTADOS SIN PROGRESO
CREACIÓN CONTINUA DE TAREAS

Si detecta un bucle:

LOOP_DETECTED
↓
PAUSE
↓
ACTION_REQUIRED

---

35. LÍMITES DE SEGURIDAD

El motor deberá tener límites configurables:

MAX_EXECUTIONS_PER_SESSION
MAX_RETRIES_PER_TASK
MAX_RUNTIME
MAX_TASK_GENERATION_DEPTH
MAX_CONSECUTIVE_FAILURES

---

36. MAX_RUNTIME

El sistema podrá tener un límite de seguridad.

No representa el final del proyecto.

Al alcanzarlo:

CHECKPOINT
↓
PAUSE
↓
NOTIFY

El trabajo podrá continuar posteriormente.

---

37. PROFUNDIDAD DE SUBTAREAS

Evitar una generación infinita:

TASK
 ↓
SUBTASK
   ↓
SUBTASK
     ↓
SUBTASK

Debe existir un límite configurable.

---

38. AISLAMIENTO DE PROYECTOS

El motor debe poder trabajar con diferentes proyectos.

Cada ejecución debe identificar:

PROJECT_ID
REPOSITORY
REPOSITORY_PATH

Nunca mezclar contexto entre proyectos.

---

39. MULTI-REPOSITORIO

El motor no debe estar construido específicamente para:

base-proyectos

Debe funcionar conceptualmente con cualquier repositorio autorizado.

Ejemplo:

davidsaenz20/base-proyectos
davidsaenz20/pensador-ideas
davidsaenz20/proyecto-futuro

---

40. DESCUBRIMIENTO DE REPOSITORIOS

En una fase posterior podrá existir:

GITHUB
↓
LIST REPOSITORIES
↓
REGISTER PROJECT

No es necesario para el primer MVP.

---

41. GITHUB

GitHub será inicialmente una herramienta de lectura.

El motor podrá:

READ REPOSITORY
READ FILE
SEARCH FILE
READ PROJECT DOCUMENTATION

No debe intentar escribir directamente en GitHub mediante las capacidades
disponibles actualmente.

Si una modificación requiere que David la realice manualmente:

ACTION_REQUIRED

---

42. REGLA DE ESCRITURA GITHUB

El motor no debe intentar actualizar directamente el repositorio cuando la
conexión disponible no tenga permisos de escritura.

Debe reconocer la limitación y continuar con el trabajo que sí pueda realizar.

---

43. INTERVENCIÓN HUMANA

La intervención humana es una excepción, no el flujo normal.

AUTONOMOUS
↓
WORK
↓
WORK
↓
WORK
↓
ACTION_REQUIRED
↓
HUMAN
↓
AUTONOMOUS

---

44. CONCEPTO DE "TRABAJO TERMINADO"

Una sesión no debe considerarse terminada simplemente porque una ejecución
haya finalizado.

Una ejecución:

TERMINA

pero el trabajo:

CONTINÚA

si existen tareas pendientes.

---

45. FIN REAL

El motor puede finalizar cuando:

NO HAY TAREAS VÁLIDAS

o:

OBJETIVO COMPLETADO

o:

ACTION_REQUIRED

o:

ERROR NO RECUPERABLE

---

46. ORDEN DE PRIORIDAD

El controlador deberá aplicar aproximadamente:

SEGURIDAD
↓
ACCIÓN HUMANA
↓
ERRORES
↓
DEPENDENCIAS
↓
PRIORIDAD
↓
SIGUIENTE TAREA

---

47. INDEPENDENCIA DE CHATGPT

El motor no dependerá de que ChatGPT permanezca abierto.

ChatGPT será únicamente un posible canal de interacción durante el diseño,
control o supervisión.

La ejecución autónoma real corresponderá a:

n8n
+
IA
+
MEMORIA
+
HERRAMIENTAS

---

48. CANAL DE CONTROL

Para el MVP:

TELEGRAM

Posteriormente:

WHATSAPP
WEB
OTROS

El canal no debe formar parte de la lógica central del motor.

---

49. CONTRATO ENTRE WORKFLOWS

Cada workflow debe devolver datos estructurados.

Ejemplo:

{
  "project_id": "...",
  "session_id": "...",
  "task_id": "...",
  "status": "...",
  "result": "...",
  "next_task_id": "...",
  "action_required": false
}

La estructura exacta podrá evolucionar durante la implementación.

---

50. REGLA DE MODULARIDAD

Cada workflow debe poder sustituirse sin reconstruir todo el sistema.

Ejemplo:

PLANIFICADOR

podrá evolucionar sin modificar:

CHECKPOINT

---

51. PRIMER MVP REAL

El primer MVP deberá demostrar solamente:

TELEGRAM
↓
CONTROL
↓
ESTADO
↓
PLANIFICADOR
↓
IA
↓
VALIDADOR
↓
CHECKPOINT
↓
SIGUIENTE TAREA

No se debe intentar construir todavía el sistema completo de producción.

---

52. PRUEBA PRINCIPAL

La prueba fundamental será:

David:
 /continuar

Después:

MOTOR
↓
TASK 1
↓
TASK 2
↓
TASK 3
↓
TASK 4
↓
...

sin que David tenga que enviar:

/continuar

entre cada tarea.

---

53. PRUEBA DE PARADA HUMANA

Durante una ejecución:

TASK
↓
ACTION_REQUIRED

El motor debe detenerse.

David realiza la acción.

Después:

HECHO

El motor debe recuperar el estado y continuar.

---

54. PRUEBA DE REINICIO

Simular:

MOTOR
↓
TASK 4
↓
CHECKPOINT
↓
STOP / RESTART
↓
LOAD CHECKPOINT
↓
TASK 5

El motor no debe volver a empezar desde TASK 1.

---

55. PRUEBA DE ERROR

Simular:

TASK
↓
ERROR
↓
RETRY

Si el error persiste:

MAX_RETRIES
↓
ACTION_REQUIRED / BLOCKED

---

56. PRUEBA DE BUCLE

Simular una tarea que produce repetidamente el mismo resultado.

El motor debe detectar falta de progreso y detener el ciclo.

---

57. CRITERIO DE ÉXITO DEL MVP

El MVP se considera técnicamente válido cuando puede:

- recibir "/continuar";
- recuperar el proyecto;
- seleccionar una tarea;
- ejecutarla;
- validar el resultado;
- guardar checkpoint;
- seleccionar otra tarea;
- continuar sin intervención;
- detenerse ante una acción humana;
- reanudar después de "HECHO";
- recuperar el estado después de reiniciarse.

---

58. FUERA DEL MVP

No implementar todavía:

WHATSAPP
MULTIUSUARIO
PANEL WEB
VPS
ESCALADO MASIVO
FALLBACK MULTI-IA
AGENTES COMPLEJOS
FACTURACIÓN
SEGURIDAD EMPRESARIAL

Primero demostrar el núcleo.

---

59. IMPLEMENTACIÓN POSTERIOR

Cuando el diseño esté terminado, la implementación seguirá:

1. INSTALAR / VERIFICAR N8N
2. CONFIGURAR TELEGRAM
3. CONFIGURAR IA
4. CONFIGURAR ALMACENAMIENTO
5. CREAR WF-06
6. CREAR WF-00
7. CREAR WF-01
8. CREAR WF-02
9. CREAR WF-03
10. CREAR WF-04
11. CREAR WF-05
12. CREAR WF-07
13. EJECUTAR PRUEBAS
14. CORREGIR
15. REPETIR

---

60. ESTADO DEL DOCUMENTO

Estado:

"READY_FOR_IMPLEMENTATION"

Siguiente documento:

"CONFIGURACION-IA-Y-HERRAMIENTAS.md"

Ese documento definirá los proveedores de IA, herramientas web, GitHub,
fallback, límites y criterios para seleccionar cada servicio.

La implementación real no comienza hasta que los documentos necesarios estén
suficientemente definidos.

