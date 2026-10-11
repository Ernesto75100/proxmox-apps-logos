# Proxmox Notes

Generador de notas en Markdown con logo para los nodos, máquinas virtuales y contenedores de Proxmox VE. Gestiona una biblioteca de logos de tus servicios, rellena los datos (IP, puerto, URL, URL de administración...) y copia un bloque listo para pegar en el campo **Notas**.

**Demo:** https://ernesto75100.github.io/proxmox-apps-logos/

Es una aplicación estática (HTML, CSS y JavaScript). No necesita servidor, base de datos ni PHP.

## Características

- **Biblioteca de logos.** Sube PNG, JPG, WebP, GIF o SVG (máx. 3 MB) arrastrándolos o con un clic. Los SVG con scripts se rechazan.
- **Campos activables.** Cada campo (nombre, descripción, IP, puerto, URL, URL Admin) se incluye o no en la nota con un interruptor, y su etiqueta se puede renombrar.
- **Campos adicionales.** Añade los que necesites; si el valor empieza por `http`, se convierte en enlace.
- **URLs automáticas.** Si dejas la URL vacía se compone con protocolo, IP y puerto. En URL Admin, una ruta como `/admin` se añade a la URL base.
- **Servicios guardados.** Guarda cada servicio con su logo y sus datos para reutilizarlo.
- **Vista previa tipo Proxmox**, en tema claro u oscuro, con el contador de caracteres de la nota.
- **Tema claro y oscuro** en la propia web.

## Estilos de nota

La pestaña **Estilos** ofrece 8 estilos base, cada uno con una miniatura de tu servicio: Clásico, Técnico, Informe, Destacado, Panel, Cabecera, Minimal y Compacto.

La pestaña **Personalizar** permite ajustar cada detalle:

| Grupo | Opciones |
|:--|:--|
| Estructura | Diseño (tarjeta, horizontal, informe, cita, lista, en línea), posición del logo, alineación, descripción como fila, cita o cursiva |
| Logo | Origen (incrustado o enlace), tamaño de 24 a 128 px, formato PNG, WebP o SVG original, fondo claro u oscuro, esquinas rectas, suaves o redondas |
| Texto | Etiquetas y valores en negrita, normal, cursiva o código; texto de los enlaces (URL completa, sin protocolo o "Abrir") |
| Extras | Unir IP y puerto, fecha de actualización, línea separadora al final |

## Uso

1. Sube los logos a la biblioteca y selecciona el del servicio.
2. Rellena los datos en la pestaña **Datos** y activa solo los campos que quieras.
3. Elige un estilo en **Estilos** y retócalo en **Personalizar**.
4. Pulsa **Copiar Markdown** y pégalo en Proxmox, en el nodo, VM o contenedor, dentro de **Notas**.
5. Pulsa **Guardar servicio** para reutilizarlo después.

Ejemplo de resultado con el estilo Clásico:

```markdown
| ![Jellyfin](data:image/png;base64,...) | **Jellyfin** |
|:-:|:--|
| **Descripción** | Servidor multimedia |
| **IP** | `192.168.1.50` |
| **Puerto** | `8096` |
| **URL** | [http://192.168.1.50:8096](http://192.168.1.50:8096) |
| **URL Admin** | [http://192.168.1.50:8096/web](http://192.168.1.50:8096/web) |
```

## Logos incrustados o enlazados

- **Incrustado (recomendado).** El logo va dentro del Markdown como imagen en base64, redimensionado al tamaño elegido. No depende de ningún servidor, pero ocupa caracteres de la nota. Con 48 px suele ser pequeño; WebP pesa menos que PNG.
- **Enlace.** El Markdown apunta a la imagen publicada en tu sitio. Solo se enlazan los logos que estén en la carpeta `logos/` del repositorio; el resto se incrusta. Si Proxmox no muestra imágenes externas, usa el modo incrustado.

Proxmox suele limitar el campo de notas de las máquinas virtuales y contenedores a unos 8192 caracteres. La web muestra el contador y te avisa si lo superas.

## Dónde se guardan los datos

- Los logos subidos desde la web se guardan en IndexedDB y los servicios en localStorage, siempre en tu navegador. No se envía nada a ningún servidor.
- Los datos no se comparten entre navegadores ni equipos. Usa **Exportar copia** e **Importar copia** para moverlos o hacer una copia de seguridad (archivo JSON que incluye los logos).
- Borrar los datos del sitio desde el navegador elimina también tus logos y servicios, así que exporta una copia de vez en cuando.

## Logos en el repositorio (opcional)

Para que ciertos logos estén disponibles en cualquier navegador:

1. Copia las imágenes a la carpeta `logos/`.
2. Añade sus nombres a `logos/manifest.json`:

```json
["jellyfin.png", "pihole.svg", "proxmox.webp"]
```

Los nombres solo pueden contener letras minúsculas, números, guiones y guiones bajos, más la extensión. Estos logos no se pueden borrar desde la web: elimínalos del repositorio.

## Ejecutar en local

Abre `index.html` en el navegador o sirve la carpeta con cualquier servidor estático:

```bash
python3 -m http.server 8080
```

Después entra en `http://localhost:8080`. Conviene usar un servidor, y no abrir el archivo directamente, para que funcionen el portapapeles y la lectura de `logos/manifest.json`.

## Estructura del proyecto

```
.
├── index.html              Aplicación completa (interfaz y lógica)
├── favicon.svg             Logo y favicon de la web
├── logos/
│   └── manifest.json       Lista de logos incluidos en el repositorio
├── assets/
│   └── tailwind-built.css  Estilos compilados
├── tailwind.css            Entrada de Tailwind y componentes
└── tailwind.config.js      Configuración de Tailwind
```

## Modificar los estilos

La interfaz usa [Tailwind CSS](https://tailwindcss.com/) v3. Si cambias clases en `index.html` o en `tailwind.css`, recompila el CSS:

```bash
npx tailwindcss@3 -i tailwind.css -o assets/tailwind-built.css --minify
```

## Compatibilidad

Navegadores actuales (Chrome, Edge, Firefox y Safari recientes). Los fondos y las esquinas redondeadas del logo usan el lienzo del navegador y, en versiones antiguas sin `roundRect`, se muestran con esquinas rectas.

## Autor

Hecho por **Ernesto A. Campos Domínguez** · [github.com/ernesto75100](https://github.com/ernesto75100)
