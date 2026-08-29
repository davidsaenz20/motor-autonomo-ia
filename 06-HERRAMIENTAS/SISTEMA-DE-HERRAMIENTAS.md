SISTEMA DE HERRAMIENTAS DEL MOTOR AUTÓNOMO

1. OBJETIVO

Definir el sistema mediante el cual el agente podrá utilizar herramientas
externas para realizar trabajo real.

El agente no debe limitarse a generar texto.

Debe poder:

PENSAR
→ ELEGIR HERRAMIENTA
→ EJECUTAR
→ ANALIZAR RESULTADO
→ VALIDAR
→ CONTINUAR.

---

2. PRINCIPIO

Cada herramienta tendrá una responsabilidad concreta.

El agente deberá seleccionar la herramienta más adecuada para cada tarea.

No utilizar una herramienta simplemente porque está disponible.

---

3. CATEGORÍAS

El sistema podrá incorporar:

- GitHub;
- investigación web;
- archivos;
- bases de datos;
- APIs;
- cálculo;
- procesamiento de datos;
- comunicación;
- automatización;
- otras herramientas futuras.

---

4. GITHUB

Función:

Permitir al agente consultar el contenido y estructura de los repositorios.

Capacidades previstas:

- identificar repositorios;
- consultar archivos;
- consultar ramas;
- consultar commits;
- consultar issues;
- consultar pull requests;
- analizar cambios;
- recuperar documentación;
- detectar estructura.

La capacidad real dependerá de los permisos disponibles.

---

5. ESCRITURA EN GITHUB

La arquitectura NO debe asumir que el agente puede escribir directamente.

Debe diferenciar:

LECTURA
→ disponible cuando existan permisos.

ESCRITURA
→ solamente mediante un mecanismo autorizado.

INTERVENCIÓN HUMANA
→ cuando David tenga que modificar o crear el archivo manualmente.

El agente no debe insistir intentando operaciones que sabe que no están
permitidas.

---

6. INVESTIGACIÓN WEB

La herramienta web permitirá:

- buscar información;
- comparar fuentes;
- investigar mercados;
- investigar competidores;
- estudiar tendencias;
- investigar SEO;
- analizar búsquedas;
- contrastar datos.

Cuando una tarea requiera información actualizada, deberá utilizar una fuente
actualizada en lugar de confiar únicamente en memoria del modelo.

---

7. FUENTES WEB

La investigación deberá registrar cuando sea relevante:

- fuente;
- URL;
- fecha;
- dato;
- contexto;
- confianza.

No guardar únicamente la conclusión sin su origen cuando la fuente sea
importante para futuras decisiones.

---

8. ARCHIVOS

El sistema podrá utilizar herramientas para:

- leer;
- analizar;
- comparar;
- transformar;
- generar;
- validar archivos.

Debe distinguir entre:

ARCHIVO DE TRABAJO

y

ARCHIVO DE FUENTE DE VERDAD.

No sobrescribir información crítica sin validación.

---

9. DATOS

El motor podrá trabajar con:

- CSV;
- JSON;
- hojas de cálculo;
- bases de datos;
- texto;
- otros formatos compatibles.

Las herramientas de datos deberán utilizarse cuando aporten mayor precisión
que una interpretación manual del modelo.

---

10. APIs

Las APIs permitirán ampliar las capacidades del motor.

Ejemplos:

- servicios SEO;
- buscadores;
- analítica;
- plataformas de automatización;
- servicios de comunicación;
- servicios de IA;
- otras fuentes de datos.

Cada API deberá registrarse con:

- función;
- proveedor;
- límites;
- coste;
- autenticación;
- riesgos;
- alternativa disponible.

---

11. CREDENCIALES

Las credenciales nunca deben almacenarse en documentos públicos del proyecto.

Deberán mantenerse en el sistema seguro de credenciales correspondiente.

El agente debe recibir únicamente lo necesario para ejecutar una operación.

---

12. HERRAMIENTA Y TAREA

El agente deberá establecer una relación:

TASK
→ REQUIRED_CAPABILITY
→ TOOL
→ RESULT.

Ejemplo:

TAREA:
"Investigar volumen de búsquedas."

CAPACIDAD:
"datos SEO"

HERRAMIENTA:
"API SEO"

RESULTADO:
"datos de búsqueda."

---

13. SELECCIÓN DE HERRAMIENTA

La selección deberá considerar:

1. adecuación;
2. precisión;
3. disponibilidad;
4. coste;
5. velocidad;
6. límites;
7. seguridad;
8. calidad de los datos.

---

14. FALLBACK

Una herramienta puede fallar.

Por tanto:

HERRAMIENTA A
↓
FALLA
↓
HERRAMIENTA B
↓
FALLA
↓
HERRAMIENTA C

El motor podrá utilizar alternativas cuando sean compatibles.

No debe cambiar de herramienta si hacerlo puede alterar la validez del
resultado sin registrarlo.

---

15. HERRAMIENTAS GRATUITAS

Cuando existan herramientas gratuitas suficientemente buenas:

PRIORIZARLAS.

Pero no utilizar una herramienta gratuita únicamente por ser gratuita si
produce resultados insuficientes.

La calidad del resultado es prioritaria.

---

16. LÍMITES

Cada herramienta deberá tener registrados, cuando se conozcan:

- límite de peticiones;
- límite diario;
- límite mensual;
- coste;
- tamaño máximo;
- velocidad;
- restricciones.

El motor deberá evitar consumir innecesariamente los límites.

---

17. CONTROL DE COSTES

Antes de utilizar una herramienta de pago:

comprobar si existe una alternativa suficientemente válida.

Si el coste puede ser significativo, deberá quedar registrado.

