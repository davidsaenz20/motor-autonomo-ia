MOTOR DE EJECUCIÓN n8n

1. OBJETIVO

Definir la arquitectura de n8n que permitirá convertir el motor autónomo en
un sistema de ejecución real.

n8n será el orquestador.

No será el cerebro del sistema.

---

2. ARQUITECTURA GENERAL

ORDEN
↓
n8n
↓
PLANIFICADOR
↓
AGENTE IA
↓
HERRAMIENTAS
↓
VALIDACIÓN
↓
MEMORIA
↓
SIGUIENTE TAREA
↓
n8n
↓
CONTINUAR

---

3. PRINCIPIO FUNDAMENTAL

Una ejecución no debe terminar simplemente porque una tarea concreta haya
terminado.

Cuando una tarea termina:

1. guardar resultado;
2. validar;
3. actualizar estado;
4. buscar siguiente tarea válida;
5. continuar.

---

4. TRIGGER

El motor podrá iniciarse mediante:

- Telegram;
- WhatsApp;
- webhook;
- ejecución programada;
- evento interno;
- otro disparador futuro.

El primer diseño deberá priorizar un mecanismo sencillo de prueba.

---

5. COMANDOS

El sistema podrá interpretar órdenes como:

"continúa"

"sigue trabajando"

"investiga esto"

"reanuda"

"pausa"

"estado"

"detén el motor"

La implementación concreta del lenguaje se definirá durante la construcción.

---

6. ACTIVACIÓN

Cuando David ordene continuar:

1. identificar proyecto;
2. recuperar repositorio;
3. recuperar estado;
4. recuperar cola;
5. recuperar reglas;
6. localizar siguiente trabajo;
7. ejecutar.

---

7. SESIÓN DE TRABAJO

Cada ejecución autónoma tendrá un identificador:

WORK_SESSION_ID.

Debe permitir relacionar:

- tareas;
- resultados;
- errores;
- decisiones;
- modelos;
- herramientas;
- checkpoints.

---

8. CICLO

El ciclo conceptual será:

START
↓
LOAD_STATE
↓
SELECT_TASK
↓
EXECUTE_TASK
↓
VALIDATE
↓
SAVE_RESULT
↓
DISCOVER_NEXT_WORK
↓
SELECT_NEXT_TASK
↓
EXECUTE

Mientras pueda continuar:

REPETIR.

---

9. NO INTERRUMPIR POR TAREA TERMINADA

Una tarea terminada no constituye motivo para enviar una respuesta a David.

Debe utilizarse como entrada para seleccionar la siguiente tarea.

---

10. CONTINUIDAD

El motor debe interpretar:

TASK_COMPLETED

como:

READY_FOR_NEXT_TASK

y no como:

SESSION_COMPLETED.

---

11. FINAL DE CICLO

El ciclo solamente deberá detenerse si:

- requiere intervención humana;
- existe un bloqueo real;
- existe un error no recuperable;
- se alcanza un límite de seguridad;
- David ordena detener;
- no existe trabajo válido restante.

---

12. COLA

La cola deberá distinguir estados:

PENDING
READY
RUNNING
BLOCKED
WAITING_HUMAN
COMPLETED
FAILED
CANCELLED

---

13. PRIORIZACIÓN

El planificador deberá seleccionar la tarea más adecuada considerando:

- dependencias;
- prioridad;
- impacto;
- urgencia;
- riesgo;
- coste;
- disponibilidad de herramientas;
- disponibilidad de modelos.

---

14. TAREAS INDEPENDIENTES

Si una tarea queda bloqueada por David pero existen otras tareas autónomas:

continuar con las tareas independientes.

---

15. TAREAS DEPENDIENTES

Si una tarea necesita el resultado de otra:

no ejecutarla hasta que la dependencia esté completada.

---

16. CHECKPOINT

El motor deberá guardar checkpoints en puntos importantes.

Como mínimo:

- inicio de tarea;
- resultado;
- error;
- intervención;
- cambio de modelo;
- cambio de proyecto;
- pausa.

---

17. RECUPERACIÓN

Si n8n se detiene:

al reiniciar:

LOAD_LAST_CHECKPOINT
↓
DETECT_STATE
↓
CONTINUE.

No empezar desde cero.

---

18. IDEMPOTENCIA

El motor deberá evitar ejecutar dos veces accidentalmente una operación que
no pueda repetirse de forma segura.

Cada tarea deberá tener un identificador único.

---

19. TASK_ID

Toda tarea deberá disponer de:

TASK_ID.

