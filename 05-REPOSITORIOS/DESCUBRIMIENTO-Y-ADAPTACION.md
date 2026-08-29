DESCUBRIMIENTO Y ADAPTACIÓN DE REPOSITORIOS

1. OBJETIVO

Definir cómo el motor descubre, analiza y adapta automáticamente su
funcionamiento a los repositorios de la cuenta "davidsaenz20".

El motor debe funcionar con los repositorios existentes y con cualquier
repositorio creado posteriormente.

---

2. PRINCIPIO

El motor no debe depender de una lista fija de repositorios.

Debe poder descubrir el entorno dinámicamente.

CUENTA
↓
REPOSITORIOS
↓
REPOSITORIO SELECCIONADO
↓
ANÁLISIS
↓
REGLAS
↓
METODOLOGÍA
↓
ESTADO
↓
PROYECTO
↓
TRABAJO

---

3. DESCUBRIMIENTO DE REPOSITORIOS

El sistema deberá poder consultar los repositorios accesibles para:

"davidsaenz20"

Deberá obtener, cuando sea posible:

- nombre;
- propietario;
- descripción;
- URL;
- rama principal;
- visibilidad;
- fecha de actualización;
- información relevante para su clasificación.

---

4. REPOSITORIOS NUEVOS

El sistema deberá poder detectar posteriormente repositorios nuevos.

No deberá ser necesario modificar manualmente el motor cada vez que David
cree un repositorio.

El inventario deberá poder actualizarse automáticamente.

---

5. INVENTARIO

El motor podrá mantener un inventario lógico:

REPOSITORIO

- ID
- nombre
- propietario
- URL
- estado
- última exploración
- metodología detectada
- proyectos detectados
- última actividad

El formato definitivo queda pendiente de implementación.

---

6. ANÁLISIS INICIAL

Cuando se seleccione un repositorio por primera vez:

1. obtener estructura;
2. localizar documentación;
3. localizar instrucciones;
4. localizar archivos maestros;
5. localizar estados;
6. localizar proyectos;
7. detectar metodología;
8. identificar restricciones;
9. crear perfil del repositorio.

---

7. ARCHIVOS PRIORITARIOS

El motor deberá buscar especialmente nombres o ubicaciones que indiquen:

- README;
- instrucciones;
- reglas;
- metodología;
- modo de trabajo;
- documentación maestra;
- estado;
- progreso;
- proyecto;
- configuración.

No debe asumir que un archivo concreto existe.

---

8. REGLAS DEL REPOSITORIO

Si existe un archivo de reglas, deberá ser leído antes de ejecutar trabajo.

Ejemplos posibles:

"MODO-TRABAJO.md"

"CONTRIBUTING.md"

"INSTRUCCIONES.md"

"REGLAS.md"

u otros equivalentes.

---

9. JERARQUÍA

El motor deberá distinguir entre:

REGLAS DEL MOTOR
↓
REGLAS DEL REPOSITORIO
↓
REGLAS DEL PROYECTO
↓
OBJETIVO
↓
TAREA

No deberá sustituir reglas específicas por suposiciones generales.

---

10. METODOLOGÍA

El motor intentará determinar cómo trabaja el repositorio.

Puede encontrar:

- fases;
- checklists;
- documentos maestros;
- convenciones;
- estados;
- procesos;
- criterios de validación;
- instrucciones especiales.

La metodología detectada deberá conservarse como información del repositorio.

---

11. ADAPTACIÓN

Una vez analizado el repositorio:

MOTOR GENERAL
+
REGLAS DEL REPOSITORIO
+
ESTADO DEL PROYECTO

→

ENTORNO DE TRABAJO ADAPTADO.

El motor debe reutilizar sus capacidades generales sin imponer una metodología
que contradiga la del repositorio.

---

12. REPOSITORIO SIN METODOLOGÍA

Si no existe una metodología clara:

El motor podrá utilizar su metodología general como respaldo.

Pero deberá registrar:

"METODOLOGIA_DETECTADA = NO"

y evitar presentar la metodología inferida como una regla oficial del repositorio.

---

13. AMBIGÜEDAD

Si existen instrucciones contradictorias:

1. identificar la contradicción;
2. determinar qué regla tiene prioridad;
3. si no puede resolverse con seguridad, registrar bloqueo;
4. solicitar intervención humana.

No inventar una resolución.

---

14. PROYECTOS

Después de analizar el repositorio, el motor deberá identificar sus proyectos.

Un repositorio puede contener:

PROYECTO A
PROYECTO B
PROYECTO C

Cada proyecto debe mantener su propio contexto.

---

15. AISLAMIENTO

La información de un repositorio no debe contaminar otro.

Por ejemplo:

BASE-PROYECTOS
≠
PENSADOR DE IDEAS

Aunque ambos sean trabajados por el mismo motor.

---

