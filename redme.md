  # 🌸 Lumi Sin Gluten — Documentación del Proyecto

Tienda web / app instalable de **Lumi Sin Gluten** (Neuquén, Argentina): catálogo de alimentos sin TACC con carrito, pedidos por WhatsApp, publicidades controladas desde Google Sheets, notificaciones push y SEO optimizado para buscadores e inteligencias artificiales.

**Sitio en producción:** https://www.lumisingluten.com.ar

---

## 1. Archivos del repositorio

| Archivo | Función |
|---|---|
| `index.html` | La app completa (diseño, catálogo, carrito, popups, push, SEO). |
| `CNAME` | Lo creó GitHub con el dominio propio. **No tocar ni borrar.** |
| `robots.txt` | Autoriza a los robots de Google, Bing y de las IA (ChatGPT, Claude, Gemini, Meta AI, Perplexity). Nombre todo en minúscula. |
| `llms.txt` | "Carta de presentación" del negocio para las inteligencias artificiales. |
| `sitemap.xml` | Mapa del sitio para Google. |
| `OneSignalSDKWorker.js` | Obligatorio para las notificaciones push. Respetar el nombre exacto. |
| `README.md` | Este documento. |

> Para actualizar cualquier archivo: entrar al archivo en GitHub → lapicito (Edit) → pegar la versión nueva → **Commit changes**. O `Add file → Upload files` para pisar varios de una. GitHub Pages publica los cambios en 1-2 minutos.

---

## 2. Conexión con Google Sheets (todo se administra desde la planilla)

La app lee en vivo la planilla publicada. Constantes en `index.html` (buscar `PUB_BASE`):

- `GID_PRODUCTOS = "957503224"` → pestaña **Productos**
- `GID_CURSOS = "BASE"` → pestaña **Cursos**
- `GID_POPUP = "1473685333"` → pestaña **Popup** (publicidades)

Si una pestaña no responde, la app usa una copia de respaldo embebida (no se rompe nunca).

### 2.1 Pestaña Productos (columnas A a R)

A: Orden en la app · B: Producto (título) · C: Etiqueta (chapita naranja) · D: Subtítulo · E: Precio · F: Formas de Cocción · G: Ingredientes · H: ¿Cómo viene? · I: Presentación · K: Imagen de portada · L-O: Img1 a Img4 (carrusel) · P: Peso por unidad · Q: Cantidad por kilo · R: Ver Video (YouTube).

Acepta links de Google Drive para las imágenes (los convierte solo). Todos los productos muestran el **logo Sin Gluten** en miniatura junto al nombre, automáticamente.

### 2.2 Pestaña Popup (publicidades de inicio)

| Columna | Contenido |
|---|---|
| A: Nombre de Texto | Título grande |
| B: Subtexto | Frase debajo del título |
| C: Imagen (URL) | Imagen de fondo (link directo o de Drive; URLs limpias, sin parámetros pegados) |
| D: Texto del Botón | Lo que dice el botón verde |
| E: URL Botón | A dónde lleva el botón |
| F: Íconos | `Descargar` / `Ir` / `Comprar` / `WhatsApp` / `Video` / `No` (sin ícono). Vacío = Descargar |
| G: Situación | `Activado` = participa / `Desactivado` = no se muestra |

**Rotación:** de todas las filas en `Activado` se muestra **una publicidad por visita**, rotando en orden. Cada cliente lleva su propio turno. Se puede tener una "biblioteca" de campañas y prender solo las vigentes.

**Diseño:** pantalla completa en celular; tarjeta vertical centrada con fondo difuminado en PC.

---

## 3. Notificaciones Push (OneSignal)

- **Proveedor:** OneSignal, plan gratuito (hasta 10.000 suscriptores web).
- **App ID:** `9c046e6f-02d8-4fc3-bba5-aed870df71bb` (configurado en `index.html`).
- **Envíos (a todos los suscriptos):** https://onesignal.com → **Messages → New Push** → título + mensaje (+ imagen y link opcionales) → Audience: *Send to subscribed users* → Send. Se puede programar fecha/hora.
- **Ver suscriptores:** Audience → Subscriptions.

### 3.1 Orden de aparición al abrir la app (encadenado y garantizado)

1. Pantalla de carga (~2,5 seg).
2. **Publicidad del Sheet** (si hay alguna en `Activado`).
3. La persona la cierra → se cuentan **2 segundos**.
4. **Popup de suscripción** ("🌸 ¡No te pierdas nada de Lumi!" con campanita animada).
   - Sin publicidades activadas, aparece directo a los 2 seg de la carga.
   - Aparece **en cada visita hasta que la persona acepte**. Los ya suscriptos no lo ven nunca más.
5. Al tocar **¡Aceptar!** → permiso del navegador → llega la **bienvenida en español** ("🌸 ¡Bienvenido/a a Lumi Sin Gluten!").

*(La segmentación por intereses/provincia se quitó a propósito: por ahora todo se envía a todos. Si se quiere reactivar en el futuro, está documentado en el historial del proyecto.)*

### 3.2 Desuscripción

