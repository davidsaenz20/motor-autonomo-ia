# MOTOR AUTÓNOMO DE IA

## Documento maestro de objetivo y arquitectura

Estado: DISEÑO INICIAL
Versión: 1.1
Fecha: 29/08/2026

---

# 1. OBJETIVO PRINCIPAL

Construir un motor autónomo de inteligencia artificial capaz de trabajar
sobre múltiples repositorios de GitHub pertenecientes a la cuenta:

davidsaenz20

El motor debe poder trabajar tanto sobre los repositorios existentes
actualmente como sobre cualquier repositorio nuevo que David cree en el futuro.

El motor NO debe estar diseñado específicamente para un único repositorio.

Debe ser un sistema reutilizable, multi-repositorio y extensible.

---

# 2. OBJETIVO REAL

David debe poder seleccionar:

- un repositorio;
- un proyecto dentro del repositorio;
- un objetivo;
- una tarea;

y ordenar al motor que trabaje autónomamente.

Ejemplo:

"Trabaja en el proyecto X del repositorio Y."

El motor deberá analizar el entorno de trabajo y continuar ejecutando tareas
sin requerir intervención humana mientras pueda seguir avanzando.

---

# 3. PRINCIPIO FUNDAMENTAL

El motor debe separar:

MOTOR
↓
REPOSITORIO
↓
PROYECTO
↓
OBJETIVO
↓
TRABAJO
↓
SUBTRABAJO

El motor es común.

El repositorio determina sus propias reglas.

El proyecto determina sus propios objetivos.

---

# 4. MULTI-REPOSITORIO

El sistema debe funcionar con todos los repositorios accesibles de:

davidsaenz20

No debe existir una lista cerrada de repositorios.

El sistema debe poder detectar nuevos repositorios posteriormente.

Por tanto:

Repositorio nuevo creado por David
↓
Motor detecta o recibe el repositorio
↓
Lee su estructura
↓
Busca sus reglas
↓
Identifica su metodología
↓
Puede comenzar a trabajar sobre él.

---

# 5. NO DEPENDER DE BASE-PROYECTOS

`base-proyectos` es solamente uno de los posibles entornos de trabajo.

El motor NO debe asumir que todos los repositorios utilizan:

MODO-TRABAJO.md

ni que todos tienen la misma estructura.

Debe detectar las instrucciones específicas de cada repositorio.

---

# 6. EJEMPLO: BASE-PROYECTOS

Si el repositorio seleccionado es:

base-proyectos

el motor deberá identificar sus reglas y metodología.

Por ejemplo:

MODO-TRABAJO.md

y posteriormente trabajar sobre:

06-PROYECTOS

respetando la metodología existente.

---

# 7. EJEMPLO: PENSADOR DE IDEAS

Si existe otro repositorio dedicado a pensar o crear ideas de negocio,
el motor deberá tratarlo como otro entorno independiente.

Ejemplo:

Repositorio:
pensador-de-ideas

Objetivo:
analizar una idea de negocio.

El motor deberá leer las reglas de ese repositorio y seguir su proceso.

No deberá aplicar automáticamente las reglas de `base-proyectos`.

---

# 8. DESCUBRIMIENTO DEL REPOSITORIO

Cuando se seleccione un repositorio, el motor deberá analizar:

- README;
- documentación;
- archivos de reglas;
- instrucciones;
- estructura de carpetas;
- proyectos;
- estados;
- documentación previa;
- archivos de configuración;
- metodología existente.

Deberá determinar qué documentos son relevantes antes de comenzar.

---

# 9. JERARQUÍA DE INSTRUCCIONES

El motor deberá respetar una jerarquía.

Nivel 1:
Reglas del propio sistema motor.

Nivel 2:
Reglas específicas del repositorio.

Nivel 3:
Reglas específicas del proyecto.

Nivel 4:
Objetivo solicitado por David.

Nivel 5:
Tarea actual.

Una regla específica del repositorio no debe confundirse con una regla
general del motor.

---

# 10. ADAPTACIÓN A CADA REPOSITORIO

El motor deberá intentar identificar automáticamente:

- cómo está organizado;
- qué metodología utiliza;
- cómo registra avances;
- dónde guarda estados;
- qué archivos son maestros;
- qué archivos son instrucciones;
- qué trabajos están pendientes;
- qué trabajos están terminados.

Si no puede determinarlo con seguridad, deberá solicitar intervención.

---

# 11. MOTOR DE TRABAJO AUTÓNOMO

Una vez iniciado un trabajo:

RECUPERAR ESTADO
↓
ANALIZAR
↓
PLANIFICAR
↓
EJECUTAR
↓
COMPROBAR
↓
REGISTRAR
↓
BUSCAR SIGUIENTE TRABAJO
↓
CONTINUAR

