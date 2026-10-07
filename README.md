# 100 Ingenieros Dicen — versión para GitHub Pages

Esta versión es una página web estática: no usa Node.js ni una base de datos propia.

## Cómo funciona

- La laptop abre **Tablero → Crear partida** y genera un código de 5 caracteres.
- Cada celular abre **Participante**, selecciona uno de los cinco equipos (Equipo 1 a Equipo 5) y escribe el código.
- Se lee la pregunta **antes** de activar los pulsadores.
- El tablero pulsa **Activar pulsadores** y el primer celular que envía el toque gana el turno.
- El equipo que gana el turno responde en su celular con **4 opciones múltiples**; el juego sabe cuál es la correcta. Tiene **3 vidas** (errores).
- Al tercer error se abre el **robo** para los otros equipos.
- Robo correcto: el equipo que roba recibe el acumulado.
- Robo fallido: el equipo que tenía el turno conserva el acumulado.
- El marcador se actualiza en todos los celulares.

## Publicarlo en GitHub Pages

1. Crea un repositorio en GitHub.
2. Sube `index.html` a la rama que uses para GitHub Pages.
3. Activa **Settings → Pages** y publica desde el repositorio.
4. Abre la URL de GitHub Pages en la laptop y en los celulares.

## Importante

La comunicación en tiempo real se hace con **WebRTC mediante PeerJS**. El archivo HTML carga la librería desde UNPKG. PeerJS usa un servidor de señalización para iniciar las conexiones, después de lo cual los dispositivos intercambian datos de juego directamente.

Necesitan conexión a internet en la laptop y en los celulares. Si la red escolar bloquea conexiones WebRTC, prueben con otra red.

## Personalizar preguntas

En `index.html`, busca `const RAW = [...]` (y `const PTS = 10` para los puntos por acierto). Hay 5 rondas, una por tema del guion, con 5 preguntas cada una. Cada pregunta es `[texto, [4 opciones], índice de la correcta]` (0 = A, 1 = B, 2 = C, 3 = D).

La ronda usa hasta 4 preguntas para el equipo en turno; la 5.ª queda como pregunta de robo si llegan a 3 errores.

## Equipos y puntos

- Equipos: Equipo 1 a Equipo 5. Un equipo elegido queda deshabilitado para los demás celulares.
- Cada pregunta tiene tiempo límite (8, 12, 15 o 20 s, por defecto 12 s) con barra de progreso. Acierto = 50 pts + hasta 100 según rapidez (50 a 150). Si se acaba el tiempo cuenta como error. La última ronda vale doble.
- El creador elige 5 o 10 rondas en el lobby antes de abrir el tablero.
- Robo correcto: se lleva el acumulado. Robo fallido: el equipo original lo conserva y quien robó pierde ese mismo acumulado (se permiten puntos negativos).
- Al final: podio del top 3 y tabla general. Constantes en `index.html`: `PTS`, `BONUS`.
- Botón **Seguir** en el tablero: si nadie pulsa se salta la pregunta; si nadie roba, el equipo original conserva el acumulado; si un equipo no responde, cuenta como error.
