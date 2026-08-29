MODELO DE ESTADO Y MEMORIA

1. OBJETIVO

Crear un sistema persistente que permita al motor autónomo recordar y recuperar
el estado de cualquier trabajo realizado sobre cualquier repositorio.

La memoria NO dependerá de la conversación de ChatGPT.

Debe poder recuperarse aunque:

- ChatGPT se cierre;
- n8n se reinicie;
- cambie el modelo de IA;
- cambie el proveedor;
- transcurran días o semanas;
- se cree un nuevo repositorio.

---

2. PRINCIPIO FUNDAMENTAL

El motor debe separar:

MEMORIA

información que debe conservarse.

ESTADO

situación actual del trabajo.

TAREA

unidad concreta de trabajo.

RESULTADO

producto generado por una tarea.

DECISIÓN

conclusión adoptada durante el proceso.

---

3. JERARQUÍA

La información se organizará conceptualmente así:

CUENTA
↓
REPOSITORIO
↓
PROYECTO
↓
OBJETIVO
↓
FASE
↓
TRABAJO
↓
SUBTRABAJO
↓
RESULTADO

Esto permite trabajar simultáneamente en múltiples repositorios sin mezclar
sus contextos.

---

4. IDENTIDAD DE UNA EJECUCIÓN

Cada ejecución deberá poder identificarse mediante un identificador único.

Conceptualmente:

SESSION_ID

Además:

REPOSITORY_ID
PROJECT_ID
OBJECTIVE_ID
TASK_ID

El formato definitivo queda pendiente de implementación.

---

5. ESTADO GLOBAL

El motor deberá conocer siempre:

- qué repositorio está trabajando;
- qué proyecto está trabajando;
- qué objetivo persigue;
- en qué fase está;
- qué tarea está ejecutando;
- qué subtrabajo está ejecutando;
- qué porcentaje ha completado;
- qué resultados existen;
- qué queda pendiente;
- si existe un bloqueo;
- si necesita intervención humana;
- cuál es el siguiente trabajo válido.

---

6. ESTADOS PRINCIPALES

IDLE

No existe trabajo activo.

RUNNING

Existe trabajo autónomo en ejecución.

WAITING_HUMAN

El motor necesita una acción de David.

PAUSED

El trabajo está detenido voluntariamente.

ERROR

Se ha producido un error que requiere tratamiento.

COMPLETED

El objetivo actual se considera terminado.

STOPPED

El motor ha sido detenido.

---

7. ESTADO DE UNA TAREA

Cada tarea debería conservar como mínimo:

- identificador;
- repositorio;
- proyecto;
- objetivo;
- fase;
- descripción;
- prioridad;
- estado;
- porcentaje;
- fecha de inicio;
- última actualización;
- resultado;
- fuentes;
- decisiones;
- errores;
- bloqueos;
- acción humana;
- siguiente acción.

---

8. ESTADOS DE TAREA

PENDING
↓
READY
↓
RUNNING
↓
VALIDATING
↓
COMPLETED

También:

BLOCKED
WAITING_HUMAN
FAILED
CANCELLED

---

9. SUBTRABAJOS

Una tarea compleja podrá dividirse:

TAREA
├── SUBTRABAJO 1
├── SUBTRABAJO 2
├── SUBTRABAJO 3
└── SUBTRABAJO 4

Cada subtrabajo tendrá su propio estado.

El porcentaje global deberá calcularse a partir del progreso real de sus
subtrabajos cuando sea posible.

---

10. SIGUIENTE TAREA

Al terminar una tarea el motor deberá preguntarse:

¿Existe otro trabajo útil y válido que pueda realizar autónomamente?

Si:

SÍ → continuar.

NO → comprobar si:

- el objetivo está terminado;
- existe una dependencia;
- necesita intervención humana;
- existe un bloqueo;
- debe esperar nueva instrucción.

---

11. NO REPETICIÓN

Antes de crear una tarea nueva, el motor deberá comprobar si ya existe una
tarea equivalente.

Debe evitar:

- investigaciones duplicadas;
- análisis repetidos;
- generación innecesaria;
- repetir tareas completadas.

---

12. CONTEXTO DE MEMORIA

La IA no recibirá toda la memoria histórica en cada llamada.

El motor seleccionará:

- memoria permanente relevante;
- estado actual;
- decisiones relevantes;
- resultados necesarios;
- tarea actual;
- restricciones;
- contexto del repositorio.

El objetivo es minimizar tokens y mantener precisión.

---

13. MEMORIA PERMANENTE

Debe conservar información que siga siendo válida a largo plazo.

Ejemplos:

- decisiones arquitectónicas;
- reglas del proyecto;
- metodología;
- información confirmada;
- restricciones;
- preferencias del sistema;
- conclusiones importantes.

---

14. MEMORIA DE SESIÓN

Información necesaria durante una ejecución concreta.

Ejemplos:

- tarea actual;
- contexto cargado;
- investigaciones recientes;
- resultados temporales;
- llamadas realizadas;
- errores recientes.

Una sesión puede terminar sin perder la memoria permanente.

---

15. MEMORIA DE PROYECTO

Debe conservar:

- objetivo;
- metodología;
- decisiones;
- investigaciones;
- resultados;
- estado;
- pendientes;
- problemas conocidos;
- próximos pasos.

