# Metodología de desarrollo — Gabriel / Vainilla

Cómo se construyó el módulo de Reclutamiento (`reclutamiento-app`), escrito para
no volver a explicarlo. **El método es fijo; el stack se confirma al arrancar.**

Leelo completo antes del primer commit de un proyecto nuevo, y de nuevo antes de
abrir un spec.

## Qué hay en este repo

| | |
|---|---|
| `README.md` | Este documento: el método, la estructura de `specs/` y las reglas. |
| `plantillas/requirements.md` | El esqueleto del spec, para llenar durante el brainstorming. |
| `plantillas/plan.md` | El esqueleto del plan de implementación, con una task de ejemplo. |

Para arrancar un sub-proyecto: creá `specs/<categoría>/<YYYY-MM-DD>-<tema>/` en
el repo del proyecto y copiá las dos plantillas ahí.

---

## 0. Antes de escribir código: confirmar el stack

El stack cambia de proyecto a proyecto. No lo supongas por parecido con el
anterior: **preguntá con `AskUserQuestion`** y anotá lo confirmado en el campo
`Tech Stack` del `plan.md`, para que quien ejecute no tenga que adivinar.

Las seis dimensiones que hay que dejar cerradas:

| Dimensión | Qué se confirma | Lo que respondió `reclutamiento-app` |
|---|---|---|
| Framework y runtime | versión exacta, y si el App Router / los Server Actions están en juego | Next.js 16 App Router, React 18.3.1, TypeScript 5.9 |
| Base de datos y auth | motor, quién autentica, si hay una base compartida con producción | Supabase (Postgres + Auth + Storage), base remota compartida |
| UI y sistema de diseño | registro de componentes, versión de Tailwind, tokens | shadcn/ui `new-york`, Tailwind v4 (`@theme inline`, sin `tailwind.config.js`) |
| Pruebas | corredor, si hay pruebas contra base real, y el gate de tipos | Vitest 2 + `tsc --noEmit` aparte |
| Despliegue | hosting, variables de entorno, cómo se prueba lo desplegado | Vercel + CI en GitHub Actions |
| Idioma | del código, de la UI y de los commits | UI, mensajes, comentarios y commits en español de Costa Rica (voseo) |

Lo de la columna derecha es **lo que eligió un proyecto**, no una ley. Lo único
que se hereda entero es lo que sigue.

---

## 1. El ciclo, de idea a producción

Cada paso cierra con un artefacto en disco. Si no hay artefacto, el paso no
terminó.

| # | Paso | Skill | Artefacto |
|---|---|---|---|
| 1 | Brainstorming | `superpowers:brainstorming` | `requirements.md` |
| 2 | Plan | `superpowers:writing-plans` | `plan.md` |
| 3 | Aislamiento | `superpowers:using-git-worktrees` | worktree + rama |
| 4 | Ejecución | `superpowers:subagent-driven-development` (o `executing-plans`) | código + checkboxes marcados |
| 5 | Revisión | `superpowers:requesting-code-review` / `receiving-code-review` | hallazgos resueltos |
| 6 | Verificación | `superpowers:verification-before-completion` | salida de los gates, pegada |
| 7 | Cierre | `superpowers:finishing-a-development-branch` | merge a `main` + deploy probado |

Tres cosas que no se saltan:

- **Brainstorming antes de plan mode.** El brainstorming es conversacional y va
  **una decisión por vez**: preguntar, esperar respuesta, anotarla, seguir. Un
  cuestionario de diez puntos de una sola vez no es brainstorming.
- **El plan se escribe antes de tocar código**, y se escribe para que lo ejecute
  otro agente sin volver a preguntar nada.
- **Cada sub-proyecto es su propia rama y su propio spec.** Un refinamiento de un
  día también.

---

## 2. Estructura de `specs/`

```
specs/
  base-datos/       esquema, migraciones estructurales
  backend/          capa de datos y acciones de servidor
  frontend/         pantallas
  infraestructura/  despliegue, storage, migraciones de plataforma
  seguridad/        anti-abuso, auditorías, hardening
  refinamientos/    todo lo que llegó después de que la pantalla ya existía
    2026-08-28-estados-de-rrhh/
      requirements.md
      plan.md
      task-5-report.md
```

- **Una carpeta por sub-proyecto**, nombrada `YYYY-MM-DD-<tema>` con la fecha del
  brainstorming (en `backend/` y `frontend/` el tema arranca con la categoría:
  `2026-08-11-backend-puestos`). La fecha no se actualiza después: dice cuándo se
  decidió, no cuándo se tocó.
- **`requirements.md` siempre.** Es el único archivo obligatorio; un spec que
  sólo documenta una decisión o un runbook se queda ahí solo.
- **`plan.md` cuando hay código que escribir.**
- **`task-N-report.md` cuando una task pide evidencia** — conteos, verificación
  contra datos reales, comparaciones. Lo escribe quien ejecutó la task, no quien
  planeó.
