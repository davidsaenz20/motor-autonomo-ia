MVP TÉCNICO

1. OBJETIVO

Construir la primera versión funcional del Motor Autónomo de IA.

El MVP debe demostrar que el sistema puede recibir una orden, recuperar su
estado, ejecutar trabajo, validar el resultado, guardar el progreso y
continuar automáticamente con la siguiente tarea.

---

2. OBJETIVO PRINCIPAL

David debe poder enviar:

CONTINÚA

y el sistema deberá:

1. identificar el proyecto;
2. recuperar el estado;
3. seleccionar trabajo;
4. ejecutar;
5. validar;
6. guardar;
7. generar la siguiente tarea;
8. continuar.

No deberá necesitar otro mensaje de David después de cada tarea.

---

3. ARQUITECTURA MVP

TELEGRAM
   ↓
n8n — ENTRADA
   ↓
CONTROLADOR
   ↓
ESTADO
   ↓
PLANIFICADOR
   ↓
AGENTE IA
   ↓
HERRAMIENTAS
   ↓
RESULTADO
   ↓
VALIDADOR
   ↓
ESTADO / CHECKPOINT
   ↓
PLANIFICADOR
   ↓
SIGUIENTE TAREA

---

4. COMPONENTES

El MVP estará formado inicialmente por:

- Telegram;
- n8n;
- estado persistente;
- cola de tareas;
- planificador;
- proveedor de IA;
- validador;
- sistema de checkpoints;
- controlador de intervención humana.

---

5. NO IMPLEMENTAR TODAVÍA

Quedan fuera del primer MVP:

- WhatsApp;
- panel web;
- múltiples usuarios;
- infraestructura compleja;
- múltiples servidores;
- escalabilidad avanzada;
- múltiples proveedores simultáneos;
- automatizaciones empresariales.

Primero debe demostrarse que el núcleo funciona.

---

6. WORKFLOW 1 — ENTRADA

Nombre:

"WF-INCOMING-COMMAND"

Responsabilidad:

recibir mensajes de Telegram y convertirlos en comandos normalizados.

---

7. COMANDOS INICIALES

Implementar:

"/continuar"

"/pausar"

"/detener"

"/estado"

---

8. COMANDO CONTINUAR

Flujo:

Telegram
↓
validar usuario
↓
identificar proyecto
↓
cargar estado
↓
comprobar bloqueos
↓
iniciar/reanudar motor

---

9. COMANDO PAUSAR

Debe:

- impedir nuevas tareas;
- conservar estado;
- mantener información de sesión;
- permitir reanudación posterior.

No borrar trabajo pendiente.

---

10. COMANDO DETENER

Debe:

- detener nuevas ejecuciones;
- guardar checkpoint;
- conservar cola;
- registrar motivo.

---

11. COMANDO ESTADO

Debe devolver información resumida:

- proyecto;
- tarea actual;
- estado;
- progreso;
- bloqueo si existe.

No mostrar información innecesaria.

---

12. WORKFLOW 2 — CONTROLADOR

Nombre:

"WF-MOTOR-CONTROL"

Responsabilidad:

determinar qué debe hacer el motor.

Entradas:

- comando;
- proyecto;
- estado.

Salidas:

- START;
- CONTINUE;
- PAUSE;
- STOP;
- WAITING_HUMAN;
- ERROR.

---

13. ESTADOS DEL MOTOR

Estados mínimos:

"IDLE"

"RUNNING"

"PAUSED"

"WAITING_HUMAN"

"ERROR"

"STOPPED"

---

14. IDLE

No existe una ejecución activa.

Puede iniciarse una nueva.

---

15. RUNNING

El motor está ejecutando trabajo.

---

16. PAUSED

No deben iniciarse nuevas tareas.

---

17. WAITING_HUMAN

Existe una dependencia que requiere a David.

No ejecutar tareas dependientes.

---

18. ERROR

Existe un error pendiente de resolver o recuperar.

---

19. STOPPED

El motor ha sido detenido deliberadamente.

---

20. MODELO DE PROYECTO

Cada sesión deberá estar asociada a:

"PROJECT_ID"

"PROJECT_NAME"

"REPOSITORY"

"SESSION_ID"

---

21. MODELO DE TAREA

Cada tarea deberá contener conceptualmente:

TASK_ID
PROJECT_ID
TITLE
DESCRIPTION
STATUS
PRIORITY
DEPENDENCIES
CREATED_AT
STARTED_AT
COMPLETED_AT
RESULT
VALIDATION

---

22. ESTADOS DE TAREA

Mínimos:

"PENDING"

