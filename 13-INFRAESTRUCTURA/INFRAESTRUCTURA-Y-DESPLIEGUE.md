INFRAESTRUCTURA Y DESPLIEGUE

1. OBJETIVO

Definir la infraestructura necesaria para ejecutar el motor autónomo de forma
estable, segura y continua.

El diseño deberá permitir comenzar con recursos mínimos y evolucionar hacia
una ejecución 24/7.

---

2. PRINCIPIO

EMPEZAR SIMPLE.

VALIDAR.

MEDIR.

ESCALAR.

No contratar infraestructura innecesaria antes de validar el sistema.

---

3. ARQUITECTURA

La arquitectura inicial estará formada conceptualmente por:

n8n
+
modelos de IA
+
herramientas
+
memoria
+
sistema de comunicación
+
repositorios.

---

4. ENTORNO LOCAL

Durante la fase de desarrollo podrá utilizarse un ordenador personal.

Ventajas:

- coste bajo;
- control completo;
- facilidad para pruebas;
- acceso directo a herramientas.

Desventaja:

si el ordenador está apagado, el motor no podrá ejecutar tareas locales.

---

5. SERVIDOR

Para ejecución continua se podrá utilizar un servidor/VPS.

Arquitectura:

INTERNET
↓
VPS
↓
n8n
↓
MOTOR
↓
IA / APIs / REPOSITORIOS / CANALES.

---

6. 24/7

El objetivo final será:

MOTOR
→ disponible continuamente.

David no deberá mantener el ordenador encendido para la ejecución normal.

---

7. FASES

FASE 1
→ diseño.

FASE 2
→ pruebas locales.

FASE 3
→ automatización básica.

FASE 4
→ VPS.

FASE 5
→ ejecución 24/7.

FASE 6
→ escalabilidad.

---

8. n8n

n8n será el orquestador principal.

Deberá ejecutar:

- triggers;
- workflows;
- llamadas a APIs;
- routing;
- gestión de errores;
- notificaciones;
- ciclos.

---

9. MOTOR

El motor lógico podrá estar implementado mediante workflows y componentes
externos.

No asumir que todo debe residir dentro de un único workflow gigantesco.

---

10. MODULARIDAD

Separar:

- comunicación;
- planificación;
- ejecución;
- validación;
- memoria;
- herramientas;
- monitorización.

---

11. BASE DE DATOS

El sistema podrá necesitar una base de datos para:

- tareas;
- estados;
- sesiones;
- checkpoints;
- eventos;
- resultados;
- intervenciones.

La tecnología concreta se decidirá durante la implementación.

---

12. MEMORIA

No depender exclusivamente de memoria dentro de un workflow temporal.

La información importante deberá persistir.

---

13. PERSISTENCIA

Como mínimo deberán persistir:

- proyecto;
- tarea;
- estado;
- resultado;
- checkpoint;
- errores;
- acciones humanas pendientes.

---

14. COPIAS DE SEGURIDAD

La información crítica deberá poder recuperarse.

Definir:

- frecuencia;
- ubicación;
- retención;
- procedimiento de restauración.

Los valores concretos se determinarán antes de producción.

---

15. GITHUB

Los repositorios serán la fuente principal de documentación y código de los
proyectos cuando corresponda.

El motor deberá poder leer los repositorios autorizados.

---

16. ESCRITURA

La capacidad de escribir dependerá de las herramientas realmente disponibles.

No asumir permisos inexistentes.

Si David debe aplicar un cambio manualmente:

ACTION_REQUIRED.

---

17. MODELOS DE IA

El motor deberá poder utilizar diferentes proveedores.

Arquitectura conceptual:

AI PROVIDER
├── Modelo A
├── Modelo B
├── Modelo C
└── Fallback.

---

18. FALLBACK

Si un proveedor falla:

1. detectar;
2. comprobar si existe alternativa;
3. cambiar de proveedor;
4. continuar;
5. registrar el cambio.

---

19. LÍMITES DE API

Cada proveedor podrá tener:

- límites de solicitudes;
- límites de tokens;
- cuotas;
- costes;
- restricciones.

El motor deberá conocerlos antes de utilizarlo en producción.

---

20. COSTE

El sistema deberá registrar, cuando sea posible:

- proveedor;
- modelo;
- solicitudes;
- consumo;
- coste estimado.

---

21. PROVEEDORES GRATUITOS

Los modelos gratuitos podrán utilizarse durante la fase inicial si ofrecen
API suficiente.

No diseñar la arquitectura suponiendo que una cuota gratuita será infinita.

---

22. ROTACIÓN

La rotación de proveedores deberá producirse únicamente cuando sea necesario.

No cambiar de modelo sin motivo.

---

23. HERRAMIENTAS WEB

El motor podrá utilizar herramientas de investigación cuando estén
disponibles.

Deberán tratarse como componentes externos.

---

24. TELEGRAM

Telegram será inicialmente el canal de control remoto.

Requiere:

- bot;
- credencial;
- usuario autorizado;
- workflow de entrada;
- workflow de salida.

---

25. WHATSAPP

WhatsApp se añadirá posteriormente.

No debe ser requisito para la primera versión funcional.

---

26. DOMINIOS

Los dominios no forman parte del núcleo del motor.

Solo serán necesarios para proyectos que los utilicen.

---

27. WORDPRESS

WordPress será una herramienta/proyecto externo.

