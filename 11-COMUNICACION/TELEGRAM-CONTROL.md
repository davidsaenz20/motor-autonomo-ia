CONTROL DEL MOTOR MEDIANTE TELEGRAM

1. OBJETIVO

Definir Telegram como canal principal de control y comunicación del motor
autónomo desde el teléfono móvil.

Telegram no será el cerebro del sistema.

Será una interfaz entre David y el motor.

---

2. ARQUITECTURA

DAVID
↓
TELEGRAM
↓
n8n
↓
MOTOR AUTÓNOMO
↓
AGENTE / HERRAMIENTAS / MEMORIA
↓
n8n
↓
TELEGRAM
↓
DAVID

---

3. FUNCIONES

Telegram deberá permitir:

- activar el motor;
- continuar;
- pausar;
- detener;
- consultar estado;
- seleccionar proyecto;
- consultar bloqueos;
- recibir acciones humanas;
- recibir errores críticos.

---

4. ACTIVACIÓN

Comando conceptual:

"/activar"

Debe iniciar una sesión de trabajo si existe un proyecto y una tarea válida.

Si el contexto ya está definido:

continuar desde el último checkpoint.

---

5. CONTINUAR

Comando:

"/continuar"

Debe significar:

"Continúa trabajando desde el último estado válido."

No debe interpretarse como una nueva tarea.

---

6. MENSAJE "SIGUE"

También deberán aceptarse expresiones naturales como:

- sigue;
- continúa;
- sigue trabajando;
- reanuda;
- adelante.

El sistema podrá normalizarlas a:

"CONTINUE".

---

7. PAUSA

Comando:

"/pausar"

Debe detener la creación de nuevas tareas.

El motor deberá guardar el estado actual.

No debe perder resultados.

---

8. DETENER

Comando:

"/detener"

Debe detener la sesión.

No debe borrar:

- memoria;
- resultados;
- tareas;
- checkpoints.

---

9. ESTADO

Comando:

"/estado"

Debe devolver un resumen útil.

Formato conceptual:

PROYECTO: X

TRABAJO: X
PROGRESO: XX%

HECHO:
X

QUEDA:
X

BLOQUEOS:
X

SIGUIENTE:
X

---

10. INFORMACIÓN DE ESTADO

La respuesta de estado deberá ser breve.

No mostrar automáticamente:

- razonamientos internos;
- historial completo;
- logs técnicos;
- todas las llamadas;
- información irrelevante.

---

11. INTERVENCIÓN HUMANA

Cuando el motor necesite a David:

Telegram deberá recibir:

"ACTION_REQUIRED"

seguido de:

- proyecto;
- acción;
- ruta si procede;
- motivo;
- resultado esperado.

---

12. EJEMPLO

ACCIÓN REQUERIDA

Proyecto: Web X

Necesitas instalar WordPress en el servidor.

Después responde:
"hecho"

El motor permanecerá esperando.

---

13. CONFIRMACIÓN

Cuando David escriba:

"hecho"

el sistema deberá:

1. identificar la acción pendiente;
2. comprobarla si es posible;
3. actualizar estado;
4. reanudar trabajo.

---

14. RESPUESTAS NATURALES

No depender exclusivamente de comandos.

Ejemplos:

"¿cómo vamos?"

"¿qué estás haciendo?"

"¿qué falta?"

"continúa"

"para"

"reanuda"

podrán convertirse en intenciones del sistema.

---

15. PRIORIDAD DE ÓRDENES

Las órdenes explícitas de David deberán tener prioridad sobre el ciclo
autónomo.

Ejemplo:

trabajando
↓
David: pausa
↓
PAUSE.

---

16. SEGURIDAD DEL USUARIO

El sistema deberá comprobar que el mensaje procede del usuario autorizado.

No ejecutar órdenes de cualquier persona que pueda contactar con el bot.

---

17. IDENTIFICACIÓN

Deberá configurarse una lista de usuarios autorizados.

Inicialmente:

SOLO DAVID.

El identificador real se configurará durante la implementación.

---

18. CREDENCIALES

Las credenciales de Telegram deberán mantenerse en el sistema seguro de n8n.

Nunca en:

- README;
- documentación pública;
- repositorio;
- mensajes;
- logs.

---

19. BOT

La implementación utilizará un bot de Telegram.

La creación y configuración del bot será una acción manual de David cuando
lleguemos a esa fase.

---

20. n8n

n8n recibirá los mensajes y los convertirá en eventos internos.

Ejemplo:

Telegram:
"continúa"

↓

EVENT:
"USER_CONTINUE"

---

21. EVENTOS

Eventos conceptuales:

"USER_START"

"USER_CONTINUE"

"USER_PAUSE"

"USER_STOP"

"USER_STATUS"

"HUMAN_ACTION_COMPLETED"

"USER_SELECT_PROJECT"

---

22. ROUTER

n8n deberá disponer de un router que determine la intención.

MENSAJE
↓
NORMALIZACIÓN
↓
INTENCIÓN
↓
EVENTO
↓
MOTOR

---

23. AMBIGÜEDAD

Si el mensaje no puede interpretarse con suficiente seguridad:

no ejecutar una operación potencialmente peligrosa.

