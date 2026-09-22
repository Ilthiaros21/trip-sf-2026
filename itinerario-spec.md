# Spec — Itinerario interactivo SF Bay Area (Raúl & Pao)

Viaje: viernes 25 sept – martes 29 sept 2026. Herramienta de referencia, no app minuto-a-minuto. Planear/consultar antes de salir + respaldo en viaje.

## Requisitos técnicos
- Single-page app, un solo `.html`, vanilla HTML/CSS/JS, sin frameworks, sin build step.
- Deps externas: solo Google Fonts vía `<link>`.
- Debe funcionar bien en **Safari iOS** y **Firefox Android** (navegadores reales de prueba, no Chrome desktop).
- `backdrop-filter` con fallback sólido de color.
- `100dvh` con fallback (barra Safari cambia tamaño).
- `viewport-fit=cover` + `env(safe-area-inset-*)` (notch/home indicator iOS).
- Sin scroll horizontal accidental.
- Área de toque mínima 44px en tabs/botones.
- Mobile-first (vertical), razonable en desktop pero no prioridad.

## Diseño
- Estética iOS: tipografía San Francisco / `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`.
- Tarjetas esquinas redondeadas grandes (16-20px), vidrio esmerilado sutil, nav tabs fija (arriba o abajo, estilo iOS).
- Paleta oscura, acentos cálidos (ref: Calendario/Wallet Apple modo oscuro).
- Reusar/expandir lenguaje visual de artefacto previo (split-flap board de vuelos) — misma paleta y tipografía. Contenido más extenso → priorizar jerarquía clara sobre efecto visual.

## Navegación
- Tabs por día: Vie 25 · Sáb 26 · Dom 27 · Lun 28 · Mar 29.
- Cada día: timeline vertical de bloques de actividad (hora, título, descripción corta, ubicación si aplica, notas).
- Tab extra "Resumen": datos fijos de todo el viaje (vuelos, hotel, boletos).

## Contenido confirmado

### Vuelos
- Ida: Aeroméxico, vie 25 sept, MEX 17:20 → SFO 21:10, directo (~4h29m).
- Regreso: Raúl, mar 29 sept, SFO 14:30 → MEX, escala LAX.
- Pao se queda hasta jueves 1 oct, vuelo por definir vía empresa.

### Hospedaje
- Base única: Mountain View, movimiento en Caltrain.
- 3 opciones sin elegir (marcar "pendiente de decisión final"):
  - Hyatt Centric Mountain View — $200-350/noche
  - Aloft Mountain View — $130-180/noche
  - Residence Inn Palo Alto/Mountain View — $130-160/noche

### Boletos partido — COMPRADOS
- 49ers vs Cardinals, dom 27 sept, 1:05pm, Levi's Stadium.
- Sección 301, Fila 1, asientos 20-21 — Ticketmaster Verified Resale, $200/boleto.
- Esquina suroeste, nivel 300, lado con sombra.

### Itinerario por día

**Viernes 25:** llegada SFO 21:10 → traslado Mountain View auto/Uber (~35-45 min) → check-in, noche de asentarse, cena ligera cerca hotel si hay hambre.

**Sábado 26:** día completo SF vía Caltrain.
- Ancla: California Academy of Sciences (bloquear 3-4h — acuario, planetario, selva tropical; reservar boleto planetario al llegar vía QR en lobby).
- Después: comida/tiendas/actividades por definir — sección editable con opciones: Ferry Building, Golden Gate/Crissy Field, Mission District, Dolores Park, Castro, Twin Peaks, Union Square.
- Comprar jersey/merch 49ers en Team Store Westfield SF (Market & Powell) este día (evitar líneas día del partido).
- Cena y regreso Mountain View.
- Nota: concierto en Shoreline Amphitheatre esta noche — posible más tráfico al volver.

**Domingo 27:** desayuno completo en Mountain View (no San José) → Caltrain a Santa Clara/Levi's Stadium → partido 1:05pm (asientos arriba) → salida estadio → comida en San José (opción Santana Row) → regreso Mountain View.

**Lunes 28:** día 100% libre (Pao tomó el día).
- Ancla: SFMOMA (10am-5pm, cerrado mar-mié).
- Espacio para 2da actividad SF no cubierta el sábado — sección editable.
- Cena noche con Vaish y esposo (compañera de trabajo de Pao) — ubicación/hora sin definir, placeholder claramente marcado.

