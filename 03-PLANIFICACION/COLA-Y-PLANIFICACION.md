SISTEMA DE PLANIFICACIÓN Y COLA DE TAREAS

1. OBJETIVO

Definir cómo el motor autónomo decide qué trabajo realizar, en qué orden y
cuándo debe continuar automáticamente con el siguiente.

La finalización de una tarea NO implica detener el motor.

---

2. PRINCIPIO

El motor debe transformar un objetivo en una secuencia dinámica de trabajos.

OBJETIVO
↓
PLAN
↓
TAREAS
↓
SUBTRABAJOS
↓
EJECUCIÓN
↓
VALIDACIÓN
↓
NUEVA TAREA
↓
CONTINUAR

El plan no será completamente rígido.

Podrá modificarse cuando aparezca nueva información.

---

3. CREACIÓN DE TAREAS

Una tarea puede proceder de:

- objetivo inicial;
- metodología del repositorio;
- tarea pendiente;
- resultado de una investigación;
- descubrimiento nuevo;
- dependencia;
- error;
- decisión;
- recomendación de la IA;
- necesidad detectada durante otra tarea.

---

4. COLA DE TAREAS

El motor mantendrá una cola lógica.

Ejemplo:

QUEUE

1. analizar mercado
2. analizar competencia
3. estudiar palabras clave
4. identificar intención de búsqueda
5. validar demanda
6. analizar monetización
7. comparar alternativas

Cuando termine la tarea 1:

→ validar
→ actualizar estado
→ continuar con tarea 2.

---

5. PRIORIDAD

Cada tarea tendrá una prioridad.

Prioridades conceptuales:

CRÍTICA
ALTA
NORMAL
BAJA

La prioridad podrá cambiar según los resultados obtenidos.

---

6. DEPENDENCIAS

Una tarea puede depender de otra.

Ejemplo:

ANALIZAR COMPETENCIA
↓
depende de
↓
IDENTIFICAR MERCADO

Una tarea bloqueada por una dependencia no deberá ejecutarse prematuramente.

---

7. TAREAS PARALELAS

Cuando técnicamente sea posible, determinadas tareas independientes podrán
ejecutarse en paralelo.

Ejemplo:

OBJETIVO
├── investigación de mercado
├── investigación SEO
├── investigación competencia
└── análisis de monetización

El uso de paralelismo dependerá de:

- coste;
- límites;
- complejidad;
- riesgo;
- capacidad de n8n.

---

8. SELECCIÓN DE LA SIGUIENTE TAREA

Después de cada tarea el motor deberá evaluar:

1. ¿Está realmente terminada?
2. ¿El resultado es válido?
3. ¿Ha aparecido una nueva información?
4. ¿Ha aparecido una nueva tarea?
5. ¿Existe alguna dependencia desbloqueada?
6. ¿Qué tarea tiene mayor valor?
7. ¿Puede realizarse autónomamente?
8. ¿Necesita intervención humana?

Si existe una tarea autónoma válida:

CONTINUAR.

---

9. VALOR DE UNA TAREA

La prioridad podrá calcularse conceptualmente considerando:

VALOR
+
URGENCIA
+
DEPENDENCIAS
+
IMPACTO

COSTE

RIESGO

La fórmula definitiva se determinará durante la implementación.

---

10. DESCUBRIMIENTO DINÁMICO

El motor debe poder descubrir trabajo nuevo durante una investigación.

Ejemplo:

Tarea:
"Analizar una idea de negocio."

Durante el análisis descubre:

- un competidor importante;
- una palabra clave desconocida;
- una oportunidad;
- un problema;
- una contradicción.

Puede crear automáticamente nuevas tareas.

Ejemplo:

ANÁLISIS DE IDEA
├── competencia
├── demanda
├── SEO
├── monetización
└── nueva tarea descubierta

---

11. NO REPETICIÓN

Antes de añadir una tarea:

COMPROBAR:

- tareas existentes;
- tareas terminadas;
- tareas bloqueadas;
- resultados existentes.

Si el trabajo ya está suficientemente realizado:

NO CREAR DUPLICADO.

---

12. DESCOMPOSICIÓN

Cuando una tarea sea demasiado grande, el motor deberá dividirla.

Ejemplo:

"Analizar el mercado español"

↓

1. tamaño;
2. demanda;
3. competencia;
4. tendencias;
5. clientes;
6. precios;
7. barreras;
8. oportunidades.

---

13. TAREAS ATÓMICAS

Siempre que sea posible, una tarea debe ser suficientemente concreta para:

- ejecutarse;
- validarse;
- registrar un resultado;
- determinar si está terminada.

Evitar tareas indefinidas como:

"Investigar todo."

Preferir:

"Analizar los 10 competidores principales del mercado X."

---

14. CRITERIO DE FINALIZACIÓN

Una tarea solo se marca como COMPLETED cuando:

- se ha ejecutado;
- se ha obtenido un resultado;
- el resultado ha sido validado razonablemente;
- se ha registrado;
- no queda una acción interna necesaria.

Si falta alguna condición:

NO COMPLETAR.

---

15. RESULTADO QUE GENERA NUEVO TRABAJO

