AUDITORÍA DE DOCUMENTACIÓN

1. PROPÓSITO

Este documento establece cómo se debe revisar, ordenar y mantener la
documentación del repositorio:

"davidsaenz20/motor-autonomo-ia"

Su finalidad es evitar:

- documentos duplicados;
- reglas contradictorias;
- decisiones repetidas;
- diseños incompatibles;
- archivos que describen la misma función con nombres diferentes;
- documentación obsoleta;
- creación innecesaria de nuevos archivos.

Este documento NO sustituye a la documentación técnica.

Su función es determinar qué documento tiene autoridad sobre cada asunto.

---

2. PRINCIPIO FUNDAMENTAL

Antes de crear un nuevo documento:

IDEA NUEVA
↓
BUSCAR DOCUMENTACIÓN EXISTENTE
↓
¿YA EXISTE UN DOCUMENTO PARA ESTO?
├── SÍ → ACTUALIZAR
└── NO → CREAR

No crear un documento nuevo simplemente porque resulte más cómodo.

---

3. REPOSITORIO DEL MOTOR

Repositorio principal:

"davidsaenz20/motor-autonomo-ia"

Este repositorio contiene el diseño del Motor Autónomo de IA.

El motor debe ser independiente de:

"davidsaenz20/base-proyectos"

y de cualquier otro repositorio concreto.

El objetivo del motor es funcionar como infraestructura reutilizable para
múltiples repositorios.

---

4. ARQUITECTURA DOCUMENTAL ACTUAL

La estructura actual contiene:

00-DIRECCION
01-SISTEMA
02-MEMORIA
03-PLANIFICACION
04-AGENTE
05-REPOSITORIOS
06-HERRAMIENTAS
07-IA
08-VALIDACION
09-INTERVENCION-HUMANA
10-N8N
11-COMUNICACION
12-SEGURIDAD
13-INFRAESTRUCTURA
14-IMPLEMENTACION

Esta estructura se considera válida como arquitectura inicial.

No debe modificarse por motivos estéticos.

---

5. DOCUMENTO MAESTRO

Documento:

"00-DIRECCION/OBJETIVO-Y-ARQUITECTURA.md"

Función:

Definir:

- qué es el motor;
- para qué existe;
- qué objetivos tiene;
- qué principios debe respetar;
- cómo se relaciona con repositorios;
- cómo se relaciona con proyectos;
- qué papel tiene n8n;
- qué papel tiene la IA;
- qué papel tiene GitHub;
- qué papel tiene la memoria;
- qué papel tiene la intervención humana.

Este documento tiene autoridad sobre la visión general del sistema.

No debe utilizarse para describir detalles concretos de implementación.

---

6. ARQUITECTURA TÉCNICA

Documento:

"01-SISTEMA/ARQUITECTURA-TECNICA.md"

Función:

Definir cómo se estructura técnicamente el motor.

Debe contener:

- componentes;
- relaciones;
- interfaces;
- flujo general;
- separación de responsabilidades;
- dependencias.

Si una decisión afecta a la arquitectura técnica general debe registrarse aquí
o en un documento especializado claramente relacionado.

---

7. MEMORIA Y ESTADO

Documento:

"02-MEMORIA/ESTADO-Y-MEMORIA.md"

Autoridad para:

- memoria;
- estado;
- persistencia;
- recuperación;
- checkpoints;
- continuidad.

No crear otro documento específico de memoria salvo que exista una necesidad
real de separar un componente suficientemente grande.

---

8. PLANIFICACIÓN

Documento:

"03-PLANIFICACION/COLA-Y-PLANIFICACION.md"

Autoridad para:

- cola de tareas;
- selección de siguiente tarea;
- prioridades;
- dependencias;
- planificación;
- generación de trabajo;
- detección de ausencia de trabajo.

---

9. AGENTE

Documento:

"04-AGENTE/PROTOCOLO-DEL-AGENTE.md"

Autoridad para:

- comportamiento del agente;
- ciclo intelectual;
- razonamiento;
- toma de decisiones;
- selección de acciones;
- comportamiento autónomo.

El agente no debe confundirse con n8n.

---

10. REPOSITORIOS

Documento:

