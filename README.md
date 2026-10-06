# Dashboard CC Farmacia la Paz — GitHub Pages

Versión para GitHub Pages con lectura automática de Excel desde el mismo repositorio. Mantiene el diseño, los filtros, el modo oscuro, las dos pestañas y las tablas mensual y horaria.

## Publicar desde cero

1. Descomprime `Dashboard-FLP-GitHub.zip`.
2. En https://github.com/new crea un repositorio llamado `dashboard-cc-farmacia-la-paz`. Para GitHub Free, utiliza un repositorio público.
3. En el repositorio selecciona **Add file → Upload files**. Arrastra los archivos y las carpetas del paquete: `index.html`, `styles.css`, `app.js`, `config.js`, `vendor` y `datos`. Incluye también `.nojekyll` si tu explorador lo muestra. Confirma con **Commit changes**. No subas el ZIP ni una carpeta contenedora; `index.html` debe quedar en la raíz.
4. En **Settings → Pages → Build and deployment**, selecciona **Deploy from a branch**, rama **main**, carpeta **/(root)** y **Save**.
5. Espera a que GitHub termine de publicar. La URL aparece en Settings → Pages y tendrá la forma `https://TU-USUARIO.github.io/dashboard-cc-farmacia-la-paz/`.

La versión preparada se guarda sin publicarse en una cuenta de GitHub hasta que completes estos pasos o conectes la integración de GitHub y solicites la publicación.

Referencia oficial: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## Archivos

| Ruta | Función |
| --- | --- |
| `index.html` | Interfaz y paneles |
| `styles.css` | Estilos y comportamiento adaptable |
| `app.js` | Lectura de archivos, cálculos y gráficos |
| `config.js` | Rutas de fuentes y selección opcional de hoja |
| `datos/llamadas.xlsx` | Fuente operativa, archivo aportado renombrado |
| `datos/ventas.xlsx` | Fuente comercial, Reporte ventas(1).xlsx aportado renombrado |
| `vendor/` | Chart.js 4.4.8, SheetJS CE 0.18.5 y licencias |
| `.nojekyll` | Publicación como archivos estáticos |

Los Excel aportados se incluyen completos. No hay un `data.js` con registros incrustados ni una copia de respaldo de datos en el código.

## Actualizar diariamente

1. Abre la carpeta **datos** del repositorio.
2. Selecciona **Add file → Upload files** y reemplaza los archivos con los nombres exactos `llamadas.xlsx` y `ventas.xlsx`. Si cambias ambos, súbelos en el mismo commit.
3. Confirma con **Commit changes**. Espera a que termine la publicación de GitHub Pages.
4. Abre el dashboard o pulsa **Actualizar datos**. El botón consulta ambos Excel; no abre un selector de archivos.

Modificar un Excel en tu computadora no modifica la copia en GitHub: debes subirlo o sincronizarlo mediante Git. No se actualiza la página abierta en segundo plano; consulta los archivos al abrirse y al pulsar el botón. Las consultas evitan la caché del navegador, pero no aceleran la publicación de GitHub.

La aplicación valida ambos archivos antes de sustituir el corte mostrado. Si falla la actualización, conserva ambos conjuntos anteriores durante esa sesión y presenta el error. Si falla la carga inicial, no presenta indicadores como si fueran datos válidos. La hora de última lectura corresponde a la consulta, no a la modificación del Excel.

Si el rango de fechas cubría todo el corte, se amplía al corte nuevo. Los rangos personalizados y Mes se conservan; si ya no contienen datos verás una selección vacía. Las selecciones de agente, región o estado/servicio que desaparezcan del archivo se limpian. La preferencia del tema se guarda localmente; los registros no se guardan en localStorage.

## Columnas y hojas

**Llamadas:** Estado y Hora Inicio o Hora Fin son obligatorias. Se reconocen Agente, Duración, Tiempo Espera, Cola y Tipo. Las llamadas sin inicio pueden usar fin menos espera. Todos los indicadores operativos consideran 08:00 inclusive hasta 23:00 exclusive. La tabla horaria muestra 15 franjas, de 08:00–09:00 a 22:00–23:00.

**Ventas:** FECHA y TOTAL son obligatorias. Se reconocen EJECUTIVO, ESTADO, SUCURSAL, ORDEN / TICKET y SERVICIO. Se conserva la exclusión de duplicados exactos y los montos ambiguos se informan en Criterios y calidad.

Se detecta el encabezado entre las primeras 20 filas de cada hoja y se selecciona la hoja con más columnas reconocidas que cumpla los campos obligatorios. Para fijar una hoja específica, cambia `sheet: null` por `sheet: 'Nombre de hoja'` en `config.js`. Si renombras archivos o cambias a CSV, modifica también sus rutas en ese archivo. Máximo 20 MB y 100,000 filas por archivo/hoja.

Fechas de texto DD/MM/AAAA se interpretan con día primero. Fechas numéricas de Excel soportan las épocas 1900/1904. Duraciones numéricas se interpretan como fracción de día. Se conservan los nombres de ejecutivos sin fusionar alias.

## Datos publicados

GitHub Pages normalmente publica el sitio y estos Excel en internet; cualquier visitante puede descargar los archivos que la página consulta. Un repositorio privado no hace privado el sitio Pages por sí solo. Esta versión no agrega autenticación. Publicar un Excel completo también publica columnas que no se muestran en las gráficas. Revisa el contenido antes de subirlo si necesitas restringir los datos.

Referencia oficial: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

## Probar en tu computadora

No abras `index.html` mediante doble clic: los navegadores restringen las consultas entre archivos locales. Con Python instalado, abre una terminal en esta carpeta y ejecuta:

```bash
python -m http.server 8000
```

Visita http://localhost:8000/. Esta prueba utiliza los Excel de `datos` y no requiere CDN ni instalación de dependencias.

## Validación

Se comprobó la lectura de los dos Excel con SheetJS real y la interfaz en un DOM simulado: 5,277 llamadas, 4,774 atendidas, 503 abandonadas; 6,079 registros de ventas incluidos y $18,886,080.47 MXN. Se verificaron los filtros, las tablas, rutas bajo el nombre de repositorio, modo oscuro, actualización de archivos, errores de carga inicial y conservación de ambos cortes ante fallos. No se ejecutó una inspección visual en navegador real ni una publicación en GitHub durante la preparación del paquete.
