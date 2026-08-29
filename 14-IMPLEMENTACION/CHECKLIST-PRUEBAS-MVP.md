CHECKLIST DE PRUEBAS MVP

1. OBJETIVO

Este documento define las pruebas que debe superar el Motor Autónomo de IA
antes de considerarlo funcional.

No se considera válido porque n8n ejecute workflows correctamente.

Se considera válido cuando el sistema demuestra:

- continuidad;
- persistencia;
- recuperación;
- validación;
- control de errores;
- intervención humana;
- separación entre proyectos;
- comunicación con David.

---

2. REGLA GENERAL

Cada prueba tendrá:

TEST_ID
OBJETIVO
PRECONDICIONES
ENTRADA
RESULTADO_ESPERADO
RESULTADO_REAL
ESTADO
OBSERVACIONES

Estados:

NOT_TESTED
PASSED
FAILED
BLOCKED
RETEST_REQUIRED

---

3. TEST-001 — N8N FUNCIONA

Objetivo

Comprobar que n8n inicia correctamente.

Entrada

Arrancar n8n.

Resultado esperado

La interfaz está disponible y permite crear workflows.

Estado

"NOT_TESTED"

---

4. TEST-002 — TELEGRAM RECIBE MENSAJES

Objetivo

Comprobar la comunicación Telegram → n8n.

Entrada

Enviar:

/estado

Resultado esperado

n8n recibe el mensaje correctamente.

---

5. TEST-003 — TELEGRAM RESPONDE

Objetivo

Comprobar la comunicación bidireccional.

Entrada

/estado

Resultado esperado

Telegram recibe una respuesta.

---

6. TEST-004 — ESTADO VACÍO

Objetivo

Comprobar que el motor puede arrancar sin un proyecto activo.

Resultado esperado

Debe informar correctamente de que no existe trabajo activo.

No debe producir un error.

---

7. TEST-005 — CREACIÓN DE PROYECTO

Objetivo

Comprobar que puede registrarse un proyecto.

Resultado esperado

Se crea:

PROJECT
PROJECT_ID
STATUS

---

8. TEST-006 — CREACIÓN DE SESIÓN

Objetivo

Comprobar que un proyecto puede iniciar una sesión.

Resultado esperado

Se crea:

SESSION_ID
STATUS = RUNNING

---

9. TEST-007 — CREACIÓN DE TAREA

Objetivo

Comprobar que el proyecto puede contener tareas.

Resultado esperado

Se crea:

TASK_ID
STATUS = READY

---

10. TEST-008 — SELECCIÓN DE TAREA

Objetivo

Comprobar que el planificador selecciona una tarea ejecutable.

Resultado esperado

El planificador devuelve:

TASK_ID
TASK_DESCRIPTION
PRIORITY

---

11. TEST-009 — EJECUCIÓN IA

Objetivo

Comprobar que una tarea llega correctamente a la IA.

Entrada

Tarea sencilla.

Resultado esperado

Se recibe una respuesta.

---

12. TEST-010 — RESULTADO REGISTRADO

Objetivo

Comprobar que la respuesta de la IA se almacena.

Resultado esperado

Existe:

RESULT_ID
EXECUTION_ID
OUTPUT

---

13. TEST-011 — VALIDACIÓN POSITIVA

Objetivo

Comprobar que un resultado correcto es aceptado.

Resultado esperado

VALIDATION_STATUS = VALID

---

14. TEST-012 — VALIDACIÓN NEGATIVA

Objetivo

Comprobar que un resultado incorrecto es rechazado.

Resultado esperado

VALIDATION_STATUS = INVALID

No debe marcarse como completada la tarea.

---

15. TEST-013 — RESULTADO PARCIAL

Objetivo

Comprobar que el sistema distingue un resultado incompleto.

Resultado esperado

PARTIAL

y debe decidir si necesita una tarea complementaria.

---

16. TEST-014 — CHECKPOINT

Objetivo

Comprobar que se crea un checkpoint después de completar una tarea.

Resultado esperado

Existe:

CHECKPOINT_ID
CURRENT_TASK
NEXT_TASK

---

17. TEST-015 — CONTINUIDAD

Objetivo

Comprobar la autonomía básica.

Entrada

/continuar

Resultado esperado

TASK-001
↓
TASK-002
↓
TASK-003