- Las carpetas **no se borran ni se reescriben** cuando el producto cambia. Un
  cambio posterior es un spec nuevo en `refinamientos/` que dice qué reemplaza.

---

## 3. Anatomía de `requirements.md`

Secciones, en este orden:

1. **`# Requirements — <Categoría>: <tema>`**
2. **`## Por qué`** — el problema, en prosa. Qué se rompe hoy o qué no se puede
   hacer. Sin esto, el spec no se puede juzgar más adelante.
3. **`## Decisiones confirmadas (con Gabriel, <fecha>, en brainstorming
   conversacional)`** — el corazón del documento. Una sub-sección numerada por
   decisión, y cada una dice **por qué** se decidió así, no sólo qué se decidió.
   Las decisiones que sorprenden llevan el razonamiento completo: dentro de un
   mes, esa explicación es lo único que evita deshacerlas por error.
4. **`## Estructura de archivos`** — qué se crea y qué se modifica.
5. **`## Fuera de alcance de este spec`** — explícito, para que nadie lo
   reabra por su cuenta ni lo dé por olvidado.
6. **`## Supuestos (a confirmar si algo no calza)`** — lo que se asumió sin
   verificar. Cuando la implementación choca, se empieza a mirar acá.

Secciones que aparecen según el caso: `Superficie de backend reutilizada`,
`Modelo de datos`, `Manejo de errores`, `Estrategia de pruebas`,
`Pasos manuales de Gabriel` (lo que sólo él puede hacer en un panel),
`Incógnitas a resolver durante la implementación`.

Si el sub-proyecto se entregó a medias, eso va **arriba, en un `## ⚠️ ESTADO`**,
antes del `Por qué`.

---

## 4. Anatomía de `plan.md`

### Encabezado

```markdown
# <Categoría>: <tema> — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use
> superpowers:subagent-driven-development (recommended) or
> superpowers:executing-plans to implement this plan task-by-task.
> Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** una frase.
**Architecture:** el diseño en un párrafo — qué es puro, qué es servidor, dónde
está el riesgo real.
**Tech Stack:** el confirmado en el paso 0.
**Spec:** ruta al `requirements.md`.
```

### `## Global Constraints`

Lo que vale para **todas** las tasks, escrito una sola vez: worktree y rama,
capas, prohibiciones de tipos, de dónde salen los controles de UI, idioma, y el
**baseline de pruebas** ("192 tests, 11 archivos, 0 fallos") para que un fallo
ajeno se reconozca como ajeno y no se declare *flaky* sin evidencia.

### `### Task N: <título>`

Cada task trae, en este orden:

- **`Files:`** — `Create:` / `Modify:` / `Test:`, con rutas exactas.
- **`Interfaces:`** — `Consumes:` qué necesita de tasks anteriores;
  `Produces:` qué deja y **qué task lo consume**. Esto es lo que permite que
  cada task la ejecute un agente distinto sin leer el plan entero.
- **Steps con checkbox**, en ciclo TDD: escribir el test que falla → correrlo y
  ver que falla (con el fallo esperado escrito, literal) → implementar →
  correrlo verde → gate de tipos.

El código de ejemplo va **literal** en el plan cuando es una adaptación o una
decisión; cuando es un port de un archivo que está en disco, el plan manda a
leerlo y sólo escribe **las diferencias** — transcribir mil líneas dentro de un
plan introduce errores de copia.

Una task es del tamaño de un commit. Si no cabe en uno, son dos tasks.

---

## 5. Reglas de código

Generales, se heredan:

- **Capas explícitas.** La capa de datos recibe el cliente como último
  parámetro y no importa nada del framework; el envoltorio de servidor va
  aparte. La capa pura —la que tiene el riesgo— vive en su propio módulo y es la
  que se prueba de verdad.
- **Prohibido forzar tipos con `as`.** También en el código de ejemplo del plan:
  revisalo antes de entregar el plan, no lo dejés para el revisor.
- **Los controles de UI salen del sistema de diseño**, nunca escritos a mano.
- **Las acciones se disparan desde el manejador de evento**, no desde un efecto.
- **Un error del backend que la persona necesita leer se devuelve, no se lanza**
  (en Next.js, un `throw` en una server action de producción llega al cliente
  enmascarado).
- **Todo el texto visible y los comentarios en el idioma confirmado**, con la
  variante confirmada (acá: español de Costa Rica, voseo).

---

## 6. Gates de verificación

Antes de decir que algo está listo, corré los gates y **pegá la salida**. Nada
se declara hecho por deducción.

- **El corredor de pruebas no typechequea.** `tsc --noEmit` es su propio gate,
  cada vez.
- **Lint** sobre las carpetas del proyecto.
- **Pruebas unitarias** — las que no hablan con ninguna base. Se corren cuantas
  veces haga falta.
