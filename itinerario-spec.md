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

## Estado
App con Itinerario + Info práctica (Documentos/Ropa/Seguridad checklists) en producción vía GitHub Pages. Docs sincronizados.
