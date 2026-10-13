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

> **Las curvas de los instrumentos son mapas tonales relativos e ilustrativos.** No son mediciones de instrumentos aislados, curvas de respuesta, objetivos en dBFS ni recetas de ecualización. Sus lóbulos marcan zonas que suelen aportar peso, cuerpo, ataque o brillo según el instrumento. La lectura real corresponde únicamente al audio de entrada seleccionado; el programa no separa instrumentos.

## Qué significa la medición

- El medidor indica RMS del audio digital entregado por el navegador en dBFS. No mide dB SPL, volumen acústico en el cuarto, dBu de una interfaz ni nivel calibrado del micrófono. Para hacer una medición SPL real se necesitarían micrófono de medición calibrado, calibrador y una cadena de ganancia conocida.
- El espectro usa `AnalyserNode.getFloatFrequencyData()` del Web Audio API. La curva se limita visualmente a −100…−12 dBFS y se traza sobre una escala horizontal logarítmica de 20 Hz a 20 kHz. Como el API entrega magnitudes FFT tras aplicar una ventana Blackman, se añade su corrección de ganancia coherente (estimada con un seno alineado a bin) antes del suavizado; la lectura puntual del cursor usa el bin FFT sin suavizado y puede variar ligeramente con tonos entre bins, el tamaño FFT y la cadena de entrada. La curva dibujada promedia bandas para facilitar la lectura, así que no debe tratarse como valor puntual de calibración. El ancho de bin depende de la frecuencia de muestreo y tamaño FFT; la interfaz muestra esos datos al conectar.
- La vista suavizada promedia magnitudes en una ventana logarítmica ajustable. Eso facilita ver tendencias tonales, pero reduce resolución y suaviza picos estrechos. El medidor RMS se calcula desde las muestras temporales y no cambia al ajustar la suavidad ni la opacidad.
- El navegador recibe las solicitudes de desactivar control automático de ganancia, supresión de ruido y cancelación de eco como preferencias. Después se consultan los ajustes activos que informa la pista; si el navegador o el sistema mantiene algún procesamiento, se muestra una advertencia. Los controladores y la ganancia física de la interfaz siguen fuera del control de la página.
- Para evaluar un preset de guitarra, es preferible una entrada directa de interfaz o una salida de modelador/carga IR. Un micrófono frente al amplificador añade respuesta del micrófono, posición, altavoz y sala. El micrófono integrado del teléfono o computadora puede añadir procesamiento automático. En esos casos Frequency Stage sigue mostrando el audio digital recibido, pero no puede aislar ni corregir esos factores.

## Cómo se construyeron las guías

Las recomendaciones profesionales consultadas describen **zonas útiles y problemáticas**, no una única curva de magnitud por instrumento. Las guías de esta versión las resumen como lóbulos anchos de prominencia relativa:

| Instrumento | Zonas de partida representadas | Lectura práctica |
| --- | --- | --- |
| Bombo | 60–100 Hz; ataque en 2.5–5 kHz | Peso y golpe; el filtro bajo puede llegar a 30–40 Hz según el sonido. |
| Bajo | 50–100 Hz; 220–400 Hz; articulación 700 Hz–1 kHz; armónicos hasta unos kHz | La fundamental sigue la nota y la afinación; comparte graves con el bombo, así que conviene decidir jerarquía en el arreglo. |
| Guitarra rítmica | cuerpo 125–300 Hz; claridad 2–5 kHz; recorte inicial de graves cerca de 65–105 Hz | El recinto/IR/cabina puede cortar mucho los agudos; una DI saturada puede conservar más fizz alto. |
| Guitarra líder | zonas parecidas a la rítmica; énfasis visual de presencia 2–6 kHz | “Líder” y “rítmica” describen función/arreglo, no dos instrumentos con espectros inherentemente distintos. |
| Voz | cuerpo 120–250 Hz; caja 200–400 Hz; presencia 2–5 kHz; aire/sibilancia 7–12 kHz | El filtro pasa-altos depende del registro, proximidad y ruido. Vigila sibilancia al enfatizar el extremo alto. |
| Platillos | armónicos/ringing 1–6 kHz; brillo 5–12 kHz; energía baja posible 100–300 Hz | La parte baja puede ser resonancia o spill y no necesariamente aporta brillo útil. |

Los rangos se solapan a propósito: el objetivo es ver dónde pueden competir las fuentes y tomar decisiones escuchando en contexto, no asignar un “cajón” exclusivo a cada instrumento. La guía de guitarra líder resalta presencia para ilustrar prioridad de mezcla, no una diferencia física entre líder y rítmica.

Estudio y directo **no tienen curvas universales diferentes**: el instrumento no cambia sus frecuencias por pasar al escenario. El botón En vivo conserva las mismas zonas y recuerda que PA, sala, posición de micrófonos y realimentación condicionan el ajuste. En directo, la realimentación debe localizarse con el sistema y el micrófono reales; no puede predecirse a partir de una curva genérica.

Fuentes consultadas:

- [Sound On Sound — Mixing Essentials](https://www.soundonsound.com/techniques/mixing-essentials) y [What’s That Frequency?](https://www.soundonsound.com/techniques/whats-frequency): zonas tonales habituales y advertencia de que los rangos son guías.
- [Sound On Sound — Recording Electric Guitar](https://www.soundonsound.com/techniques/recording-electric-guitar): variabilidad del tono de guitarra eléctrica y áreas de cuerpo, mordida y límite típico del altavoz.
- [Sound On Sound — Mix Tips: Kick & Bass](https://www.soundonsound.com/techniques/mix-tips-kick-bass): complementariedad de bombo/bajo y carácter del bajo en medios.
- [Yamaha — Equalization for Live Sound](https://hub.yamaha.com/proaudio/livesound/eq/): ejemplos de ajustes prácticos de EQ en directo; son puntos de partida, no respuesta espectral de cada fuente.
- [Shure — How to Control Feedback in a Sound System](https://www.shure.com/en-MEA/insights/how-to-control-feedback-in-a-sound-system): realimentación como interacción del sistema, la sala y los micrófonos.
- [W3C Web Audio API](https://www.w3.org/TR/webaudio-1.0/#AnalyserNode): definición de FFT, ventana Blackman y suavizado del `AnalyserNode`.
- [MDN — `getFloatFrequencyData()`](https://developer.mozilla.org/en-US/docs/Web/API/AnalyserNode/getFloatFrequencyData) y [ajustes activos de MediaStreamTrack](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack/getSettings): significado de los datos de frecuencia y consulta de los ajustes efectivos del dispositivo.

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