- **Pruebas contra base real** — en serie, a pedido explícito, y **preguntá
  antes si no fue Gabriel quien las pidió**. Van con freno de mano: en terminal
  piden confirmación escrita; sin terminal exigen una variable de entorno que
  afirma que sabés que vas a escribir sobre datos reales.
- **CI** corre los gates para que no dependan de que alguien se acuerde, y
  **no aplica migraciones** a propósito.
- Las pruebas que escriben llevan **invariantes de cierre** que aseveran que la
  base quedó como estaba, y esas redes **cuentan las filas que tocaron** antes
  de anunciar que restauraron algo: un `update` que no encontró ninguna fila no
  es un error en SQL, y una vez anunció "ya restauré" con el sistema caído.

---

## 7. Git

- **Una rama por sub-proyecto**, nombrada por el tema (`estados-rrhh`,
  `buscar-por-telefono`), y un **worktree propio** fuera del checkout principal
  (`C:\wt\<rama>`). Durante la ejecución, los specs se editan **por la ruta del
  worktree**, nunca por la del checkout principal.
- **Mensajes de commit en español, que nombran el efecto**, no el archivo:
  «La búsqueda deja de hacer tres viajes a Oregon para hacer uno». Los que sólo
  toman documentación o infraestructura llevan prefijo (`docs:`, `fix(cron):`).
- **Cerrá con merge a `main`**, con commit de merge, y desplegá.
- **Commiteá los archivos nuevos temprano.** Una sesión paralela corriendo
  `git clean` ya borró catorce archivos sin versionar.
- Lo que es un bypass local (un permiso abierto para probar) **no se commitea**;
  se deja sin versionar a propósito para que se note.

---

## 8. Base de datos y entorno real

Estas reglas salieron de incidentes, no de teoría. Se re-confirman por proyecto,
pero el reflejo se hereda:

- **Averiguá a qué base apunta el entorno local antes de escribir.** Si es la de
  producción —lo fue en `reclutamiento-app`—, entonces no existe "una prueba
  rápida": nunca un reset, nunca borrar una fila real, y **cada escritura
  precedida por un `SELECT`** que muestre exactamente qué se va a tocar.
- **Resolvé la fila objetivo por `id`, nunca por posición en una lista.** «El
  primero» de una pantalla suele ser el que está en uso.
- **Los fixtures de prueba se marcan** con un prefijo reservado (`__test__`), el
  prefijo va en el `where` de cada `delete` —no en la selección de ids— y se usa
  `starts_with(col,'__test__')`, no `like`: en SQL el `_` es comodín.
- **Un renombre o un `drop column` va en dos migraciones**: expandir → desplegar
  → contraer. Con una sola, la app rompe en las dos direcciones.
- **Levantá staging** si el proyecto va a durar. Reproducir las migraciones
  desde cero es lo que revela que no se podían reproducir.
- **Los secretos van por stdin**, no por `argv` (`printf | <cli> env add`).
- **Si una herramienta te deniega o bloquea una llamada: parate y reportalo.**
  No la reintentés por otra shell ni con otra herramienta — ese desvío ya
  convirtió un bloqueo en un incidente real.
- **Lo desplegado se prueba desplegado.** Un servidor local puede estar
  sirviendo otra rama.

---

## 9. Documentación viva del proyecto

Tres lugares, tres trabajos distintos:

| Archivo | Qué lleva |
|---|---|
| `CLAUDE.md` | Sólo lo que **muerde** si no se lee: la trampa del entorno, el gate que no es obvio, el rol que no es lo que su nombre dice. Cada punto con el incidente que lo justifica. |
| `docs/*.md` | Runbooks y referencias: sistema de diseño, despliegue, recuperación de contraseña, seguridad. Lo que se consulta cuando algo falla. |
| `specs/` | El **porqué** de cada decisión de producto. No se poda. |

Un doc nuevo se escribe cuando un incidente enseñó algo **que el código no
confiesa**. Lo que el código ya dice —estructura, comandos de `package.json`,
nombres de archivos— no se copia a un doc: se deja donde no puede quedar viejo.

Si hay más de un documento describiendo lo mismo, uno gana: escribí la
**jerarquía** explícita (vigente / superado / retirado) en el README, con el
porqué de que los superados se sigan citando.

---

## 10. Cómo trabajar con Gabriel

- **Las decisiones se preguntan con `AskUserQuestion`**, con opciones y la
  recomendación primero. No en texto plano.
- **Sus preguntas son para entender, no pedidos de construir.** Respondé la
  pregunta y ofrecé «la versión que se entrega hoy» antes de ampliar el alcance.
- **Vocabulario:** confirmá los términos del dominio que tienen dos sentidos
  antes de usarlos en un spec (en Reclutamiento, «sucursal» tenía dos sentidos
  opuestos y «la entrevista» era el formulario público).
- **Los pasos que sólo él puede dar** —un panel de terceros, una variable de
  producción, un correo— van listados aparte en el spec, y se le entregan como
  pasos numerados, no mezclados con el trabajo del agente.
