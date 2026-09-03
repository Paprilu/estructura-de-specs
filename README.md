# Desarrollo guiado por especificaciones — método y estructura

Cómo se lleva un proyecto de software de la idea a producción: el ciclo de
trabajo, la estructura de las especificaciones y las reglas que sostienen las
dos cosas.

> **Este documento no describe ningún proyecto.** No fija stack, ni nombres de
> tablas, ni rutas, ni reglas de entorno. **El método es fijo; el contexto se
> confirma en cada proyecto** y se anota donde dice la §0.
>
> Si estás leyendo esto dentro de un proyecto, ninguna de las decisiones de
> contexto está tomada todavía por el hecho de aparecer acá. Las reglas propias
> del proyecto viven en **su** archivo de instrucciones (`CLAUDE.md`,
> `AGENTS.md` o equivalente), no en este documento.

## Qué hay en este repo

| | |
|---|---|
| `README.md` | Este documento: el método, la estructura de especificaciones y las reglas. |
| `plantillas/requirements.md` | El esqueleto de la especificación, para llenar durante el brainstorming. |
| `plantillas/plan.md` | El esqueleto del plan de implementación, con una task de ejemplo. |

Para arrancar un sub-proyecto: creá `specs/<categoría>/<YYYY-MM-DD>-<tema>/` en
el repo del proyecto y copiá las dos plantillas ahí.

---

## 0. Antes de escribir código: fijar el contexto del proyecto

Nada de lo que sigue se hereda de otro proyecto por parecido. **Preguntá y
confirmá** cada dimensión al arrancar —con preguntas de opción múltiple, no
abiertas— y anotá la respuesta donde corresponda, para que quien ejecute no
tenga que adivinar.

| Dimensión | Qué se confirma | Dónde se anota |
|---|---|---|
| Framework y runtime | Cuál, y la **versión exacta**: las convenciones cambian entre mayores. | `Tech Stack` del plan |
| Datos y autenticación | Motor, quién autentica, y **a qué base apunta el entorno local** (ver §8). | `CLAUDE.md` del proyecto |
| UI y sistema de diseño | De dónde salen los componentes, qué motor de estilos y dónde viven los tokens. | Un `docs/` propio, citado desde `CLAUDE.md` |
| Pruebas | Corredor, qué se prueba de verdad, si hay pruebas contra datos reales, y **si el corredor typechequea o no**. | `CLAUDE.md` + `Global Constraints` del plan |
| Despliegue | Hosting, variables de entorno, y **cómo se comprueba lo desplegado**. | Un runbook en `docs/` |
| Idioma | Del código, de la UI, de los comentarios y de los commits, con la variante regional. | `Global Constraints` del plan |

Dos dimensiones más que conviene cerrar antes de la primera task: **quién decide**
cuando hay una ambigüedad de producto, y **qué pasos sólo puede dar una persona**
(un panel de terceros, una variable de producción, un correo).

---

## 1. El ciclo, de idea a producción

Siete pasos. Cada uno cierra con un artefacto en disco: si no hay artefacto, el
paso no terminó.

| # | Paso | Artefacto |
|---|---|---|
| 1 | **Brainstorming** — entender el problema y decidir el alcance | `requirements.md` |
| 2 | **Plan** — el diseño y las tasks, antes de tocar código | `plan.md` |
| 3 | **Aislamiento** — rama y espacio de trabajo propios | worktree + rama |
| 4 | **Ejecución** — task por task, en ciclo de prueba primero | código + checkboxes marcados |
| 5 | **Revisión** — código revisado contra la especificación | hallazgos resueltos |
| 6 | **Verificación** — los gates corridos, con la salida a la vista | evidencia pegada |
| 7 | **Cierre** — integración, despliegue y comprobación | merge + deploy probado |

Tres cosas que no se saltan:

- **El brainstorming va antes del plan, y es conversacional:** una decisión por
  vez —preguntar, esperar la respuesta, anotarla, seguir—. Un cuestionario de
  diez puntos disparado de una sola vez no es brainstorming; es un formulario, y
  se responde peor.
- **El plan se escribe antes de tocar código**, y se escribe para que lo ejecute
  alguien más sin volver a preguntar nada.
- **Cada sub-proyecto es su propia rama y su propia especificación.** Un
  refinamiento de un día también.