"05-REPOSITORIOS/DESCUBRIMIENTO-Y-ADAPTACION.md"

Autoridad para:

- descubrimiento de repositorios;
- identificación de repositorios nuevos;
- lectura inicial;
- adaptación;
- detección de reglas;
- separación entre reglas del motor y reglas del repositorio;
- aislamiento de proyectos.

El motor debe poder trabajar con:

REPOSITORIO A
REPOSITORIO B
REPOSITORIO C
...
REPOSITORIO NUEVO

sin modificar su arquitectura central.

---

11. HERRAMIENTAS

Documento:

"06-HERRAMIENTAS/SISTEMA-DE-HERRAMIENTAS.md"

Autoridad para:

- herramientas;
- capacidades externas;
- selección;
- disponibilidad;
- interfaces de herramientas.

No introducir aquí la lógica del agente.

---

12. INTELIGENCIA ARTIFICIAL

Documento:

"07-IA/SISTEMA-MULTI-IA-Y-SELECTOR.md"

Autoridad para:

- proveedores;
- modelos;
- selección de modelo;
- fallback;
- disponibilidad;
- límites;
- rotación.

El proveedor de IA debe ser intercambiable.

El estado del proyecto nunca debe depender de un proveedor concreto.

---

13. VALIDACIÓN

Documento:

"08-VALIDACION/SISTEMA-DE-VALIDACION-Y-CALIDAD.md"

Autoridad para:

- validación;
- calidad;
- comprobación de resultados;
- detección de resultados incorrectos;
- criterios de aceptación.

Una respuesta generada por una IA no equivale automáticamente a una tarea
válida.

---

14. INTERVENCIÓN HUMANA

Documento:

"09-INTERVENCION-HUMANA/PROTOCOLO-DE-INTERVENCION.md"

Autoridad para:

- solicitudes a David;
- bloqueos humanos;
- acciones manuales;
- espera;
- confirmación;
- reanudación.

La intervención humana debe ser excepcional cuando el motor pueda continuar
por sí mismo.

---

15. N8N

Documento:

"10-N8N/MOTOR-DE-EJECUCION-N8N.md"

Autoridad para:

- funcionamiento de n8n;
- ejecución;
- orquestación;
- ciclos;
- disparadores;
- encadenamiento.

n8n es el orquestador.

No es el cerebro intelectual del sistema.

---

16. COMUNICACIÓN

Documentos:

"11-COMUNICACION/SISTEMA-MULTICANAL.md"

y:

"11-COMUNICACION/TELEGRAM-CONTROL.md"

Responsabilidades:

SISTEMA-MULTICANAL
↓
arquitectura general de comunicación

TELEGRAM-CONTROL
↓
Telegram específicamente

No duplicar la definición del control en ambos documentos.

---

17. SEGURIDAD

Documento:

"12-SEGURIDAD/SEGURIDAD-Y-CONTROL-DE-ACCESO.md"

Autoridad para:

- credenciales;
- permisos;
- autenticación;
- acceso;
- secretos;
- exposición externa;
- seguridad de operaciones.

---

18. INFRAESTRUCTURA

Documento:

"13-INFRAESTRUCTURA/INFRAESTRUCTURA-Y-DESPLIEGUE.md"

Autoridad para:

- PC;
- servidor;
- VPS;
- Docker;
- despliegue;
- disponibilidad;
- migración;
- funcionamiento 24/7.

La decisión de utilizar PC inicialmente y VPS posteriormente pertenece a esta
área.

No crear documentos adicionales de infraestructura sin necesidad.

---

19. IMPLEMENTACIÓN

Actualmente existen varios documentos dentro de:

"14-IMPLEMENTACION"

Entre ellos:

CONFIGURACION-IA-Y-HERRAMIENTAS.md
IMPLEMENTACION-PC.md
MODELO-DATOS.md
MVP-TECNICO.md
PLAN-MAESTRO-DE-IMPLEMENTACION.md
WORKFLOWS-N8N.md
CHECKLIST-PRUEBAS-MVP.md

Estos documentos tienen funciones diferentes y no deben fusionarse
automáticamente.

---

20. FUNCIÓN DE CADA DOCUMENTO DE IMPLEMENTACIÓN

