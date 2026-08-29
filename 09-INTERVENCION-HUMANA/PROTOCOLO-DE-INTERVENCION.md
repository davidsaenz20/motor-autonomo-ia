PROTOCOLO DE INTERVENCIÓN HUMANA

1. OBJETIVO

Definir cuándo el motor autónomo debe detener temporalmente su ejecución
porque necesita una acción de David.

La intervención humana debe ser una excepción, no el funcionamiento normal.

---

2. PRINCIPIO FUNDAMENTAL

AUTONOMÍA PRIMERO.

INTERVENCIÓN SOLO CUANDO SEA NECESARIA.

El motor deberá intentar resolver autónomamente cualquier problema que pueda
resolver de forma segura y autorizada.

---

3. CUÁNDO INTERVENIR

El motor podrá solicitar intervención cuando:

- falte una credencial;
- falte información imprescindible;
- sea necesaria una acción física;
- sea necesaria una acción en un ordenador;
- sea necesaria una autorización;
- sea necesario modificar manualmente un archivo;
- una operación requiera permisos que el motor no posee;
- exista un bloqueo que no pueda resolver;
- exista una decisión que las reglas hayan reservado a David.

---

4. CUÁNDO NO INTERVENIR

No solicitar intervención simplemente porque:

- una tarea terminó;
- apareció una nueva tarea;
- existe una pequeña duda solucionable;
- el resultado podría mejorarse ligeramente;
- existe una alternativa autónoma;
- se ha completado un subtrabajo.

El motor debe continuar.

---

5. EJEMPLO

TAREA:

"Preparar una web."

Si puede:

- investigar;
- diseñar;
- redactar;
- preparar estructura;
- generar contenido;
- analizar competencia;

debe hacerlo.

Si finalmente necesita:

"Instalar WordPress en el servidor."

entonces:

ACTION_REQUIRED.

---

6. ARCHIVOS DEL REPOSITORIO

Si el motor puede preparar el contenido pero David debe modificarlo
manualmente:

detener únicamente cuando el archivo esté listo para intervención.

Debe indicar:

- repositorio;
- ruta;
- archivo;
- operación;
- contenido;
- motivo.

---

7. NO INTENTAR OPERACIONES IMPOSIBLES

Si el entorno no proporciona permisos de escritura:

NO REPETIR INTENTOS.

Registrar:

WRITE_ACCESS = UNAVAILABLE.

Preparar el cambio y solicitar a David que lo aplique si es necesario.

---

8. INFORMACIÓN NECESARIA

Si falta información imprescindible:

WAITING_HUMAN.

La solicitud debe indicar exactamente:

QUÉ FALTA
+
POR QUÉ FALTA
+
QUÉ PERMITE CONTINUAR.

---

9. CREDENCIALES

Nunca solicitar ni registrar secretos en texto público.

Cuando sea necesaria una credencial:

solicitar que David la configure mediante el mecanismo seguro correspondiente.

No almacenar claves en:

- README;
- archivos de instrucciones;
- logs;
- memoria pública.

---

10. ACCIONES EN ORDENADOR

Ejemplos:

- instalar software;
- configurar servidor;
- instalar WordPress;
- configurar DNS;
- configurar una aplicación;
- iniciar un servicio local.

El motor deberá detenerse y explicar la acción concreta.

---

11. ACCIONES EN n8n

Si una parte requiere que David configure manualmente n8n:

indicar:

- workflow;
- nodo;
- configuración;
- credencial;
- valor requerido;
- resultado esperado.

Después esperar.

---

12. ACCIONES IRREVERSIBLES

Si una operación puede producir consecuencias importantes y no existe
autorización previa:

WAITING_HUMAN.

Ejemplos:

- eliminar información;
- publicar;
- comprar;
- contratar;
- enviar comunicaciones masivas;
- modificar infraestructura crítica.

---

13. OPERACIONES REVERSIBLES

Las operaciones seguras y reversibles podrán realizarse autónomamente si
existen permisos.

No solicitar confirmación innecesaria.

---

14. DECISIONES ECONÓMICAS

Las operaciones que impliquen gasto económico deberán seguir las reglas
económicas definidas por el sistema.

Si no existe autorización previa:

ACTION_REQUIRED.

---

15. SEGURIDAD

Ante una duda razonable sobre seguridad:

DETENER.

Registrar:

- riesgo;
- operación;
- motivo;
- información disponible;
- acción recomendada.

---

16. BLOQUEOS

Antes de pedir ayuda:

INTENTAR RESOLVER.

Si no puede:

BLOCKED.

Después:

ACTION_REQUIRED.

---

17. TIEMPO DE ESPERA

Una intervención pendiente no debe hacer que el motor continúe ejecutando
tareas dependientes de ella.

Debe:

PAUSAR DEPENDENCIAS.

Pero podrá continuar otras tareas independientes si el planificador lo
permite.

---

18. COLA DE TRABAJO

Ejemplo:

TAREA A
→ requiere David

TAREA B
→ autónoma

TAREA C
→ autónoma

El motor podrá hacer:

A → PAUSED
B → EXECUTE
C → EXECUTE

No debe bloquear todo el proyecto innecesariamente.

---

19. MENSAJE AL USUARIO

La notificación deberá ser breve y accionable.