La instalación requerirá infraestructura correspondiente.

Si David debe instalarlo:

ACTION_REQUIRED.

---

28. SERVICIOS EXTERNOS

Cada servicio externo deberá tener:

- proveedor;
- API;
- credencial;
- límite;
- función;
- fallback cuando proceda.

---

29. MONITORIZACIÓN

El sistema deberá poder detectar:

- n8n detenido;
- workflow fallido;
- API caída;
- errores repetidos;
- consumo anómalo;
- intervención pendiente.

---

30. HEALTH CHECK

Deberá existir un mecanismo periódico para comprobar que los componentes
principales funcionan.

---

31. ALERTAS

Las alertas deberán enviarse únicamente para eventos relevantes.

Ejemplos:

- motor detenido;
- error crítico;
- intervención humana;
- proveedor indisponible durante demasiado tiempo.

---

32. LOGS

Los logs deberán permitir investigar errores.

Pero no deberán contener secretos.

---

33. RETENCIÓN

Los logs deberán tener una política de retención razonable.

No conservar indefinidamente información innecesaria.

---

34. SEGURIDAD DEL SERVIDOR

Antes de producción:

- actualizar sistema;
- proteger accesos;
- utilizar credenciales seguras;
- limitar puertos;
- realizar backups;
- revisar servicios expuestos.

---

35. ACCESO

El acceso administrativo deberá estar separado del acceso del motor.

---

36. ACTUALIZACIONES

No actualizar componentes críticos automáticamente sin evaluar posibles
incompatibilidades.

---

37. VERSIONADO

Registrar versiones de:

- n8n;
- workflows;
- modelos;
- componentes;
- configuraciones importantes.

---

38. DESPLIEGUE

El despliegue deberá poder reproducirse.

Documentar:

- dependencias;
- variables;
- credenciales;
- servicios;
- configuración;
- procedimientos.

Los secretos se excluyen de la documentación pública.

---

39. RECUPERACIÓN

Si el servidor falla:

RESTORE
↓
LOAD STATE
↓
VALIDATE
↓
CONTINUE.

---

40. ESCALABILIDAD

La primera versión no necesita una infraestructura compleja.

La arquitectura deberá permitir posteriormente:

- más tareas;
- más proyectos;
- más repositorios;
- más proveedores;
- más canales.

---

41. RECURSOS

Antes de elegir VPS se deberán medir:

- CPU;
- RAM;
- almacenamiento;
- tráfico;
- número de workflows;
- frecuencia de ejecución;
- volumen de datos.

No sobredimensionar sin datos.

---

42. COSTE MENSUAL

Registrar:

VPS
+
APIs
+
BASE DE DATOS
+
SERVICIOS
+
DOMINIOS
+
BACKUPS.

El coste deberá compararse con el valor generado.

---

43. PORTABILIDAD

Evitar depender innecesariamente de un único proveedor.

Siempre que sea razonable:

configuración reproducible.

---

44. DESARROLLO → PRODUCCIÓN

No trasladar directamente un sistema experimental a producción.

Secuencia:

DESARROLLO
↓
PRUEBAS
↓
PILOTO
↓
PRODUCCIÓN.

---

45. ENTORNO DE PRUEBAS

Debe existir un entorno donde se puedan probar:

- workflows;
- modelos;
- credenciales;
- errores;
- recuperación.

Sin afectar proyectos reales.

---

46. PRODUCCIÓN

Producción deberá disponer de:

- persistencia;
- backups;
- monitorización;
- seguridad;
- recuperación;
- control de costes.

---

47. MIGRACIÓN

Cuando el sistema pase de ordenador a VPS:

deberá conservar:

- configuración;
- memoria;
- tareas;
- estados;
- workflows.

---

48. PRINCIPIO DE NO BLOQUEO

La caída de un componente secundario no debe detener todo el sistema si
existe una alternativa segura.

---

49. AISLAMIENTO

Un proyecto problemático no debería comprometer automáticamente los demás
proyectos.

---

50. MULTI-REPOSITORIO

El motor deberá poder trabajar con repositorios existentes y futuros de la
cuenta autorizada de David.

El descubrimiento de nuevos repositorios deberá diseñarse posteriormente.

---

51. OPERACIÓN MÓVIL

Una vez desplegado:

David deberá poder:

- activar;
- continuar;
- pausar;
- detener;
- consultar;
- recibir alertas;

desde el móvil.

---

52. ORDENADOR

El ordenador será principalmente una herramienta de desarrollo y
configuración cuando exista un entorno 24/7.

---

53. TRANSICIÓN

ORDENADOR
→ desarrollo.

VPS
→ ejecución continua.

---

54. PRIMER DESPLIEGUE

No instalar todavía todos los componentes.

Primero construir y probar el núcleo.

Después añadir infraestructura progresivamente.

---

55. CRITERIO PARA PRODUCCIÓN

El sistema podrá considerarse preparado cuando:

- complete tareas;
- conserve estado;
- se recupere de errores;
- gestione intervenciones;
- controle costes;
- mantenga seguridad;
- pueda ejecutarse sin supervisión constante.

---

56. PRÓXIMO TRABAJO

El siguiente documento deberá cerrar:

PLAN DE IMPLEMENTACIÓN Y PRUEBAS

Ese documento convertirá toda la documentación anterior en fases concretas
de construcción y definirá el orden exacto en que se desarrollará el motor.