MVP-TECNICO.md

Define qué debe contener la primera versión funcional.

Autoridad:

MVP

---

PLAN-MAESTRO-DE-IMPLEMENTACION.md

Define el orden global de construcción.

Autoridad:

ORDEN DE IMPLEMENTACIÓN

---

WORKFLOWS-N8N.md

Define los workflows necesarios.

Autoridad:

WORKFLOWS

---

MODELO-DATOS.md

Define la estructura de datos.

Autoridad:

DATOS

---

CONFIGURACION-IA-Y-HERRAMIENTAS.md

Define la configuración de proveedores de IA y herramientas.

Autoridad:

CONFIGURACIÓN IA + HERRAMIENTAS

---

IMPLEMENTACION-PC.md

Define cómo comenzar físicamente la implementación utilizando el PC.

Autoridad:

PUESTA EN MARCHA INICIAL

---

CHECKLIST-PRUEBAS-MVP.md

Define las pruebas necesarias para considerar funcional el MVP.

Autoridad:

PRUEBAS

---

21. POSIBLE DUPLICIDAD

Los documentos anteriores pueden contener información relacionada.

Esto NO significa automáticamente que exista un problema.

La regla será:

VISIÓN
→ OBJETIVO-Y-ARQUITECTURA

DISEÑO
→ ARQUITECTURA-TECNICA

IMPLEMENTACIÓN
→ PLAN-MAESTRO-DE-IMPLEMENTACION

MVP
→ MVP-TECNICO

N8N
→ WORKFLOWS-N8N / MOTOR-DE-EJECUCION-N8N

DATOS
→ MODELO-DATOS

IA/HERRAMIENTAS
→ CONFIGURACION-IA-Y-HERRAMIENTAS

PC
→ IMPLEMENTACION-PC

PRUEBAS
→ CHECKLIST-PRUEBAS-MVP

---

22. REGLA DE AUTORIDAD

Cuando dos documentos parezcan contradecirse:

DOCUMENTO ESPECIALIZADO

tendrá prioridad sobre un documento general respecto a su propia materia.

Ejemplo:

OBJETIVO-Y-ARQUITECTURA

define que debe existir memoria.

Pero:

ESTADO-Y-MEMORIA

define cómo funciona técnicamente la memoria.

---

23. DECISIONES

Una decisión importante debe tener un único lugar de autoridad.

No copiar la misma decisión en diez documentos.

Si es necesario mencionarla en otro archivo:

REFERENCIA

en lugar de duplicar toda la explicación.

---

24. CAMBIOS

Si cambia una decisión:

IDENTIFICAR DOCUMENTO AUTORITATIVO
↓
ACTUALIZAR
↓
BUSCAR REFERENCIAS ANTIGUAS
↓
CORREGIR CONTRADICCIONES
↓
REGISTRAR CAMBIO

---

25. DOCUMENTACIÓN OBSOLETA

Un documento no debe borrarse inmediatamente por estar desactualizado.

Primero comprobar:

¿TIENE INFORMACIÓN HISTÓRICA ÚTIL?

Si la tiene:

HISTÓRICO

Si no:

ELIMINAR

pero únicamente después de comprobar dependencias y referencias.

---

26. NO CREAR ARCHIVOS POR CADA IDEA

No se debe crear un archivo nuevo para cada:

- decisión;
- problema;
- prueba;
- duda;
- mejora.

Debe utilizarse el documento existente adecuado.

---

27. CRITERIO PARA CREAR UN NUEVO DOCUMENTO

Solo crear un nuevo documento cuando:

1. exista una responsabilidad claramente diferenciada;
2. el contenido tenga suficiente entidad;
3. no encaje razonablemente en un documento existente;
4. separar el contenido mejore la mantenibilidad;
5. no genere duplicación.

---

28. AUDITORÍA PERIÓDICA

Antes de iniciar una nueva fase importante:

REVISAR DOCUMENTACIÓN
↓
DETECTAR DUPLICADOS
↓
DETECTAR CONTRADICCIONES
↓
ACTUALIZAR AUTORIDADES
↓
CONTINUAR

---

29. ESTADO ACTUAL

La arquitectura documental actual se considera:

