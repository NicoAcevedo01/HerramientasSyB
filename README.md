# Portal de Herramientas — Seguridad y Bienestar

Página de inicio que unifica las dos herramientas en accesos directos, con el estado de cada una a la vista.

## Estructura

```
portal-seguridad/
├── index.html              ← el portal (abrí este)
├── observaciones/
│   └── index.html          ← Dashboard de Observaciones de Seguridad
└── capacitaciones/
    ├── index.html          ← Control de Capacitaciones Críticas
    └── data.js             ← datos de demo
```

**Mantené esta estructura de carpetas.** Los enlaces son relativos: si movés una carpeta de lugar, se rompen.

## Qué hace el portal

Además de los dos accesos directos, cada tarjeta **muestra en vivo el estado de su herramienta**: qué archivo cargaste por última vez, cuándo, y los números principales (observaciones pendientes y vencidas; capacitaciones sin programar, programadas y vigentes). Los lee de la memoria del navegador de cada app, así que se actualiza solo cada vez que cargás un Excel nuevo.

Si todavía no cargaste nada, te avisa cuál Excel falta adjuntar.

## Cambios que hice en tus archivos

**En `capacitaciones/index.html`** (dos ediciones, el resto quedó intacto):
1. Botón **← Portal** en el encabezado.
2. Función `publishPortalStats()`, que guarda un resumen liviano en `localStorage` bajo la clave `portal_stats_capacitaciones` para que el portal muestre los números sin tener que cargar todo el dataset.

**En `observaciones/index.html`**: botón **← Portal** en el encabezado y guardado del mismo tipo de resumen dentro de su caché de IndexedDB.

Verificado: las dos apps siguen funcionando igual que antes.

## Dos cosas a tener en cuenta

**1. El portal funciona offline; la app de capacitaciones no.**
El portal y el dashboard de observaciones son autocontenidos (cero dependencias externas). La app de capacitaciones carga dos recursos de internet: las tipografías de Google Fonts y la librería `xlsx` desde cdnjs. **Sin conexión, esa app no puede leer Excel** — y en muchas redes corporativas cdnjs está bloqueado. Si te pasa, avisame y te la dejo autocontenida como la de observaciones.

**2. Los indicadores en vivo necesitan servidor.**
Si abrís el portal con doble clic (`file://`), Chrome bloquea IndexedDB y la tarjeta de observaciones no va a mostrar los números (la herramienta funciona igual, solo hay que adjuntar el Excel cada vez). El portal te lo avisa con un cartel. Se resuelve publicando la carpeta o levantando un servidor local:

```bash
cd portal-seguridad
python -m http.server 8000
```
Y entrás a `http://localhost:8000`.

## Publicar en GitHub Pages

1. Creá un repo y subí **toda la carpeta** respetando la estructura.
2. *Settings → Pages* → Source: rama `main`, carpeta `/ (root)`.
3. Queda en `https://<usuario>.github.io/<repo>/`, con el portal como página de inicio.

Recordá lo que ya hablamos: con plan gratuito el sitio es **público**. No hay datos sensibles en el código (cada usuario carga su propio Excel desde su máquina), pero conviene el visto bueno de IT antes de publicar una herramienta interna. La alternativa sin exposición es dejar la carpeta en SharePoint/OneDrive del equipo.