El motor no debe detenerse simplemente porque haya terminado una tarea.

---

# 12. CONTINUIDAD

La continuidad debe conseguirse mediante un sistema externo de orquestación.

La conversación de ChatGPT no debe utilizarse como mecanismo de ejecución
permanente.

El sistema deberá utilizar un motor como n8n para ejecutar sucesivos ciclos.

Cada ciclo debe recuperar el estado dejado por el ciclo anterior.

---

# 13. PAPEL DE N8N

n8n será el orquestador principal.

Funciones previstas:

- recibir órdenes;
- seleccionar repositorio;
- seleccionar proyecto;
- recuperar estado;
- llamar a modelos;
- ejecutar ciclos;
- encadenar tareas;
- gestionar errores;
- registrar resultados;
- controlar límites;
- detectar bloqueos;
- detectar intervención humana;
- pausar;
- reanudar;
- notificar.

---

# 14. PAPEL DE LA IA

La inteligencia artificial realizará el trabajo intelectual.

Puede encargarse de:

- investigación;
- análisis;
- razonamiento;
- planificación;
- comparación;
- validación;
- clasificación;
- generación;
- revisión;
- documentación;
- detección de problemas;
- propuesta de soluciones;
- selección del siguiente trabajo.

---

# 15. MULTI-IA

El sistema no debe depender obligatoriamente de un único proveedor.

Se estudiarán proveedores con API como:

- Gemini;
- Claude;
- Groq;
- OpenRouter;
- OpenAI;
- otros proveedores compatibles.

El motor deberá estar diseñado para poder cambiar de modelo.

---

# 16. ROTACIÓN DE PROVEEDORES

Cuando sea técnicamente posible, el motor podrá utilizar proveedores alternativos
si uno alcanza:

- límite;
- error;
- indisponibilidad;
- restricción de cuota.

La rotación no debe provocar pérdida de contexto.

El estado debe permanecer independiente del proveedor.

---

# 17. MEMORIA

La memoria del motor debe ser independiente de una conversación concreta.

Debe conservar:

- repositorio;
- proyecto;
- objetivo;
- fase;
- tarea;
- subtrabajo;
- resultados;
- decisiones;
- hipótesis;
- errores;
- bloqueos;
- pendientes;
- intervención requerida;
- siguiente acción.

---

# 18. ESTADO PERSISTENTE

Cada ciclo debe dejar un estado recuperable.

Como mínimo:

REPOSITORIO
PROYECTO
OBJETIVO
FASE
TRABAJO
SUBTRABAJO
ESTADO
RESULTADO
DECISIONES
PENDIENTES
BLOQUEOS
ACCIÓN MANUAL
SIGUIENTE TRABAJO

El formato definitivo queda pendiente de diseño técnico.

---

# 19. VALIDACIÓN

El motor no debe considerar una tarea terminada únicamente porque la IA
haya generado una respuesta.

Debe comprobar, cuando sea posible:

- que el resultado existe;
- que es coherente;
- que cumple el objetivo;
- que no contradice información previa;
- que no convierte hipótesis en hechos.

---

# 20. ESTADOS DE INFORMACIÓN

Cuando corresponda se utilizarán:

CONFIRMADO
PROBABLE
HIPÓTESIS
PENDIENTE
DESCARTADO
DESCONOCIDO

El motor debe evitar presentar como hechos las hipótesis.

---

# 21. CONDICIÓN DE CONTINUACIÓN

Si existe un trabajo útil que pueda realizarse autónomamente:

CONTINUAR.

No detenerse simplemente para informar a David.

No enviar mensajes por cada tarea completada.

No solicitar confirmación innecesaria.

---

# 22. CONDICIONES DE PARADA

El motor podrá detenerse cuando:

1. necesite una acción manual imprescindible;
2. exista un bloqueo que no pueda resolver;
3. exista un límite técnico;
4. exista un error crítico;
5. David ordene PAUSAR;
6. David ordene DESACTIVAR.

---

# 23. INTERVENCIÓN HUMANA

Ejemplos:

- instalar software;
- configurar WordPress;
- configurar un servidor;
- introducir credenciales;
- introducir API keys;
- modificar manualmente un archivo;
- copiar y pegar contenido;
- aprobar una acción;
- contratar un servicio;
- realizar una acción física.

Cuando sea necesaria:

PAUSAR
↓
AVISAR A DAVID
↓
ESPERAR

---

# 24. REANUDACIÓN

Después de realizar la acción manual, David podrá enviar:

.

o:

CONTINUAR

El motor deberá:

1. recuperar estado;
2. comprobar el resultado;
3. continuar desde el punto válido;
4. evitar repetir trabajo terminado.

---

# 25. CANAL DE CONTROL

Primera opción:

Telegram.

Se estudiará posteriormente:

WhatsApp.

