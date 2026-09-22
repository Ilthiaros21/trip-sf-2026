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

## Estado
App con estética ref1 (pastel, Familjen/Hanken, liquid glass en nav) + hotel Aloft Sunnyvale + Itinerario granular + Info práctica de 4 tabs, en producción vía GitHub Pages. Docs sincronizados.
