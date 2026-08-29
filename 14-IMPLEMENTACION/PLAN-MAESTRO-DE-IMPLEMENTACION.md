PLAN MAESTRO DE IMPLEMENTACIÓN

1. OBJETIVO

Convertir la documentación del motor autónomo en un sistema funcional,
probable de ejecutar inicialmente en entorno local y posteriormente 24/7.

Este documento establece el orden de construcción.

---

2. PRINCIPIO

NO CONSTRUIR TODO A LA VEZ.

Construir:

NÚCLEO
↓
PRUEBA
↓
PERSISTENCIA
↓
AUTONOMÍA
↓
COMUNICACIÓN
↓
HERRAMIENTAS
↓
PRODUCCIÓN.

---

3. FASE 0 — DOCUMENTACIÓN

Estado:

COMPLETADA.

Debe existir documentación sobre:

- funcionamiento;
- autonomía;
- validación;
- intervención humana;
- n8n;
- comunicación;
- seguridad;
- infraestructura.

---

4. FASE 1 — REVISIÓN

Antes de construir:

1. revisar documentación;
2. detectar contradicciones;
3. eliminar duplicaciones;
4. identificar decisiones pendientes;
5. definir MVP.

Resultado:

IMPLEMENTATION_READY.

---

5. FASE 2 — MVP

El primer MVP deberá ser deliberadamente pequeño.

Debe demostrar:

David
↓
mensaje
↓
n8n
↓
motor
↓
tarea
↓
resultado
↓
siguiente tarea.

---

6. PRIMER OBJETIVO

Conseguir que una orden:

"continúa"

active realmente el sistema.

El sistema deberá recuperar el contexto y comenzar o reanudar trabajo.

---

7. FASE 3 — ESTADO

Implementar persistencia de:

- proyecto;
- sesión;
- tarea;
- estado;
- resultado;
- checkpoint.

---

8. FASE 4 — COLA

Crear sistema de tareas.

Cada tarea tendrá:

- TASK_ID;
- PROJECT_ID;
- estado;
- prioridad;
- dependencias;
- resultado;
- timestamps.

---

9. FASE 5 — PLANIFICADOR

Construir el componente que decide:

"¿qué tarea hago ahora?"

Debe priorizar trabajo útil.

---

10. FASE 6 — AGENTE IA

Conectar el primer proveedor de IA.

El proveedor inicial será elegido según:

- API disponible;
- coste;
- límites;
- capacidad;
- facilidad de integración.

---

11. FASE 7 — HERRAMIENTAS

Añadir progresivamente:

- lectura de repositorios;
- investigación web;
- análisis;
- procesamiento de documentos;
- otras herramientas.

No añadir herramientas antes de necesitarlas.

---

12. FASE 8 — VALIDACIÓN

Integrar el sistema de validación.

Flujo:

RESULTADO
↓
VALIDADOR
↓
VALIDATED / INVALID / PARTIAL.

---

13. FASE 9 — CHECKPOINTS

Implementar recuperación.

Si el proceso se interrumpe:

RESTART
↓
LOAD_CHECKPOINT
↓
CONTINUE.

---

14. FASE 10 — CICLO AUTÓNOMO

Construir el mecanismo fundamental:

TASK_COMPLETED
↓
VALIDATE
↓
SAVE
↓
SELECT_NEXT_TASK
↓
EXECUTE.

No detenerse artificialmente después de cada tarea.

---

15. FASE 11 — CONTROL DE BUCLES

Añadir:

- límites;
- retries;
- timeout;
- backoff;
- circuit breaker;
- detección de tareas duplicadas.

---

16. FASE 12 — INTERVENCIÓN HUMANA

Integrar:

ACTION_REQUIRED.

El motor deberá poder detectar cuándo David es imprescindible.

---

17. FASE 13 — REANUDACIÓN

Cuando David confirme una acción:

HECHO
↓
VALIDATE
↓
UPDATE_STATE
↓
CONTINUE.

---

18. FASE 14 — TELEGRAM

Crear bot.

Conectar:

Telegram
↓
n8n
↓
router
↓
motor.

---

19. COMANDOS INICIALES

Implementar como mínimo:

"/continuar"

"/pausar"

"/detener"

"/estado"

---