David deberá poder:

ACTIVAR
PAUSAR
CONTINUAR
DESACTIVAR
CONSULTAR ESTADO

---

# 26. NOTIFICACIONES

Mientras el motor pueda continuar:

NO MOLESTAR.

Solo informar cuando sea relevante:

- acción manual;
- bloqueo;
- error crítico;
- límite;
- solicitud expresa de estado.

---

# 27. GITHUB

GitHub será una fuente principal de proyectos y documentación.

El motor deberá poder leer los repositorios accesibles de:

davidsaenz20

y trabajar sobre ellos respetando sus reglas.

No se debe asumir que el motor puede escribir directamente en GitHub desde
la conversación de ChatGPT.

Las operaciones de escritura deberán diseñarse mediante mecanismos autorizados
y seguros.

Si una modificación requiere intervención manual de David, el motor deberá
detenerse y proporcionar:

- repositorio;
- ruta;
- archivo;
- contenido;
- acción necesaria.

---

# 28. SEGURIDAD

El motor no debe:

- ejecutar acciones destructivas sin control;
- borrar información sin autorización;
- sobrescribir datos importantes sin protección;
- publicar contenido automáticamente sin reglas;
- exponer credenciales;
- guardar API keys dentro de documentación pública.

Las credenciales deberán mantenerse fuera de los archivos de proyecto cuando
sea técnicamente posible.

---

# 29. ESCALABILIDAD

El sistema debe poder crecer desde:

1 repositorio
↓
varios repositorios
↓
todos los repositorios de David
↓
nuevos repositorios futuros.

No debe ser necesario reconstruir el motor cada vez que aparezca un repositorio.

---

# 30. DESCUBRIMIENTO DE NUEVOS REPOSITORIOS

El motor deberá poder:

- consultar repositorios disponibles;
- detectar nuevos repositorios;
- actualizar su inventario;
- permitir seleccionar un nuevo repositorio;
- analizar automáticamente su estructura.

No debe existir una configuración rígida que obligue a modificar el código
cada vez que David cree un repositorio nuevo.

---

# 31. AISLAMIENTO

Cada repositorio debe conservar:

- sus reglas;
- sus proyectos;
- sus estados;
- su documentación;
- su metodología.

El motor debe evitar mezclar información entre repositorios.

---

# 32. CONTEXTO

El motor debe construir el contexto necesario para cada ejecución.

No debe enviar indiscriminadamente todo un repositorio a una IA.

Debe seleccionar la información relevante:

REGLAS
+
ESTADO
+
OBJETIVO
+
TAREA
+
CONTEXTO NECESARIO

---

# 33. COSTES

El sistema deberá priorizar:

- APIs gratuitas cuando sean suficientes;
- modelos económicos;
- rotación inteligente;
- evitar llamadas innecesarias;
- almacenamiento eficiente;
- control de consumo.

No asumir que una API gratuita es ilimitada.

---

# 34. FUNCIONAMIENTO 24/7

El objetivo final es que el motor pueda funcionar de forma permanente.

Se estudiarán:

OPCIÓN A
ordenador encendido;

OPCIÓN B
servidor/VPS;

OPCIÓN C
otras infraestructuras adecuadas.

La decisión se tomará después de analizar:

- coste;
- estabilidad;
- mantenimiento;
- seguridad;
- escalabilidad;
- facilidad de administración.

---

# 35. DESARROLLO DESDE MÓVIL

Antes de disponer de ordenador se puede realizar:

- arquitectura;
- documentación;
- diseño de agentes;
- diseño de estados;
- diseño de workflows;
- selección de proveedores;
- planificación;
- análisis;
- preparación de archivos.

---

# 36. DESARROLLO DESDE ORDENADOR

Cuando sea necesario se realizará:

- instalación de n8n;
- configuración del servidor;
- APIs;
- credenciales;
- workflows;
- Telegram;
- WhatsApp;
- GitHub;
- pruebas;
- funcionamiento 24/7.

---

# 37. ARQUITECTURA PREVISTA

MOTOR AUTÓNOMO
│
├── ORQUESTADOR
│     └── n8n
│
├── CAPA IA
│     ├── Gemini
│     ├── Claude
│     ├── Groq
│     ├── OpenRouter
│     └── otros
│
├── CAPA REPOSITORIOS
│     └── GitHub / davidsaenz20
│
├── CAPA MEMORIA
│
├── CAPA ESTADO
│
├── CAPA VALIDACIÓN
│
├── CAPA CONTROL
│     └── Telegram / WhatsApp
│
└── CAPA SEGURIDAD

---

# 38. REPOSITORIOS INICIALES

El sistema deberá poder trabajar inicialmente con los repositorios existentes
en la cuenta:

davidsaenz20