**Martes 29:** mañana libre → traslado Mountain View → SFO (~35-45 min, salir con 2h margen antes del vuelo) → vuelo Raúl 14:30 escala LAX.

## Contenido pendiente (marcar "por confirmar", no inventar detalle)
- Hotel específico.
- Actividades extra sábado y lunes en SF.
- Restaurantes concretos (casual $15-25, mesa buena $30-50, especial $50-70/persona — aplica en algún punto, sin fijar cuál).
- Hora y lugar cena con Vaish.
- Alergia de Raúl: almendras y nueces de Castilla (pistaches sí) — nota fija visible en tab Resumen, para tener presente al elegir restaurante.

## Comportamiento esperado
- Bloques de actividad expandibles/colapsables (notas extra sin saturar vista inicial).
- Elementos "por confirmar" distinguidos visualmente de confirmados (borde punteado o etiqueta) — obvio de un vistazo qué falta cerrar.
- Sin persistencia ni edición en vivo — solo lectura, contenido se actualiza regenerando archivo.

## Decisiones — Paso 0 (docs + blindaje de datos)

- Documento narrativo creado: [viaje-resumen.md](viaje-resumen.md) (local) + copia Google Doc en Drive, carpeta "Trip SF 2026", compartida con Pao (paola.danaerz@gmail.com, rol writer).
- **Set in stone**: vuelos (ida/regreso, ambos tramos), asientos de vuelo, horario/asientos del partido, ventanas de traslado aeropuerto. En el código van en objeto JS separado del resto del itinerario, con `Object.freeze()` + comentario `// NO EDITAR — datos confirmados` — mutación accidental debe fallar/no tener efecto.
- **Tabs nav final**: Antes de viajar · Vie 25 · Sáb 26 · Dom 27 · Lun 28 · Mar 29 · Mié 30 · Jue 1 · Resumen.
- **Tab "Antes de viajar"** (nuevo): checklist de prep, NO vuelos/reservaciones que yo pueda ejecutar — solo referencia de fechas sugeridas. Solo lectura, sin checkbox interactivo (consistente con "sin persistencia ni edición en vivo"). Items:
  - Cuanto antes: cerrar reservación hotel (Aloft Mountain View).
  - Cuanto antes / cuando Pao tenga info: vuelo de regreso de Pao.
  - Cuando se pueda: confirmar tramo SFO-LAX con Delta.
  - ~22-23 sept (2-3 días antes): reservar transporte SFO→MV (Uber Reserve), cambiar ~$100-150 USD cash en banco en México, agregar Clipper card a Apple/Google Wallet.
- **Restaurantes = componente interactivo**: cada bloque de comida (Vie, Sáb, Dom, Lun) lleva sub-componente expandible con 2 opciones "walkable" + 1 "vale la pena moverse", con tag/badge visual distinguiendo cada tipo (no solo texto). Mismo patrón de expand/collapse que el resto de bloques.
- **Equipaje**: 1 carry-on + 1 documentada, compartidas entre los dos (no por persona) — dato de referencia en Resumen.
- **Presupuesto**: $25,000 USD total (incluye todo lo ya pagado). Dato de referencia en Resumen. Principio de diseño: recomendaciones de transporte en el itinerario deben default a Caltrain/caminar, Uber solo en casos puntuales ya justificados (no gastar de más solo porque el budget da margen).
- **Bag policy Levi's Stadium**: nota fija visible en el bloque del domingo (no solo en Resumen) — bolsa clara máx 12"x6"x12" o clutch no-clara máx 4.5"x6.5", nada de bolsas normales/mochilas. Pao no lleva su bolsa Coach ese día.
- **Packing list**: sección en Resumen, generada a partir del clima histórico de fin de sept (SF fresco/neblina, MV cálido/soleado) — lista fija, no editable.
- Vuelo de regreso de Pao: aún sin datos — no inventar, dejar bloque vacío marcado pendiente en esos días.

## Regla de sincronización

Cualquier cambio de contenido (info nueva, corrección, decisión de Pao, etc.) se refleja en LOS TRES lugares: `itinerario.html` (o su fuente de datos), `viaje-resumen.md` local, y el Google Doc en Drive. No dejar ninguno desactualizado.

## Decisiones — Restructura Info práctica