"RUNNING"

"COMPLETED"

"FAILED"

"BLOCKED"

"WAITING_HUMAN"

"CANCELLED"

---

23. WORKFLOW 3 — PLANIFICADOR

Nombre:

"WF-TASK-PLANNER"

Responsabilidad:

determinar la siguiente tarea válida.

Proceso:

CARGAR ESTADO
↓
CARGAR COLA
↓
ELIMINAR TAREAS NO EJECUTABLES
↓
COMPROBAR DEPENDENCIAS
↓
PRIORIZAR
↓
SELECCIONAR

---

24. REGLA DEL PLANIFICADOR

No crear una tarea nueva si existe una tarea pendiente válida que pueda
ejecutarse.

---

25. AUSENCIA DE TAREAS

Si no existe trabajo válido:

pedir al agente que determine si debe:

- investigar;
- crear siguiente tarea;
- finalizar proyecto;
- esperar;
- requerir intervención.

---

26. WORKFLOW 4 — AGENTE

Nombre:

"WF-AI-EXECUTOR"

Responsabilidad:

ejecutar la tarea seleccionada mediante el proveedor de IA disponible.

---

27. ENTRADA DEL AGENTE

El agente recibirá:

- proyecto;
- objetivo;
- tarea;
- contexto;
- reglas relevantes;
- resultados anteriores;
- herramientas disponibles.

---

28. SALIDA DEL AGENTE

Deberá producir:

RESULT
TASK_STATUS
FINDINGS
NEXT_WORK
ACTION_REQUIRED
CONFIDENCE

---

29. NO INVENTAR EJECUCIONES

El agente nunca deberá afirmar que ha realizado una operación externa que
realmente no pudo ejecutar.

Ejemplo:

Si no puede escribir en GitHub:

"WRITE_NOT_AVAILABLE"

No:

"FILE_UPDATED".

---

30. HERRAMIENTAS

Las herramientas se añadirán progresivamente.

Primera herramienta prioritaria:

LECTURA DE GITHUB.

Después:

INVESTIGACIÓN WEB.

Posteriormente se añadirán otras.

---

31. GITHUB

El MVP deberá poder conocer:

- repositorios autorizados;
- estructura;
- archivos;
- documentación;
- estado del proyecto.

La escritura se considerará una capacidad independiente.

---

32. MULTI-REPOSITORIO

El motor no estará limitado a un repositorio concreto.

Deberá trabajar mediante:

"REPOSITORY_ID"

o equivalente.

---

33. DESCUBRIMIENTO DE REPOSITORIOS

El sistema deberá poder incorporar nuevos repositorios de David sin tener
que rediseñar el motor.

---

34. MEMORIA

La memoria deberá separar:

PROJECT_MEMORY
SESSION_MEMORY
TASK_MEMORY
SYSTEM_MEMORY

---

35. MEMORIA DEL PROYECTO

Contiene:

- objetivos;
- decisiones;
- descubrimientos;
- resultados;
- problemas;
- contexto permanente.

---

36. MEMORIA DE SESIÓN

Contiene:

- tareas ejecutadas;
- estado actual;
- decisiones temporales;
- checkpoint.

---

37. MEMORIA DE TAREA

Contiene:

- entrada;
- proceso;
- resultado;
- validación;
- siguiente trabajo.

---

38. CHECKPOINT

Después de cada tarea válida:

RESULTADO
↓
VALIDAR
↓
GUARDAR
↓
CHECKPOINT

---

39. RECUPERACIÓN

Si n8n se reinicia:

LOAD CHECKPOINT
↓
VERIFY STATE
↓
FIND CURRENT TASK
↓
CONTINUE

---

40. AUTONOMÍA

La autonomía se implementará mediante encadenamiento de ejecuciones.

No depender de una única ejecución infinita.

---

41. CICLO AUTÓNOMO

START
↓
SELECT TASK
↓
EXECUTE
↓
VALIDATE
↓
SAVE
↓
CHECK HUMAN ACTION
↓
SELECT NEXT TASK
↓
EXECUTE

---

42. FINALIZACIÓN DE UNA EJECUCIÓN

Una ejecución de n8n puede terminar.

Eso no significa que el proyecto haya terminado.

El estado persistente permitirá que la siguiente ejecución continúe.

---

43. CONTINUACIÓN

Después de completar una tarea:

TASK_COMPLETED
↓
SAVE
↓
NEXT_TASK?

Si:

"YES"

continuar.

Si:

"NO"

evaluar generación de trabajo.

---

44. GENERACIÓN DE TRABAJO