"FUNCIONAL PERO PENDIENTE DE REVISIÓN DETALLADA"

No se debe considerar todavía que todos los documentos sean definitivos.

La auditoría debe continuar sobre el contenido real de los documentos.

---

30. DOCUMENTOS PRIORITARIOS PARA REVISAR

La siguiente revisión debe comparar especialmente:

00-DIRECCION/OBJETIVO-Y-ARQUITECTURA.md
01-SISTEMA/ARQUITECTURA-TECNICA.md

02-MEMORIA/ESTADO-Y-MEMORIA.md
03-PLANIFICACION/COLA-Y-PLANIFICACION.md
04-AGENTE/PROTOCOLO-DEL-AGENTE.md
05-REPOSITORIOS/DESCUBRIMIENTO-Y-ADAPTACION.md

07-IA/SISTEMA-MULTI-IA-Y-SELECTOR.md
08-VALIDACION/SISTEMA-DE-VALIDACION-Y-CALIDAD.md
10-N8N/MOTOR-DE-EJECUCION-N8N.md

14-IMPLEMENTACION/MVP-TECNICO.md
14-IMPLEMENTACION/PLAN-MAESTRO-DE-IMPLEMENTACION.md
14-IMPLEMENTACION/WORKFLOWS-N8N.md
14-IMPLEMENTACION/MODELO-DATOS.md
14-IMPLEMENTACION/CONFIGURACION-IA-Y-HERRAMIENTAS.md
14-IMPLEMENTACION/IMPLEMENTACION-PC.md
14-IMPLEMENTACION/CHECKLIST-PRUEBAS-MVP.md

---

31. NO MODIFICAR TODAVÍA

Esta auditoría no autoriza todavía a borrar, fusionar o reescribir documentos.

Primero debe realizarse la comparación de contenido.

---

32. SIGUIENTE ACCIÓN DEL MOTOR

El siguiente trabajo válido será:

LEER DOCUMENTOS PRIORITARIOS
↓
COMPARAR RESPONSABILIDADES
↓
DETECTAR DUPLICIDADES
↓
DETECTAR CONTRADICCIONES
↓
DETECTAR HUECOS
↓
PROPONER CAMBIOS

---

33. RESULTADO ESPERADO

Al terminar la auditoría deberá existir:

DOCUMENTO
↓
RESPONSABILIDAD
↓
AUTORIDAD
↓
DEPENDENCIAS
↓
ESTADO

para cada pieza importante de la arquitectura.

---

34. PRINCIPIO DE CONSERVACIÓN

No cambiar algo que ya funciona simplemente para hacerlo diferente.

La evolución debe ser:

ACTUAL
↓
ANALIZAR
↓
MEJORAR

y no:

ACTUAL
↓
BORRAR
↓
RECONSTRUIR

sin una razón demostrada.

---

35. RELACIÓN CON OTROS REPOSITORIOS

Este documento pertenece exclusivamente al Motor Autónomo.

No debe imponer su estructura documental a:

"base-proyectos"

ni a cualquier otro repositorio.

El motor debe adaptarse a cada repositorio.

---

36. PRINCIPIO MULTI-REPOSITORIO

El sistema debe poder trabajar sobre:

davidsaenz20/base-proyectos

y sobre cualquier otro repositorio accesible de:

davidsaenz20

incluidos repositorios que David cree posteriormente.

La arquitectura del motor no debe depender de conocer previamente el nombre
de todos esos repositorios.

---

37. CONCLUSIÓN

La documentación actual ya contiene una arquitectura suficientemente amplia
para continuar.

Por tanto:

NO CREAR MÁS DOCUMENTACIÓN ARQUITECTÓNICA GENERAL HASTA TERMINAR ESTA
AUDITORÍA.

El siguiente objetivo es consolidar lo existente.

---

38. ESTADO

"AUDITORIA_PENDIENTE"

---

39. SIGUIENTE ARCHIVO / ACCIÓN

No crear todavía otro documento.

La siguiente acción válida es revisar el contenido de los documentos
prioritarios y, a partir de esa revisión, decidir si:

MANTENER
ACTUALIZAR
FUSIONAR
RENOMBRAR
ARCHIVAR
ELIMINAR

cada documento.

Fin del documento.