- Nav de dos modos: **Itinerario** (tabs de días, sin cambios) / **Info práctica** (Antes de viajar, Documentos, Ropa, Seguridad, Resumen).
- **Documentos** y **Ropa**: checklists con checkboxes reales, persistidos en `localStorage` por dispositivo (no sincronizan entre el teléfono de Raúl y el de Pao).
- **Ropa**: mismo `PACKING_ITEMS` (fuente única) duplicado en dos columnas independientes, Raúl / Pao.
- **Seguridad**: informativo, con botones tap-to-call / tap-to-map. Datos verificados vía búsqueda (no inventados):
  - 911 (emergencias EE.UU.)
  - Sutter Urgent Care, 701 East El Camino Real, Mountain View, (650) 934-7800, 8am–8pm
  - El Camino Hospital — Urgencias, 2500 Grant Rd, Mountain View, (650) 940-7055, 24h
  - Consulado de México en SF, 532 Folsom St, (415) 354-1700 — fuente oficial consulmex.sre.gob.mx. Línea de emergencia fuera de horario NO confirmada — no inventada, queda anotado así en la UI.

## Hosting

Publicado en GitHub Pages: **https://ilthiaros21.github.io/trip-sf-2026/** (repo `Ilthiaros21/trip-sf-2026`, rama `master`, root). Cada cambio se sube con `git push` y el link se actualiza solo — no depende de login de Claude ni de reenviar archivos. El Artifact (`itinerario-artifact.html`, mismo contenido con scoping `#trip-app`) se mantiene como respaldo secundario, pero GitHub Pages es la vía principal para consultar desde los teléfonos.

## Decisiones — Itinerario granular + quitar Resumen

- **Nav Info práctica ahora 4 tabs** (Resumen eliminado): Antes de viajar · Documentos · Ropa · Seguridad. Su contenido se redistribuyó:
  - Alergia → prepended en Seguridad (siempre visible, no colapsable, mismo trato que antes).
  - Hospedaje (comparación 3 opciones) + Presupuesto/dinero/transporte → movidos a Antes de viajar, debajo del checklist.
  - Equipaje (maletas + resumen de clima) → prepended en Ropa.
  - Vuelos/Boletos/Traslados (antes resumidos aparte) → ya no se duplican en una vista aparte; sus datos viven directamente en los bloques del día correspondiente (Viernes/Domingo/Martes), interpolados desde `FIXED_DATA` vía template string (no copiados a mano) para no romper la regla de una sola fuente de verdad.