El agente podrá proponer nuevas tareas cuando no existan tareas pendientes.

Las propuestas deberán validarse antes de incorporarse a la cola.

---

45. VALIDACIÓN DE NUEVAS TAREAS

Una tarea nueva deberá comprobar:

- utilidad;
- relación con el objetivo;
- ausencia de duplicados;
- dependencias;
- riesgo.

---

46. WORKFLOW 5 — VALIDACIÓN

Nombre:

"WF-RESULT-VALIDATOR"

Responsabilidad:

comprobar el resultado producido.

---

47. RESULTADOS DE VALIDACIÓN

Mínimos:

"VALID"

"PARTIAL"

"INVALID"

"NEEDS_HUMAN"

---

48. VALID

El resultado puede utilizarse.

---

49. PARTIAL

El resultado puede conservarse pero requiere trabajo adicional.

---

50. INVALID

El resultado debe descartarse o rehacerse.

---

51. NEEDS_HUMAN

La validación determina que David debe intervenir.

---

52. WORKFLOW 6 — INTERVENCIÓN

Nombre:

"WF-HUMAN-ACTION"

Responsabilidad:

detectar, registrar y comunicar acciones necesarias para David.

---

53. ACTION_REQUIRED

Debe contener:

PROJECT
TASK
ACTION
REASON
EXPECTED_RESULT

---

54. EJEMPLO

ACTION_REQUIRED

Proyecto: Web X
Acción: instalar WordPress
Motivo: se necesita acceso al servidor
Resultado esperado: instalación funcional

---

55. PAUSA AUTOMÁTICA

Cuando una tarea requiera intervención:

WAITING_HUMAN

El motor no deberá continuar con tareas que dependan de ella.

---

56. REANUDACIÓN

Cuando David confirme:

HECHO

el sistema deberá:

LOCALIZAR TASK
↓
VALIDAR CAMBIO
↓
ACTUALIZAR STATE
↓
CONTINUAR

---

57. NO DUPLICAR INTERVENCIONES

Si ya existe una acción humana pendiente equivalente:

no enviar otra notificación idéntica.

---

58. NOTIFICACIONES

Enviar únicamente:

- intervención requerida;
- error crítico;
- bloqueo;
- finalización importante;
- solicitud explícita de estado.

---

59. PROGRESO

El motor conservará:

"TASKS_COMPLETED"

"TASKS_TOTAL"

cuando sea posible calcularlo.

El porcentaje será aproximado si el trabajo total no está definido.

---

60. PORCENTAJE

No inventar un porcentaje exacto cuando no exista una base objetiva.

Utilizar:

"UNKNOWN"

cuando no pueda calcularse razonablemente.

---

61. CONTROL DE BUCLES

Cada ciclo deberá poder comprobar:

- número de reintentos;
- tareas repetidas;
- errores consecutivos;
- tiempo;
- consumo.

---

62. RETRY

Los errores temporales podrán reintentarse.

Los errores permanentes no deberán repetirse indefinidamente.

---

63. BACKOFF

Los reintentos deberán poder aumentar progresivamente el intervalo.

---

64. LÍMITE

El sistema deberá tener límites configurables.

Ejemplos:

- máximo de retries;
- máximo de tareas consecutivas;
- máximo de tiempo;
- máximo de consumo.

---

65. SEGURIDAD

Antes de ejecutar una operación:

IDENTITY
↓
AUTHORIZATION
↓
TOOL
↓
EXECUTION

---

66. TELEGRAM

Solo usuarios autorizados podrán controlar el motor.

---

67. SECRETOS

No guardar:

- tokens;
- API keys;
- contraseñas;
- claves privadas;

en este archivo ni en el repositorio.

---

68. PROVEEDOR DE IA

El MVP comenzará con un único proveedor.

La arquitectura deberá permitir sustituirlo posteriormente.

---

69. FALLBACK

El fallback entre proveedores se implementará después del MVP inicial.

---

70. COSTE

Registrar cuando sea posible:

- proveedor;
- modelo;
- llamadas;
- consumo;
- coste.

---

71. OBSERVABILIDAD

El motor deberá registrar:

- inicio;
- tarea;
- resultado;
- validación;
- error;
- checkpoint;
- intervención.

---

72. ERROR CRÍTICO

Si el motor detecta una condición insegura:

STOP NEW TASKS
↓
SAVE STATE
↓
NOTIFY

---

73. PRIMERA PRUEBA

Prueba mínima:

David
↓
/continuar
↓
n8n
↓
motor
↓
tarea
↓
IA
↓
resultado
↓
validación
↓
checkpoint