Formato conceptual:

ACCIÓN REQUERIDA

Proyecto: X

Necesitas:
[acción concreta]

Ruta:
[ruta]

Después:
[resultado esperado]

---

20. NO INFORMAR DE MÁS

No enviar:

- razonamientos internos;
- historial completo;
- pasos irrelevantes;
- tareas ya terminadas;
- información redundante.

La comunicación debe permitir actuar rápidamente.

---

21. ESTADO

Una intervención deberá generar:

HUMAN_ACTION_REQUIRED = true

y registrar:

- ID;
- proyecto;
- tarea;
- motivo;
- acción;
- fecha;
- dependencias;
- estado.

---

22. ESTADOS DE INTERVENCIÓN

PENDING

→ esperando a David.

IN_PROGRESS

→ David ha comenzado la acción.

COMPLETED

→ acción realizada.

FAILED

→ acción no realizada correctamente.

CANCELLED

→ intervención cancelada.

---

23. CONFIRMACIÓN

Cuando David indique que ha realizado la acción:

1. recuperar contexto;
2. comprobar cambios;
3. validar;
4. continuar.

No reiniciar el proyecto.

---

24. COMPROBACIÓN POSTERIOR

Nunca asumir automáticamente que la acción humana funcionó.

Cuando sea posible:

David indica "hecho"
↓
motor comprueba
↓
VALIDATED
↓
continuar.

---

25. ACCIÓN INCORRECTA

Si la comprobación falla:

informar únicamente del problema concreto.

No comenzar nuevamente todo el proceso.

---

26. MÚLTIPLES ACCIONES

Si necesita varias acciones humanas:

agruparlas cuando sea posible.

Ejemplo:

ACCIÓN HUMANA:

1. instalar WordPress;
2. configurar dominio;
3. conectar API.

No generar tres interrupciones separadas si pueden hacerse juntas.

---

27. PRIORIZACIÓN

Si existen varias acciones:

PRIORIDAD AL BLOQUEO QUE MÁS TRABAJO DESBLOQUEE.

---

28. DEPENDENCIAS

Cada acción humana deberá poder indicar qué tareas dependen de ella.

Ejemplo:

INSTALL_WORDPRESS
↓
DEPENDE:

- configuración web;
- publicación;
- pruebas.

---

29. ACCIÓN NO BLOQUEANTE

Si una acción humana no impide continuar:

registrarla como:

HUMAN_ACTION_PENDING

y continuar con trabajo independiente.

---

30. REANUDACIÓN

Después de una intervención:

RECUPERAR CHECKPOINT
↓
VALIDAR ACCIÓN
↓
ACTUALIZAR ESTADO
↓
REACTIVAR TAREAS
↓
CONTINUAR.

---

31. NO PERDER CONTEXTO

La intervención humana nunca debe provocar:

- pérdida de memoria;
- pérdida de resultados;
- pérdida de fuentes;
- repetición innecesaria;
- reinicio del proyecto.

---

32. INTERVENCIÓN COMO EVENTO

Una intervención deberá considerarse un evento del sistema.

Ejemplo:

EVENT:
HUMAN_ACTION_REQUIRED

PAYLOAD:

- project;
- task;
- action;
- reason;
- dependencies;
- status.

---

33. CANALES

El evento podrá notificarse mediante:

- Telegram;
- WhatsApp;
- otros canales futuros.

El canal de comunicación es independiente del motor.

---

34. FALLA DEL CANAL

Si Telegram o WhatsApp no están disponibles:

la intervención deberá permanecer registrada.

La pérdida temporal del canal no debe perder la acción pendiente.

---

35. DUPLICADOS

No enviar repetidamente la misma alerta si David todavía no ha respondido,
salvo que exista una política explícita de recordatorio.

---

36. RECORDATORIOS

Los recordatorios deberán ser opcionales y controlados.

Nunca deben generar un bucle de mensajes.

---

37. ESCALADO

Si una intervención permanece pendiente durante mucho tiempo:

registrar:

STALE_HUMAN_ACTION.

El sistema podrá enviar un recordatorio controlado.

---

38. CANCELACIÓN

David podrá cancelar una intervención.

El planificador deberá decidir qué hacer con las tareas dependientes.

---

39. DECISIÓN RESERVADA A DAVID

Si las reglas del proyecto establecen que una decisión requiere aprobación
humana:

no intentar sustituirla mediante una inferencia.

Solicitar intervención.

---

40. PRINCIPIO DE MÍNIMA INTERRUPCIÓN

Antes de detenerse preguntar internamente:

"¿Realmente necesito a David para continuar?"

Si:

NO → CONTINUAR.

SÍ → ACTION_REQUIRED.

---

41. PRINCIPIO FINAL

El motor debe convertir la intervención humana en:

UNA ACCIÓN CONCRETA

y no en:

UNA CONVERSACIÓN ABIERTA.

David debe poder realizar la acción y devolver el sistema al trabajo autónomo
con la mínima fricción.

---

42. PRÓXIMO TRABAJO

El siguiente documento deberá definir:

ARQUITECTURA DE n8n Y MOTOR DE EJECUCIÓN

Deberá convertir toda la arquitectura anterior en workflows, estados,
disparadores, ciclos de ejecución, recuperación y comunicación.