- **"Antes de viajar" ahora es checklist real** (antes era lista `<ul>` de solo lectura) — mismo componente `.check-row`/localStorage que Documentos y Ropa, agrupado bajo cada `when` como sub-encabezado.
- **Itinerario mucho más granular**, con horas concretas donde hay dato real que las respalde (verificado, no inventado):
  - Viernes: agregado bloque "Salir de casa hacia AICM" (pendiente — depende de zona en CDMX, no se inventa un tiempo de traslado) con meta de llegar al aeropuerto ~2h50 antes (14:30) de un vuelo internacional, + check-in 14:30, abordaje ~16:50, despegue 17:20.
  - Sábado: despertar ~7:00am, Caltrain ~7:45am, Cal Academy 9:30am–1:30pm (hora de apertura real, verificada — [fuente](https://www.calacademy.org/hours-admission)).
  - Domingo: despertar ~8:00am, desayuno ~8:30am, Caltrain ~10:30am, "Gates abren" 11:05am (gates públicos abren 2h antes del kickoff, verificado — no la cifra de 3.5h que aplica solo a tailgate de parking).
  - Lunes: despertar ~8:00am, Caltrain ~8:45am (SFMOMA abre 10am, menos prisa que sábado).
  - Martes: despertar ~9:00am, checkout ~11:00am (marcado pendiente — hora real depende de política del hotel, aún sin reservar), traslado ~11:45am calculado desde el vuelo real (14:30 − 2h buffer − 45min traslado).
  - Miércoles/Jueves (Pao sola): sin cambios — nada planeado aún, no se inventan horas ahí.

## Decisiones — Rediseño ref1 + cambio de hotel a Sunnyvale

- **Estética**: pivote completo a la referencia visual "ref1" (screenshot de app de viajes pastel lavanda/periwinkle) — tarjetas blancas redondeadas, pills de hora coloreadas, línea de timeline con degradado conectando puntos, tipografía redondeada humanista. Reemplaza la dirección "Ethereal Glass"/agencia oscura de fases anteriores (ver `.impeccable.md`).
- **Tipografía**: Familjen Grotesk (display: título, headings, títulos de tarjeta) + Hanken Grotesk (cuerpo/UI) — reemplaza Bricolage Grotesque + JetBrains Mono. Motivo: usuario pidió algo "más estilizado", Bricolage se sentía "proyecto de niño chiquito".
- **Liquid glass**: aplicado solo en `.app-bar` (header + mode-switch + nav de tabs) vía blur+saturate en capas, highlight especular arriba, línea de refracción abajo. NO en tarjetas de contenido — glassmorphism en todo es un patrón de AI slop.
- **Bezel-shell neutralizado**: ya no es doble marco concéntrico — las tarjetas cargan su propio peso visual (sombra suave, borde de 1px), `.bezel-shell` es ahora un wrapper vacío sin estilos propios.
- **Spine con degradado**: la línea vertical del timeline usa una función JS (`spineColor`) que interpola en OKLCH azul→ámbar→rosa a lo largo de los bloques del día — puramente decorativo, nunca en los puntos de confirmado/pendiente (esos siguen siendo semánticos, no decorativos).
- **`--confirmed` oscurecido de nuevo** (`oklch(0.4 0.1 155)`) por la misma trampa de contraste en tono verde de fases anteriores — luminancia WCAG pondera fuerte el canal verde, así que valores de L moderados en verde dan contraste engañosamente alto.
- **Hotel cambia de Aloft Mountain View a Aloft Sunnyvale**: mismo precio/tipo de habitación pero $5,024 MXN (vs. Aloft MV $5,800 MXN, Hampton Inn $7,700 MXN). Motivo: a pasos de la estación de Caltrain de Sunnyvale (la de Mountain View quedaba a 40 min caminando). Efectos secundarios positivos: trayecto más corto a Santa Clara el domingo (Sunnyvale→Santa Clara ~8 min en Caltrain), evita la presión de precio del concierto de Riley Green en Shoreline Amphitheatre (Mountain View) el sábado 26. La noche lunes-a-martes comparten el cuarto de Pao (cargo de persona extra en su reservación) — ya no hotel aparte para Raúl.
- **Transporte**: Sunnyvale reemplaza a Mountain View como base de Caltrain en todo el itinerario. Viernes desde SFO: evaluando BART (SFO→Millbrae) + transbordo a Caltrain (Millbrae→Sunnyvale) vs. Uber directo — riesgo real es tiempo de migración/aduana de un vuelo internacional; recomendación propia: límite ~22:00, si no están claros de la terminal para esa hora, default a Uber directo. Pago en BART/Caltrain: tap con tarjeta contactless o Apple/Google Pay — **no hace falta Clipper card** (dato corregido, antes decía agregarla a Apple Wallet).
- **Seguridad — Urgent Care corregido**: Sutter Urgent Care reemplazado por **Instant Urgent Care — Sunnyvale**, 970 W El Camino Real Ste 8, Sunnyvale, (408) 212-7420, Lun–Vie 9am–6pm / Sáb–Dom 9am–5pm (no es 24h — para eso, El Camino Hospital sigue siendo la referencia 24h, ahora en Mountain View a distancia razonable en auto).

## Decisiones — Spine continuo por categoría + tarjetas 3D + quitar resumen de día

- **Resumen de día eliminado**: `day.note` (el párrafo de una línea arriba del timeline de cada día) se quitó por completo — el usuario no lo quería, sentía que duplicaba lo que ya está en las tarjetas. Su contenido único (no ya repetido en alguna tarjeta) se movió al campo `notes` de la tarjeta más relevante: el margen de 2h50 del viernes → tarjeta "Salir de casa"; el aviso del concierto de Riley Green → tarjeta "Cena y regreso a Sunnyvale" del sábado; "Pao pidió el día" → tarjeta "Despertar" del lunes; el acuerdo de cuarto compartido + Pao se queda hasta el jueves → tarjetas "Checkout del hotel" y "Vuelo de Raúl" del martes; "Raúl ya se fue" → la tarjeta única del miércoles. El campo `note` sigue existiendo a nivel de día para las 4 pestañas de Info práctica (Antes de viajar, Documentos, Ropa, Seguridad) — ahí es una instrucción corta, no un resumen redundante, así que se queda.
- **Tarjetas con 3D**: `.card` ahora tiene `transform: perspective(1000px) rotateX(1.5deg)` en reposo (inclinación sutil, no skeuomorfismo) que se aplana a `rotateX(0deg)` al abrir (`data-open="true"`) o al presionar (`:has(.card-head:active)`, con `@supports selector(:has(a))` como guard — degrada a solo la inclinación de reposo en navegadores sin soporte, sin romper nada). Sombra en 3 capas: bisel superior (`inset 0 1px 0` blanco, filo de luz), sombra de canto inferior, contacto+ambiente, y un glow de categoría (`--cat-glow`, ver abajo) en vez del `box-shadow` genérico de 2 capas anterior. `.summary-card` recibió un tratamiento similar (bisel + glow con `--accent`) pero sin la inclinación, para no sobrecargar tarjetas que no son "momentos" del itinerario.
- **Spine continuo por categoría** (reemplaza el spine decorativo posicional azul→ámbar→rosa de la fase anterior): una sola tira de vidrio líquido por día (`.spine`, `.spine-track` + `.spine-fill`), no un segmento corto por tarjeta. Cada tarjeta ahora tiene un campo `cat` (`flight` / `transfer` / `activity` / `food` / `event` / `rest`) y el spine se pinta con un degradado construido en JS a partir del color de cada categoría, medido contra la posición real de cada dot (`getBoundingClientRect`) — sigue el layout real, no un índice fijo. El "reveal" (de transparente al color, conforme se hace scroll) usa una custom property `--reveal` registrada con `@property` (`syntax: '<percentage>'`, transicionable) actualizada en cada frame de scroll (rAF-throttled) — un solo listener global que sigue al día activo, no uno por día. `ResizeObserver` por timeline recalcula posición/degradado cuando el día se activa (pasa de `display:none` a visible), se abre/cierra una tarjeta, o hay resize de ventana — un solo mecanismo cubre los tres casos. El color semántico confirmado/pendiente de los dots (`.block-dot`) no cambió — el spine es puramente decorativo/informativo, nunca reemplaza ese significado (principio de diseño #3).
  - **Sin librería externa**: se evaluó explícitamente si hacía falta una librería (GSAP ScrollTrigger, Framer Motion, etc.) para el scroll-linked color reveal, y la respuesta fue no — `ResizeObserver`, `getBoundingClientRect`, `requestAnimationFrame` y `@property` (custom properties tipadas y transicionables) son APIs nativas del navegador con soporte amplio en Safari iOS 16.4+ y Firefox Android modernos, exactamente los navegadores objetivo del proyecto. Añadir una librería habría roto el principio de diseño #5 (zero external JS deps, zero build step) sin ganar nada — el problema era completamente resoluble con lo que el navegador ya trae.
  - **Fix de CSS colateral**: como el spine ahora es un `<div>` hermano de las tarjetas dentro de `.timeline` (antes cada tarjeta tenía su propio segmento de línea, ahora es una sola tira externa), los selectores de stagger que usaban `:nth-child(N)` en `.timeline > *` se rompían (contaban al spine como hijo 1, corriendo el índice de todas las tarjetas). Se cambiaron a `:nth-of-type(N)` sobre `.timeline > .block` — como las tarjetas son siempre `<article>` y el spine es `<div>`, `:nth-of-type` cuenta solo entre hermanos del mismo tag, inmune a la presencia del spine.
- **Viernes — hora de salida confirmada**: "Salir de casa hacia AICM" pasa de `~14:00`/pendiente a `2:00pm` confirmado (decisión ya tomada, no estimado).

## Decisiones — Limpieza de texto tipo Wallet + fórmula de reveal corregida

- **Texto de relleno eliminado en todas las tarjetas**: frases sin dato real (ej. "Hora ya decidida", "Para llegar a Cal Academy cerca de su apertura, antes de que se llene", "No forzar nada — llegan cansados de un vuelo largo") se quitaron de `desc`/`notes` en todo `TRIP_DATA`. Regla aplicada: si el texto no aporta un dato concreto (hora, precio, dirección, política, nombre propio), se corta. Lo que queda es deliberadamente terso, estilo Apple Wallet — un pase no explica por qué abordas, solo dice la puerta y la hora. Tarjetas que se quedaron sin `notes`/`food`/`warn` (ej. "Despertar", "Segunda actividad — por definir") ya no son expandibles — sin chevron, sin body — porque no había nada real que mostrar en el tap.
- **Fórmula del reveal del spine corregida**: la versión anterior (`(vh - rect.top) / (rect.height + vh)`) dependía del alto de la ventana (`vh`), así que el mismo scroll físico llenaba distinto según el tamaño de pantalla. Nueva fórmula: `pct = clamp(-rect.top / rect.height * 100, 0, 100)` — depende solo de cuánto ha pasado el spine por encima del viewport como fracción de su propio alto total (el largo del día), sin `vh` en la ecuación. Un piso fijo de 8% asegura que arriba del todo (antes de scrollear) ya se vea un poco de color cubriendo el primer punto, en vez de arrancar en blanco.

## Decisiones — Progreso tri-estado, categoría junto a la hora, spine con línea de lectura

- **Se quita el pill "Confirmado"/"Por confirmar" de las tarjetas del timeline** — sigue existiendo (misma clase CSS) en Antes de viajar (estado del hotel) y Documentos (placeholder del vuelo de Pao), donde sí describe planeación. En el timeline lo reemplaza un botón de progreso tri-estado (`.progress-btn`): nada (aro vacío) → en curso (medio relleno, `conic-gradient` — el mismo lenguaje visual que un anillo de progreso de macOS) → hecho (círculo sólido + check). Toca para ciclar. Persiste por teléfono en `localStorage` (`trip_progress_<id>`), igual que los checklists — no sincroniza entre el de Raúl y el de Pao.
- **Cheurón sin cápsula**: el círculo de fondo de 28px se quita — ahora es un cheurón simple de 14px, tono secundario, indicador de disclosure nada más (no un botón aparte).
- **Animaciones del progreso**: pasar de "nada" a "en curso" dispara un pulso en toda la tarjeta (zoom leve + glow del color de categoría, ~550ms, un solo golpe). Marcar "hecho" dispara un burst de confetti/serpentinas (~16 piezas, generado a mano en JS, sin librería) que sale de la tarjeta hacia arriba y cae. Ambas respetan `prefers-reduced-motion` (se saltan por completo, no solo más rápido).
- **El dot de la barra ya no marca confirmado/pendiente — marca categoría y progreso**: el aro siempre es del color de categoría (vuelo/traslado/actividad/comida/evento/libre); su relleno sigue el mismo tri-estado que el botón de progreso, y es completamente independiente del scroll — bajar la página no lo llena, solo tocar el botón de progreso lo hace.
- **Etiqueta de tipo junto a la hora**: cada tarjeta muestra "Vuelo", "Transporte", "Actividad", "Comida", "Evento" o "Libre" junto al pill de hora, en el color de su categoría. Al tocar la tarjeta (el mismo tap que la expande), tanto la hora como el tipo brillan con un glow de ese color.
- **Fórmula del reveal del spine, segunda vuelta**: ya no es solo "cuánto pasó por arriba del viewport" — ahora combina dos señales con el máximo entre ambas: (1) una "línea de lectura" al 38% de la altura del viewport (no el 50% exacto — es donde de verdad se concentra la mirada al leer en móvil), y (2) la cercanía al fondo real de la página (`scrollY / maxScroll`), que garantiza llegar a 100% exactamente al tocar fondo sin importar qué tan corto sea el día (un día que cabe entero en pantalla ya no se queda trabado por debajo de 100% para siempre).
- **Bug de confiabilidad encontrado y corregido verificando esto en el navegador**: `.timeline > .block` tenía `opacity: 0` fijo junto a su animación por `animation-timeline: view()`. Si ese timeline nunca resuelve su tiempo actual (se observó directamente esta sesión: una pestaña de navegador en segundo plano no corre su compositor, así que ni `animation-timeline: view()` ni `ResizeObserver` llegan a evaluar), la animación simplemente no aplica y el elemento se queda pegado en ese `opacity: 0` — invisible para siempre, sin manera de recuperarse. Se quitó el `opacity: 0` estático; el `from{opacity:0}` del propio keyframe sigue dando el fade-in cuando el timeline sí resuelve (el caso normal), pero si no resuelve, ahora cae en su opacidad por default (1, visible) en vez de desaparecer.

## Estado
App con estética ref1 (pastel, Familjen/Hanken, liquid glass en nav) + spine continuo por categoría con reveal de scroll (línea de lectura + cercanía al fondo) + tarjetas 3D con progreso tri-estado (pulso/confetti) + tipo de tarjeta junto a la hora + hotel Aloft Sunnyvale + Itinerario granular sin resumen de día + Info práctica de 4 tabs, en producción vía GitHub Pages. Docs sincronizados.
