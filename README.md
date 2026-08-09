================================================================================
GPP — Mapa de Percepción Personal
v2.0 | HTML autónomo | Event-driven | Sin dependencias
================================================================================

REPO: gpp_agp3
ARCHIVO PRINCIPAL: gpp_grafo_procesamiento_personal.html
AUTOR: [tu nombre]
FECHA: 2026-08-09

================================================================================
1. QUÉ ES
================================================================================

Esto NO es un test de personalidad. NO es un diagnóstico. NO te dice "eres esto".

Es una herramienta de autoexploración, un mapa de coordenadas donde cada persona
marca qué formas de percibir y procesar el mundo reconoce en sí misma.

Parte de una idea personal: no existen "cajas neurológicas" (TEA, TDAH, etc.)
como categorías fijas. Todo es espectro, y cada persona tiene su propia mezcla de
mecanismos. El mapa solo describe dónde te posicionas en varias capas funcionales,
sin juicio de valor y sin pretender ser una verdad objetiva.

Esta herramienta es un espejo, no un manual de instrucciones. No sé si el modelo
es "cierto" o si funciona para todo el mundo; es una hipótesis que pongo a
disposición para que cada cual juegue con ella y saque sus propias conclusiones.

================================================================================
2. QUÉ HACE
================================================================================

Presenta cinco capas de procesamiento, cada una con preguntas con ejemplos
cotidianos (no términos técnicos):

  Capa 0 — ¿Cómo te entra la información?   (visual, auditivo, corporal, etc.)
  Capa 1 — ¿Cómo le das vueltas?            (paso a paso, saltos, verbal, etc.)
  Capa 2 — ¿Qué haces después de actuar?    (observas, analizas, sientes, etc.)
  Capa 3 — ¿Cómo actúas y te recuperas?     (espontaneidad, pausas, etc.)
  Capa 4 — ¿Cómo te relacionas con los demás? (empatía, normas, ajuste, etc.)

El usuario marca los nodos que le ocurren (checkboxes). Puede marcar varios por
capa, incluso contradictorios. No hay límite.

Antes de las capas hay una "brújula" con cuatro modos principales (visual,
verbal, kinestésico, abstracto) que pre-selecciona nodos típicos, para que no
empieces de cero.

Al generar el perfil, se muestran tres visualizaciones:

  a) NARRATIVA
     Texto en segunda persona que describe tu mapa capa por capa, con frases
     cotidianas y observaciones sobre patrones detectados.

  b) MAPA DE CALOR (barras)
     Barras horizontales que muestran qué porcentaje de nodos tienes activos
     en cada capa. Rápido y visual.

  c) GRAFO DE RED
     Cinco anillos concéntricos (sensorial en el centro, relacional fuera).
     Los nodos activos brillan y se conectan con líneas teóricas o de flujo
     entre capas adyacentes.

Persistencia: guarda el perfil en localStorage del navegador. Al volver a abrir
la página, recupera tu selección.

================================================================================
3. CÓMO SE USA
================================================================================

1. Abre el archivo HTML en cualquier navegador moderno.
   No necesitas servidor, ni instalar nada.

2. Opcional: elige un modo en la brújula para que se marquen automáticamente
   algunos nodos (luego puedes desmarcar los que quieras).

3. Lee cada capa y marca las casillas que describan tu experiencia.
   No hay respuestas correctas. Marca todo lo que reconozcas.

4. Pulsa "Ver mi mapa →".

5. Navega entre las tres pestañas (Narrativa, Mapa de calor, Grafo).

6. Para empezar de cero, pulsa "Empezar de cero".

================================================================================
4. ARQUITECTURA TÉCNICA
================================================================================

