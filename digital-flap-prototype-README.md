# Digital Flap — prototipo web (sin Android Studio)

Un solo archivo `index.html`, sin build step: React, Babel y OpenCV.js se
cargan por CDN al abrir la página. El warp de la carta se hace con WebGL
(homografía real, no una aproximación afín) y la detección con OpenCV.js
(mismo algoritmo — Canny + contornos — que la versión Kotlin anterior).

## Importante: necesita HTTPS (o localhost)
`getUserMedia` (acceso a cámara) **no funciona abriendo el archivo
directo por `file://`** en el navegador — es una restricción de "contexto
seguro" del propio Chrome/Android, no de este código. Opciones rápidas
para probarlo:

- **Más simple para desarrollo**: en la compu, `python3 -m http.server 8000`
  dentro de esta carpeta, y en el Android (misma red) entrar a
  `http://TU-IP-LOCAL:8000` — Chrome permite cámara en `http://` solo si
  es `localhost`, así que para probar desde el celular necesitás HTTPS
  real igual (ver siguiente punto) o usar `adb reverse` + abrir
  `localhost:8000` en el celular conectado por USB.
- **Más simple para probar ya en el celular**: subir la carpeta a
  cualquier hosting estático gratis (GitHub Pages, Netlify, Vercel,
  Cloudflare Pages) — todos dan HTTPS automático. Es el mismo tipo de
  hosting que probablemente ya usás para instalar tu PWA de
  forced-result-app.
- Una vez servido por HTTPS, "Agregar a pantalla de inicio" desde Chrome
  Android lo instala como si fuera una app.

## Uso
1. Abrí la página en el celular (por HTTPS, ver arriba) y dale permiso
   de cámara.
2. Apuntá al dorso de una Bicycle sobre fondo con contraste. El texto
   arriba a la izquierda cambia a "carta detectada" cuando la encuentra.
3. Tocá la esquina inferior derecha (zona invisible) para alternar entre
   la textura A (roja) y B (azul) sobre la carta trackeada.

## Qué es placeholder todavía
- Las texturas A/B son solo un canvas con texto — reemplazalas por arte
  real de cartas cuando el tracking esté afinado.
- El botón invisible simula el disparador del panel de mago; se puede
  conectar directo al mismo patrón de "slots configurables" y
  `BroadcastChannel` que ya usás en `forced-result-app` para controlarlo
  desde otro dispositivo.
- OpenCV.js pesa ~8MB la primera carga (se cachea después como PWA). Si
  se siente pesado en celulares viejos, el siguiente paso es cambiar la
  detección por contornos por tracking de features (ORB) sobre el propio
  patrón del dorso, que además tolera que los dedos tapen una esquina.
