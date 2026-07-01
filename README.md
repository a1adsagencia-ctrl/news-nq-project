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
- Logo con "NEWS" dentro del recuadro (antes decía "NQ"), y el nombre "NQ PROJECT" en Helvetica. El resto del sitio usa Inter, una tipografía sobria y profesional (parecida a la que usa la interfaz de Claude), reservando la fuente monoespaciada solo para relojes, horas y datos — para que se sienta serio, no como un sitio genérico de IA.

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
- **Cambiar la frecuencia de actualización**: cambia `REFRESH_SECONDS` (está en 120s).

## Limitaciones a tener en cuenta

- **rss2json.com gratis** tiene un límite compartido de uso (~10k peticiones/día entre todos sus usuarios). Para uso personal o de un grupo pequeño funciona bien. Si quieres más estabilidad, puedes crear una cuenta gratis en rss2json.com, sacar tu propia API key, y agregarla a la URL en el código (`&api_key=TU_KEY`).
- Los RSS de algunos medios (ej. Reuters, Bloomberg, WSJ) ya no ofrecen feed público gratis — por eso no están incluidos. Si encuentras uno que sí funcione, solo agrégalo al array `FEEDS`.
- No es un feed *tick-by-tick* de noticias (eso requiere proveedores pagos como Benzinga Pro o Squawk). Esto es un agregador de titulares con polling cada 2 minutos, suficiente para no perderte un FOMC o un dato de CPI mientras operas.
- No es asesoría financiera ni reemplaza el calendario económico (ForexFactory, Investing.com Calendar) para horarios exactos de publicación de datos.

## Próximos pasos posibles

- Agregar el calendario económico (Investing.com/ForexFactory) como segunda pestaña.
- Conectar una alerta de sonido cuando aparezca una noticia de "alto impacto" mientras tienes la pestaña abierta.
- Conectar esto a tu plataforma `funded-tracker.nqproject.app` como un widget embebido.
