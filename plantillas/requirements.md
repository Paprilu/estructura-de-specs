<!--
PLANTILLA DE REQUIREMENTS

Se llena DURANTE el brainstorming, no después: una decisión por vez, preguntar,
esperar la respuesta, anotarla, seguir.

Antes de entregarlo, tres controles:
  1. ¿El «Por qué» se entiende sin conocer la conversación?
  2. ¿Cada decisión dice POR QUÉ, y no sólo qué?
  3. ¿«Fuera de alcance» y «Supuestos» están llenos? Vacíos casi siempre
     significan que no se pensaron, no que no existan.

Borrá este comentario y las secciones opcionales que no apliquen.
-->

# Requirements — <Categoría>: <tema>

<!-- OPCIONAL, y va acá arriba si aplica: si el sub-proyecto se entregó a
medias, decilo antes que nada, con lo que sí se construyó y lo que no.

## ⚠️ ESTADO: <qué se construyó de verdad>
-->

## Por qué

<!-- El problema, en prosa. Qué se rompe hoy, o qué no se puede hacer, o quién
lo pidió y para qué. Sin esto el spec no se puede juzgar dentro de seis meses. -->

## Decisiones confirmadas (con <nombre>, <YYYY-MM-DD>, en brainstorming conversacional)

<!-- El corazón del documento. Una sub-sección numerada por decisión.

Cada una dice POR QUÉ se decidió así. Las que sorprenden llevan el razonamiento
completo: esa explicación es lo único que, dentro de un mes, evita que alguien
las deshaga por error creyendo que fue un descuido. -->

### 1. <la decisión, como afirmación>

<!-- Qué se hace, y por qué esa y no la alternativa que se descartó. -->

### 2. <la decisión, como afirmación>

## Estructura de archivos

<!-- Qué se crea y qué se modifica, con rutas. Si todavía depende del plan,
titulalo «(a confirmar en el plan de implementación)». -->

## Fuera de alcance de este spec

<!-- Explícito, para que nadie lo reabra por su cuenta ni lo dé por olvidado.
Si algo se pospuso a propósito, decí a dónde va. -->

## Supuestos (a confirmar si algo no calza)

<!-- Lo que se asumió sin verificar. Cuando la implementación choque contra la
realidad, acá es donde se empieza a mirar. -->

<!--
SECCIONES OPCIONALES — agregá sólo las que el sub-proyecto necesite:

## Superficie de backend reutilizada
   Qué ya existe y se consume tal cual, con la firma exacta. Evita que la
   implementación reescriba lo que ya está.

## Modelo de datos
   Tablas y columnas nuevas o modificadas, con el tipo y el porqué de cada una.

## Manejo de errores
   Las clases de error y el tratamiento distinto de cada una.

## Estrategia de pruebas
   Qué se prueba de verdad (la lógica pura y riesgosa) y qué se deja fuera, con
   el porqué de dejarlo fuera.

## Pasos manuales de <nombre> (panel de <servicio>)
   Lo que sólo una persona puede hacer, en pasos numerados y verificables.

## Incógnitas a resolver durante la implementación
   Lo que se decidió no decidir todavía, y quién lo decide.
-->
