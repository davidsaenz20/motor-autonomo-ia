SISTEMA DE VALIDACIÓN Y CONTROL DE CALIDAD

1. OBJETIVO

Definir cómo el motor comprueba que un resultado producido por una IA,
herramienta o investigación es suficientemente fiable antes de utilizarlo
para tomar nuevas decisiones.

El sistema debe evitar que un error se propague por todo el proyecto.

---

2. PRINCIPIO

GENERAR
↓
COMPROBAR
↓
VALIDAR
↓
REGISTRAR
↓
UTILIZAR

Un resultado no debe convertirse automáticamente en conocimiento válido.

---

3. NIVELES DE VALIDACIÓN

Se utilizarán conceptualmente cuatro niveles:

NIVEL 0 — SIN VALIDAR

NIVEL 1 — COMPROBACIÓN BÁSICA

NIVEL 2 — CONTRASTE

NIVEL 3 — VALIDACIÓN CRÍTICA

La intensidad dependerá de la importancia del resultado.

---

4. NIVEL 0

Se aplica a resultados preliminares.

Puede utilizarse para:

- ideas;
- hipótesis;
- exploración;
- brainstorming.

Debe marcarse claramente como no validado.

---

5. NIVEL 1

Comprobación básica:

- coherencia;
- formato;
- cumplimiento de la tarea;
- ausencia de errores evidentes.

Adecuado para tareas de bajo riesgo.

---

6. NIVEL 2

Validación mediante contraste.

Puede incluir:

- segunda fuente;
- segunda búsqueda;
- otro modelo;
- comparación con datos;
- comprobación independiente.

Adecuado para información que afectará a decisiones importantes.

---

7. NIVEL 3

Validación crítica.

Se utilizará cuando un error pueda provocar:

- pérdida económica;
- decisión estratégica equivocada;
- publicación incorrecta;
- modificación importante;
- daño al proyecto;
- riesgo de seguridad.

Debe existir una comprobación independiente siempre que sea posible.

---

8. IMPORTANCIA DEL RESULTADO

La necesidad de validación dependerá de:

IMPACTO
+
RIESGO
+
REVERSIBILIDAD
+
INCERTIDUMBRE.

A mayor riesgo, mayor validación.

---

9. HECHOS

Un dato considerado hecho deberá tener evidencia suficiente.

Cuando sea posible:

HECHO
+
FUENTE
+
FECHA.

---

10. HIPÓTESIS

Las hipótesis deberán marcarse como:

HIPÓTESIS.

Nunca deben almacenarse como hechos confirmados.

---

11. CONCLUSIONES

Una conclusión deberá distinguir entre:

DATOS
→ evidencia disponible.

INTERPRETACIÓN
→ análisis de los datos.

CONCLUSIÓN
→ resultado razonado.

---

12. FUENTES

Las fuentes importantes deberán registrarse.

Como mínimo:

- referencia;
- URL cuando exista;
- fecha;
- información respaldada;
- nivel de confianza.

---

13. CONTRADICCIONES

Si dos fuentes proporcionan información incompatible:

1. detectar;
2. registrar;
3. comparar;
4. buscar información adicional;
5. determinar cuál es más fiable;
6. conservar la contradicción si no puede resolverse.

No ocultar contradicciones.

---

14. VALIDACIÓN CRUZADA DE IA

Cuando sea apropiado:

MODELO A
↓
RESULTADO

MODELO B
↓
RESULTADO

COMPARAR.

Una discrepancia importante deberá investigarse.

---

15. LIMITACIONES DE LOS MODELOS

El sistema debe asumir que una IA puede:

- inventar información;
- interpretar mal una fuente;
- calcular incorrectamente;
- omitir información;
- generar datos plausibles pero falsos.

Por ello:

IA ≠ FUENTE ABSOLUTA DE VERDAD.

---

16. VALIDACIÓN DE CÁLCULOS

Los cálculos importantes deberán comprobarse mediante:

- herramienta matemática;
- código;
- cálculo independiente;
- segunda comprobación.

No confiar únicamente en cálculo mental del modelo.

---

17. VALIDACIÓN DE DATOS

Los datos deberán comprobarse cuando sea relevante:

- formato;
- unidades;
- fechas;
- duplicados;
- valores imposibles;
- consistencia.

---

18. VALIDACIÓN DE INVESTIGACIÓN

Una investigación deberá comprobar:

- suficiente cobertura;
- fuentes relevantes;
- actualidad;
- posibles sesgos;
- contradicciones;
- conclusiones justificadas.

---

19. VALIDACIÓN SEO

Cuando se investiguen palabras clave, búsquedas o intención:

No considerar un único dato como prueba suficiente de demanda.

Cuando sea posible combinar:

- volumen;
- intención;
- competencia;
- tendencias;
- SERP;
- comportamiento observable;
- fuentes independientes.

---

