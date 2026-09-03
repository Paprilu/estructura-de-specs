<!--
PLANTILLA DE PLAN DE IMPLEMENTACIÓN

Se escribe ANTES de tocar código, y se escribe para que lo ejecute otro agente
sin volver a preguntar nada.

Antes de entregarlo, cuatro controles:
  1. ¿Cada task cabe en un commit? Si no, son dos tasks.
  2. ¿`Interfaces` dice qué produce cada task y QUÉ TASK lo consume?
  3. ¿El código de ejemplo está libre de `as` para forzar tipos?
  4. ¿El baseline de pruebas está escrito, para que un fallo ajeno se reconozca
     como ajeno?

Borrá este comentario y la task de ejemplo cuando escribas las reales.
-->

# <Categoría>: <tema> — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use
> superpowers:subagent-driven-development (recommended) or
> superpowers:executing-plans to implement this plan task-by-task.
> Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** <una frase: qué queda funcionando cuando esto termina>

**Architecture:** <el diseño en un párrafo: qué es puro, qué corre en el
servidor, dónde está el riesgo real y por eso qué se prueba de verdad>

**Tech Stack:** <el confirmado al arrancar, con versiones exactas>

**Spec:** `specs/<categoría>/<YYYY-MM-DD>-<tema>/requirements.md`

<!-- OPCIONAL: si hay código que se porta de otro lado, decí de dónde y que está
en disco y es legible. Portar leyendo el archivo original produce menos errores
que transcribirlo dentro del plan; acá van sólo las adaptaciones, literales.

**Fuente del port:** `<ruta>`
-->

## Global Constraints

<!-- Lo que vale para TODAS las tasks, escrito una sola vez. -->

- **Worktree:** todo ocurre en `<ruta del worktree>` (rama `<rama>`). Nunca
  editar por la ruta del checkout principal.
- **Capas:** <qué importa qué, y qué no puede importar nada del framework>.
- **Prohibido `as`** para forzar tipos.
- **Los controles de UI salen de `<carpeta del sistema de diseño>`**, nunca
  escritos a mano.
- **Idioma:** <el confirmado, con la variante: español de Costa Rica, voseo>.
- **Fixtures de test** con prefijo `__test__`, limpiadas por el propio test.
  Las filas de semilla no se mutan.
- **Baseline antes de empezar:** <N tests, M archivos, 0 fallos>. Si aparecen
  fallos que no son tuyos, reportalos con nombre y mensaje; no los declares
  *flaky* sin evidencia.

---

### Task 1: <título de la task>

**Files:**
- Create: `<ruta>`
- Modify: `<ruta>` (`<qué función o sección>`)
- Test: `<ruta del archivo de test>`

**Interfaces:**
- Consumes: <qué necesita de tasks anteriores, o «nada»>.
- Produces: `<lo que deja>`. Lo consume la Task <N>.

- [ ] **Step 1: Escribir el test que falla**

<!-- El test va literal acá, no descrito. Los comentarios dentro del test dicen
qué protege cada aserción. -->

```ts
```

- [ ] **Step 2: Correr el test para verificar que falla**

```bash
<comando, apuntado a este test>
```

Expected: FALLA con <el mensaje esperado, literal>.

- [ ] **Step 3: <implementar la pieza mínima>**

<!-- El código literal, y las advertencias de lo que NO hay que re-declarar o
duplicar porque ya existe. -->

```ts
```

- [ ] **Step 4: Correr el test y verlo verde**

```bash
<comando>
```

- [ ] **Step 5: Gate de tipos**

```bash
npx tsc --noEmit
```

<!-- El corredor de pruebas no typechequea. Este gate va en cada task, no una
vez al final. -->

---

### Task 2: <título de la task>

<!-- Misma estructura. -->

---

## Cierre

- [ ] Los gates completos en verde, con la salida pegada: tipos, lint, pruebas
      unitarias.
- [ ] <Las pruebas contra base real, si aplican: a pedido explícito, en serie.>
- [ ] Revisión de código pedida y hallazgos resueltos.
- [ ] `requirements.md` actualizado si la implementación cambió una decisión —
      y editado **por la ruta del worktree**.
- [ ] Merge a `main` y despliegue, probado donde quedó desplegado.
