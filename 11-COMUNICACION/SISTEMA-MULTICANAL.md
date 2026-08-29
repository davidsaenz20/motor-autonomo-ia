SISTEMA MULTICANAL

1. OBJETIVO

Permitir que el motor autónomo pueda comunicarse con David mediante
diferentes canales sin modificar su núcleo.

El motor debe ser independiente del canal utilizado.

---

2. CANALES

Canales previstos:

- Telegram;
- WhatsApp;
- Web;
- email;
- futuros canales.

Telegram será el primer canal de implementación.

---

3. PRINCIPIO

CANAL
↓
ADAPTADOR
↓
EVENTO NORMALIZADO
↓
MOTOR
↓
EVENTO NORMALIZADO
↓
ADAPTADOR
↓
CANAL

---

4. INDEPENDENCIA

El núcleo del motor no deberá contener lógica específica de Telegram,
WhatsApp u otro canal.

Cada canal tendrá su propio adaptador.

---

5. EVENTO ENTRANTE

Todos los mensajes deberán transformarse a una estructura común.

Conceptualmente:

INCOMING_MESSAGE

- channel;
- user;
- message;
- timestamp;
- conversation;
- metadata.

---

6. EVENTO SALIENTE

El motor generará:

OUTGOING_MESSAGE

- recipient;
- message;
- priority;
- type;
- project;
- action_required;
- metadata.

El adaptador decidirá cómo entregarlo.

---

7. TIPOS DE MENSAJE

Como mínimo:

COMMAND
STATUS
NOTIFICATION
HUMAN_ACTION
ERROR
ALERT
RESULT

---

8. COMMAND

Representa una orden de David.

Ejemplos:

- continuar;
- pausar;
- detener;
- seleccionar proyecto.

---

9. STATUS

Información solicitada sobre el estado del motor.

---

10. NOTIFICATION

Información que el motor decide enviar automáticamente.

---

11. HUMAN_ACTION

Notificación de que David debe realizar una acción.

Tiene prioridad alta.

---

12. ERROR

Comunicación de un error que no pudo resolverse automáticamente.

---

13. ALERT

Evento que requiere atención especial.

---

14. RESULT

Resultado final o intermedio que el motor necesita comunicar.

No debe utilizarse para interrumpir continuamente el trabajo autónomo.

---

15. TELEGRAM

Primer adaptador.

Responsabilidades:

- recibir mensajes;
- autenticar usuario;
- enviar eventos;
- entregar notificaciones;
- procesar confirmaciones.

---

16. WHATSAPP

Segundo canal previsto.

Su implementación deberá ser independiente de Telegram.

La tecnología concreta se decidirá después de evaluar:

- API;
- coste;
- restricciones;
- facilidad;
- estabilidad;
- automatización.

---

17. WEB

Podrá existir posteriormente un panel web.

Podrá mostrar:

- proyectos;
- sesiones;
- tareas;
- progreso;
- errores;
- acciones pendientes;
- estado del motor.

---

18. EMAIL

Podrá utilizarse para:

- alertas;
- informes;
- errores importantes;
- acciones humanas.

No será el canal principal de control.

---

19. IDENTIDAD

Todos los canales deberán asociar cada mensaje a un usuario autorizado.

El motor no deberá asumir que cualquier mensaje recibido es válido.

---

20. USUARIO

La identidad normalizada deberá contener conceptualmente:

USER_ID
CHANNEL
CHANNEL_USER_ID
AUTHENTICATED

Los identificadores reales se configurarán durante la implementación.

---

21. AUTORIZACIÓN

Las operaciones deberán clasificarse por permisos.

Ejemplo:

READ
→ consultar estado.

CONTROL
→ pausar/continuar.

CRITICAL
→ operaciones reservadas.

---

22. SEGURIDAD

Un canal comprometido no debe proporcionar automáticamente acceso ilimitado
al motor.

La autenticación y autorización deberán verificarse antes de ejecutar órdenes.

---

23. DUPLICADOS

Los mensajes deberán poder identificarse mediante un identificador único
cuando el canal lo proporcione.

Esto permite evitar procesar dos veces el mismo evento.

---

24. IDEMPOTENCIA

Una orden repetida accidentalmente no debería provocar consecuencias
duplicadas cuando sea posible evitarlo.

Ejemplo:

CONTINUE
CONTINUE

No debería iniciar dos motores simultáneos.

---

25. ORDEN DE MENSAJES

El sistema deberá intentar mantener el orden lógico de los eventos.

Si llegan:

PAUSE
CONTINUE

el segundo deberá procesarse después del primero.

---

26. PRIORIDAD

Los mensajes críticos tendrán prioridad.

Orden conceptual:

STOP

«»

HUMAN_ACTION

«»

CONTROL

«»

ALERT

«»

STATUS

«»

NORMAL.

---