20. VALIDACIÓN DE IDEAS DE NEGOCIO

Una idea no deberá considerarse viable únicamente porque:

"suena bien".

Debe analizarse, cuando corresponda:

- problema;
- cliente;
- demanda;
- competencia;
- diferenciación;
- adquisición;
- monetización;
- costes;
- dificultad;
- riesgos;
- escalabilidad.

---

21. VALIDACIÓN DE CÓDIGO

Cuando el motor trabaje con código:

1. revisar lógica;
2. comprobar sintaxis;
3. ejecutar pruebas disponibles;
4. analizar errores;
5. revisar efectos secundarios.

Si no puede ejecutar una prueba:

registrarlo.

---

22. VALIDACIÓN DE ARCHIVOS

Antes de considerar un archivo terminado:

- comprobar estructura;
- comprobar contenido;
- comprobar referencias;
- comprobar coherencia;
- comprobar que no se han eliminado datos importantes.

---

23. VALIDACIÓN DE CAMBIOS

Antes de aplicar una modificación:

COMPROBAR:

- archivo correcto;
- ruta correcta;
- objetivo correcto;
- cambio mínimo necesario;
- ausencia de efectos no deseados.

---

24. ESCRITURA

Una modificación real solo debe producirse mediante un mecanismo autorizado.

Si David debe realizarla manualmente:

ACTION_REQUIRED.

El motor debe proporcionar:

- ruta;
- contenido;
- instrucciones;
- motivo.

---

25. RESULTADO NO VÁLIDO

Si la validación falla:

INVALID.

La tarea no debe marcarse como completada.

El agente deberá:

- corregir;
- volver a validar;
- o solicitar intervención si no puede resolverlo.

---

26. RESULTADO PARCIAL

Puede existir:

PARTIALLY_VALIDATED.

Esto significa que parte del resultado está comprobada y otra parte no.

---

27. CONFIANZA

El resultado podrá tener:

ALTA
MEDIA
BAJA

La confianza no sustituye la evidencia.

---

28. PROPAGACIÓN DE ERRORES

Un resultado de baja confianza no debería convertirse automáticamente en
entrada crítica para otras tareas.

Debe mantenerse etiquetado.

---

29. REGISTRO

Cada validación importante deberá poder registrar:

- resultado;
- nivel;
- método;
- fuentes;
- modelo utilizado;
- fecha;
- problemas;
- confianza;
- decisión.

---

30. AUTOMATIZACIÓN

La validación deberá automatizarse cuando sea posible.

Ejemplos:

- comprobación de formato;
- tests;
- comparación de datos;
- detección de duplicados;
- validación de estructura;
- comprobación de URLs;
- comprobaciones matemáticas.

---

31. VALIDACIÓN HUMANA

El sistema no debe intentar automatizar a toda costa decisiones que requieren
criterio humano.

Cuando la validación automática no sea suficiente:

WAITING_HUMAN.

---

32. UMBRAL DE CALIDAD

Cada tipo de tarea podrá definir un umbral mínimo.

Ejemplo:

TAREA SIMPLE
→ NIVEL 1.

DECISIÓN DE NEGOCIO
→ NIVEL 2.

OPERACIÓN CRÍTICA
→ NIVEL 3.

---

33. COSTE DE VALIDACIÓN

La validación también tiene coste.

El motor debe equilibrar:

CALIDAD
vs.
COSTE
vs.
TIEMPO
vs.
RIESGO.

No realizar validaciones excesivas en tareas triviales.

---

34. RESULTADO VALIDADO

Cuando el resultado supera el umbral:

VALIDATED = true

Entonces puede:

- almacenarse;
- utilizarse;
- alimentar nuevas tareas;
- influir en decisiones.

---

35. REVALIDACIÓN

La información puede quedar obsoleta.

Podrá requerirse nueva validación cuando:

- cambie el contexto;
- pase suficiente tiempo;
- cambien los datos;
- aparezca información contradictoria.

---

36. FUENTE DE VERDAD

La fuente de verdad dependerá del tipo de información.

Puede ser:

- documento oficial;
- datos primarios;
- API;
- repositorio;
- resultado validado;
- fuente independiente.

La IA por sí sola no será considerada fuente de verdad salvo que el
contexto lo justifique explícitamente.

---

37. REGLA FINAL

Antes de utilizar un resultado importante:

¿ESTÁ VALIDADO?

Si:

SÍ → utilizar.

NO → validar.

NO PUEDE VALIDARSE → marcar incertidumbre o solicitar intervención.

---

38. PRÓXIMO TRABAJO

El siguiente documento deberá definir:

PROTOCOLO DE INTERVENCIÓN HUMANA

Debe establecer exactamente cómo el motor detecta que necesita a David,
cómo registra la acción necesaria, cómo le informa y cómo reanuda el trabajo
cuando David confirma que la acción está realizada.