Las operaciones de gasto directo podrán requerir intervención humana según
las reglas de seguridad.

---

18. VALIDACIÓN DE HERRAMIENTAS

El resultado de una herramienta externa no debe considerarse automáticamente
verdadero.

Cuando sea importante:

HERRAMIENTA
↓
RESULTADO
↓
VALIDACIÓN
↓
MEMORIA.

---

19. HERRAMIENTAS DE COMUNICACIÓN

El motor podrá utilizar:

- Telegram;
- WhatsApp;
- correo;
- otros canales futuros.

Su función principal será comunicar:

- intervención requerida;
- errores críticos;
- estado solicitado;
- órdenes de control.

No deben utilizarse para enviar cada pequeño avance.

---

20. TELEGRAM

Será candidato para el primer canal de control.

Comandos previstos:

/activar
/pausar
/continuar
/estado
/desactivar

También podrá utilizarse lenguaje natural.

La implementación concreta queda pendiente.

---

21. WHATSAPP

WhatsApp podrá incorporarse posteriormente.

No debe convertirse en dependencia arquitectónica del motor.

El motor debe seguir funcionando aunque WhatsApp no esté disponible.

---

22. HERRAMIENTAS INTERNAS

El propio motor podrá disponer de funciones internas como:

- gestor de tareas;
- gestor de memoria;
- gestor de checkpoints;
- selector de modelos;
- validador;
- detector de bloqueos.

Estas funciones no son herramientas externas, sino componentes del sistema.

---

23. REGISTRO DE USO

Cada utilización importante de una herramienta deberá poder registrarse:

- herramienta;
- tarea;
- fecha;
- resultado;
- error;
- coste si procede;
- siguiente acción.

Esto permitirá auditar el funcionamiento.

---

24. IDEMPOTENCIA

Cuando sea posible, las operaciones deberán poder repetirse sin producir
duplicados o daños.

Especialmente importante para:

- escritura;
- creación de recursos;
- llamadas externas;
- notificaciones;
- operaciones de pago.

---

25. OPERACIONES DE RIESGO

Las operaciones potencialmente destructivas deberán tener controles
adicionales.

Ejemplos:

- eliminar archivos;
- modificar configuración crítica;
- publicar contenido;
- ejecutar acciones irreversibles;
- gastar dinero.

---

26. MODO SIMULACIÓN

Cuando sea posible, el motor podrá utilizar una herramienta en modo
simulación antes de ejecutar una operación real.

Ejemplo:

SIMULAR
↓
VALIDAR
↓
EJECUTAR.

---

27. DESCUBRIMIENTO DE NUEVAS HERRAMIENTAS

El sistema podrá incorporar nuevas herramientas en el futuro.

Para añadir una herramienta:

1. identificar necesidad;
2. evaluar alternativas;
3. comprobar seguridad;
4. comprobar coste;
5. comprobar API;
6. definir interfaz;
7. probar;
8. registrar.

No modificar arbitrariamente la arquitectura por una herramienta concreta.

---

28. ABSTRACCIÓN

El agente debería trabajar con capacidades abstractas siempre que sea posible.

Ejemplo:

No pensar:

"Necesito utilizar la API X."

Pensar:

"Necesito datos de volumen de búsqueda."

Después el selector determina:

API X
o
API Y
o
fuente alternativa.

Esto permite sustituir proveedores.

---

29. DISPONIBILIDAD

Cada herramienta podrá tener:

AVAILABLE
UNAVAILABLE
LIMITED
ERROR
REQUIRES_HUMAN

El agente debe adaptar su estrategia.

---

30. HERRAMIENTA REQUIERE HUMANO

Si una herramienta necesita una acción que solamente David puede realizar:

ACTION_REQUIRED = true

Ejemplo:

- introducir credenciales;
- aprobar acceso;
- activar servicio;
- instalar software;
- configurar servidor.

---

31. AISLAMIENTO

Las herramientas deben respetar el contexto del repositorio.

Una herramienta utilizada para:

BASE-PROYECTOS

no debe modificar o contaminar:

PENSADOR-DE-IDEAS.

---

32. ORDEN DE PREFERENCIA

Cuando varias herramientas puedan realizar una tarea:

1. herramienta disponible;
2. suficientemente precisa;
3. segura;
4. de menor coste;
5. rápida;
6. alternativa.

La prioridad puede cambiar según el tipo de tarea.

---

33. HERRAMIENTAS Y MODELOS

El modelo de IA y las herramientas son componentes independientes.

Ejemplo:

MODELO A
→ herramienta web

MODELO B
→ herramienta web

MODELO C
→ herramienta web.

La sustitución del modelo no debe romper las herramientas.

---

34. FALLBACK GLOBAL

Si una herramienta crítica deja de funcionar:

1. detectar;
2. intentar alternativa;
3. comprobar resultado;
4. continuar si es válido;
5. registrar incidencia.

Si no existe alternativa válida:

WAITING_HUMAN o ERROR.

---

35. OBJETIVO FINAL

El agente debe disponer de un conjunto creciente de capacidades sin quedar
atado a proveedores concretos.

CAPACIDADES
↓
HERRAMIENTAS
↓
PROVEEDORES

El motor debe poder sustituir proveedores sin reconstruirse.

---

36. PRÓXIMO TRABAJO

El siguiente documento deberá definir:

SISTEMA MULTI-IA Y SELECTOR DE MODELOS

Debe determinar:

- qué modelos utilizar;
- cómo seleccionar el modelo;
- cómo cambiar de proveedor;
- cómo controlar límites;
- cómo aprovechar APIs gratuitas;
- cómo controlar costes;
- cómo conservar contexto al cambiar de modelo.

- 