Cada proyecto debe estar aislado del resto.

---

16. MEMORIA DEL REPOSITORIO

Debe conservar información sobre:

- estructura;
- reglas;
- documentos importantes;
- metodología;
- proyectos existentes;
- convenciones;
- restricciones;
- estado general.

Esta memoria servirá para evitar redescubrir continuamente el mismo repositorio.

---

17. FUENTES

Los resultados obtenidos mediante investigación deberán conservar sus fuentes
cuando sea relevante.

Cada fuente debería registrar:

- URL o referencia;
- fecha;
- información obtenida;
- relevancia;
- nivel de confianza.

---

18. NIVEL DE CONFIANZA

La información podrá clasificarse como:

CONFIRMADO
PROBABLE
HIPÓTESIS
DESCONOCIDO
DESCARTADO

El motor no deberá convertir automáticamente una hipótesis en información
confirmada.

---

19. DECISIONES

Las decisiones importantes deberán conservar:

- decisión;
- motivo;
- información utilizada;
- fecha;
- proyecto afectado;
- estado.

Esto evita que el motor vuelva a debatir indefinidamente decisiones ya tomadas.

---

20. BLOQUEOS

Cuando el motor no pueda continuar deberá registrar:

- qué intenta hacer;
- qué impide continuar;
- qué ha intentado;
- qué alternativas existen;
- qué acción necesita David.

---

21. ACCIÓN HUMANA

Cuando sea necesaria una acción humana, el estado deberá contener:

ACTION_REQUIRED = true

y:

- acción exacta;
- motivo;
- instrucciones;
- ubicación;
- resultado esperado.

Ejemplo:

"Instalar WordPress en el servidor X."

El motor deberá detener el ciclo y esperar.

---

22. REANUDACIÓN

Cuando David indique:

.

o:

CONTINUAR

el motor deberá:

1. recuperar la sesión;
2. recuperar el estado;
3. comprobar la acción realizada;
4. validar el resultado;
5. continuar desde el último punto válido.

No deberá empezar nuevamente desde cero.

---

23. CHECKPOINT

El motor deberá guardar checkpoints durante el trabajo.

Un checkpoint deberá permitir reconstruir:

- dónde estaba;
- qué había hecho;
- qué resultado tenía;
- qué estaba haciendo;
- qué quedaba;
- cuál era la siguiente acción.

---

24. RECUPERACIÓN ANTE ERROR

Si n8n, el proveedor de IA o cualquier herramienta falla:

1. conservar el último estado válido;
2. registrar el error;
3. intentar recuperación cuando sea seguro;
4. evitar duplicar una operación;
5. continuar si es posible;
6. solicitar intervención únicamente si resulta imprescindible.

---

25. CAMBIO DE MODELO

Cambiar de IA no debe destruir el contexto.

Por ejemplo:

Gemini
↓
límite
↓
Groq
↓
continuar

La memoria y el estado pertenecen al motor, no al modelo.

---

26. CAMBIO DE REPOSITORIO

Cuando David seleccione otro repositorio:

1. cerrar o pausar la sesión actual;
2. guardar checkpoint;
3. cargar el nuevo repositorio;
4. cargar su memoria;
5. cargar sus reglas;
6. recuperar su estado;
7. continuar su propio trabajo.

Nunca mezclar contextos.

---

27. ESTRUCTURA CONCEPTUAL

Una futura implementación podrá utilizar una estructura similar a:

MEMORIA
├── cuentas
├── repositorios
├── proyectos
├── objetivos
├── tareas
├── subtrabajos
├── resultados
├── decisiones
├── fuentes
├── errores
└── checkpoints

El sistema de almacenamiento definitivo queda pendiente de decisión técnica.

---

28. FUENTE DE VERDAD

Debe existir una fuente de verdad para el estado.

La conversación de ChatGPT NO será la fuente de verdad.

El estado persistente será la referencia principal.

Los documentos del repositorio también forman parte de la fuente de verdad
cuando las reglas del proyecto así lo establezcan.

---

29. CONSISTENCIA

Antes de continuar una sesión el motor deberá comprobar:

- que el repositorio existe;
- que el proyecto existe;
- que el objetivo sigue siendo válido;
- que el estado es coherente;
- que no hay una tarea marcada como terminada pero incompleta;
- que no existe un bloqueo pendiente ignorado.

---

30. OBJETIVO FINAL

El sistema debe conseguir:

David ordena trabajar
↓
motor recupera estado
↓
motor trabaja
↓
guarda resultado
↓
selecciona siguiente trabajo
↓
trabaja
↓
guarda resultado
↓
selecciona siguiente trabajo
↓
CONTINÚA

hasta que:

ACCIÓN HUMANA
o
BLOQUEO
o
ERROR CRÍTICO
o
OBJETIVO COMPLETADO
o
PAUSAR/DESACTIVAR.

---

31. PRÓXIMO TRABAJO

Después de este documento se deberá diseñar:

SISTEMA DE PLANIFICACIÓN Y COLA DE TAREAS

Deberá definir cómo el motor decide:

- qué hacer;
- en qué orden;
- qué puede hacer autónomamente;
- qué depende de otra tarea;
- cuándo crear subtrabajos;
- cuándo considerar una tarea terminada;
- cuándo pasar automáticamente a la siguiente.