Una tarea puede generar:

RESULTADO
↓
DECISIÓN
↓
NUEVA TAREA

Por tanto, la cola debe poder modificarse continuamente.

---

16. BLOQUEOS

Si una tarea requiere:

- credenciales;
- instalación;
- acción manual;
- aprobación;
- información que solo puede proporcionar David;

debe marcarse:

WAITING_HUMAN.

El motor no debe fingir que puede continuar.

---

17. ACCIÓN HUMANA Y COLA

Cuando exista una acción humana:

1. guardar checkpoint;
2. registrar acción;
3. pausar ejecución;
4. notificar;
5. esperar;
6. comprobar el resultado;
7. desbloquear tareas dependientes;
8. continuar.

---

18. PAUSA VOLUNTARIA

Si David ordena:

PAUSAR

el motor debe:

- guardar estado;
- conservar cola;
- conservar prioridades;
- conservar dependencias;
- detener nuevas ejecuciones.

---

19. REANUDACIÓN

Si David ordena:

CONTINUAR

o envía:

.

el motor debe:

1. recuperar estado;
2. revisar cola;
3. comprobar bloqueos;
4. seleccionar la siguiente tarea;
5. continuar.

No debe comenzar nuevamente desde cero.

---

20. DESACTIVACIÓN

Si David ordena:

DESACTIVAR

el motor debe:

- guardar estado;
- guardar checkpoint;
- detener la cola;
- no iniciar nuevas tareas.

Los datos no deben perderse.

---

21. PLANIFICACIÓN POR FASES

Los proyectos complejos podrán organizarse:

FASE 1
Descubrimiento

FASE 2
Investigación

FASE 3
Análisis

FASE 4
Validación

FASE 5
Construcción

FASE 6
Revisión

FASE 7
Optimización

El repositorio podrá utilizar otra estructura si sus reglas establecen una
metodología diferente.

---

22. RESPETO A LA METODOLOGÍA DEL REPOSITORIO

La cola no debe imponer fases universales.

Primero:

LEER REGLAS DEL REPOSITORIO.

Después:

ADAPTAR PLANIFICACIÓN.

El motor general proporciona el mecanismo.

El repositorio proporciona el método específico.

---

23. PLANIFICACIÓN ADAPTATIVA

El motor debe poder cambiar el plan cuando:

- aparecen nuevos datos;
- una hipótesis resulta falsa;
- una tarea deja de ser relevante;
- aparece una oportunidad mejor;
- aparece un bloqueo;
- cambia el objetivo.

Todo cambio importante deberá quedar registrado.

---

24. CONTROL DE COSTES

La planificación deberá evitar:

- llamadas innecesarias;
- investigaciones duplicadas;
- tareas de bajo valor;
- modelos excesivamente caros;
- contexto innecesariamente grande.

---

25. CONTROL DE RIESGO

Antes de ejecutar tareas sensibles el motor deberá considerar:

- impacto;
- reversibilidad;
- seguridad;
- posibilidad de pérdida;
- necesidad de autorización.

Cuanto mayor sea el riesgo, mayor será el nivel de control requerido.

---

26. BUCLE PRINCIPAL

El comportamiento deseado es:

MIENTRAS:

exista trabajo válido
Y
no exista bloqueo humano
Y
no exista error crítico
Y
el motor esté RUNNING

HACER:

1. seleccionar mejor tarea;
2. ejecutar;
3. validar;
4. registrar;
5. actualizar estado;
6. crear nuevas tareas si procede;
7. seleccionar siguiente tarea;
8. continuar.

---

27. FINALIZACIÓN DE OBJETIVO

Cuando no queden tareas relevantes:

El motor debe comprobar:

¿El objetivo está realmente cumplido?

Si:

SÍ → COMPLETED.

Si:

NO → generar tareas adicionales.

No debe declarar terminado un proyecto simplemente porque se vacíe
accidentalmente la cola.

---

28. PRIORIDAD DE INTERVENCIÓN

La prioridad será:

1. seguridad;
2. acción humana imprescindible;
3. errores críticos;
4. dependencias;
5. tareas de alto impacto;
6. tareas normales;
7. optimización;
8. tareas de bajo valor.

---

29. TRANSPARENCIA

El motor deberá poder explicar brevemente:

- qué está haciendo;
- qué porcentaje lleva;
- qué queda;
- qué ha encontrado;
- si existe algún problema.

Pero no debe interrumpir el trabajo para informar de cada tarea.

---

30. PRINCIPIO DE AUTONOMÍA

La regla principal es:

TERMINAR UNA TAREA
≠
TERMINAR EL TRABAJO.

Terminar una tarea significa evaluar inmediatamente cuál es el siguiente
trabajo válido.

---

31. PRÓXIMO TRABAJO

Después de este documento se deberá diseñar:

PROTOCOLO DEL AGENTE AUTÓNOMO

Deberá definir exactamente cómo la IA:

- recibe contexto;
- razona sobre la tarea;
- utiliza herramientas;
- decide si puede continuar;
- crea subtrabajos;
- valida resultados;
- actualiza memoria;
- detecta intervención humana;
- entrega el control a n8n.

