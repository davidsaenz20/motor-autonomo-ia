SISTEMA MULTI-IA Y SELECTOR DE MODELOS

1. OBJETIVO

Crear una capa de inteligencia artificial independiente del proveedor.

El motor debe poder utilizar diferentes modelos y cambiar entre ellos sin
perder el estado del trabajo.

MODELO A
↓
LÍMITE / ERROR
↓
MODELO B
↓
CONTINUAR

---

2. PRINCIPIO

El motor no debe depender de una única IA.

La inteligencia será una capacidad abstracta:

AI_PROVIDER
+
AI_MODEL
+
CAPABILITIES
+
LIMITS
+
COST

El proveedor concreto podrá cambiar.

---

3. PROVEEDORES CANDIDATOS

Podrán evaluarse:

- Gemini;
- Groq;
- OpenRouter;
- Claude;
- OpenAI;
- otros proveedores con API compatible.

La lista no es definitiva.

No se debe asumir que todos ofrecen las mismas condiciones gratuitas,
límites o capacidades.

---

4. CAPACIDADES

Cada modelo deberá clasificarse según capacidades como:

- razonamiento;
- investigación;
- programación;
- análisis;
- generación de texto;
- resumen;
- extracción;
- clasificación;
- contexto largo;
- velocidad.

---

5. PERFIL DEL MODELO

Cada modelo podrá tener conceptualmente:

MODEL_PROFILE

- proveedor;
- modelo;
- capacidades;
- contexto;
- velocidad;
- coste;
- límites;
- disponibilidad;
- calidad estimada;
- tareas recomendadas;
- alternativas.

---

6. SELECCIÓN

La selección del modelo deberá considerar:

1. tipo de tarea;
2. dificultad;
3. herramientas necesarias;
4. tamaño del contexto;
5. calidad requerida;
6. velocidad;
7. coste;
8. límites disponibles.

---

7. MODELO ADECUADO

Ejemplo:

TAREA SIMPLE
→ modelo rápido/económico.

TAREA DE RAZONAMIENTO
→ modelo con mejor razonamiento.

TAREA DE CONTEXTO GRANDE
→ modelo con contexto suficiente.

TAREA DE PROGRAMACIÓN
→ modelo especializado o con buen rendimiento de código.

---

8. MODELO PRINCIPAL

El sistema podrá definir un modelo preferente para cada categoría.

Ejemplo conceptual:

GENERAL
→ modelo principal.

RAZONAMIENTO
→ modelo avanzado.

INVESTIGACIÓN
→ modelo rápido + herramientas web.

CÓDIGO
→ modelo especializado.

Los modelos concretos se decidirán después de evaluar APIs disponibles.

---

9. FALLBACK

Si el modelo principal falla:

1. detectar error;
2. comprobar motivo;
3. seleccionar alternativa;
4. reconstruir contexto;
5. continuar.

No reiniciar la tarea.

---

10. TIPOS DE FALLLO

Distinguir:

- límite de peticiones;
- límite de tokens;
- cuota agotada;
- API caída;
- timeout;
- error de autenticación;
- modelo no disponible;
- respuesta inválida;
- error de herramienta.

Cada tipo puede requerir una estrategia diferente.

---

11. ROTACIÓN

La rotación de modelos no debe ser arbitraria.

No cambiar de modelo simplemente porque existe otro.

Cambiar cuando:

- se alcance un límite;
- falle el proveedor;
- otro modelo sea claramente más adecuado;
- el coste sea excesivo;
- una tarea requiera una capacidad diferente.

---

12. CONTINUIDAD

Al cambiar de modelo:

RECUPERAR:

- objetivo;
- tarea;
- subtrabajo;
- estado;
- resultados;
- decisiones;
- fuentes;
- restricciones.

Después:

CONTINUAR.

---

13. CONTEXTO NORMALIZADO

Para facilitar el cambio de modelo, el motor utilizará un contexto normalizado.

Ejemplo conceptual:

{
objetivo,
tarea,
estado,
memoria_relevante,
resultados,
restricciones,
herramientas,
instrucciones
}

El formato definitivo se determinará durante la implementación.

---

14. CONTROL DE TOKENS

El motor deberá evitar enviar información innecesaria.

Antes de una llamada:

1. seleccionar memoria relevante;
2. eliminar duplicados;
3. resumir información antigua;
4. conservar decisiones importantes;
5. conservar datos necesarios.

---

15. CONTEXTO LARGO

Cuando el contexto sea demasiado grande:

DIVIDIR
o
RESUMIR
o
RECUPERAR POR PARTES.

No eliminar información crítica.

---

16. COSTE

Cada llamada podrá registrar:

- proveedor;
- modelo;
- tokens;
- duración;
- coste estimado;
- resultado.

Esto permitirá controlar el gasto.

---

17. MODO GRATUITO

El sistema priorizará APIs gratuitas o con cuota gratuita cuando sean
suficientes.

Pero:

GRATUITO ≠ AUTOMÁTICAMENTE MEJOR.

