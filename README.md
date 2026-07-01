# NQ PROJECT — Terminal de Noticias

Sitio estático y gratuito que muestra en tiempo real (casi) los titulares más importantes de medios financieros, pensado para traders de NQ/ES. Se actualiza solo cada 2 minutos y marca con **"Alto impacto"** las noticias que mencionan temas que mueven el mercado (FED, CPI, NFP, tasas, aranceles, etc.).

## Cómo funciona

- Es **un solo archivo** (`index.html`), sin backend, sin base de datos, sin costos.
- Lee los RSS públicos de Investing.com, Yahoo Finance, CNBC, MarketWatch y la Reserva Federal.
- Usa [rss2json.com](https://rss2json.com) (gratis, sin API key) para convertir cada RSS a JSON en el navegador, porque los navegadores no pueden leer RSS directo de otros dominios (CORS).
- Filtra titulares de alto impacto buscando palabras clave (editable, ver abajo).
- Cinta superior tipo "ticker de bolsa" con las noticias de mayor impacto corriendo en loop.
- Panel lateral con estado de cada fuente (si un feed falla, lo ves ahí), conteo de titulares y de alto impacto.
- **Selector de zona horaria**: en la parte superior puedes elegir tu zona horaria (o cualquier otra del mundo). Se guarda en el navegador, así que la próxima vez que entres ya queda en la que elegiste. Cada noticia muestra su hora relativa ("hace 5m") y su hora absoluta en la zona que elegiste.
- Franja de sesiones (Asia/Londres/NY + tu hora) que se resalta sola según la sesión activa en tiempo real.
- **Modo claro / oscuro**: botón 🌙/☀️ en la esquina superior. El oscuro es negro mate real (no azulado), el claro es fondo blanco. Se guarda tu preferencia en el navegador.
- Logo con "NEWS" dentro del recuadro, y el nombre "NQ PROJECT" en Helvetica. El resto del sitio usa Inter (tipografía sobria, parecida a la de la interfaz de Claude), reservando la fuente monoespaciada solo para relojes, horas y datos.
- Ya no muestra las sesiones de Asia/Londres/NY — solo tu hora actual, grande y clara, en la zona horaria que elijas.

## Muro de noticias

- Cada noticia muestra hora relativa ("hace 5m"), hora exacta en tu zona horaria, y una etiqueta **HOY / AYER / fecha**.
- Filtro **"Solo hoy"**.
- Filtro **por moneda, de selección múltiple** (USD, EUR, GBP, JPY, AUD, CAD, CHF, CNY): puedes activar varias a la vez — por ejemplo USD + EUR — y te muestra las noticias que mencionan cualquiera de las que elegiste. Detección por palabras clave en el titular, es heurística, no una etiqueta oficial del medio.
- Filtro **de idiomas** ("Idiomas", esquina superior derecha, junto al indicador "En vivo"): Todos / Español / Inglés. Cada noticia trae su etiqueta ES/EN. Se agregó Investing.com en español como fuente en español; el resto de fuentes son en inglés.
- Todos los filtros (impacto, fecha, monedas, idioma) se combinan entre sí.

## Calendario económico (pestaña nueva)

Rediseñado en formato de tabla intuitivo, inspirado en cómo Forex Factory presenta su calendario:

- **Navegación de semana** (‹ Semana actual ›): puedes ver la semana pasada, la actual o la siguiente.
- La hora **no se repite** en cada fila si varios eventos caen a la misma hora (igual que en Forex Factory) — solo aparece una vez por bloque de horario.
- **Cuadro de color** por nivel de impacto (rojo=alto, ámbar=medio, gris=bajo) en vez de texto, más rápido de escanear de un vistazo.
- Un **punto verde** marca cuál es el próximo evento que viene, sin importar el filtro que tengas activo.
- Cada evento sigue mostrando **Previsto** y **Anterior** debajo del título (el detalle que ya teníamos).
- Estado automático:
  - **Ya pasó**: fila atenuada (gris).
  - **Ahora**: fondo rojo pulsante durante los primeros 15 minutos después de la hora programada.
  - **Próximo en 1 hora**: etiqueta ámbar tipo "En 42 min" que cuenta hacia atrás en tiempo real.
- Filtros: Todos / Ya pasaron / Próximos, más filtro por moneda **de selección múltiple** (incluye NZD) — puedes marcar varias monedas a la vez.
- Se agrupa por día (Hoy, Mañana, Ayer, o el nombre del día).

### Sobre la fuente del calendario

Usa el endpoint público `nfs.faireconomy.media/ff_calendar_thisweek.json`, el mismo que usan cientos de indicadores gratuitos de MetaTrader. Por eso el calendario se actualiza cada 5 minutos (no cada 2 como las noticias) — esa fuente limita a pocas peticiones cada 5 minutos por IP, y refrescar más seguido puede hacer que te bloqueen temporalmente. Si alguna vez ves "No se pudo cargar el calendario", espera unos minutos y dale a actualizar de nuevo.

Como es una fuente de terceros no oficial, existe el riesgo de que en el futuro cambie de formato o deje de estar disponible. Si eso pasa, avísame y te ayudo a cambiarla por otra.

## Desplegar gratis en GitHub Pages (5 minutos)

1. Crea un repo nuevo en GitHub, por ejemplo `nq-project-news`.
2. Sube el archivo `index.html` a la raíz del repo.
3. Ve a **Settings → Pages**, en "Branch" selecciona `main` y carpeta `/ (root)`. Guarda.
4. En 1-2 minutos tu sitio queda publicado en `https://tu-usuario.github.io/nq-project-news/`.

También funciona igual de bien en Netlify o Vercel (arrastra la carpeta y listo).

## Personalizar

Todo el código relevante está al inicio del `<script>` en `index.html`:

```js
const FEEDS = [
  { name: "Investing.com", url: "https://www.investing.com/rss/news.rss" },
  ...
];
```

- **Agregar una fuente**: agrega un objeto `{ name, url }` con cualquier RSS público.
- **Quitar una fuente**: borra esa línea.
- **Cambiar las palabras de "alto impacto"**: edita el array `IMPACT_KEYWORDS`.
- **Cambiar la frecuencia de actualización**: cambia `REFRESH_SECONDS` (noticias) o `CAL_REFRESH_SECONDS` (calendario, no bajarlo de 300 para evitar bloqueos).
- **Agregar o ajustar palabras clave de moneda**: edita el objeto `CURRENCY_KEYWORDS`.

## Limitaciones a tener en cuenta

- **rss2json.com gratis** tiene un límite compartido de uso (~10k peticiones/día entre todos sus usuarios). Para uso personal o de un grupo pequeño funciona bien. Si quieres más estabilidad, puedes crear una cuenta gratis en rss2json.com, sacar tu propia API key, y agregarla a la URL en el código (`&api_key=TU_KEY`).
- Los RSS de algunos medios (ej. Reuters, Bloomberg, WSJ) ya no ofrecen feed público gratis — por eso no están incluidos. Si encuentras uno que sí funcione, solo agrégalo al array `FEEDS`.
- No es un feed *tick-by-tick* de noticias (eso requiere proveedores pagos como Benzinga Pro o Squawk). Esto es un agregador de titulares con polling cada 2 minutos, suficiente para no perderte un FOMC o un dato de CPI mientras operas.
- No es asesoría financiera ni reemplaza el calendario económico (ForexFactory, Investing.com Calendar) para horarios exactos de publicación de datos.

## Próximos pasos posibles

- Agregar el calendario económico (Investing.com/ForexFactory) como segunda pestaña.
- Conectar una alerta de sonido cuando aparezca una noticia de "alto impacto" mientras tienes la pestaña abierta.
- Conectar esto a tu plataforma `funded-tracker.nqproject.app` como un widget embebido.