20. FASE 15 — NOTIFICACIONES

Implementar únicamente eventos relevantes:

- acción humana;
- error crítico;
- bloqueo;
- límite;
- finalización.

No notificar cada tarea.

---

21. FASE 16 — MULTIPROYECTO

Permitir:

PROJECT_A
PROJECT_B
PROJECT_C

sin mezclar sus contextos.

---

22. FASE 17 — MULTI-REPOSITORIO

Permitir trabajar con repositorios autorizados de:

"davidsaenz20"

El sistema deberá descubrir repositorios actuales y futuros según la
arquitectura que se implemente.

---

23. FASE 18 — SEGURIDAD

Antes de producción comprobar:

- autenticación;
- autorización;
- secretos;
- aislamiento;
- logs;
- prompt injection;
- permisos.

---

24. FASE 19 — PRUEBAS

Crear pruebas para:

TEST 1

Activación.

TEST 2

Continuación.

TEST 3

Pausa.

TEST 4

Detención.

TEST 5

Recuperación.

TEST 6

Intervención humana.

TEST 7

Error recuperable.

TEST 8

Error no recuperable.

TEST 9

Tareas independientes.

TEST 10

Múltiples proyectos.

---

25. PRUEBA DE AUTONOMÍA

La prueba más importante:

ordenar:

"continúa"

y comprobar que el sistema:

1. ejecuta;
2. termina tarea;
3. encuentra siguiente;
4. ejecuta;
5. repite;
6. continúa hasta encontrar un bloqueo real.

---

26. PRUEBA DE INTERVENCIÓN

Crear deliberadamente una tarea que necesite a David.

Debe ocurrir:

MOTOR
↓
ACTION_REQUIRED
↓
TELEGRAM
↓
DAVID
↓
HECHO
↓
VALIDATE
↓
CONTINUE.

---

27. PRUEBA DE RECUPERACIÓN

Interrumpir el sistema durante una sesión.

Reiniciar.

Debe:

- recuperar estado;
- evitar duplicados;
- continuar.

---

28. PRUEBA DE ERROR

Provocar un error temporal.

Debe:

RETRY
↓
BACKOFF
↓
RECOVER
↓
CONTINUE.

---

29. PRUEBA DE PROVEEDOR

Simular caída de un proveedor de IA.

Si existe fallback:

CAMBIAR MODELO
↓
CONTINUAR.

---

30. PRUEBA DE SEGURIDAD

Intentar ejecutar una orden desde un usuario no autorizado.

Resultado esperado:

DENIED.

---

31. PRUEBA DE PROMPT INJECTION

Introducir instrucciones maliciosas dentro de una fuente externa.

Resultado esperado:

el motor las trata como datos, no como instrucciones superiores.

---

32. PRUEBA MULTIPROYECTO

Ejecutar tareas simultáneas de dos proyectos.

Comprobar:

- separación;
- memoria;
- estados;
- resultados.

---

33. CRITERIO MVP

El MVP estará conseguido cuando pueda:

1. recibir una orden;
2. recuperar proyecto;
3. seleccionar tarea;
4. ejecutar;
5. validar;
6. guardar;
7. continuar;
8. detenerse únicamente por una condición real.

---

34. CRITERIO DE AUTONOMÍA

La autonomía será válida si el motor puede encadenar varias tareas sin que
David tenga que escribir "sigue" después de cada una.

---

35. CRITERIO DE INTERVENCIÓN

La intervención será válida si el sistema puede distinguir:

NO NECESITO A DAVID
→ continuar.

NECESITO A DAVID
→ pausar dependencias + notificar.

---

36. CRITERIO DE RECUPERACIÓN

La recuperación será válida si una interrupción no obliga a empezar desde
cero.

---

37. CRITERIO DE SEGURIDAD

No considerar el sistema preparado para producción si:

- expone secretos;
- permite acceso no autorizado;
- genera loops incontrolados;
- afirma haber realizado operaciones que no realizó.

---

38. CRITERIO DE PRODUCCIÓN

Antes de producción deberán estar funcionando:

- persistencia;
- backups;
- monitorización;
- seguridad;
- recuperación;
- control de costes.

---

39. DESPLIEGUE LOCAL

Primero:

ORDENADOR
↓
n8n
↓
MOTOR.

