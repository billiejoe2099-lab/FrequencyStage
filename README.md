# Frequency Stage

Analizador visual de espectro para guitarra, hecho con HTML, CSS y JavaScript sin dependencias ni proceso de compilación. Puede publicarse directamente en GitHub Pages.

## Qué incluye

- Entrada en vivo desde una interfaz de audio, micrófono u otro dispositivo de entrada compatible con `getUserMedia`.
- Analizador FFT Web Audio con escala de frecuencia logarítmica de 20 Hz a 20 kHz.
- Curvas translúcidas de referencia para bombo, bajo, guitarra rítmica, guitarra líder, voz y platillos.
- Curvas aproximadas separadas para los perfiles Estudio y En vivo; cada instrumento se puede ocultar y ajustar su opacidad.
- Suavizado temporal y suavizado de frecuencia ajustables para hacer más legible la curva de guitarra sin cambiar el audio de entrada.
- Medidor de nivel de entrada, lectura de frecuencia y zona tonal bajo el cursor, congelado de espectro y captura A/B del nivel.
- Ajustes visuales y preset local en el navegador.

> **Las curvas de los instrumentos son referencias ilustrativas aproximadas.** No son mediciones de instrumentos aislados ni una receta de ecualización. La lectura real corresponde únicamente al audio de entrada seleccionado; el programa no separa instrumentos.

## Publicar en GitHub Pages

1. Crea un repositorio en GitHub y sube el contenido de esta carpeta a la raíz del repositorio. `index.html` debe quedar en la raíz.
2. Abre **Settings → Pages** en el repositorio.
3. En **Build and deployment**, selecciona **Deploy from a branch**, elige la rama `main` y la carpeta `/(root)`, y guarda.
4. Espera a que GitHub indique la URL publicada en la misma sección. Abre esa dirección por HTTPS.

También se puede publicar desde una carpeta `/docs` si colocas dentro de ella los archivos de esta carpeta y seleccionas `/docs` en la configuración de Pages.

## Usar la entrada de audio

1. Conecta la guitarra a una interfaz de audio. Para una señal limpia, usa la entrada directa de la interfaz; evita conectar una salida de amplificador a una entrada de micrófono sin el equipo apropiado.
2. Abre la web publicada en un navegador moderno y pulsa **Activar micrófono**.
3. Concede permiso de micrófono y selecciona la entrada correspondiente. En algunos navegadores, los nombres de los dispositivos aparecen solo después de conceder permiso.
4. Toca la guitarra y ajusta la ganancia de entrada para evitar saturación.

El navegador exige un contexto seguro: GitHub Pages sirve HTTPS; para probar localmente, usa `http://localhost` mediante un servidor estático. Abrir `index.html` directamente como `file://` normalmente no permite el acceso al micrófono.

El audio se procesa en memoria en el navegador. No se graba, transmite ni almacena. Los presets guardan solo preferencias visuales en `localStorage` de ese navegador.

## Archivos

- `index.html` — interfaz y estructura de la aplicación.
- `styles.css` — diseño adaptable, oscuro, inspirado en herramientas de audio.
- `app.js` — audio, FFT, dibujo del espectro, curvas, controles y presets.
- `.nojekyll` — evita el procesamiento Jekyll innecesario en GitHub Pages.

## Navegadores

Se recomienda una versión actual de Chrome, Edge, Firefox o Safari con Web Audio API y permiso de micrófono. La latencia, respuesta del dispositivo y frecuencia de muestreo dependen del navegador y el hardware.

## Licencia

MIT. Consulta el archivo `LICENSE`.
