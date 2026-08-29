IMPLEMENTACIÓN PC

1. OBJETIVO

Este documento define las acciones necesarias para poner en funcionamiento el
Motor Autónomo de IA utilizando inicialmente el ordenador de David.

El PC será la infraestructura inicial.

No será necesario contratar un VPS para el MVP.

---

2. ARQUITECTURA INICIAL

PC
 ↓
n8n
 ↓
Motor Autónomo
 ├── IA
 ├── Internet
 ├── GitHub
 ├── almacenamiento
 └── Telegram

Mientras el PC esté encendido y n8n esté funcionando, el motor podrá trabajar.

---

3. OBJETIVO DEL MVP

El primer objetivo no es construir un sistema empresarial.

Es demostrar que:

ORDEN
↓
PLAN
↓
TRABAJO
↓
VALIDACIÓN
↓
GUARDADO
↓
SIGUIENTE TAREA

puede ejecutarse automáticamente.

---

4. REQUISITOS

Cuando se utilice el PC deberán comprobarse:

Sistema operativo
Node.js
npm
n8n
Conexión a Internet
Navegador
Git

No instalar software innecesario antes de comprobar qué existe.

---

5. COMPROBACIÓN DE N8N

Primera comprobación:

n8n --version

Si funciona:

N8N DISPONIBLE

Si no funciona:

N8N NO DISPONIBLE

No asumir que una instalación anterior está correctamente configurada.

---

6. COMPROBACIÓN DE NODE

Comprobar:

node --version

y:

npm --version

Registrar las versiones.

---

7. COMPROBACIÓN DE GIT

Comprobar:

git --version

Git será útil para trabajar con los repositorios.

---

8. ARRANQUE DE N8N

Si n8n está correctamente instalado:

n8n

Comprobar que inicia correctamente.

---

9. INTERFAZ

Abrir la interfaz web local de n8n desde el navegador.

No modificar todavía workflows existentes sin comprobar qué contienen.

---

10. COPIA DE SEGURIDAD

Antes de realizar cambios importantes:

BACKUP

Guardar cualquier configuración o workflow que pueda ser importante.

---

11. CREDENCIALES

Las credenciales deberán introducirse mediante el sistema seguro de n8n.

Nunca escribir claves dentro de:

.md
.json
.js
.txt

versionados en GitHub.

---

12. TELEGRAM

Crear un bot de Telegram para el MVP.

El bot será el canal inicial de control.

Objetivo:

David
 ↓
Telegram
 ↓
n8n

---

13. COMANDOS TELEGRAM

Inicialmente:

/continuar
/pausar
/detener
/estado

Más adelante podrá utilizarse lenguaje natural.

---

14. PRIMER WORKFLOW

Crear:

WF-06 ENTRADA TELEGRAM

Objetivo:

Telegram
↓
recibir mensaje
↓
identificar comando
↓
normalizar
↓
enviar al controlador

---

15. PRUEBA TELEGRAM

Enviar:

/estado

El sistema deberá responder.

Si no responde:

NO CONTINUAR

hasta solucionar la conexión.

---

16. ALMACENAMIENTO

El motor necesita almacenamiento persistente.

Antes de elegir la tecnología definitiva se evaluarán:

SQLite
PostgreSQL
Google Sheets
Airtable
otro almacenamiento adecuado

---

17. CRITERIO DE ELECCIÓN

Para el MVP priorizar:

SIMPLE
BARATO
PERSISTENTE
COMPATIBLE CON N8N
FÁCIL DE RESPALDAR

No utilizar una infraestructura compleja sin necesidad.

---

18. PRIMER PROYECTO DE PRUEBA

No comenzar directamente con un proyecto empresarial real.

Crear un proyecto de prueba:

TEST-AUTONOMOUS-MOTOR

---

19. OBJETIVO DE PRUEBA

Ejemplo:

Investigar tres ideas de pequeños negocios online
y producir una comparación final.

El objetivo debe poder dividirse en tareas.

---

20. CREACIÓN DEL PROYECTO

Registrar:

PROJECT_ID
PROJECT_NAME
DESCRIPTION
REPOSITORY
STATUS

---

21. CREACIÓN DE SESIÓN

Crear:

SESSION_ID

Estado:

RUNNING

---

22. PRIMERA TAREA

Crear:

TASK-001

Estado:

READY

---

23. CONTROLADOR

Crear:

WF-00 CONTROL

Debe leer el estado y determinar qué hacer.

---

24. REGLA PRINCIPAL

Si:

MOTOR_STATUS = AUTONOMOUS

y existe:

TASK_STATUS = READY

entonces:

EXECUTE

---

25. ESTADO

Crear:

WF-01 ESTADO

Debe recuperar:

PROJECT
SESSION
TASK
CHECKPOINT
MEMORY
QUEUE

---

26. PLANIFICADOR

Crear:

WF-02 PLANIFICADOR

Su primera versión debe ser sencilla.