Event-driven, sin orquestador central. El flujo emerge de los cambios de estado.

  - El usuario hace clic en un checkbox → se emite un evento interno.
  - El perfil (Profile) se actualiza y se guarda en localStorage.
  - Se emite un evento "PROFILE_CHANGED".
  - Los tres renderizadores (narrativa, heatmap, grafo) escuchan ese evento y
    se redibujan automáticamente.
  - No hay un "jefe" que coordine el orden; cada componente reacciona al evento
    cuando le llega. El flujo emerge, no se impone.

Tecnologías:
  - HTML5 + CSS3 (sin frameworks)
  - JavaScript vanilla (ES6+)
  - SVG nativo para el grafo
  - localStorage para persistencia
  - Sin dependencias externas (no npm, no build, no bundler)

La lógica está organizada en bloques claros dentro del mismo HTML:

  - CONFIGURACIÓN: LAYERS y EDGES (nodos y aristas)
  - DOMINIO: clase Profile (entidad pura)
  - EVENT BUS: pub/sub simple
  - PERSISTENCIA: load/save en localStorage
  - UI: renderizado de checkboxes y brújula
  - RENDERIZADORES: narrativa, heatmap, grafo
  - INICIO: carga del perfil y suscripciones

================================================================================
5. POR QUÉ EXISTE
================================================================================

Los tests existentes suelen ser categóricos ("eres esto") o jerárquicos
("te desvías de la norma"). Esta herramienta intenta lo contrario:

  - Dimensional: cada mecanismo es independiente.
  - Horizontal: no hay una norma, no hay un centro privilegiado.
  - Cualitativo: el resultado es una descripción en lenguaje natural,
    no un número ni un porcentaje.

El objetivo es dar un vocabulario estructurado pero flexible para que cada
persona se describa a sí misma sin pasar por etiquetas médicas o psicológicas.

No sé si el modelo es cierto. Es una idea, una hipótesis de trabajo.
Si te sirve para reflexionar sobre ti mismo, genial. Si no, también.

================================================================================
6. CÓMO MODIFICAR / EXTENDER
================================================================================

Para añadir un nuevo nodo a una capa:
  1. Edita la constante LAYERS en el script (al principio).
  2. Añade un objeto { id, label, desc } en la capa correspondiente.
  3. Si quieres que tenga aristas con otros nodos, añade la entrada en EDGES
     (from, to, type).
  4. Recarga la página.

Para cambiar los colores de las capas:
  1. Busca en LAYERS la propiedad "color" de cada capa y cámbiala (hex o nombre).
  2. También puedes cambiar los estilos CSS asociados a :root (--c0 a --c4).

Para modificar la brújula (presets):
  1. Busca el objeto COMPASS_PRESETS.
  2. Cada clave es un modo, y su valor es un array de IDs de nodos.
  3. Añade o quita IDs según quieras.

Para cambiar el grafo (posiciones, radios, etc.):
  1. Busca la función renderGraph().
  2. Modifica el objeto radii (cambia los radios de los anillos).
  3. O ajusta el cálculo de ángulos (offset o la distribución).

Para cambiar persistencia (ej: pasar a una API):
  1. Reemplaza las funciones loadProfile() y saveProfile() por otras que
     se comuniquen con tu backend.
  2. El resto del código no se entera.

================================================================================
7. DISCLAIMER
================================================================================

Esta herramienta NO es un diagnóstico clínico. NO sustituye la evaluación
profesional. Es un espejo para la autoexploración y un vocabulario para describir
la variación cognitiva sin etiquetas.

No se recopilan datos. Todo se guarda localmente en tu navegador.

El modelo subyacente es una idea personal, no una teoría validada científicamente.
Tómala como una invitación a pensar sobre ti mismo, no como una verdad.

================================================================================
8. LICENCIA
================================================================================

agp3

================================================================================
9. CONTACTO / CRÉDITOS
================================================================================

Desarrollado como prueba de concepto para un modelo de cognición basado en
percepción y procesamiento, sin categorías fijas ni normas implícitas.

Si usas este código, modifícalo, rómpelo, mejóralo. No hay autoridad aquí.

================================================================================
