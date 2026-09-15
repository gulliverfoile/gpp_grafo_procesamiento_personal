================================================================================
GPP — Mapa de Percepción Personal
v2.1 | HTML autónomo | Event-driven | Sin dependencias
================================================================================

REPO: gpp_agp3
ARCHIVO PRINCIPAL: gpp_grafo_procesamiento_personal.html
AUTOR: [tu nombre]
FECHA: 2026-08-09

================================================================================
1. QUÉ ES
================================================================================

Esto NO es un test de personalidad. NO es un diagnóstico. NO es una teoría
científica. NO te dice "eres esto".

Es un mapa personal, tosco y subjetivo. Un intento de poner nombre a formas de
percibir y procesar el mundo para poder hablar de ellas sin pasar por etiquetas
médicas. No mide: describe lo que tú mismo marcas. No clasifica: hace visibles
mecanismos que normalmente no se ven.

No sé si el modelo es "cierto". No creo que lo sea en sentido científico. Es
una idea, una hipótesis de trabajo, hecha desde mi experiencia y mi forma de
mirar. Si te sirve para reflexionar sobre ti, bien. Si no, también.

================================================================================
1.b DESDE DÓNDE HABLO
================================================================================

Esto está hecho desde una perspectiva concreta. No es neutro.

Tengo aphantasia: no veo imágenes cuando recuerdo o imagino. Eso significa que
mi forma de procesar el mundo es más verbal, abstracta y conceptual que visual.

Cuando alguien dice "el elefante en la habitación", a mí no me llega una
imagen. Me llega la idea. Eso condiciona cómo he construido este mapa: los
nodos, las descripciones, los ejemplos. Todo está pensado desde ahí.

Si tú sí ves imágenes, este mapa puede no encajar igual. No es un defecto
del mapa ni tuyo. Es que está hecho desde una forma concreta de procesar, y no
pretende ser universal.

Hay otras condiciones de fondo que no se eligen: oído absoluto, hiperfantasia,
sinestesia, sensibilidad sensorial alta o baja, interocepción fina o gruesa.
No son nodos del mapa. Son moduladores. Están antes de todo lo demás y sesgan
el resto. No los meto como nodos porque no son algo que marcas: son algo que
ya está.

================================================================================
2. QUÉ HACE
================================================================================

Presenta cinco capas de procesamiento, cada una con preguntas con ejemplos
cotidianos (no términos técnicos):

  Capa 0 — ¿Cómo te entra la información?     (visual, auditivo, corporal, etc.)
  Capa 1 — ¿Cómo le das vueltas?              (paso a paso, saltos, verbal, etc.)
  Capa 2 — ¿Qué haces después de actuar?      (observas, analizas, sientes, etc.)
  Capa 3 — ¿Cómo actúas y te recuperas?       (espontaneidad, pausas, etc.)
  Capa 4 — ¿Cómo te relacionas con los demás? (empatía, normas, ajuste, etc.)

El usuario marca cada nodo con una intensidad de 0 a 3:
  Nunca / A veces / A menudo / Siempre

Puede marcar varios por capa, incluso contradictorios. No hay límite. La
intensidad no es una medida clínica: es una forma de que el mapa tenga algo de
textura en lugar de ser una lista de casillas.

Antes de las capas hay una "brújula" con cuatro modos principales (visual,
verbal, kinestésico, abstracto) que pre-selecciona algunos nodos, para que no
empieces de cero. La brújula no es una categoría: es solo un punto de partida.

Al generar el perfil, se muestran tres visualizaciones:

  a) NARRATIVA
     Texto en segunda persona que describe tu mapa capa por capa, con frases
     cotidianas y observaciones sobre patrones detectados. Escrito en tono
     suave, sin veredictos: "si esto te describe...", no "tú eres...".

  b) RADAR
     Polígono que muestra la intensidad media de cada capa. Rápido y visual.

  c) GRAFO DE RED
     Cinco anillos concéntricos (sensorial en el centro, relacional fuera).
     Los nodos activos brillan y se conectan con líneas teóricas o de flujo
     entre capas adyacentes. Las aristas son intuiciones de co-ocurrencia,
     no medidas: están puestas a mano porque me parecía que encajaban.

Persistencia: guarda el perfil en localStorage del navegador. Al volver a abrir
la página, recupera tu selección.

================================================================================
3. CÓMO SE USA
================================================================================

1. Abre el archivo HTML en cualquier navegador moderno.
   No necesitas servidor, ni instalar nada.

2. Opcional: elige un modo en la brújula para que se marquen automáticamente
   algunos nodos (luego puedes desmarcar los que quieras).

3. Lee cada capa y marca la intensidad que corresponda a tu experiencia.
   No hay respuestas correctas. Marca todo lo que reconozcas.

4. Pulsa "Ver mi mapa →".

5. Navega entre las tres pestañas (Narrativa, Radar, Grafo).

6. Para empezar de cero, pulsa "Empezar de cero".

================================================================================
4. ARQUITECTURA TÉCNICA
================================================================================

Arquitectura hexagonal ligera (puertos y adaptadores). Event-driven, sin
orquestador central. El flujo emerge de los cambios de estado.

