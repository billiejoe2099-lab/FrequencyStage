# Frequency Stage

Analizador visual de espectro para guitarra, hecho con HTML, CSS y JavaScript sin dependencias ni proceso de compilación. Puede publicarse directamente en GitHub Pages.

## Qué incluye

- Entrada en vivo desde una interfaz de audio, micrófono u otro dispositivo de entrada compatible con `getUserMedia`.
- Analizador FFT Web Audio con escala de frecuencia logarítmica de 20 Hz a 20 kHz.
- Curvas translúcidas de referencia para bombo, bajo, guitarra rítmica, guitarra líder, voz y platillos.
- Curvas aproximadas de referencia para los contextos Casa / Backing y En vivo; cada instrumento se puede ocultar y ajustar su opacidad.
- Suavizado temporal y suavizado de frecuencia ajustables para hacer más legible la curva de guitarra sin cambiar el audio de entrada.
- Medidor de nivel de entrada, lectura de frecuencia y zona tonal bajo el cursor, congelado de espectro y captura A/B del nivel.
- Ajustes visuales y preset local en el navegador.

> **Las curvas de los instrumentos son mapas tonales relativos e ilustrativos.** No son mediciones de instrumentos aislados, curvas de respuesta, objetivos en dBFS ni recetas de ecualización. Sus lóbulos marcan zonas que suelen aportar peso, cuerpo, ataque o brillo según el instrumento. La lectura real corresponde únicamente al audio de entrada seleccionado; el programa no separa instrumentos.

## Qué significa la medición

- El medidor indica RMS del audio digital entregado por el navegador en dBFS. No mide dB SPL, volumen acústico en el cuarto, dBu de una interfaz ni nivel calibrado del micrófono. Para hacer una medición SPL real se necesitarían micrófono de medición calibrado, calibrador y una cadena de ganancia conocida.
- El espectro usa `AnalyserNode.getFloatFrequencyData()` del Web Audio API. La curva se limita visualmente a −100…−12 dBFS y se traza sobre una escala horizontal logarítmica de 20 Hz a 20 kHz. Cada frecuencia se busca con la relación correcta entre frecuencia, Nyquist y `frequencyBinCount`; el cursor consulta su bin FFT directo. Como el API aplica una ventana Blackman, se añade una corrección de ganancia coherente estimada para un seno alineado a bin antes de graficar. La curva dibujada promedia bandas para facilitar la lectura; la lectura puntual puede variar con tonos entre bins, el tamaño FFT y la cadena de entrada, así que no es instrumento de calibración. El ancho de bin depende de la frecuencia de muestreo y tamaño FFT; la interfaz muestra esos datos al conectar.
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

La guitarra no adquiere otro espectro por pasar del cuarto al escenario, y no hay una curva estándar de preset válida para todos. Sí cambia la percepción con el nivel de escucha: a volumen doméstico moderado, los extremos suelen percibirse menos; al subir mucho el nivel, graves y agudos se perciben relativamente más y un tono exageradamente “sonriente” puede tapar información de nota en medios. Line 6 recomienda mantener presets distintos para nivel de sala y escenario y hacer el ajuste final en ensayo. Por eso Casa / Backing muestra una inclinación relativa amplia y suave hacia extremos, mientras En vivo reduce esa inclinación y resalta medios de forma moderada. Esto es una ayuda visual cualitativa, **no una compensación de sonoridad medida, una curva de ecualización aplicada ni una norma**. El modo no modela tu volumen SPL, altavoz, PA, sala, IR ni arreglo; ajusta por oído y en el sistema real. En directo, realimentación y resonancias se localizan con el sistema y micrófono reales, no con una curva genérica. Yamaha también plantea la EQ en vivo como ajuste por fuente y contexto, favoreciendo recortes moderados en zonas que compiten.

Fuentes consultadas:

- [Sound On Sound — Mixing Essentials](https://www.soundonsound.com/techniques/mixing-essentials) y [What’s That Frequency?](https://www.soundonsound.com/techniques/whats-frequency): zonas tonales habituales y advertencia de que los rangos son guías.
- [Sound On Sound — Recording Electric Guitar](https://www.soundonsound.com/techniques/recording-electric-guitar): variabilidad del tono de guitarra eléctrica y áreas de cuerpo, mordida y límite típico del altavoz.
- [Sound On Sound — Mix Tips: Kick & Bass](https://www.soundonsound.com/techniques/mix-tips-kick-bass): complementariedad de bombo/bajo y carácter del bajo en medios.
- [Yamaha — Equalization for Live Sound](https://hub.yamaha.com/proaudio/livesound/eq/): ejemplos de ajustes prácticos de EQ en directo; son puntos de partida, no respuesta espectral de cada fuente.
- [Line 6 — Studio Tone vs. Live Tone and the Fletcher-Munson Curve](https://kb.line6.com/studio-tone-vs-live-tone-and-the-fletcher-munson-curve): explica cómo el nivel de escucha cambia la percepción de graves/agudos y recomienda probar versiones de casa y escenario.
- [Yamaha — PA Beginners Guide](https://usa.yamaha.com/products/contents/proaudio/musicianspa/setting/band_pa.html): aborda el espacio de frecuencias entre fuentes y la EQ como decisiones de mezcla en contexto.
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

El navegador exige un contexto seguro: GitHub Pages sirve HTTPS; para probar localmente, usa `http://localhost` mediante un servidor estático. Abrir `index.html` directamente como `file://` normalmente no permite el acceso al micrófono. Al pulsar activar, autoriza el permiso del sitio y selecciona la entrada de la interfaz (el navegador puede ocultar nombres hasta obtener permiso). Si aparece **CONECTADO · SIN SEÑAL**, el permiso abrió el dispositivo pero no llegan muestras por esa entrada: prueba otra entrada, revisa el selector físico/ganancia de la interfaz y toca una cuerda. Si aparece error de permiso, dispositivo ocupado o no encontrado, el mensaje indica el tipo de problema. El audio se monitoriza dentro de la página y se envía a la salida con ganancia cero para que no se escuche por duplicado.

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
