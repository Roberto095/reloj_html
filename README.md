# Reloj vertical

Reloj de escritorio para dejar un celular Android encendido mostrando la hora.
Un solo archivo HTML, sin dependencias, sin conexión a internet.

**Demo:** https://roberto095.github.io/reloj_html/

## Qué hace

- Hora y minutos en dígitos grandes: apilados en vertical, en fila en horizontal
- Mantiene la pantalla encendida con la **Screen Wake Lock API**
- Rieles de segundos en ambos bordes laterales
- **Rutina diaria editable**: bloques con hora y texto que se repiten cada día,
  con aviso sonoro al cambiar de bloque (sonido sintetizado, sin archivos)
- **Modo día / noche** automático por horario, con ajuste manual
- Selector de color y de tipografía (fuentes del sistema, sin descargas)
- Bloqueo de pantalla: se sale manteniendo pulsado
- Atenuador propio, por debajo del brillo mínimo de Android
- Toda la configuración se guarda en el equipo
- Desplaza los dígitos unos píxeles cada minuto para no desgastar la pantalla

## Uso

Abrir la demo en el navegador del celular y usar "Añadir a la pantalla de inicio".
Instalada, abre a pantalla completa y funciona sin conexión.

> El Wake Lock y el service worker exigen HTTPS. Abriendo el archivo con
> `file://` no funcionan.

## Estructura

| Archivo | Para qué |
|---|---|
| `index.html` | Toda la app: marcado, estilos y lógica |
| `sw.js` | Service worker (caché offline) |
| `manifest.json` | Metadatos de instalación |
| `icon-192.png`, `icon-512.png` | Íconos de la app |
| `fuentes/` | DSEG7 Classic (SIL OFL 1.1) autoalojada |

## Desarrollo

No hay build. Se edita `index.html` y se recarga.

Para probar desde el celular con HTTPS válido, reenviar el puerto por USB:

```bash
python -m http.server 8000
adb reverse tcp:8000 tcp:8000
```

Y abrir `http://localhost:8000` en el navegador del celular.

### Al publicar un cambio

Subir el número de `VERSION` en `sw.js`. Si no, los equipos que ya
instalaron la app seguirán viendo la versión anterior indefinidamente.

## Personalización

- **Colores y medidas:** tokens al inicio del `<style>`, en `:root`.
  Cambiar `--ambar` basta; los tonos derivados se recalculan con `color-mix()`.
  La paleta `--ui-*` es la de los controles y no depende del acento.
- **Rutina inicial:** array `BLOQUES_INICIALES`. Solo aplica en el primer
  arranque; después manda lo que el usuario haya guardado.
- **Horario del modo noche:** constantes `HORA_NOCHE` y `HORA_DIA`.

## Pendientes

- [ ] Repintar solo cuando el valor cambió, en vez de en cada tic
- [ ] Aviso visible cuando el Wake Lock no se pudo activar
- [ ] Leer la duración del desbloqueo desde CSS en vez de duplicarla en JS

## Licencia

MIT, salvo la tipografía DSEG7 Classic de keshikan, bajo SIL Open Font
License 1.1 (ver `fuentes/DSEG-LICENSE.txt`).