Si la calidad es insuficiente, podrá utilizar otra alternativa.

---

18. ESTRATEGIA DE COSTE

Prioridad conceptual:

1. gratuito adecuado;
2. bajo coste;
3. modelo avanzado cuando sea necesario;
4. intervención humana si el coste supera límites establecidos.

---

19. CUOTAS

El sistema deberá registrar el consumo conocido de cada proveedor.

Ejemplo:

PROVIDER
→ DAILY_LIMIT
→ USED
→ REMAINING

La implementación real dependerá de las APIs utilizadas.

---

20. PROVEEDORES INDEPENDIENTES

Si un proveedor deja de funcionar:

no debe detener todo el motor si existe una alternativa válida.

Ejemplo:

Gemini
↓
sin cuota
↓
Groq
↓
sin cuota
↓
OpenRouter
↓
continuar.

---

21. CONSISTENCIA

Cambiar de modelo no debe cambiar las reglas del proyecto.

Las reglas proceden de:

MOTOR
+
REPOSITORIO
+
PROYECTO

No del modelo.

---

22. MODELOS DE DIFERENTE CALIDAD

El sistema podrá utilizar modelos diferentes dentro de una misma tarea.

Ejemplo:

MODELO RÁPIDO
→ recopilación

MODELO AVANZADO
→ análisis

MODELO RÁPIDO
→ clasificación

Esto puede reducir costes.

---

23. DELEGACIÓN

Una tarea compleja puede dividirse entre modelos.

Ejemplo:

INVESTIGACIÓN
→ modelo rápido.

ANÁLISIS
→ modelo avanzado.

RESUMEN
→ modelo económico.

VALIDACIÓN
→ segundo modelo independiente.

---

24. VALIDACIÓN CRUZADA

Para resultados críticos podrá utilizarse más de un modelo.

MODELO A
+
MODELO B

→ COMPARAR.

Si coinciden:

mayor confianza.

Si discrepan:

analizar discrepancia.

No asumir automáticamente que la mayoría tiene razón.

---

25. MODELO DE VALIDACIÓN

En tareas de alto impacto, el modelo que produce un resultado no debería ser
la única fuente de validación cuando exista una alternativa razonable.

---

26. DISPONIBILIDAD

Cada modelo podrá tener estado:

AVAILABLE
LIMITED
UNAVAILABLE
ERROR

El selector deberá excluir temporalmente modelos que no puedan utilizarse.

---

27. RECUPERACIÓN

Después de un error temporal:

esperar según estrategia de backoff y volver a comprobar disponibilidad.

No realizar llamadas repetitivas sin control.

---

28. CIRCUIT BREAKER

Si un proveedor falla repetidamente:

marcar temporalmente:

UNAVAILABLE

y utilizar una alternativa.

Posteriormente podrá comprobarse de nuevo.

---

29. SEGURIDAD

Las claves API deberán almacenarse fuera de los documentos públicos.

El selector nunca deberá registrar claves ni secretos en logs.

---

30. OBSERVABILIDAD

Debe poder conocerse:

- qué modelo trabajó;
- por qué fue seleccionado;
- cuánto consumió;
- si falló;
- qué modelo lo sustituyó;
- si el resultado fue validado.

---

31. DECISIÓN DEL SELECTOR

El selector recibe:

TASK_PROFILE

y devuelve:

MODEL_SELECTION.

Ejemplo conceptual:

TASK_PROFILE
→ dificultad: ALTA
→ contexto: GRANDE
→ calidad: ALTA
→ coste: MODERADO

↓

MODEL_SELECTION
→ proveedor X
→ modelo Y

---

32. REGLA DE NO DEPENDENCIA

Nunca diseñar una parte del motor suponiendo que un único proveedor estará
siempre disponible.

---

33. NUEVOS PROVEEDORES

Añadir un nuevo proveedor deberá consistir en:

1. registrar proveedor;
2. registrar modelos;
3. definir capacidades;
4. definir límites;
5. definir coste;
6. probar;
7. habilitar.

No reconstruir el motor.

---

34. DECISIONES PENDIENTES

Antes de implementación se deberán comprobar realmente:

- APIs gratuitas disponibles;
- límites actuales;
- modelos disponibles;
- compatibilidad con n8n;
- coste;
- velocidad;
- capacidades;
- restricciones de uso.

No convertir información antigua en una regla permanente.

---

35. PRINCIPIO FINAL

El motor no debe preguntar:

"¿Qué IA usamos?"

Debe preguntar:

"¿Qué capacidad necesita esta tarea?"

Y después:

"¿Cuál es el mejor proveedor disponible para proporcionar esa capacidad
ahora?"

---

36. PRÓXIMO TRABAJO

El siguiente documento será:

SISTEMA DE VALIDACIÓN Y CONTROL DE CALIDAD

Definirá cómo comprobar que el trabajo realizado por las distintas IAs es
correcto antes de incorporarlo a la memoria y utilizarlo para tomar nuevas
decisiones.