Organización del código, de dentro hacia fuera:

  [NÚCLEO]       Dominio puro, sin I/O.
                 - Profile: entidad inmutable.
                 - ProfileUseCases: operaciones puras.

  [PUERTOS]      Contratos/abstracciones (documentados como JSDoc).
                 - PersistencePort, EventBusPort, RendererPort.

  [ADAPTADORES]  Implementaciones concretas de los puertos.
                 - LocalStoragePersistence, SimpleEventBus,
                   FormRenderer, NarrativeRenderer,
                   RadarRenderer, GraphRenderer,
                   CompassController, TabsController.

  [APLICACIÓN]   Servicio que orquesta el núcleo con los puertos.
                 - ProfileService.

  [COMPOSICIÓN]  Wiring único en bootstrap().

Reglas de dependencia:
  - El núcleo no conoce nada de fuera.
  - Los casos de uso no conocen adaptadores.
  - Los adaptadores no se conocen entre sí.
  - Solo bootstrap() los conoce a todos y los conecta.

Esto prepara el terreno para sustituir, por ejemplo, LocalStoragePersistence
por una API remota, o los renderizadores por componentes de otro framework,
sin tocar el núcleo.

Flujo:
  - El usuario cambia una intensidad → el adaptador llama al servicio.
  - El servicio aplica el caso de uso puro, persiste y emite
    "PROFILE_CHANGED" (y "PROFILE_REPLACED" si el cambio es global).
  - Los renderizadores escuchan y se redibujan solos.
  - No hay un "jefe" que coordine el orden; cada componente reacciona al
    evento cuando le llega.

Tecnologías:
  - HTML5 + CSS3 (sin frameworks)
  - JavaScript vanilla (ES6+)
  - SVG nativo para el radar y el grafo
  - localStorage para persistencia
  - Sin dependencias externas (no npm, no build, no bundler)

================================================================================
5. POR QUÉ EXISTE
================================================================================

Los tests existentes suelen ser categóricos ("eres esto") o jerárquicos
("te desvías de la norma"). Esta herramienta intenta lo contrario:

  - Dimensional: cada mecanismo es independiente y tiene intensidad.
  - Horizontal: no hay una norma, no hay un centro privilegiado.
  - Cualitativo: el resultado es una descripción en lenguaje natural,
    no un número ni un porcentaje diagnóstico.

El objetivo es dar un vocabulario estructurado pero flexible para que cada
persona se describa a sí misma sin pasar por etiquetas médicas o psicológicas.

No sé si el modelo es cierto. Es una idea, un mapa tosco, hecho desde mi
experiencia. Tómalo como una invitación a pensar sobre ti mismo, no como una
verdad.

================================================================================
6. QUÉ NO ES
================================================================================

  - No es un test psicométrico.
  - No es un diagnóstico.
  - No es una teoría validada.
  - No es universal.
  - No mide: describe.
  - No clasifica: muestra.
  - No juzga: visibiliza.
  - No sustituye a un profesional.

Es un espejo hecho a mano. Tosco, personal, incompleto.

================================================================================
7. CÓMO MODIFICAR / EXTENDER
================================================================================

Para añadir un nuevo nodo a una capa:
  1. Edita la constante LAYERS en el script (bloque 0).
  2. Añade un objeto { id, label, desc } en la capa correspondiente.
  3. Si quieres que tenga aristas con otros nodos, añade la entrada en EDGES
     (from, to, type).
  4. Recarga la página.

Para cambiar los colores de las capas:
  1. Busca en LAYERS la propiedad "color"/"hex" de cada capa y cámbiala.
  2. También puedes cambiar los estilos CSS asociados.

Para modificar la brújula (presets):
  1. Busca el objeto COMPASS_PRESETS en el script.
  2. Cada clave es un modo, y su valor es un objeto { nodeId: intensidad }.
  3. Añade o quita nodos según quieras.

Para cambiar el grafo (posiciones, radios, etc.):
  1. Busca la clase GraphRenderer.
  2. Modifica el objeto radii (radios de los anillos).
  3. O ajusta el cálculo de ángulos (offset o distribución).

Para cambiar la persistencia (ej: pasar a una API):
  1. Crea un adaptador que implemente PersistencePort (load/save).
  2. Sustitúyelo en bootstrap() por LocalStoragePersistence.
  3. El resto del código no se entera: el núcleo no conoce adaptadores.

Para cambiar los renderizadores (ej: usar otro framework):
  1. Crea clases que implementen RendererPort (método render(profile)).
  2. Sustitúyelas en bootstrap().
  3. El servicio y el núcleo no se enteran.

================================================================================
8. DISCLAIMER
================================================================================

Esta herramienta NO es un diagnóstico clínico. NO sustituye la evaluación
profesional. Es un espejo para la autoexploración y un vocabulario para
describir la variación cognitiva sin etiquetas.

No se recopilan datos. Todo se guarda localmente en tu navegador.

El modelo subyacente es una idea personal, no una teoría validada
científicamente. Tómala como una invitación a pensar sobre ti mismo, no como
una verdad.

Está hecho desde una perspectiva concreta (aphantasia incluida). Puede no
encajar en otras. Las descripciones y los ejemplos están sesgados hacia una
forma verbal y abstracta de procesar. Si tu forma de procesar es muy visual,
es posible que algunos nodos no te digan gran cosa: eso no es un fallo tuyo,
es un límite del mapa.

================================================================================
9. LICENCIA
================================================================================

agp3

================================================================================
10. CONTACTO / CRÉDITOS
================================================================================

Desarrollado como prueba de concepto para un modelo de cognición basado en
percepción y procesamiento, sin categorías fijas ni normas implícitas.

Si usas este código, modifícalo, rómpelo, mejóralo. No hay autoridad aquí.