sin que David tenga que enviar "/continuar" entre tareas.

---

18. TEST-016 — DOS TAREAS

Objetivo

Comprobar continuidad mínima.

Resultado esperado

Las dos tareas se completan automáticamente.

---

19. TEST-017 — TRES TAREAS

Objetivo

Comprobar continuidad prolongada.

Resultado esperado

Las tres tareas se ejecutan en secuencia.

---

20. TEST-018 — DEPENDENCIAS

Objetivo

Comprobar que una tarea dependiente no se ejecuta antes de tiempo.

Ejemplo:

TASK-003
depends_on:
TASK-001
TASK-002

Resultado esperado

TASK-003 espera hasta que sus dependencias estén completas.

---

21. TEST-019 — PRIORIDADES

Objetivo

Comprobar que el planificador respeta prioridades.

Resultado esperado

Una tarea "HIGH" puede ser seleccionada antes que una "LOW" cuando sus
dependencias lo permitan.

---

22. TEST-020 — PAUSA

Entrada

/pausar

Resultado esperado

MOTOR_STATUS = PAUSED

y se crea checkpoint.

---

23. TEST-021 — REANUDACIÓN

Entrada

/continuar

Resultado esperado

El motor recupera el checkpoint y continúa.

---

24. TEST-022 — DETENCIÓN

Entrada

/detener

Resultado esperado

MOTOR_STATUS = STOPPED

El motor no debe continuar automáticamente.

---

25. TEST-023 — CONSULTA DE ESTADO

Entrada

/estado

Resultado esperado

Debe mostrar como mínimo:

PROJECT
CURRENT_TASK
STATUS
COMPLETED
PENDING
BLOCKED

---

26. TEST-024 — ACCIÓN HUMANA

Objetivo

Comprobar que el motor sabe detenerse cuando necesita a David.

Resultado esperado

WAITING_HUMAN

---

27. TEST-025 — NOTIFICACIÓN HUMANA

Objetivo

Comprobar que David recibe la petición.

El mensaje debe explicar:

QUÉ NECESITO
POR QUÉ
QUÉ TIENE QUE HACER DAVID
CÓMO CONFIRMARLO

---

28. TEST-026 — HECHO

Entrada

HECHO

Resultado esperado

El motor identifica la acción pendiente.

---

29. TEST-027 — REANUDACIÓN HUMANA

Después de "HECHO":

VALIDATE ACTION
↓
CHECKPOINT
↓
CONTINUE

---

30. TEST-028 — ERROR RECUPERABLE

Provocar un error temporal.

Resultado esperado

El sistema reintenta.

---

31. TEST-029 — LÍMITE DE REINTENTOS

Provocar un error que nunca se soluciona.

Resultado esperado

Después de alcanzar:

MAX_RETRIES

el motor deja de intentar.

---

32. TEST-030 — ERROR NO RECUPERABLE

Provocar un error crítico.

Resultado esperado

BLOCKED

o:

ACTION_REQUIRED

según la naturaleza del error.

---

33. TEST-031 — ERROR REGISTRADO

Comprobar que el error contiene:

ERROR_ID
PROJECT_ID
TASK_ID
EXECUTION_ID
TYPE
MESSAGE
TIMESTAMP

---

34. TEST-032 — ERROR WORKFLOW

Configurar un workflow de error.

n8n permite utilizar un "Error Trigger" para recibir información de ejecuciones
fallidas y centralizar la gestión de errores.

Resultado esperado

Un error real en una ejecución automática activa el workflow de error.

---

35. TEST-033 — REINICIO DE N8N

Procedimiento

1. Ejecutar varias tareas.
2. Crear checkpoint.
3. Detener n8n.
4. Volver a iniciar n8n.

Resultado esperado

El sistema conserva el estado.

---

36. TEST-034 — RECUPERACIÓN

Si antes del apagado:

TASK-003 = COMPLETED
TASK-004 = READY

después del reinicio:

CURRENT_TASK = TASK-004

Nunca volver a TASK-001.

---

37. TEST-035 — NO DUPLICACIÓN

Reiniciar después de completar una tarea.

Resultado esperado

La tarea completada no vuelve a ejecutarse accidentalmente.

---

38. TEST-036 — DUPLICACIÓN DE TAREAS