Si trabajás con un agente que tiene skills de proceso disponibles, este ciclo se
mapea uno a uno con ellas (*brainstorming*, *writing-plans*, *using-git-worktrees*,
*subagent-driven-development* o *executing-plans*, *requesting-code-review*,
*verification-before-completion*, *finishing-a-development-branch*). Sin ellas el
ciclo es el mismo: cambia quién se acuerda de cada paso.

---

## 2. Estructura de `specs/`

```
specs/
  <categoría>/
    <YYYY-MM-DD>-<tema>/
      requirements.md      obligatorio
      plan.md              cuando hay código que escribir
      task-N-report.md     cuando una task pide evidencia
```

**Las categorías son las capas del proyecto**, y se eligen una vez al arrancar.
Un juego que funciona bien como punto de partida: `base-datos/`, `backend/`,
`frontend/`, `infraestructura/`, `seguridad/` y `refinamientos/` —esta última
para todo lo que llega después de que la pantalla ya existía, que con el tiempo
es la mayoría—. Adaptalas al proyecto; lo que importa es que sean pocas y que
cada sub-proyecto caiga en una sin discusión.

Reglas de la estructura:

- **Una carpeta por sub-proyecto**, nombrada con la **fecha del brainstorming**,
  no la del último cambio: la carpeta dice cuándo se decidió.
- **`requirements.md` siempre.** Es el único archivo obligatorio: una
  especificación que sólo documenta una decisión o un runbook se queda ahí sola.
- **`task-N-report.md`** lo escribe quien **ejecutó** la task, no quien la
  planeó, y lleva evidencia: conteos, comparaciones, verificación contra datos
  reales.
- **Las carpetas no se reescriben ni se borran** cuando el producto cambia. Un
  cambio posterior es una especificación nueva que dice qué reemplaza. El
  histórico es el único lugar donde queda por qué algo se hizo así.

---

## 3. Anatomía de `requirements.md`

Seis secciones, en este orden:

1. **`# Requirements — <Categoría>: <tema>`**
2. **`## Por qué`** — el problema, en prosa: qué se rompe hoy, qué no se puede
   hacer, o quién lo pidió y para qué. Sin esto, la especificación no se puede
   juzgar dentro de seis meses.
3. **`## Decisiones confirmadas (con <quién decide>, <fecha>)`** — el corazón del
   documento. Una sub-sección numerada por decisión, y **cada una dice por qué**,
   no sólo qué. Las que sorprenden llevan el razonamiento completo: esa
   explicación es lo único que evita que alguien las deshaga más adelante
   creyendo que fueron un descuido.
4. **`## Estructura de archivos`** — qué se crea y qué se modifica.
5. **`## Fuera de alcance`** — explícito, para que nadie lo reabra por su cuenta
   ni lo dé por olvidado. Si algo se pospuso, decí a dónde va.
6. **`## Supuestos (a confirmar si algo no calza)`** — lo que se asumió sin
   verificar. Cuando la implementación choque contra la realidad, acá es donde se
   empieza a mirar.

Secciones que se agregan según el caso: superficie ya existente que se reutiliza
tal cual, modelo de datos, manejo de errores, estrategia de pruebas, pasos
manuales de una persona, e incógnitas que se decidió no resolver todavía.

Si el sub-proyecto se entregó a medias, eso va **arriba de todo**, en un
`## ⚠️ ESTADO`, antes del `Por qué`.

---

## 4. Anatomía de `plan.md`

### Encabezado

- **`Goal`** — una frase: qué queda funcionando cuando esto termina.
- **`Architecture`** — el diseño en un párrafo: qué es puro, qué corre en el
  servidor, **dónde está el riesgo real** y por eso qué se prueba de verdad.
- **`Tech Stack`** — el confirmado en la §0, con versiones exactas.
- **`Spec`** — la ruta al `requirements.md`.
- **Fuente del port**, si hay código que se trae de otro lado: decilo, y si el
  original está en disco y es legible, **mandá a leerlo** en vez de transcribirlo
  dentro del plan. Copiar cientos de líneas a mano introduce errores; lo que sí
  va escrito literal son **las adaptaciones**.

### `## Global Constraints`