---

74. SEGUNDA PRUEBA

Dos tareas consecutivas:

TASK A
↓
COMPLETED
↓
TASK B
↓
COMPLETED

David no debe intervenir entre A y B.

---

75. TERCERA PRUEBA

Intervención:

TASK A
↓
ACTION_REQUIRED
↓
TELEGRAM
↓
DAVID
↓
HECHO
↓
TASK A VALIDATED
↓
TASK B

---

76. CUARTA PRUEBA

Reinicio de n8n durante el proceso.

Resultado esperado:

el motor recupera el checkpoint y continúa sin empezar innecesariamente
desde cero.

---

77. QUINTA PRUEBA

Error temporal.

Resultado:

ERROR
↓
RETRY
↓
SUCCESS
↓
CONTINUE

---

78. SEXTA PRUEBA

Usuario no autorizado.

Resultado:

DENIED

---

79. SÉPTIMA PRUEBA

Dos proyectos diferentes.

Resultado:

sus estados y memorias permanecen separados.

---

80. CRITERIO DE ÉXITO

El MVP se considerará funcional cuando:

- reciba órdenes;
- identifique proyecto;
- mantenga estado;
- ejecute tareas;
- valide;
- guarde checkpoints;
- encadene tareas;
- detecte intervención;
- pueda recuperarse.

---

81. CRITERIO DE AUTONOMÍA

La prueba fundamental será:

David envía una única orden.

El motor realiza múltiples tareas consecutivas sin solicitar a David que
vuelva a escribir "continúa".

---

82. CRITERIO DE BLOQUEO

El motor solo deberá detener el ciclo cuando:

- necesite una acción humana;
- exista un error no recuperable;
- exista una condición de seguridad;
- se alcance un límite;
- no exista trabajo válido.

---

83. NO CONFUNDIR

TASK_COMPLETED
≠
PROJECT_COMPLETED

EXECUTION_COMPLETED
≠
SESSION_COMPLETED

---

84. PRIMER PROYECTO DE PRUEBA

No utilizar inicialmente un proyecto crítico.

Crear un proyecto controlado exclusivamente para probar el motor.

---

85. FASE LOCAL

El primer entorno será:

ORDENADOR
↓
n8n
↓
MOTOR
↓
TELEGRAM

---

86. FASE VPS

Después de validar:

VPS
↓
n8n
↓
MOTOR
↓
TELEGRAM

---

87. PRODUCCIÓN

No pasar a producción hasta superar las pruebas principales.

---

88. ORDEN DE CONSTRUCCIÓN

Construir en este orden:

1. n8n básico;
2. Telegram;
3. estado;
4. cola;
5. planificador;
6. IA;
7. ejecución;
8. validación;
9. checkpoint;
10. ciclo autónomo;
11. intervención;
12. recuperación;
13. seguridad;
14. pruebas.

---

89. REGLA DE IMPLEMENTACIÓN

Cada componente deberá probarse antes de añadir el siguiente.

---

90. REGLA DE CAMBIOS

No realizar varios cambios críticos simultáneamente.

Cada cambio deberá poder localizarse y revertirse.

---

91. DOCUMENTACIÓN DE IMPLEMENTACIÓN

Cada componente terminado deberá registrar:

- estado;
- versión;
- configuración relevante;
- problemas;
- solución;
- siguiente paso.

No guardar secretos.

---

92. ESTADO DEL MVP

Estado inicial:

"NOT_STARTED"

Estados posteriores:

"BUILDING"

"TESTING"

"BLOCKED"

"READY"

"PRODUCTION"

---

93. PRIMER COMPONENTE REAL

El primer componente que se construirá será:

"WF-INCOMING-COMMAND"

Pero antes de implementarlo deberá determinarse:

- dónde estará ejecutándose n8n;
- qué proveedor de IA utilizaremos;
- cómo persistiremos el estado;
- cómo se conectará Telegram.

---

94. ACCIÓN HUMANA

La primera intervención necesaria de David probablemente será configurar
n8n y las credenciales externas desde un ordenador.

Cuando llegue ese punto:

"ACTION_REQUIRED"

---

95. REGLA FINAL

El objetivo no es crear un workflow que permanezca ejecutándose
eternamente.

El objetivo es crear un sistema persistente que pueda:

EJECUTAR
→ GUARDAR
→ TERMINAR EJECUCIÓN
→ RECUPERAR
→ CONTINUAR
→ EJECUTAR

tantas veces como sea necesario.

Así la autonomía depende del estado persistente y del orquestador, no de
mantener una única ejecución abierta indefinidamente.