Intentar crear dos tareas equivalentes.

Resultado esperado

El sistema detecta la posible duplicación.

---

39. TEST-037 — BUCLE

Crear deliberadamente una situación donde el planificador pueda repetir la
misma tarea.

Resultado esperado

El motor detecta:

NO PROGRESS

y detiene el ciclo.

---

40. TEST-038 — PROFUNDIDAD

Provocar generación repetida de subtareas.

Resultado esperado

Se respeta:

MAX_TASK_GENERATION_DEPTH

---

41. TEST-039 — TIEMPO MÁXIMO

Configurar un límite artificial de ejecución.

Resultado esperado

Al alcanzar el límite:

CHECKPOINT
↓
PAUSE

No se pierde el trabajo.

---

42. TEST-040 — PROYECTO A

Crear:

PROJECT-A

Ejecutar una tarea.

---

43. TEST-041 — PROYECTO B

Crear:

PROJECT-B

Ejecutar una tarea.

---

44. TEST-042 — AISLAMIENTO

Comprobar que:

PROJECT-A

no recibe memoria ni tareas de:

PROJECT-B

---

45. TEST-043 — GITHUB

Conectar GitHub.

Resultado esperado

El motor puede leer un repositorio autorizado.

---

46. TEST-044 — ARCHIVO GITHUB

Solicitar lectura de un archivo concreto.

Resultado esperado

El contenido se recupera correctamente.

---

47. TEST-045 — BASE-PROYECTOS

Registrar el repositorio:

davidsaenz20/base-proyectos

Comprobar que el motor puede utilizarlo como fuente de contexto.

---

48. TEST-046 — OTRO REPOSITORIO

Registrar el repositorio del pensador de ideas de negocio.

Debe tratarse como proyecto independiente.

---

49. TEST-047 — NUEVO REPOSITORIO

Crear posteriormente otro repositorio de prueba.

Resultado esperado

El motor puede registrarlo sin modificar su arquitectura central.

---

50. TEST-048 — WEB SEARCH

Añadir búsqueda web.

Resultado esperado

Una tarea puede:

SEARCH
↓
COLLECT
↓
RETURN SOURCES

---

51. TEST-049 — WEB FETCH

A partir de una URL:

FETCH
↓
CONTENT

---

52. TEST-050 — FUENTES

Comprobar que una investigación guarda:

URL
TITLE
DATE
SOURCE_TYPE

cuando esos datos estén disponibles.

---

53. TEST-051 — CONTRASTE

Solicitar una investigación sobre un hecho verificable.

Resultado esperado

El sistema puede contrastar más de una fuente cuando sea necesario.

---

54. TEST-052 — KEYWORDS

Ejecutar una tarea de:

KEYWORD_RESEARCH

Resultado esperado

Se generan datos estructurados de palabras clave según las herramientas
disponibles.

---

55. TEST-053 — INTENCIÓN

Analizar varias consultas.

Resultado esperado

El sistema clasifica la intención de búsqueda de forma razonada y distingue
cuando una consulta tiene características mixtas.

---

56. TEST-054 — INVESTIGACIÓN DE NEGOCIO

Introducir una idea de negocio.

El motor debe poder investigar:

PROBLEMA
TARGET
DEMANDA
COMPETENCIA
KEYWORDS
INTENCIÓN
MONETIZACIÓN
RIESGOS

---

57. TEST-055 — HECHO VS HIPÓTESIS

Comprobar que el sistema distingue:

CONFIRMED
PROBABLE
HYPOTHESIS
UNKNOWN

No debe presentar hipótesis como hechos.

---

58. TEST-056 — CONTEXTO

Comprobar que una tarea recibe únicamente el contexto relevante.

No debe enviarse todo el proyecto indiscriminadamente a cada llamada.

---

59. TEST-057 — IA NO DISPONIBLE

Simular que el proveedor principal no está disponible.

En el MVP puede detenerse.

Posteriormente deberá utilizar fallback.

---

60. TEST-058 — FALLBACK IA

Una vez implementado el fallback:

PRIMARY
↓ ERROR
SECONDARY

Resultado esperado

La tarea continúa sin perder contexto.

---

61. TEST-059 — CUOTA IA

Simular agotamiento de cuota.

Resultado esperado

El sistema detecta:

LIMIT

