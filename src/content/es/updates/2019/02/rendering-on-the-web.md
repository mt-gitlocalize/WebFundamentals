project_path: "/web/_project.yaml"
book_path: "/web/updates/_book.yaml"

{# wf_updated_on: 2019-08-27 #} {# wf_published_on: 2019-02-06 #} {# wf_tags:
fundamentals, performance, app-shell #} {# wf_featured_image:
/web/updates/images/2019/02/rendering-on-the-web/icon.png #} {#
wf_featured_snippet: Where should we implement logic and rendering in our
applications? Should we use Server Side Rendering? What about Rehydration? Let's
find some answers! #} {# wf_blink_components: N/A #}

# Renderizado en la Web {: .page-title }

{% include "web/_shared/contributors/developit.html" %} {% include
"web/_shared/contributors/addyosmani.html" %}

Como desarrolladores, a menudo nos enfrentamos con decisiones que afectarán toda
la arquitectura de nuestras aplicaciones. Una de las decisiones centrales que
deben tomar los desarrolladores web es dónde implementar la lógica y el
renderizado en su aplicación. Esto puede ser difícil, ya que hay varias formas
diferentes de crear un sitio web.

Nuestra comprensión de este espacio se basa en nuestro trabajo en Chrome
hablando con sitios grandes en los últimos años. En términos generales,
alentaríamos a los desarrolladores a considerar la representación del servidor o
la representación estática en lugar de un enfoque de rehidratación completo.

Con el fin de comprender mejor las arquitecturas que estamos eligiendo cuando
tomamos esta decisión, necesitamos tener una comprensión sólida de cada enfoque
y una terminología consistente para usar al hablar sobre ellos. Las diferencias
entre estos enfoques ayudan a ilustrar las compensaciones de renderizar en la
web a través de la lente del rendimiento.

## Terminología {: #terminology }

**Representación**

- **SSR:** Representación del lado del servidor **:** representa una aplicación
del lado del cliente o universal en HTML en el servidor.
- **CSR:** Representación del lado del cliente **:** representación de una
aplicación en un navegador, generalmente utilizando DOM.
- **Rehidratación:** "arrancando" las vistas de JavaScript en el cliente de modo
que reutilicen el árbol y los datos DOM del HTML del servidor.
- **Representación previa:** ejecutar una aplicación del lado del cliente en
tiempo de compilación para capturar su estado inicial como HTML estático.

**Actuación**

- **TTFB:** Tiempo hasta el primer byte: visto como el tiempo entre hacer clic
en un enlace y el primer fragmento de contenido que ingresa.
- **FP:** Primera pintura: la primera vez que un píxel se vuelve visible para el
usuario.
- **FCP:** Primera pintura con contenido: el momento en que el contenido
solicitado (cuerpo del artículo, etc.) se hace visible.
- **TTI:** Time To Interactive: el momento en que una página se vuelve
interactiva (eventos conectados, etc.).

## Representación del servidor {: #server-rendering }

*La representación del servidor genera el HTML completo para una página en el
servidor en respuesta a la navegación. Esto evita viajes de ida y vuelta
adicionales para la obtención de datos y la creación de plantillas en el
cliente, ya que se maneja antes de que el navegador obtenga una respuesta.*

La representación del servidor generalmente produce una [primera
pintura](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics#first_paint_and_first_contentful_paint)
rápida (FP) y una [primera pintura
satisfactoria](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics#first_paint_and_first_contentful_paint)
(FCP). La ejecución de la lógica de página y la representación en el servidor
hace posible evitar el envío de mucho JavaScript al cliente, lo que ayuda a
lograr un [tiempo de
interacción](https://developers.google.com/web/tools/lighthouse/audits/time-to-interactive)
(TTI) rápido. Esto tiene sentido, ya que con la representación del servidor
realmente solo está enviando texto y enlaces al navegador del usuario. Este
enfoque puede funcionar bien para un amplio espectro de dispositivos y
condiciones de red, y abre interesantes optimizaciones del navegador, como el
análisis de documentos en tiempo real.

<img src="../../images/2019/02/rendering-on-the-web/server-rendering-tti.png"
alt="Diagram showing server rendering and JS execution affecting FCP and TTI"
width="350">

Con la representación del servidor, es poco probable que los usuarios esperen a
que se procese JavaScript vinculado a la CPU antes de que puedan usar su sitio.
Incluso cuando no se puede evitar [JS de
terceros](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/loading-third-party-javascript/)
, usar la representación del servidor para reducir sus propios [costos de JS
de](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4)
terceros puede brindarle más "
[presupuesto](https://medium.com/@addyosmani/start-performance-budgeting-dabde04cf6a3)
" para el resto. Sin embargo, hay un inconveniente principal en este enfoque:
generar páginas en el servidor lleva tiempo, lo que a menudo puede resultar en
un [Tiempo de primer byte](https://en.wikipedia.org/wiki/Time_to_first_byte)
(TTFB) más lento.

Si la representación del servidor es suficiente para su aplicación depende en
gran medida del tipo de experiencia que esté creando. Existe un debate de larga
data sobre las aplicaciones correctas de la representación del servidor frente a
la representación del lado del cliente, pero es importante recordar que puede
optar por utilizar la representación del servidor para algunas páginas y no para
otras. Algunos sitios han adoptado técnicas de renderizado híbrido con éxito.
[El](https://medium.com/dev-channel/a-netflix-web-performance-case-study-c0bcde26a9d9)
servidor de
[Netflix](https://medium.com/dev-channel/a-netflix-web-performance-case-study-c0bcde26a9d9)
renderiza sus páginas de aterrizaje relativamente estáticas, mientras
[busca](https://dev.to/addyosmani/speed-up-next-page-navigations-with-prefetching-4285)
el JS para páginas con mucha interacción, dando a estas páginas más pesadas
representadas por el cliente una mejor oportunidad de cargar rápidamente.

Muchos marcos, bibliotecas y arquitecturas modernas hacen posible renderizar la
misma aplicación tanto en el cliente como en el servidor. Estas técnicas se
pueden utilizar para la representación del servidor, sin embargo, es importante
tener en cuenta que las arquitecturas en las que la representación se produce
tanto en el servidor ***como*** en el cliente son su propia clase de solución
con características de rendimiento y compensaciones muy diferentes. Los usuarios
de React pueden usar [renderToString
()](https://reactjs.org/docs/react-dom-server.html) o soluciones creadas encima
como [Next.js](https://nextjs.org) para la representación del servidor. Los
usuarios de Vue pueden consultar la [guía de
representación](https://ssr.vuejs.org) del [servidor](https://ssr.vuejs.org) de
Vue o [Nuxt](https://nuxtjs.org) . Angular tiene
[Universal](https://angular.io/guide/universal) . Sin embargo, las soluciones
más populares emplean alguna forma de hidratación, así que tenga en cuenta el
enfoque en uso antes de seleccionar una herramienta.

## Renderizado estático {: #static-rendering }

[El renderizado
estático](https://frontarm.com/articles/static-vs-server-rendering/) ocurre en
el momento de la compilación y ofrece una primera pintura rápida, una primera
pintura satisfactoria y un tiempo para interactuar, suponiendo que la cantidad
de JS del lado del cliente es limitada. A diferencia de la Representación del
servidor, también logra lograr un Tiempo hasta el primer byte consistentemente
rápido, ya que el HTML de una página no tiene que generarse sobre la marcha. En
general, la representación estática significa producir un archivo HTML separado
para cada URL con anticipación. Con las respuestas HTML generadas de antemano,
los renders estáticos se pueden implementar en múltiples CDN para aprovechar el
almacenamiento en caché de borde.

<img src="../../images/2019/02/rendering-on-the-web/static-rendering-tti.png"
alt="Diagram showing static rendering and optional JS execution affecting FCP
and TTI" width="280">

Las soluciones para el renderizado estático vienen en todas las formas y
tamaños. Las herramientas como [Gatsby](https://www.gatsbyjs.org) están
diseñadas para hacer que los desarrolladores sientan que su aplicación se
procesa dinámicamente en lugar de generarse como un paso de compilación. Otros
como [Jekyl](https://jekyllrb.com) y [Metalsmith](https://metalsmith.io) adoptan
su naturaleza estática, proporcionando un enfoque más basado en plantillas.

Una de las desventajas de la representación estática es que se deben generar
archivos HTML individuales para cada URL posible. Esto puede ser desafiante o
incluso inviable cuando no puede predecir cuáles serán esas URL con
anticipación, o para sitios con una gran cantidad de páginas únicas.

Los usuarios de React pueden estar familiarizados con
[Gatsby](https://www.gatsbyjs.org) , [la exportación estática
Next.js](https://nextjs.org/learn/excel/static-html-export/) o
[Navi](https://frontarm.com/navi/) ; todo esto hace que sea conveniente crear
componentes. Sin embargo, es importante comprender la diferencia entre la
representación estática y la representación previa: las páginas estáticas
representadas son interactivas sin la necesidad de ejecutar mucho JS del lado
del cliente, mientras que la representación previa mejora la primera pintura o
la primera pintura satisfactoria de una aplicación de página única que debe
iniciarse en el cliente para que las páginas sean realmente interactivas.

Si no está seguro de si una solución dada es la representación estática o la
representación previa, intente esta prueba: deshabilite JavaScript y cargue las
páginas web creadas. Para las páginas representadas estáticamente, la mayor
parte de la funcionalidad seguirá existiendo sin JavaScript habilitado. Para
páginas prerrepresentadas, aún puede haber algunas funciones básicas como
enlaces, pero la mayoría de la página será inerte.

Otra prueba útil es ralentizar su red usando Chrome DevTools y observar cuánto
JavaScript se ha descargado antes de que una página se vuelva interactiva. La
representación previa generalmente requiere más JavaScript para ser interactivo,
y ese JavaScript tiende a ser más complejo que el enfoque de [Mejora
progresiva](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement)
utilizado por la representación estática.

## Representación del servidor frente a representación estática {: #server-vs-static }

La representación del servidor no es una bala de plata: su naturaleza dinámica
puede venir con costos [generales de cómputo
significativos](https://medium.com/airbnb-engineering/operationalizing-node-js-for-server-side-rendering-c5ba718acfc9)
. Muchas soluciones de renderizado de servidores no se descargan temprano,
pueden retrasar TTFB o duplicar los datos que se envían (por ejemplo, el estado
en línea utilizado por JS en el cliente). En React, renderToString () puede ser
lento ya que es síncrono y de subproceso único. Lograr que la representación del
servidor sea "correcta" puede implicar encontrar o construir una solución para
el [almacenamiento en caché de
componentes](https://medium.com/@reactcomponentcaching/speedier-server-side-rendering-in-react-16-with-component-caching-e8aa677929b1)
, administrar el consumo de memoria, aplicar técnicas de
[memorización](https://speakerdeck.com/maxnajim/hastening-react-ssr-with-component-memoization-and-templatization)
y muchas otras preocupaciones. Generalmente, está procesando / reconstruyendo la
misma aplicación varias veces, una vez en el cliente y otra en el servidor. El
hecho de que la representación del servidor pueda hacer que algo aparezca antes
no significa que tenga menos trabajo por hacer.

El renderizado del servidor produce HTML a pedido para cada URL, pero puede ser
más lento que solo servir contenido renderizado estático. Si puede agregar el
trabajo adicional, la representación del servidor + el [almacenamiento en caché
HTML](https://freecontent.manning.com/caching-in-react/) puede reducir
enormemente el tiempo de representación del servidor. La ventaja de la
representación del servidor es la capacidad de extraer más datos "en vivo" y
responder a un conjunto más completo de solicitudes de lo que es posible con la
representación estática. Las páginas que requieren personalización son un
ejemplo concreto del tipo de solicitud que no funcionaría bien con la
representación estática.

La representación del servidor también puede presentar decisiones interesantes
al construir un [PWA](https://developers.google.com/web/progressive-web-apps/) .
¿Es mejor usar el almacenamiento en caché de [trabajadores de servicio
de](https://developers.google.com/web/fundamentals/primers/service-workers/)
página completa, o simplemente procesar piezas individuales de contenido del
servidor?

## Representación del lado del cliente (CSR) {: #csr }

*La representación del lado del cliente (CSR) significa renderizar páginas
directamente en el navegador usando JavaScript. Toda la lógica, la obtención de
datos, la creación de plantillas y el enrutamiento se manejan en el cliente en
lugar del servidor.*

La representación del lado del cliente puede ser difícil de obtener y mantenerse
rápida para dispositivos móviles. Puede acercarse al rendimiento de la
representación pura del servidor si hace un trabajo mínimo, mantiene un
[presupuesto ajustado de
JavaScript](https://mobile.twitter.com/HenrikJoreteg/status/1039744716210950144)
y entrega valor en la menor cantidad de
[RTT](https://en.wikipedia.org/wiki/Round-trip_delay_time) posible. Los scripts
y datos críticos se pueden entregar antes utilizando [HTTP / 2 Server
Push](https://www.smashingmagazine.com/2017/04/guide-http2-server-push/) o
`<link rel=preload>` , lo que hace que el analizador funcione para usted antes.
[Vale
la](https://developers.google.com/web/fundamentals/performance/prpl-pattern/)
pena evaluar patrones como
[PRPL](https://developers.google.com/web/fundamentals/performance/prpl-pattern/)
para garantizar que las navegaciones iniciales y posteriores se sientan
instantáneas.

<img src="../../images/2019/02/rendering-on-the-web/client-rendering-tti.png"
alt="Diagram showing client-side rendering affecting FCP and TTI" width="500">

El principal inconveniente de la representación del lado del cliente es que la
cantidad de JavaScript requerida tiende a crecer a medida que crece una
aplicación. Esto se vuelve especialmente difícil con la adición de nuevas
bibliotecas JavaScript, polyfills y código de terceros, que compiten por el
poder de procesamiento y a menudo deben procesarse antes de que se pueda
procesar el contenido de una página. Las experiencias creadas con CSR que
dependen de grandes paquetes de JavaScript deberían considerar [la división
agresiva de
código](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting/)
y asegurarse de cargar JavaScript de forma diferida: "sirve solo lo que
necesita, cuando lo necesita". Para experiencias con poca o ninguna
interactividad, la representación del servidor puede representar una solución
más escalable para estos problemas.

Para las personas que crean una aplicación de página única, la identificación de
las partes centrales de la interfaz de usuario compartida por la mayoría de las
páginas significa que puede aplicar la técnica de [almacenamiento en caché de
Application Shell](https://developers.google.com/web/updates/2015/11/app-shell)
. En combinación con los trabajadores del servicio, esto puede mejorar
dramáticamente el rendimiento percibido en las visitas repetidas.

## Combinando la representación del servidor y la CSR a través de la rehidratación {: #rehydration }

A menudo denominado Representación universal o simplemente "SSR", este enfoque
intenta suavizar las compensaciones entre la Representación del lado del cliente
y la Representación del servidor al hacer ambas cosas. Las solicitudes de
navegación, como las cargas o recargas de página completa, son manejadas por un
servidor que procesa la aplicación en HTML, luego el JavaScript y los datos
utilizados para la representación se incrustan en el documento resultante.
Cuando se implementa con cuidado, esto logra una primera pintura rápida y
satisfactoria, al igual que la representación del servidor, y luego "se
recupera" al volver a renderizar en el cliente utilizando una técnica llamada
[(re)
hidratación](https://docs.electrode.io/guides/general/server-side-data-hydration)
. Esta es una solución novedosa, pero puede tener algunos inconvenientes de
rendimiento considerables.

El principal inconveniente de SSR con rehidratación es que puede tener un
impacto negativo significativo en Time To Interactive, incluso si mejora First
Paint. Las páginas SSR a menudo se ven engañosamente cargadas e interactivas,
pero en realidad no pueden responder a las entradas hasta que se ejecuta el JS
del lado del cliente y se han adjuntado los controladores de eventos. Esto puede
llevar segundos o incluso minutos en dispositivos móviles.

Quizás haya experimentado esto usted mismo: durante un período de tiempo después
de que parece que se ha cargado una página, hacer clic o tocar no hace nada.
Esto rápidamente se vuelve frustrante ... *“¿Por qué no pasa nada? ¿Por qué no
puedo desplazarme?*

### Un problema de rehidratación: una aplicación por el precio de dos {: #rehydration-issues }

Los problemas de rehidratación a menudo pueden ser peores que la interactividad
retrasada debido a JS. Para que el JavaScript del lado del cliente pueda
"retomar" con precisión donde el servidor lo dejó sin tener que volver a
solicitar todos los datos que el servidor utilizó para representar su HTML, las
soluciones SSR actuales generalmente serializan la respuesta de una interfaz de
usuario dependencias de datos en el documento como etiquetas de script. El
documento HTML resultante contiene un alto nivel de duplicación:

<img src="../../images/2019/02/rendering-on-the-web/html.png" alt="HTML document
containing serialized UI, inlined data and a bundle.js script">

Como puede ver, el servidor devuelve una descripción de la interfaz de usuario
de la aplicación en respuesta a una solicitud de navegación, pero también
devuelve los datos de origen utilizados para componer esa interfaz de usuario y
una copia completa de la implementación de la interfaz de usuario que luego se
inicia en el cliente . Solo después de que bundle.js haya terminado de cargarse
y ejecutarse, esta IU se vuelve interactiva.

Las métricas de rendimiento recopiladas de sitios web reales que utilizan la
rehidratación de SSR indican que su uso debe ser altamente desaconsejado. En
última instancia, la razón se reduce a la experiencia del usuario: es
extremadamente fácil terminar dejando a los usuarios en un "valle extraño".

<img src="../../images/2019/02/rendering-on-the-web/rehydration-tti.png"
alt="Diagram showing client rendering negatively affecting TTI" width="600">

Sin embargo, hay esperanza para la RSS con rehidratación. En el corto plazo,
solo el uso de SSR para contenido altamente almacenable en caché puede reducir
el retraso TTFB, produciendo resultados similares a la prerrepresentación. La
rehidratación
[gradual](https://www.emberjs.com/blog/2017/10/10/glimmer-progress-report.html)
, progresiva o parcial puede ser la clave para hacer que esta técnica sea más
viable en el futuro.

## Representación del servidor de transmisión y rehidratación progresiva {: #progressive-rehydration }

La representación del servidor ha tenido una serie de desarrollos en los últimos
años.

[La representación del servidor de transmisión por secuencias
le](https://zeit.co/blog/streaming-server-rendering-at-spectrum) permite enviar
HTML en fragmentos que el navegador puede representar progresivamente a medida
que se recibe. Esto puede proporcionar una primera pintura rápida y una primera
pintura satisfactoria a medida que el marcado llega a los usuarios más rápido.
En React, las secuencias que son asíncronas en [renderToNodeStream
()](https://reactjs.org/docs/react-dom-server.html#rendertonodestream) , en
comparación con renderToString síncrono, significa que la contrapresión se
maneja bien.

También vale la pena vigilar la rehidratación progresiva, y algo que React ha
estado [explorando](https://github.com/facebook/react/pull/14717) . Con este
enfoque, las piezas individuales de una aplicación procesada por el servidor se
"inician" con el tiempo, en lugar del enfoque común actual de inicializar toda
la aplicación a la vez. Esto puede ayudar a reducir la cantidad de JavaScript
requerido para hacer que las páginas sean interactivas, ya que la actualización
del lado del cliente de partes de baja prioridad de la página puede diferirse
para evitar el bloqueo del hilo principal. También puede ayudar a evitar uno de
los escollos de rehidratación SSR más comunes, donde un árbol DOM representado
por el servidor se destruye y luego se reconstruye de inmediato, la mayoría de
las veces porque la representación síncrona inicial del lado del cliente
requería datos que aún no estaban listos, tal vez en espera de Promise
resolución.

### Rehidratación parcial {: #partial-rehydration }

La rehidratación parcial ha resultado difícil de implementar. Este enfoque es
una extensión de la idea de la rehidratación progresiva, donde se analizan las
piezas individuales (componentes / vistas / árboles) que se rehidratan
progresivamente y se identifican aquellos con poca interactividad o sin
reactividad. Para cada una de estas partes mayormente estáticas, el código
JavaScript correspondiente se transforma en referencias inertes y funcionalidad
decorativa, reduciendo su huella del lado del cliente a casi cero. El enfoque de
hidratación parcial viene con sus propios problemas y compromisos. Presenta
algunos desafíos interesantes para el almacenamiento en caché, y la navegación
del lado del cliente significa que no podemos asumir que el HTML representado
por el servidor para las partes inertes de la aplicación estará disponible sin
una carga de página completa.

### Representación trisomórfica {: #trisomorphic }

Si [los trabajadores de
servicios](https://developers.google.com/web/fundamentals/primers/service-workers/)
son una opción para usted, la representación "trisomórfica" también puede ser de
interés. Es una técnica en la que puede usar la representación del servidor de
transmisión para navegaciones iniciales / no JS, y luego hacer que su trabajador
de servicio asuma la representación de HTML para las navegaciones después de que
se haya instalado. Esto puede mantener actualizados los componentes y las
plantillas en caché y permite las navegaciones de estilo SPA para representar
nuevas vistas en la misma sesión. Este enfoque funciona mejor cuando puede
compartir el mismo código de plantilla y enrutamiento entre el servidor, la
página del cliente y el trabajador del servicio.

<img src="../../images/2019/02/rendering-on-the-web/trisomorphic.png"
alt="Diagram of Trisomorphic rendering, showing a browser and service worker
communicating with the server">

## Consideraciones SEO {: #seo }

Los equipos a menudo tienen en cuenta el impacto del SEO al elegir una
estrategia para renderizar en la web. La representación del servidor a menudo se
elige para ofrecer una experiencia de "aspecto completo" que los rastreadores
pueden interpretar con facilidad. Los rastreadores [pueden entender
JavaScript](https://web.dev/discoverable/how-search-works) , pero a menudo hay
[limitaciones que](/search/docs/guides/rendering) vale la pena tener en cuenta
en cómo se procesan. La representación del lado del cliente puede funcionar,
pero a menudo no sin pruebas adicionales y trabajo de piernas. Más
recientemente, [el renderizado dinámico](/search/docs/guides/dynamic-rendering)
también se ha convertido en una opción que vale la pena considerar si su
arquitectura está fuertemente impulsada por JavaScript del lado del cliente.

En caso de duda, la herramienta [Mobile Friendly
Test](https://search.google.com/test/mobile-friendly) es invaluable para probar
que su enfoque elegido hace lo que espera. Muestra una vista previa visual de
cómo aparece cualquier página para el rastreador de Google, el contenido HTML
serializado encontrado (después de ejecutar JavaScript) y cualquier error
encontrado durante la representación.

<img src="../../images/2019/02/rendering-on-the-web/mobile-friendly-test.png"
alt="Screenshot of the Mobile Friendly Test UI">

## Terminando ... {: #wrapup }

Al decidir un enfoque para el renderizado, mida y comprenda cuáles son sus
cuellos de botella. Considere si la representación estática o la representación
del servidor pueden llevarlo al 90% del camino. Está perfectamente bien enviar
HTML con JS mínimo para obtener una experiencia interactiva. Aquí hay una útil
infografía que muestra el espectro servidor-cliente:

<img src="../../images/2019/02/rendering-on-the-web/infographic.png"
alt="Infographic showing the spectrum of options described in this article">

## Créditos {: #credits }

Gracias a todos por sus comentarios e inspiración:

Jeffrey Posnick, Houssein Djirdeh, Shubhie Panicker, Chris Harrelson y Sebastian
Markbåge

<div class="clearfix"></div>

{% include "web/_shared/helpful.html" %}

{% include "web/_shared/rss-widget-updates.html" %}