- Panel **"🔔 Notificaciones"** en la pestaña **Ubicación**: muestra el estado real ("Estás suscripto 💚" / "No estás recibiendo novedades") con botón **Activar / Desactivar**. Funciona siempre.
- Campanita flotante de OneSignal (abajo a la derecha) como vía alternativa.

### 3.3 Cosas a saber

- **iPhone:** las push solo funcionan con la app **instalada en la pantalla de inicio desde Safari** (Compartir → Agregar a pantalla de inicio) y iOS 16.4+. Si el acceso directo es viejo, borrarlo y reinstalarlo desde el dominio nuevo.
- **No probar en incógnito:** los navegadores bloquean la suscripción ahí. Probar en ventana normal; para volver a ver el popup: candadito junto a la URL → Configuración del sitio → Borrar datos.
- El **"from/de + dominio"** de cada notificación lo pone el dispositivo del receptor en su idioma; no se puede modificar.

---

## 4. Dominio y DNS (cómo quedó armado)

Cadena: **NIC.ar → Cloudflare → GitHub Pages**.

1. **NIC.ar:** `lumisingluten.com.ar` delegado a los 2 nameservers de Cloudflare.
2. **Cloudflare (Free):** 5 registros DNS, todos con la nube gris ("Solo DNS"):
   - 4 registros **A** en `@` → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - 1 **CNAME** `www` → `ivanezlopezpablo.github.io`
3. **GitHub Pages:** Custom domain `www.lumisingluten.com.ar` + **Enforce HTTPS** activado.

**Pendiente opcional:** `lumisingluten.ar` → agregarlo a Cloudflare, delegarlo en NIC.ar y crear una *Redirect Rule* `lumisingluten.ar/*` → `https://www.lumisingluten.com.ar` con código **301**.

---

## 5. SEO y posicionamiento en IA

- **Palabras clave** en título, descripción, keywords, textos y FAQ: *sin gluten*, *sin TACC*, *alimento libre de gluten (ALG)* + ciudades (Neuquén, Cipolletti, Plottier, Centenario, Fernández Oro, Cinco Saltos).
- **Datos estructurados** (JSON-LD): ficha del negocio (dirección, horarios, teléfono, catálogo, dominio, redes) y preguntas frecuentes.
- **Canonical / Open Graph** → `https://www.lumisingluten.com.ar/`.
- **Redes** `@lumisingluten` (Instagram y Facebook): botones en Ubicación, links en el pie, declaradas para Google. *Recíproco:* poner el dominio en la bio de Instagram y en la página de Facebook.
- **Para las IA:** `robots.txt` + `llms.txt` + FAQ con preguntas literales ("¿Dónde comprar comida sin gluten en Neuquén o el Alto Valle?", "¿Sin gluten y sin TACC es lo mismo?").

### Aceleradores recomendados (fuera del código)

1. **Google Business Profile** (perfil gratuito en Google Maps con fotos, horarios, link al sitio) — la fuente #1 que consultan las IA.
2. **Reseñas** de clientes en Google Maps.
3. **Menciones** en grupos de celíacos, directorios, medios locales.
4. **Google Search Console** — ver pendiente abajo.

### ⏳ PENDIENTE: Google Search Console

1. Entrar a https://search.google.com/search-console con la cuenta de Google.
2. **Agregar propiedad** → tipo **"Prefijo de URL"** → `https://www.lumisingluten.com.ar`.
3. **Verificar la propiedad** con el método **Etiqueta HTML**: copiar la meta etiqueta que da Google y agregarla en el `<head>` del `index.html` (o método "Archivo HTML": subir el archivo que da Google a la raíz del repo). Volver a Search Console → Verificar.
4. Menú **Sitemaps** → escribir `sitemap.xml` → **Enviar**.
5. En unos días, revisar en Search Console: qué búsquedas traen visitas, clics, y errores de indexación.

---

## 6. Otras funciones de la app

- Catálogo con buscador, fichas de producto (carrusel, ingredientes, cocción, video YouTube con pantalla completa).
- Carrito con **descuentos progresivos por kilos** (pollo desde 3 kg, carne desde 2 kg, hasta 15 kg) y pedido por WhatsApp al +54 9 299 674-2365, con validación de días y horarios.
- Sección Cursos (Hotmart) y Ubicación con mapa.
- App instalable (PWA) con banner de instalación para Android y iPhone.
- Reseñas de clientes (widget Elfsight).

---

## 7. Checklist rápido para cambios frecuentes

| Quiero... | Dónde |
|---|---|
| Cambiar precios, fotos, textos de productos | Google Sheets → pestaña Productos |
| Prender/apagar o editar una publicidad | Google Sheets → pestaña Popup → columna Situación |
| Cambiar el ícono del botón de la publicidad | Google Sheets → pestaña Popup → columna Íconos |
| Enviar un push masivo a todos | onesignal.com → Messages → New Push |
| Ver cuántos suscriptores tengo | onesignal.com → Audience |
| Editar textos del popup de suscripción o la bienvenida | `index.html` (buscar `push-invite` / `welcomeNotification`) |
| Cambiar el número de WhatsApp | `index.html` (buscar `PHONE_DESTINO`) |
| Ver cómo me encuentra la gente en Google | Google Search Console (una vez dado de alta) |