Objetivo:

validar arquitectura.

---

40. DESPLIEGUE VPS

Después:

VPS
↓
n8n
↓
MOTOR
↓
24/7.

Solo migrar cuando el MVP sea estable.

---

41. WHATSAPP

Después de Telegram.

No retrasar el MVP intentando implementar todos los canales.

---

42. ESCALABILIDAD

Cuando el sistema funcione:

optimizar para:

- más tareas;
- más proyectos;
- más repositorios;
- más modelos;
- más usuarios si algún día procede.

---

43. COSTES

Antes de ampliar:

medir.

Registrar:

- IA;
- n8n;
- servidor;
- APIs;
- almacenamiento;
- tráfico.

---

44. CAMBIOS

No introducir grandes cambios simultáneamente.

Cada modificación deberá poder validarse.

---

45. VERSIONES

Registrar versiones del motor.

Ejemplo:

v0.1
→ MVP.

v0.2
→ persistencia.

v0.3
→ autonomía.

v0.4
→ Telegram.

v0.5
→ multiproyecto.

v1.0
→ producción.

---

46. DOCUMENTACIÓN VIVA

Este documento deberá actualizarse cuando cambien decisiones importantes.

No utilizarlo como registro de cada pequeño detalle.

---

47. ESTADOS DEL PROYECTO

DOCUMENTATION
→ documentación.

DESIGN
→ diseño.

BUILD
→ construcción.

TEST
→ pruebas.

PILOT
→ piloto.

PRODUCTION
→ producción.

BLOCKED
→ bloqueado.

---

48. ACCIÓN MANUAL

Cuando llegue un punto que requiera a David:

DETENER DEPENDENCIAS
↓
ACTION_REQUIRED
↓
INFORMAR
↓
ESPERAR.

No seguir solicitando acciones si ya existe una acción pendiente equivalente.

---

49. CONTINUIDAD

Cuando no sea necesaria ninguna acción humana:

CONTINUAR.

El sistema debe pasar automáticamente de un trabajo a otro.

---

50. FINAL DE TAREA

TASK_COMPLETED no significa:

SESSION_COMPLETED.

Significa:

"buscar siguiente trabajo".

---

51. FINAL DE SESIÓN

Una sesión solo deberá finalizar cuando:

- David detenga;
- no exista trabajo válido;
- exista bloqueo no recuperable;
- exista condición de seguridad;
- se alcance un límite que no pueda continuar automáticamente.

---

52. PRINCIPIO DE UTILIDAD

No crear tareas artificiales para mantener el motor funcionando.

El objetivo es producir trabajo útil.

---

53. ORDEN DE CONSTRUCCIÓN

ORDEN DEFINITIVO:

1. documentación;
2. revisión;
3. MVP;
4. estado;
5. cola;
6. planificador;
7. IA;
8. herramientas;
9. validación;
10. checkpoints;
11. autonomía;
12. control de loops;
13. intervención;
14. Telegram;
15. multiproyecto;
16. multi-repositorio;
17. seguridad;
18. pruebas;
19. local;
20. VPS;
21. producción.

---

54. PRIMERA ACCIÓN REAL

A partir de este documento termina la fase principal de documentación.

El siguiente trabajo ya no será crear documentación por crear.

Será:

REVISAR EL REPOSITORIO COMPLETO Y PREPARAR EL DISEÑO REAL DEL MVP DE n8n.

Antes de construir workflows se deberá comprobar qué documentación existe,
qué decisiones están realmente cerradas y qué componentes son necesarios.

---

55. REGLA FINAL

No avanzar por avanzar.

En cada fase:

ANALIZAR
↓
CONSTRUIR
↓
VALIDAR
↓
REGISTRAR
↓
CONTINUAR.

Si no existe una acción humana necesaria:

CONTINUAR AUTOMÁTICAMENTE.

Si existe una acción humana:

ACTION_REQUIRED.

---

56. OBJETIVO FINAL

Crear un motor que permita a David decir:

"continúa"

y que el sistema pueda trabajar de manera autónoma durante el máximo tiempo
posible, encadenando trabajos útiles, utilizando diferentes proyectos y
repositorios, hasta encontrar una dependencia real de David o una condición
que obligue legítimamente a detenerse.