16. PERFIL DEL REPOSITORIO

El motor deberá poder construir un perfil como:

REPOSITORY_PROFILE

- identidad;
- estructura;
- reglas;
- metodología;
- documentos importantes;
- proyectos;
- estado;
- restricciones;
- capacidades;
- última exploración.

---

17. ACTUALIZACIÓN DEL PERFIL

El perfil no debe considerarse permanente si el repositorio cambia.

El motor podrá actualizarlo cuando:

- cambie la estructura;
- aparezcan nuevos documentos;
- cambien reglas;
- aparezcan nuevos proyectos;
- se detecten nuevas instrucciones.

---

18. COSTE DE DESCUBRIMIENTO

No deberá analizar todo el repositorio repetidamente.

Después del análisis inicial:

1. conservar información útil;
2. detectar cambios;
3. analizar solamente lo necesario;
4. actualizar el perfil.

Objetivo:

REDUCIR TIEMPO
+
REDUCIR TOKENS
+
REDUCIR LLAMADAS

---

19. REPOSITORIOS FUTUROS

Cuando aparezca un repositorio nuevo:

DETECTAR
↓
ANALIZAR
↓
CLASIFICAR
↓
DESCUBRIR REGLAS
↓
CREAR PERFIL
↓
DISPONIBILIZAR PARA TRABAJO

No deberá requerir reconstruir el motor.

---

20. REPOSITORIO SELECCIONADO

Cuando David indique un repositorio, el motor deberá identificarlo de forma
inequívoca.

Preferiblemente mediante:

- nombre;
- URL;
- identificador;
- combinación de propietario y nombre.

Si existen varias posibilidades y no puede determinar cuál es correcta,
deberá solicitar aclaración.

---

21. CAMBIO DE REPOSITORIO

Antes de cambiar:

1. guardar estado actual;
2. crear checkpoint;
3. cerrar o pausar sesión;
4. cargar nuevo repositorio;
5. recuperar su perfil;
6. recuperar su estado;
7. continuar.

---

22. DETECCIÓN DE CAMBIOS

El motor podrá comprobar periódicamente si:

- existen nuevos repositorios;
- han cambiado reglas;
- han aparecido proyectos;
- ha cambiado documentación;
- existe nueva información relevante.

---

23. CONFIANZA

Cada elemento detectado deberá distinguir entre:

CONFIRMADO
→ encontrado explícitamente.

INFERIDO
→ deducido por estructura o contexto.

DESCONOCIDO
→ no existe evidencia suficiente.

---

24. NO INVENTAR

El motor no debe inventar:

- proyectos;
- reglas;
- metodología;
- estados;
- archivos;
- estructura.

Si no puede determinar algo:

DESCONOCIDO.

---

25. REPOSITORIOS NO APTOS

Si un repositorio no contiene suficiente información para realizar el trabajo:

deberá explicar qué falta.

Ejemplos:

- no existe objetivo;
- no existe documentación;
- no existe proyecto identificable;
- permisos insuficientes;
- estructura ilegible.

---

26. CAPACIDADES

Durante el descubrimiento podrá detectar capacidades del repositorio.

Ejemplo:

- documentación;
- código;
- datos;
- contenido;
- automatizaciones;
- configuración;
- proyectos web.

Estas capacidades servirán para seleccionar herramientas.

---

27. RESULTADO DEL DESCUBRIMIENTO

El resultado deberá producir:

REPOSITORY_PROFILE
+
PROJECTS
+
RULES
+
METHODOLOGY
+
STATE
+
CONSTRAINTS
+
NEXT_VALID_WORK

---

28. SIGUIENTE TRABAJO

Después del descubrimiento, el motor deberá determinar cuál es el siguiente
trabajo válido para ese repositorio.

No comenzar a trabajar sin comprender primero el entorno cuando el análisis
sea necesario.

---

29. ACTUALIZACIÓN SIN INTERRUPCIÓN

Si el motor detecta durante el trabajo un cambio no crítico:

actualizar perfil y continuar.

No detenerse para informar.

---

30. INTERVENCIÓN

Solo solicitar intervención si el descubrimiento encuentra una situación que
impida continuar con seguridad.

---

31. PRINCIPIO FINAL

El motor debe ser:

GENERAL EN SU ARQUITECTURA

y

ESPECÍFICO EN SU EJECUCIÓN.

Es decir:

Mismo motor.

Diferentes repositorios.

Diferentes reglas.

Diferentes metodologías.

Diferentes proyectos.

---

32. PRÓXIMO TRABAJO

El siguiente documento deberá definir:

SISTEMA DE HERRAMIENTAS Y CAPACIDADES DEL AGENTE

Deberá establecer cómo el agente podrá utilizar:

- GitHub;
- web;
- archivos;
- APIs;
- análisis;
- otras herramientas;

y cómo se decidirá qué herramienta utilizar en cada tarea.