y selecciona una alternativa válida o solicita intervención.

---

62. TEST-060 — COSTE

Comprobar que las llamadas de IA pueden registrarse cuando el proveedor
proporciona información de uso.

---

63. TEST-061 — CREDENCIALES

Comprobar que ninguna credencial aparece en:

GitHub
.md
logs públicos
mensajes Telegram

---

64. TEST-062 — DATOS SENSIBLES

Comprobar que los resultados no exponen accidentalmente:

API_KEYS
TOKENS
PASSWORDS
PRIVATE_KEYS

---

65. TEST-063 — BACKUP

Realizar una copia de seguridad.

Resultado esperado

La información necesaria puede recuperarse.

---

66. TEST-064 — RESTORE

Eliminar o simular pérdida de una parte del entorno de prueba.

Restaurar.

Resultado esperado

El motor vuelve a un estado funcional.

---

67. TEST-065 — EJECUCIÓN PROLONGADA

Permitir que el motor complete varias tareas consecutivas.

Comprobar:

RAM
CPU
ERRORES
TIEMPO
CONSUMO

---

68. TEST-066 — EJECUCIONES

Revisar las ejecuciones generadas por n8n.

n8n permite consultar ejecuciones y filtrar por estado, incluyendo "Failed",
"Running", "Success" y "Waiting".

Comprobar que la trazabilidad es suficiente para diagnosticar problemas.

---

69. TEST-067 — RETRY MANUAL

Provocar una ejecución fallida.

Comprobar que puede revisarse y repetirse desde el historial de ejecuciones
cuando corresponda.

---

70. TEST-068 — STOP AND ERROR

Utilizar deliberadamente un error controlado.

n8n dispone del nodo "Stop And Error" para provocar fallos controlados y
enviarlos al workflow de errores.

---

71. TEST-069 — PRODUCCIÓN

No activar el motor autónomo completo hasta que las pruebas anteriores
esenciales hayan sido superadas.

Las ejecuciones manuales y las automáticas de producción tienen
comportamientos distintos en n8n.

---

72. TEST-070 — PRUEBA AUTÓNOMA REAL

Esta es la prueba principal.

David envía:

/continuar

Después no envía ningún mensaje.

El sistema debe:

PLAN
↓
TASK
↓
EXECUTE
↓
VALIDATE
↓
CHECKPOINT
↓
NEXT TASK
↓
EXECUTE
↓
VALIDATE
↓
CHECKPOINT
↓
...

---

73. TEST-071 — INTERVENCIÓN HUMANA DURANTE AUTONOMÍA

Durante la prueba autónoma debe producirse deliberadamente una situación
que requiera a David.

Resultado esperado:

AUTONOMOUS
↓
ACTION_REQUIRED
↓
NOTIFY
↓
WAIT

---

74. TEST-072 — CONTINUACIÓN DESPUÉS DE INTERVENCIÓN

David responde:

HECHO

Resultado:

VALIDATE
↓
CHECKPOINT
↓
AUTONOMOUS
↓
CONTINUE

---

75. TEST-073 — REINICIO DURANTE AUTONOMÍA

Detener n8n durante una sesión autónoma.

Reiniciar.

Resultado esperado:

RECOVER
↓
CHECKPOINT
↓
CONTINUE

---

76. TEST-074 — ERROR DURANTE AUTONOMÍA

Provocar un error temporal.

Resultado:

RETRY

Si no se recupera:

BLOCKED / ACTION_REQUIRED

---

77. TEST-075 — SIN TRABAJO

Cuando no existan tareas válidas:

NO_WORK

Resultado:

SESSION_COMPLETED

o:

WAITING

según el objetivo del proyecto.

---

78. TEST-076 — OBJETIVO COMPLETADO

Cuando el validador determine que el objetivo se ha cumplido:

OBJECTIVE_COMPLETED

El motor debe dejar de generar trabajo artificial.

---

79. TEST-077 — NO TRABAJO ARTIFICIAL

Comprobar que el planificador no crea tareas simplemente para mantener el
motor ejecutándose.

---

80. TEST-078 — NO BUCLE DE NOTIFICACIONES

Provocar una acción humana.

El sistema debe enviar la notificación necesaria, pero no repetirla
continuamente sin motivo.

---

81. TEST-079 — NO BUCLE DE ERRORES