27. INTERRUPCIÓN

Los mensajes entrantes no deberán interrumpir una operación crítica de forma
insegura.

El motor deberá procesarlos en el punto seguro correspondiente.

---

28. RESPUESTA

Una orden podrá generar:

ACKNOWLEDGED
→ evento recibido.

EXECUTING
→ trabajo en curso.

COMPLETED
→ operación completada.

WAITING_HUMAN
→ necesita David.

ERROR
→ no pudo ejecutarse.

---

29. NO RESPONDER POR RESPONDER

Una respuesta de confirmación no debe sustituir la ejecución real.

Ejemplo incorrecto:

David:
"sigue"

Motor:
"Vale, sigo."

Ejemplo correcto:

David:
"sigue"

Motor:
ACTIVA/REANUDA LA EJECUCIÓN.

---

30. COMUNICACIÓN MÍNIMA

El sistema debe minimizar mensajes automáticos.

No informar de cada subtrabajo.

---

31. ACCIÓN HUMANA

Cuando exista una acción requerida:

el mensaje deberá incluir:

- proyecto;
- acción;
- motivo;
- ruta si existe;
- resultado esperado.

---

32. RECUPERACIÓN

Si un canal falla:

el evento no debe perderse.

Debe conservarse en el sistema de estado o cola.

---

33. REINTENTO

Los envíos podrán reintentarse ante errores temporales.

Los reintentos deberán tener límite.

---

34. FALLBACK DE CANAL

Si existe más de un canal configurado:

Telegram
↓
fallo

podrá utilizarse otro canal disponible.

No debe generarse un bucle de notificaciones.

---

35. NOTIFICACIONES IMPORTANTES

Las notificaciones de:

HUMAN_ACTION_REQUIRED
CRITICAL_ERROR

podrán utilizar más de un canal si David lo configura.

---

36. ESTADO DE ENTREGA

Cuando sea posible registrar:

SENT
DELIVERED
FAILED

Esto permite detectar problemas de comunicación.

---

37. PRIVACIDAD

No enviar información sensible por un canal que no tenga la seguridad
adecuada.

---

38. SECRETOS

Nunca incluir:

- API keys;
- tokens;
- contraseñas;
- credenciales;
- secretos de infraestructura.

---

39. REGISTRO

Registrar únicamente la información necesaria para auditoría.

---

40. HISTORIAL

El historial completo de trabajo permanecerá en el sistema de memoria.

Los canales solo mostrarán el contexto necesario.

---

41. CAMBIO DE CANAL

David podrá pasar de Telegram a otro canal.

El estado de la sesión deberá mantenerse.

Ejemplo:

Telegram
→ iniciar

WhatsApp
→ continuar

Web
→ consultar estado.

---

42. MULTIDISPOSITIVO

La identidad debe representar al usuario, no al dispositivo.

Así David podrá utilizar:

- móvil;
- ordenador;
- tablet;
- web.

---

43. SESIÓN

Los canales no deberán crear necesariamente una nueva sesión cada vez.

Deberán localizar la sesión activa del proyecto correspondiente.

---

44. PROYECTO

Toda orden deberá poder asociarse a un proyecto.

Si existe un proyecto activo:

utilizarlo.

Si existen varios y la orden es ambigua:

solicitar selección únicamente cuando sea necesario.

---

45. CAMBIO DE PROYECTO

Al cambiar de proyecto:

guardar estado anterior.

Cargar estado nuevo.

Nunca mezclar contextos.

---

46. COMPATIBILIDAD FUTURA

El sistema deberá diseñarse para que añadir un nuevo canal requiera
principalmente crear un nuevo adaptador.

No modificar el núcleo.

---

47. PRINCIPIO DE ESCALABILIDAD

NUEVO CANAL

→ NUEVO ADAPTADOR

NO:

→ NUEVO MOTOR.

---

48. ARQUITECTURA

CANALES
├── Telegram
├── WhatsApp
├── Web
└── Email

↓

ADAPTER LAYER

↓

EVENT BUS / ROUTER

↓

MOTOR AUTÓNOMO

↓

MEMORIA + PLANIFICADOR + IA + HERRAMIENTAS

---

49. PRIMERA IMPLEMENTACIÓN

La primera versión deberá utilizar:

TELEGRAM
+
n8n
+
MOTOR AUTÓNOMO.

No implementar todos los canales simultáneamente.

---

50. PRINCIPIO FINAL

El canal es únicamente la interfaz.

La autonomía, memoria, planificación, validación y ejecución pertenecen al
motor.

---

51. PRÓXIMO TRABAJO

El siguiente documento será:

SEGURIDAD Y CONTROL DE ACCESO DEL MOTOR

Definirá cómo proteger repositorios, APIs, credenciales, proyectos y órdenes
remotas antes de poner el sistema en funcionamiento real.


