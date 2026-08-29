ARQUITECTURA TÉCNICA DEL MOTOR AUTÓNOMO DE IA

Estado

DISEÑO INICIAL — pendiente de validación mediante implementación.

1. Principio

El motor será una capa de orquestación independiente de los repositorios y de los proveedores de IA.

Flujo:

USUARIO → CANAL → ORQUESTADOR → CONTEXTO → IA → HERRAMIENTAS → VALIDACIÓN → ESTADO → SIGUIENTE TAREA

El ciclo se repite mientras exista trabajo autónomo válido.

2. Capas

A — Control

Recibe órdenes de David y devuelve únicamente avisos relevantes.

Funciones:

- activar;
- pausar;
- continuar;
- desactivar;
- consultar estado.

Telegram será el primer canal. WhatsApp queda para una fase posterior.

B — Orquestación

n8n coordinará ciclos, llamadas, herramientas, estados, errores y notificaciones.

n8n no debe contener la metodología específica de cada proyecto.

C — Descubrimiento

Al seleccionar un repositorio, el motor localizará:

- instrucciones;
- README;
- documentos maestros;
- reglas;
- metodología;
- estado;
- estructura del proyecto.

D — Contexto

Cada ejecución recibirá solamente el contexto necesario:

- reglas aplicables;
- objetivo;
- estado;
- tarea;
- resultados relevantes;
- restricciones.

No se enviará indiscriminadamente todo el repositorio al modelo.

E — IA

La capa IA será intercambiable.

Proveedores candidatos:

- Gemini;
- Claude;
- Groq;
- OpenRouter;
- OpenAI;
- otros compatibles.

El modelo podrá seleccionarse según tarea, disponibilidad, coste y límites.

F — Herramientas

El motor podrá disponer progresivamente de herramientas para:

- GitHub;
- investigación web;
- análisis de datos;
- archivos;
- APIs externas;
- otras herramientas necesarias.

Las capacidades reales deberán validarse antes de automatizarlas.

G — Memoria y estado

El estado debe sobrevivir a cada ejecución y a cada cambio de modelo.

Debe permitir recuperar:

- repositorio;
- proyecto;
- objetivo;
- fase;
- tarea;
- subtrabajo;
- resultados;
- decisiones;
- pendientes;
- bloqueos;
- intervención requerida;
- siguiente tarea.

H — Validación

Una tarea no se considera terminada solo porque la IA haya producido texto.

Cuando sea posible, el sistema comprobará el resultado mediante datos, archivos,
fuentes, reglas o verificaciones posteriores.

I — Intervención humana

Si una acción imprescindible no puede realizarla el motor:

PAUSAR → NOTIFICAR → ESPERAR

3. Multi-repositorio

La cuenta objetivo es:

davidsaenz20

El motor no tendrá una lista rígida de repositorios como arquitectura fundamental.

Debe poder descubrir repositorios actuales y futuros accesibles para la cuenta.

Cada repositorio conserva su propia metodología.

Ejemplo:

MOTOR
├── base-proyectos
├── pensador-de-ideas
├── otro-repositorio
└── futuros repositorios

4. Adaptación por repositorio

El motor deberá construir un perfil temporal:

REPOSITORIO
→ reglas
→ metodología
→ estructura
→ proyectos
→ estado
→ capacidades

Si existe ambigüedad crítica, deberá pedir intervención en lugar de inventar reglas.

5. Modelo de ejecución

Cada ciclo seguirá aproximadamente:

1. cargar sesión;
2. cargar estado;
3. identificar reglas;
4. identificar objetivo;
5. seleccionar tarea;
6. construir contexto;
7. seleccionar modelo;
8. ejecutar;
9. validar;
10. registrar resultado;
11. actualizar estado;
12. decidir siguiente tarea;
13. continuar o pausar.

6. Máquina de estados

Estados mínimos previstos:

IDLE
RUNNING
WAITING_HUMAN
PAUSED
ERROR
COMPLETED
STOPPED

El significado definitivo se concretará durante el diseño de workflows.

7. Continuidad

La continuidad no dependerá de mantener abierta una conversación de ChatGPT.

n8n deberá iniciar y encadenar ejecuciones utilizando el estado persistente.

La finalización de una tarea NO es una condición de parada.

Si existe otra tarea autónoma válida, debe iniciarse.

8. Paradas

El motor se detendrá ante:

- acción manual imprescindible;
- bloqueo irresoluble;
- error crítico;
- límite técnico o de seguridad;
- PAUSAR;
- DESACTIVAR.

No debe detenerse simplemente para informar de un avance normal.

9. Seguridad

Principios:

- mínimo privilegio;
- credenciales fuera de documentación pública;
- evitar acciones destructivas;
- validación antes de operaciones sensibles;
- trazabilidad;
- separación entre repositorios;
- protección frente a pérdida de estado.

10. GitHub

GitHub será el entorno principal de repositorios.

El motor debe distinguir:

- lectura;
- preparación de cambios;
- escritura autorizada;
- intervención manual.

ChatGPT no debe intentar escribir directamente en GitHub desde la conversación.

El mecanismo de escritura definitivo queda pendiente de diseño y permisos reales.

11. Escalabilidad

La arquitectura debe permitir:

1 repositorio → varios → todos los actuales → nuevos repositorios futuros.

Añadir un repositorio no debe requerir reconstruir el motor.

Añadir un proveedor IA tampoco debería requerir rediseñar el sistema.

12. Costes

Se priorizará:

- modelos adecuados;
- proveedores gratuitos cuando sean suficientes;
- proveedores alternativos;
- reducción de llamadas;
- contexto optimizado.

Los límites reales de cada API deberán comprobarse antes de establecer la rotación.

13. Próximo diseño

Antes de construir workflows definitivos se deberán especificar:

1. esquema de estado;
2. esquema de memoria;
3. registro de tareas;
4. descubrimiento de repositorios;
5. descubrimiento de reglas;
6. selector de modelos;
7. sistema de validación;
8. máquina de estados;
9. mecanismo de continuidad;
10. intervención humana;
11. workflows n8n;
12. seguridad;
13. Telegram;
14. WhatsApp.

14. Regla de evolución

Este documento es técnico y vivo.

Toda decisión importante deberá quedar registrada aquí o en documentación técnica específica.

No convertir hipótesis en arquitectura confirmada sin validación.