Solicitar aclaración.

Para órdenes simples como "sigue", no deberá solicitar aclaración
innecesariamente.

---

24. NOTIFICACIONES AUTOMÁTICAS

El motor podrá enviar mensajes automáticamente cuando:

- requiera intervención;
- exista un error crítico;
- una sesión quede bloqueada;
- alcance un límite;
- el proyecto se complete.

---

25. NO NOTIFICAR CADA TAREA

No enviar mensajes por:

- cada tarea;
- cada subtrabajo;
- cada búsqueda;
- cada resultado normal.

La autonomía depende de reducir interrupciones.

---

26. RESUMEN DE PROGRESO

Cuando sea necesario informar:

TRABAJO:
[trabajo actual]

SUBTRABAJOS:
[resumen]

PROGRESO:
[porcentaje]

ESTADO:
[resumen breve]

---

27. LÍMITE DE TEXTO

La explicación breve de estado deberá ser concisa.

Objetivo:

máximo aproximado de 100 caracteres para la explicación descriptiva
solicitada por el sistema.

Los datos estructurados de estado podrán aparecer fuera de esa explicación.

---

28. ACCIONES HUMANAS

Las notificaciones de intervención no estarán limitadas a 100 caracteres.

Deben contener toda la información necesaria para realizar la acción.

---

29. ERRORES

Un error normal no debe generar una cascada de mensajes.

El motor deberá intentar recuperación antes de notificar.

---

30. RECORDATORIOS

Las acciones humanas pendientes podrán generar recordatorios.

Deberán estar limitados para evitar spam.

---

31. DESCONEXIÓN

Si Telegram no está disponible:

el motor deberá continuar funcionando.

Las notificaciones pendientes permanecerán registradas.

---

32. RECONEXIÓN

Cuando Telegram vuelva a estar disponible:

podrá enviarse el estado pendiente más importante.

No enviar automáticamente todo el historial.

---

33. MULTIPROYECTO

David podrá indicar qué proyecto desea trabajar.

Ejemplo conceptual:

"/proyecto base-proyectos"

El motor deberá cargar:

- repositorio;
- proyecto;
- reglas;
- estado;
- cola.

---

34. CAMBIO DE PROYECTO

Antes de cambiar:

guardar checkpoint.

Después:

cargar contexto del nuevo proyecto.

No mezclar memorias.

---

35. LISTADO

Podrá existir un comando:

"/proyectos"

que muestre los proyectos disponibles.

La implementación dependerá del sistema de descubrimiento de repositorios.

---

36. TRABAJO ACTUAL

Podrá existir:

"/trabajo"

para consultar exclusivamente la tarea actual.

---

37. BLOQUEOS

Podrá existir:

"/bloqueos"

para consultar acciones pendientes.

---

38. CONTROL MÍNIMO

El sistema deberá poder funcionar con pocos comandos.

Como mínimo:

"/continuar"

"/pausar"

"/detener"

"/estado"

---

39. PRINCIPIO DE FRICCIÓN MÍNIMA

David debe poder controlar el motor desde el móvil sin necesidad de abrir
n8n, servidores o repositorios para las operaciones normales.

---

40. WHATSAPP

WhatsApp podrá añadirse posteriormente.

No deberá requerir modificar el núcleo del motor.

Telegram y WhatsApp serán simplemente canales.

---

41. CANAL ABSTRACTO

El motor deberá trabajar con:

"MESSAGE_CHANNEL"

en lugar de depender directamente de Telegram.

Así podrá existir:

Telegram
WhatsApp
Email
Web
otros.

---

42. SEGURIDAD OPERATIVA

Nunca ejecutar mediante Telegram una operación crítica únicamente porque
el texto parezca una orden si no existe suficiente autenticación o contexto.

---

43. AUDITORÍA

Registrar:

- usuario;
- evento;
- fecha;
- proyecto;
- resultado;
- error si existe.

No registrar secretos.

---

44. RESPUESTA A "SIGUE"

Cuando David envíe:

"/continuar"

o:

"sigue"

el sistema deberá:

1. recuperar estado;
2. comprobar bloqueos;
3. localizar trabajo válido;
4. ejecutar;
5. continuar automáticamente.

No responder simplemente:

"Vale, sigo."

El objetivo es activar trabajo real.

---

45. FIN DE SESIÓN

El sistema no debe interpretar una tarea terminada como fin de sesión.

Debe buscar siguiente trabajo válido.

---

46. ÚNICO MOTIVO DE PARADA HUMANA

Si existe trabajo autónomo disponible:

CONTINUAR.

Si no existe:

evaluar:

- intervención;
- bloqueo;
- límite;
- proyecto idle.

---

47. PRINCIPIO FINAL

Telegram debe convertirse en:

CONTROL REMOTO DEL MOTOR

y no en:

CHAT CON EL MOTOR.

La conversación debe servir para controlar el trabajo, no para sustituirlo.

---

48. PRÓXIMO TRABAJO

El siguiente documento deberá definir:

WHATSAPP Y SISTEMA DE CANALES

Se determinará cómo añadir WhatsApp y otros canales sin modificar el núcleo
del motor autónomo.