Esto permitirá detectar:

- tarea ya ejecutada;
- tarea en ejecución;
- tarea pendiente;
- tarea duplicada.

---

20. TIMEOUT

Cada operación podrá tener un timeout apropiado.

Un timeout no significa automáticamente:

TASK_FAILED.

Primero deberá determinarse si:

- la operación continúa;
- puede recuperarse;
- puede repetirse;
- debe cambiar de herramienta.

---

21. RETRIES

Los reintentos deberán estar controlados.

No realizar:

RETRY INFINITO.

Cada herramienta deberá definir una política apropiada.

---

22. BACKOFF

Ante errores temporales:

esperar progresivamente antes de repetir.

Objetivo:

evitar sobrecargar APIs y evitar bucles.

---

23. CIRCUIT BREAKER

Si una herramienta o proveedor falla repetidamente:

marcar temporalmente:

UNAVAILABLE.

Utilizar alternativa cuando exista.

---

24. BUCLE CONTROLADO

Aunque el objetivo sea trabajo autónomo continuo, el sistema debe disponer
de controles técnicos contra:

- bucles infinitos;
- tareas duplicadas;
- errores repetidos;
- consumo excesivo;
- llamadas API excesivas.

---

25. CONTINUIDAD AUTÓNOMA

El límite técnico no debe confundirse con una interrupción lógica.

Si n8n necesita iniciar otra ejecución para continuar:

deberá hacerlo automáticamente siempre que sea seguro.

---

26. LÍMITE DE EJECUCIÓN

El sistema podrá utilizar límites de seguridad como:

- máximo de iteraciones por ciclo;
- máximo de tiempo por ejecución;
- máximo de coste;
- máximo de errores consecutivos.

Estos límites deberán permitir reanudar automáticamente cuando proceda.

---

27. ROTACIÓN DE EJECUCIONES

Ejemplo:

EJECUCIÓN 001
→ trabajo

checkpoint

EJECUCIÓN 002
→ continúa

checkpoint

EJECUCIÓN 003
→ continúa.

Desde el punto de vista del proyecto:

UNA SESIÓN CONTINUA.

---

28. ESTADO GLOBAL

El motor deberá conocer:

- sesión;
- repositorio;
- proyecto;
- tarea actual;
- cola;
- progreso;
- bloqueos;
- intervención;
- último resultado;
- siguiente tarea.

---

29. ESTADO DE PARADA

Estados principales:

RUNNING
PAUSED
WAITING_HUMAN
BLOCKED
ERROR
STOPPED
COMPLETED

---

30. PAUSA

Si David ordena:

PAUSA

el motor deberá:

1. terminar operación segura actual cuando corresponda;
2. guardar checkpoint;
3. detener nuevas tareas;
4. conservar estado.

---

31. REANUDACIÓN

Si David ordena:

CONTINUAR

el motor deberá:

1. recuperar checkpoint;
2. comprobar estado;
3. seleccionar siguiente tarea;
4. continuar.

---

32. DETENCIÓN

Si David ordena:

DETENER

el motor deberá detener el ciclo.

No eliminar memoria ni resultados.

---

33. ERROR RECUPERABLE

Si existe una recuperación posible:

RECUPERAR
↓
VALIDAR
↓
CONTINUAR.

---

34. ERROR NO RECUPERABLE

Si no existe recuperación:

ERROR
↓
GUARDAR ESTADO
↓
NOTIFICAR.

No continuar realizando operaciones que puedan empeorar el problema.

---

35. INTERVENCIÓN HUMANA

Si:

ACTION_REQUIRED = true

el motor deberá:

1. guardar checkpoint;
2. registrar acción;
3. notificar;
4. pausar tareas dependientes;
5. continuar tareas independientes si procede.

---

36. FINALIZACIÓN DEL PROYECTO

Si no existe ninguna tarea válida:

PROJECT_IDLE.

No debe inventar trabajo simplemente para mantenerse ocupado.

Podrá:

- revisar pendientes;
- buscar oportunidades si está permitido;
- esperar una nueva orden.

---

37. DESCUBRIMIENTO DE TRABAJO

Después de cada tarea:

CHECK:

¿HA APARECIDO NUEVO TRABAJO?

Si:

SÍ → registrar.

NO → buscar siguiente tarea pendiente.

---

38. PLANIFICADOR

El planificador será responsable de:

- seleccionar;
- priorizar;
- bloquear;
- desbloquear;
- crear;
- completar;
- cancelar.

No será responsable de ejecutar directamente herramientas externas.

---