Debe seleccionar la siguiente tarea existente.

No intentar todavía crear un planificador extremadamente inteligente.

---

27. EJECUTOR

Crear:

WF-03 EJECUTOR

Primera versión:

TASK
↓
IA
↓
RESULT

---

28. PRIMERA IA

Utilizar inicialmente un único proveedor.

No implementar todavía fallback múltiple.

Primero demostrar:

n8n → IA → respuesta

---

29. PRUEBA IA

Crear una tarea sencilla.

Ejemplo:

Explica en cinco puntos qué hace un asistente virtual para empresas.

Comprobar que n8n recibe una respuesta válida.

---

30. RESULTADO

Guardar:

RESULT_ID
TASK_ID
EXECUTION_ID
OUTPUT
STATUS

---

31. VALIDADOR

Crear:

WF-04 VALIDADOR

Primera versión:

RESULT
↓
¿RESPONDE A LA TAREA?
├── YES → VALID
└── NO → INVALID

---

32. CHECKPOINT

Crear:

WF-05 CHECKPOINT

Después de completar una tarea:

TASK
↓
RESULT
↓
VALIDATION
↓
CHECKPOINT

---

33. PRUEBA DE CONTINUACIÓN

Crear dos tareas:

TASK-001
TASK-002

Ejecutar:

/continuar

Esperar:

TASK-001
↓
TASK-002

sin enviar otro comando.

---

34. PRUEBA DE TRES TAREAS

Después de superar la prueba anterior:

TASK-001
TASK-002
TASK-003

El motor deberá completar las tres automáticamente.

---

35. PRUEBA DE PAUSA

Enviar:

/pausar

El sistema deberá:

guardar checkpoint
↓
PAUSED

---

36. PRUEBA DE REANUDACIÓN

Enviar:

/continuar

El sistema deberá:

LOAD CHECKPOINT
↓
CONTINUE

---

37. PRUEBA DE DETENCIÓN

Enviar:

/detener

El motor deberá detenerse.

---

38. PRUEBA DE ESTADO

Enviar:

/estado

Debe devolver:

PROJECT
TASK ACTUAL
COMPLETED
PENDING
STATUS

---

39. PRUEBA DE ACCIÓN HUMANA

Crear una tarea que requiera una acción de David.

Ejemplo:

Necesito que David confirme un dato.

El motor deberá:

WAITING_HUMAN
↓
NOTIFY

---

40. RESPUESTA HUMANA

David responde:

HECHO

El motor deberá identificar la acción pendiente.

---

41. CONTINUACIÓN DESPUÉS DE HECHO

HECHO
↓
VALIDATE ACTION
↓
CHECKPOINT
↓
CONTINUE

---

42. PRUEBA DE ERROR

Provocar un error controlado.

El motor deberá registrar:

ERROR

y determinar si puede reintentarse.

---

43. REINTENTO

Si es recuperable:

ERROR
↓
RETRY
↓
EXECUTE

---

44. ERROR NO RECUPERABLE

Si no puede solucionarse automáticamente:

BLOCKED

o:

ACTION_REQUIRED

---

45. PRUEBA DE REINICIO

Detener n8n después de crear un checkpoint.

Volver a iniciar n8n.

Comprobar:

LOAD CHECKPOINT

El motor no deberá perder el estado.

---

46. PRUEBA DE RECUPERACIÓN

Si:

TASK-003 = COMPLETED

y:

TASK-004 = READY

después del reinicio debe continuar desde:

TASK-004

No desde TASK-001.

---

47. GITHUB

Una vez validado el núcleo:

conectar GitHub

Inicialmente con operaciones de lectura.

---

48. PRUEBA GITHUB

El motor deberá poder identificar un repositorio autorizado y leer la
documentación necesaria.

---

49. MULTI-REPOSITORIO

Después de superar la prueba anterior:

PROJECT A → REPOSITORY A
PROJECT B → REPOSITORY B

Comprobar que el contexto permanece separado.

---

50. BASE-PROYECTOS

Con el motor ya validado:

registrar base-proyectos

como uno de los repositorios que puede utilizar el motor.

---

51. PENSADOR DE IDEAS

Registrar posteriormente el repositorio del sistema de pensamiento de ideas
de negocio.

El motor debe tratarlo como otro proyecto/repositorio, no como una excepción
arquitectónica.

---

52. NUEVOS REPOSITORIOS

La arquitectura debe permitir registrar nuevos repositorios posteriormente.

No modificar el núcleo para cada repositorio nuevo.

---

53. INVESTIGACIÓN WEB

Una vez validado el ciclo básico:

añadir WEB_SEARCH

y posteriormente:

WEB_FETCH

---

54. PRIMERA INVESTIGACIÓN REAL

Probar:

SEARCH
↓
COLLECT
↓
ANALYZE
↓
VALIDATE
↓
STORE

---

55. KEYWORDS

Después:

KEYWORD_RESEARCH

para probar investigaciones de demanda y búsqueda.