Lo que vale para **todas** las tasks, escrito una sola vez: dónde se trabaja,
las capas, las prohibiciones, de dónde salen los componentes, el idioma, la
convención de fixtures, y el **baseline de pruebas** ("N pruebas, M archivos, 0
fallos") para que un fallo ajeno se reconozca como ajeno y no se declare
*flaky* sin evidencia.

### `### Task N`

- **`Files:`** — `Create:` / `Modify:` / `Test:`, con rutas exactas.
- **`Interfaces:`** — `Consumes:` qué necesita de tasks anteriores; `Produces:`
  qué deja y **qué task lo consume**. Esto es lo que permite que cada task la
  ejecute alguien distinto sin leer el plan entero.
- **Steps con checkbox (`- [ ]`)**, en ciclo de prueba primero: escribir la
  prueba que falla → correrla y ver que falla, **con el fallo esperado escrito
  literal** → implementar lo mínimo → verla verde → correr el gate de tipos.

**Una task es del tamaño de un commit.** Si no cabe en uno, son dos tasks.

---

## 5. Reglas de código

Las que se heredan de proyecto a proyecto son pocas, y todas son de forma, no de
tecnología:

- **Capas explícitas.** La capa que habla con los datos recibe su cliente como
  parámetro y no importa nada del framework; el envoltorio del framework va
  aparte. **La lógica riesgosa vive en su propio módulo y es pura** — es la que
  se prueba de verdad, y separarla es lo que hace que se pueda.
- **No forzar tipos.** Ni en el código, ni en el código de ejemplo del plan:
  revisalo antes de entregar el plan, no lo dejés para el revisor.
- **Los componentes de UI salen del sistema de diseño**, nunca escritos a mano.
- **Un error que una persona necesita leer se devuelve, no se lanza.** Varios
  frameworks enmascaran las excepciones del servidor en producción; comprobá cómo
  se comporta el tuyo antes de confiar en un mensaje.
- **El idioma confirmado, con su variante regional**, en todo el texto visible,
  los mensajes y los comentarios.

Todo lo demás —qué se puede importar, qué patrón está prohibido, qué trampa tiene
este framework— es **de cada proyecto** y se escribe en su archivo de
instrucciones, con el incidente que lo justifica.

---

## 6. Gates de verificación

Antes de decir que algo está listo, corré los gates y **pegá la salida**. Nada se
declara hecho por deducción, y una suite verde no es lo mismo que una rama que
compila.

- **El gate de tipos es propio.** Muchos corredores de pruebas **no**
  typechequean: comprobalo, y si no lo hace, el chequeo de tipos se corre aparte
  **en cada task**.
- **Lint** sobre las carpetas del proyecto.
- **Pruebas que no tocan ningún dato real** — se corren cuantas veces haga falta.
- **Pruebas que escriben en datos reales** —si el proyecto las tiene— en serie, a
  pedido explícito, y con **freno de mano**: en terminal, confirmación escrita;
  sin terminal, una variable de entorno que afirme que quien la puso sabe sobre
  qué va a escribir. Y **preguntá antes de correrlas** si no fue quien decide
  quien las pidió.
- **Invariantes de cierre** en las pruebas que escriben: aseveran al terminar que
  el entorno quedó como estaba. Una red así **cuenta las filas que tocó** antes
  de anunciar que restauró algo — un `update` que no encontró ninguna fila no es
  un error en SQL, y una red que no cuenta puede anunciar "ya restauré" con el
  sistema caído.
- **CI corre los gates** para que no dependan de que alguien se acuerde. Decidí
  explícitamente si aplica migraciones o no, y escribilo: es la clase de cosa que
  después nadie sabe.

---

## 7. Git

- **Una rama por sub-proyecto**, nombrada por el tema, y un **espacio de trabajo
  propio** (worktree) fuera del checkout principal. Durante la ejecución, incluso
  las especificaciones se editan **por la ruta del worktree**: editarlas por el
  checkout principal deja el cambio en la rama equivocada.
- **Mensajes de commit que nombran el efecto, no el archivo.** «La búsqueda
  filtra mientras se escribe» dice más que «actualizar componente». Los de
  documentación o infraestructura llevan prefijo.
- **Cerrá con merge a la rama principal** y desplegá.
- **Commiteá los archivos nuevos temprano.** Lo que no está versionado lo puede
  borrar cualquier limpieza, propia o de una sesión paralela.
- **Lo que es un bypass local** —un permiso abierto para poder probar— **no se
  commitea**: se deja sin versionar a propósito, para que se note.

---

## 8. Cuando el entorno toca datos reales

**Averiguá a qué apunta tu entorno local antes de la primera escritura.** Si
apunta a datos de producción —pasa más de lo que parece—, entonces no existe "una
prueba rápida" y estas reglas dejan de ser celo:

- **Nunca un reset**, y nunca borrar una fila real: sólo lo que un fixture propio
  creó.
- **Cada escritura, precedida por una lectura** que muestre exactamente qué se va
  a tocar. Si esa lectura no te deja seguro de cuál es la fila, todavía no
  escribas.
- **Resolvé la fila objetivo por identificador, nunca por posición en una lista.**
  «El primero» de una pantalla suele ser justo el que está en uso, porque el
  producto lo ordena primero.
- **Marcá los fixtures con un prefijo reservado** (`__test__` sirve), y que el
  prefijo vaya en la **condición de cada borrado**, no sólo en la selección de
  ids: así ninguna línea puede tocar una fila real aunque el diagnóstico previo
  estuviera equivocado. Ojo con los comodines: en SQL `_` matchea cualquier
  carácter, así que usá una comparación de prefijo, no un `like`.
- **La limpieza no corre si el proceso muere.** Dejá escrito cómo borrar los
  restos de una corrida interrumpida, en el orden que respetan las llaves
  foráneas.
- **Un renombre o un borrado de columna va en dos migraciones**: expandir →
  desplegar → contraer. Con una sola, la aplicación rompe en las dos direcciones.
- **Levantá un entorno de staging** si el proyecto va a durar. Reproducir las
  migraciones desde cero es lo que revela que no se podían reproducir.
- **Los secretos van por entrada estándar, no en la línea de comandos.**
- **Lo desplegado se prueba desplegado.** Un servidor local puede estar sirviendo
  otra rama.
- **Si una herramienta te deniega o bloquea una llamada: parate y reportalo.** No
  la reintentés por otra vía — ese desvío es exactamente cómo un bloqueo que
  funcionó se convierte en un incidente.

---

## 9. La documentación viva del proyecto

Tres lugares, tres trabajos distintos. Confundirlos es lo que hace que nadie lea
ninguno:

| Dónde | Qué lleva |
|---|---|
| **Archivo de instrucciones** (`CLAUDE.md`, `AGENTS.md`) | Sólo lo que **muerde** si no se lee: la trampa del entorno, el gate que no es obvio, el rol que no hace lo que su nombre dice. Cada punto con el incidente que lo justifica. Es el lugar de las reglas **de este proyecto**. |
| **`docs/`** | Runbooks y referencias: sistema de diseño, despliegue, seguridad, procedimientos. Lo que se consulta cuando algo falla. |
| **`specs/`** | El **porqué** de cada decisión de producto. No se poda. |

Dos reglas para que no se degrade:

- **Lo que el código ya dice, no se copia a un documento.** Estructura de
  carpetas, comandos del gestor de paquetes, nombres de archivos: se dejan donde
  no pueden quedar viejos. Un documento gana su lugar cuando dice lo que el
  código **no confiesa**: la convención no escrita, el porqué de una decisión, la
  trampa que ningún archivo de configuración admite.
- **Si dos documentos describen lo mismo, uno gana.** Escribí la **jerarquía**
  explícita —vigente / superado / retirado— y el porqué de que los superados se
  sigan citando, si se siguen citando.

---

## 10. Cómo trabajar con quien pide el trabajo

- **Las decisiones se preguntan con opciones**, y con una recomendación primero.
  Una pregunta abierta traslada trabajo en vez de resolverlo.
- **Una pregunta no es un pedido de construir.** Contestá la pregunta, y ofrecé
  «la versión que se entrega hoy» antes de ampliar el alcance por tu cuenta.
- **Confirmá el vocabulario del dominio antes de usarlo en una especificación.**
  Los términos que en la organización tienen dos sentidos —y casi siempre hay
  alguno— son la fuente de error más cara: el malentendido se detecta recién en
  la pantalla terminada.
- **Los pasos que sólo puede dar una persona** —un panel de terceros, una
  variable de producción, un correo— van listados aparte y numerados, no
  mezclados con el trabajo técnico.