39. AGENTE

El agente será responsable de:

- comprender;
- investigar;
- razonar;
- ejecutar trabajo intelectual;
- utilizar herramientas;
- validar resultados.

---

40. n8n

n8n será responsable de:

- triggers;
- routing;
- llamadas;
- estados;
- ciclos;
- retries;
- timeouts;
- checkpoints;
- notificaciones;
- coordinación.

---

41. MEMORIA

La memoria deberá conservar:

- contexto;
- resultados;
- decisiones;
- fuentes;
- errores;
- checkpoints.

El motor deberá recuperar solamente lo relevante.

---

42. COMUNICACIÓN

Las notificaciones deberán producirse únicamente por eventos relevantes.

Ejemplos:

HUMAN_ACTION_REQUIRED
CRITICAL_ERROR
USER_REQUESTED_STATUS
PROJECT_COMPLETED
SYSTEM_LIMIT_REACHED

No notificar cada tarea terminada.

---

43. PROGRESO

El sistema podrá calcular:

TASKS_COMPLETED
/
TASKS_TOTAL

Pero el porcentaje será aproximado cuando el número total de tareas todavía
no esté definido.

---

44. PROGRESO DINÁMICO

Si aparecen nuevas tareas:

el porcentaje podrá cambiar.

No debe interpretarse como una medida absoluta del trabajo restante.

---

45. REGISTRO

Cada ciclo deberá poder registrar:

- timestamp;
- session_id;
- task_id;
- proyecto;
- modelo;
- herramientas;
- resultado;
- validación;
- errores;
- siguiente tarea.

---

46. SEGURIDAD

n8n no deberá almacenar secretos en nodos de texto o documentos públicos.

Las credenciales deberán utilizar el sistema seguro correspondiente.

---

47. ESCALABILIDAD

La arquitectura deberá permitir inicialmente:

1 proyecto
→ varias tareas

y posteriormente:

varios proyectos
→ varios repositorios
→ múltiples sesiones.

---

48. MULTI-REPOSITORIO

El motor no deberá estar limitado a:

BASE-PROYECTOS.

Podrá trabajar con cualquier repositorio autorizado de "davidsaenz20".

---

49. CONCURRENCIA

La ejecución paralela solo deberá utilizarse cuando:

- las tareas sean independientes;
- no exista riesgo de conflicto;
- los límites lo permitan.

Por defecto:

priorizar simplicidad y consistencia.

---

50. PRINCIPIO DE CONTINUIDAD

La pregunta principal después de cada operación será:

"¿Existe trabajo autónomo válido que pueda ejecutar ahora?"

Si:

SÍ → ejecutar.

NO → determinar el motivo.

---

51. REGLA CONTRA LA PARADA ARTIFICIAL

No terminar una sesión porque:

- se terminó una tarea;
- se terminó un subtrabajo;
- se obtuvo un resultado;
- se creó una nueva tarea;
- se completó una investigación.

La sesión continúa mientras exista trabajo válido.

---

52. REGLA CONTRA EL TRABAJO INÚTIL

No continuar únicamente para aparentar actividad.

Si no existe trabajo útil:

PROJECT_IDLE.

---

53. ACCIÓN HUMANA COMO INTERRUPTOR

La intervención humana será uno de los principales interruptores del ciclo.

Cuando sea necesaria:

RUNNING
→
WAITING_HUMAN.

Después:

WAITING_HUMAN
→
VALIDATE
→
RUNNING.

---

54. LÍMITE DE SEGURIDAD

El motor podrá detener temporalmente una ejecución si alcanza un límite
técnico.

Debe guardar:

- estado;
- checkpoint;
- motivo.

Si puede continuar mediante una nueva ejecución segura:

reanudar automáticamente.

---

55. ARQUITECTURA FINAL

TRIGGER
↓
LOAD CONTEXT
↓
PLANIFICADOR
↓
SELECT TASK
↓
AGENTE
↓
HERRAMIENTAS
↓
VALIDADOR
↓
MEMORIA
↓
CHECKPOINT
↓
PLANIFICADOR
↓
¿SIGUIENTE TAREA?
├── SÍ → CONTINUAR
└── NO → EVALUAR BLOQUEO / HUMANO / IDLE

---

56. PRÓXIMO TRABAJO

El siguiente documento deberá definir:

SISTEMA DE CONTROL POR TELEGRAM

Será el primer canal recomendado para controlar el motor desde el móvil,
permitiendo activar, pausar, continuar, consultar estado y recibir avisos
cuando sea necesaria una acción humana.