La lista exacta de repositorios deberá descubrirse mediante GitHub y no
codificarse manualmente en este documento.

---

# 39. REPOSITORIOS FUTUROS

Cualquier repositorio nuevo creado posteriormente por David deberá poder
incorporarse al sistema sin rediseñar el motor.

El sistema debe tratar GitHub como un entorno dinámico.

---

# 40. PRINCIPIO DE NO ACOPLAMIENTO

El motor no debe depender de:

- un único repositorio;
- una única IA;
- un único proyecto;
- un único canal;
- una única estructura documental.

El diseño debe permitir sustituir cada componente.

---

# 41. OBJETIVO FINAL DE USO

David debe poder indicar:

"Trabaja en este repositorio y este proyecto."

El motor deberá encargarse de:

LEER
→
COMPRENDER
→
RECUPERAR ESTADO
→
PLANIFICAR
→
TRABAJAR
→
VALIDAR
→
REGISTRAR
→
ELEGIR SIGUIENTE TRABAJO
→
CONTINUAR

sin intervención humana innecesaria.

---

# 42. PRINCIPIO DE INTERVENCIÓN MÍNIMA

La intervención humana debe ser una excepción.

El objetivo no es que David supervise cada paso.

El objetivo es que David únicamente intervenga cuando el sistema no pueda
continuar legítimamente por sí mismo.

---

# 43. PRINCIPIO DE TRAZABILIDAD

Todo trabajo importante deberá poder reconstruirse.

Debe ser posible saber:

qué se hizo;
por qué se hizo;
qué información se utilizó;
qué resultado produjo;
qué decisión se tomó;
qué queda pendiente;
qué debe hacerse después.

---

# 44. PRINCIPIO DE NO REPETICIÓN

Antes de realizar un trabajo, el motor debe comprobar si:

- ya se realizó;
- existe un resultado válido;
- existe una investigación previa;
- existe una decisión previa.

No repetir trabajo innecesariamente.

---

# 45. PRINCIPIO DE AUTONOMÍA CONTROLADA

Autonomía no significa ausencia de límites.

El sistema debe poder trabajar solo, pero debe detenerse cuando:

- necesite autorización;
- necesite acción física;
- necesite credenciales;
- pueda producir una consecuencia importante;
- exista incertidumbre crítica;
- exista riesgo de pérdida de información.

---

# 46. ESTADO DEL PROYECTO

Estado actual:

DISEÑO CONCEPTUAL.

Decisiones confirmadas:

- repositorio independiente;
- motor multi-repositorio;
- compatibilidad con todos los repositorios de `davidsaenz20`;
- compatibilidad con repositorios futuros;
- n8n como orquestador previsto;
- IA mediante APIs;
- arquitectura multi-IA;
- memoria persistente;
- estados persistentes;
- adaptación a las reglas de cada repositorio;
- Telegram como primera opción de control;
- WhatsApp como integración posterior a estudiar;
- funcionamiento por ciclos;
- continuidad automática;
- intervención humana únicamente cuando sea necesaria.

---

# 47. DECISIONES TODAVÍA PENDIENTES

Pendiente de diseñar:

- arquitectura técnica definitiva;
- estructura exacta de n8n;
- modelo de memoria;
- modelo de estados;
- sistema de agentes;
- sistema de planificación;
- sistema de validación;
- detección de bloqueos;
- mecanismo de continuidad;
- selección de APIs;
- rotación de modelos;
- acceso GitHub;
- seguridad;
- Telegram;
- WhatsApp;
- servidor/VPS;
- monitorización;
- recuperación ante errores.

---

# 48. PRÓXIMO TRABAJO

El siguiente trabajo válido es:

DISEÑAR LA ARQUITECTURA TÉCNICA DEL MOTOR MULTI-REPOSITORIO.

No comenzar todavía instalando n8n.

Primero definir:

1. componentes;
2. flujo;
3. datos;
4. memoria;
5. estados;
6. agentes;
7. planificación;
8. continuidad;
9. GitHub;
10. IA;
11. Telegram;
12. seguridad.

---

# 49. REGLA DE RECUPERACIÓN

Cuando el proyecto se retome:

1. leer este documento;
2. comprobar el estado real;
3. revisar documentación existente;
4. identificar decisiones confirmadas;
5. identificar pendientes;
6. localizar el último punto válido;
7. continuar desde ahí.

No empezar desde cero.

No inventar avances.

No repetir trabajos ya terminados.

---

# 50. DOCUMENTO VIVO

Este documento es una guía maestra.

Debe actualizarse cuando exista una decisión estructural importante.

Toda modificación debe conservar:

- objetivo;
- arquitectura;
- decisiones;
- pendientes;
- estado;
- próximo punto válido.

El motor debe poder recuperar el proyecto incluso después de largos periodos
sin depender de la memoria de una conversación concreta.