---

56. INTENCIÓN

Incorporar:

SEARCH_INTENT

como tipo de análisis.

---

57. INVESTIGACIÓN DE PROYECTOS

Una vez que el motor pueda investigar:

PROJECT
↓
OBJECTIVE
↓
TASKS
↓
RESEARCH
↓
RESULTS
↓
DECISIONS

---

58. AUTOMATIZACIÓN DE URLs

Solo después de validar el motor podrá utilizarse para el trabajo de
generación e investigación relacionado con proyectos de URLs.

---

59. NO GENERAR MASIVAMENTE AL PRINCIPIO

Primero:

1 proyecto
↓
1 investigación
↓
validación

Después:

10
↓
100
↓
1000+

según los resultados.

---

60. MONITORIZACIÓN

Durante las primeras sesiones comprobar:

CPU
RAM
DISCO
ERRORES
TIEMPO
CONSUMO DE API

No asumir que el PC puede trabajar indefinidamente sin supervisión.

---

61. SESIONES DE TRABAJO

Mientras el motor esté en fase experimental:

PC ENCENDIDO
↓
N8N ACTIVO
↓
MOTOR ACTIVO
↓
SUPERVISIÓN OCASIONAL

---

62. VPS

No contratar un VPS antes de comprobar que realmente es necesario.

Solo migrar cuando:

- el motor necesite funcionar 24/7;
- el PC no sea suficiente;
- se requiera acceso permanente;
- la estabilidad lo justifique.

---

63. VPS GRATUITO

Antes de contratar un VPS de pago se investigarán opciones gratuitas
válidas.

No asumir que un nivel gratuito es ilimitado.

Comprobar siempre:

RAM
CPU
DISCO
TRÁFICO
REGIÓN
LÍMITES
CONDICIONES

---

64. MIGRACIÓN

Si posteriormente se utiliza VPS:

PC
↓
BACKUP
↓
VPS
↓
DOCKER
↓
N8N
↓
RESTORE / RECONFIGURE

La migración no debe cambiar la arquitectura lógica.

---

65. SEGURIDAD

Antes de exponer n8n a Internet:

AUTHENTICATION
HTTPS
FIREWALL
BACKUPS
CREDENTIAL SECURITY

deben estar configurados correctamente.

---

66. ACCESO REMOTO

No exponer directamente una instancia de n8n a Internet sin protección.

La configuración de acceso remoto se realizará cuando sea realmente
necesaria.

---

67. WHATSAPP

WhatsApp no forma parte del primer MVP.

Primero:

Telegram

Después:

WhatsApp

si aporta una ventaja suficiente.

---

68. FALLBACK MULTI-IA

No implementar hasta que:

MVP = FUNCIONANDO

Después:

PRIMARY
↓
SECONDARY
↓
TERTIARY

---

69. CRITERIO DE NO SOBREDISEÑO

Cada nueva pieza debe responder a una necesidad demostrada.

No añadir:

AGENTES
VPS
MULTI-IA
WHATSAPP
BASES COMPLEJAS

solo porque podrían ser útiles algún día.

---

70. ORDEN DEFINITIVO

1. PC
2. N8N
3. TELEGRAM
4. STORAGE
5. CONTROL
6. ESTADO
7. PLANIFICADOR
8. IA
9. EJECUTOR
10. VALIDADOR
11. CHECKPOINT
12. CONTINUACIÓN
13. PAUSA
14. REANUDACIÓN
15. HUMAN ACTION
16. ERROR / RETRY
17. REINICIO / RECUPERACIÓN
18. GITHUB
19. MULTI-REPOSITORIO
20. WEB
21. KEYWORDS
22. SEARCH INTENT
23. INVESTIGACIÓN REAL
24. FALLBACK IA
25. WHATSAPP
26. VPS SI ES NECESARIO

---

71. CRITERIO DE ÉXITO

La primera versión se considerará válida cuando David pueda enviar:

/continuar

y el motor pueda trabajar automáticamente durante una sesión completa,
detenerse únicamente cuando exista una razón válida y recuperar el trabajo
sin perder el estado.

---

72. ACCIÓN REQUERIDA DE DAVID

Hasta disponer del PC:

NINGUNA

Cuando David tenga acceso al ordenador:

EMPEZAR POR EL PASO 5

No realizar pasos posteriores hasta validar cada etapa.

---

73. ESTADO

"READY_FOR_PC_IMPLEMENTATION"

Este documento es una guía de implementación, no una garantía de que todos
los comandos o versiones futuras de n8n sean idénticos.

Antes de ejecutar cualquier instalación o configuración, comprobar las
versiones actuales y adaptar los pasos si fuera necesario.

---

74. SIGUIENTE DOCUMENTO

Una vez completada esta guía, el siguiente documento de diseño será:

"CHECKLIST-PRUEBAS-MVP.md"

Contendrá las pruebas que el motor deberá superar antes de considerarlo
funcional.