Provocar un error persistente.

El sistema no debe generar:

ERROR
↓
RETRY
↓
ERROR
↓
RETRY

indefinidamente.

---

82. TEST-080 — NO MEZCLA DE MEMORIA

Ejecutar dos proyectos simultáneamente.

Comprobar que las respuestas de IA no mezclan sus contextos.

---

83. TEST-081 — CAMBIO DE IA

Cuando exista más de un proveedor:

TASK
↓
AI-A

y posteriormente:

TASK
↓
AI-B

El resultado debe seguir utilizando el mismo contexto del proyecto.

---

84. TEST-082 — CAMBIO DE HERRAMIENTA

Sustituir una herramienta de búsqueda por otra.

El workflow central no debe necesitar reconstruirse.

---

85. TEST-083 — MIGRACIÓN

En una fase posterior:

PC
↓
BACKUP
↓
VPS

Comprobar que el estado puede recuperarse.

Este test no pertenece al MVP inicial.

---

86. TEST-084 — TELEGRAM COMO CANAL

Comprobar que Telegram únicamente comunica con el motor y no contiene la
lógica principal.

---

87. TEST-085 — INDEPENDENCIA DEL CHAT

Cerrar ChatGPT.

El motor debe continuar funcionando.

ChatGPT no puede ser una dependencia necesaria para la ejecución autónoma.

---

88. TEST-086 — PARADA SEGURA

Enviar:

/detener

durante una ejecución.

Resultado esperado:

CHECKPOINT
↓
STOPPED

No perder el estado.

---

89. TEST-087 — RECUPERACIÓN FINAL

Después de una parada segura:

/continuar

Resultado:

LOAD CHECKPOINT
↓
CONTINUE

---

90. CRITERIO DE APROBACIÓN

El MVP no se considerará aprobado si falla cualquiera de estos grupos:

A — CONTINUIDAD
B — PERSISTENCIA
C — RECUPERACIÓN
D — VALIDACIÓN
E — INTERVENCIÓN HUMANA
F — ERRORES
G — AISLAMIENTO

---

91. PRUEBAS CRÍTICAS

Las pruebas mínimas obligatorias antes de producción son:

TEST-015
TEST-020
TEST-021
TEST-024
TEST-026
TEST-029
TEST-033
TEST-034
TEST-037
TEST-042
TEST-047
TEST-070
TEST-071
TEST-072
TEST-073
TEST-075
TEST-076
TEST-080
TEST-085
TEST-086
TEST-087

---

92. REGISTRO

Durante la implementación, cada prueba se marcará:

[ ] NOT_TESTED
[ ] PASSED
[ ] FAILED
[ ] BLOCKED
[ ] RETEST_REQUIRED

No marcar una prueba como PASSED sin haberla ejecutado realmente.

---

93. REGLA DE EVIDENCIA

Para cada prueba crítica deberá existir una evidencia mínima:

CAPTURA
LOG
EXECUTION_ID
RESULTADO

cuando sea útil.

---

94. CORRECCIÓN

Cuando una prueba falle:

FAILED
↓
IDENTIFY CAUSE
↓
FIX
↓
RETEST

No saltar la prueba simplemente porque el resultado parezca suficientemente
bueno.

---

95. REGRESIÓN

Después de modificar un componente importante:

RETEST

las pruebas críticas relacionadas.

---

96. CRITERIO FINAL

El sistema estará listo para comenzar un proyecto real cuando:

CONTINÚA SOLO
+
GUARDA ESTADO
+
SE RECUPERA
+
VALIDA
+
SABE PARAR
+
PIDE AYUDA
+
CONTINÚA DESPUÉS DE AYUDA
+
NO CREA BUCLES
+
NO MEZCLA PROYECTOS

---

97. ESTADO DEL DOCUMENTO

"READY_FOR_TESTING"

---

98. SIGUIENTE PASO

Una vez copiado este documento, la documentación del MVP queda prácticamente
cerrada.

El siguiente trabajo será revisar la estructura completa del repositorio y
comprobar que no haya documentación duplicada o contradictoria antes de
empezar la implementación.

Después de esa revisión:

DOCUMENTACIÓN
↓
REVISIÓN
↓
PC
↓
N8N
↓
MVP
↓
PRUEBAS


