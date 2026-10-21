# Fase 2 — Herramientas por objetivo

**Qué desbloquea:** APKs de Android con JADX, capturas de red pcap, WebAssembly, firmware y el navegador de Playwright para los comandos web de REA.
**Tiempo orientativo:** 15-30 minutos.

## Prompt para tu Muse

```text
Instala la Fase 2 de REA en tu máquina (la Fase 1 ya está hecha):
1) El JAR jadx-headless-mcp 0.7.1 desde la release oficial que indica la
   documentación de REA, verificando su SHA-256 contra el fijado por REA
   (6e5eacf500b64292bfb73c49797c1958f6ee44646e43e868039ae7feb573ff75), y la
   variable REA_JADX_MCP_JAR apuntando a él.
2) adb desde las platform-tools oficiales de Google, portables.
3) tshark, capinfos, gdb, binwalk y sigrok-cli por apt.
4) WABT 1.0.42 portable y la variable REA_WABT_BIN_DIRECTORY con su carpeta
   bin (REA no lo busca en el PATH).
5) mitmproxy en un venv propio, y pwntools + unblob en otro.
6) El Chromium de Playwright usando el playwright-core que trae rea-agents.
   Si el CDN corta la descarga, no lo des por imposible: diagnostica primero
   y, si la ruta está sana, descárgalo por rangos en paralelo desde el
   almacenamiento de Google y colócalo en ~/.cache/ms-playwright con su
   marcador INSTALLATION_COMPLETE.
Actualiza rea-env.sh con todo, déjalo en un script reproducible, y verifícame:
`rea doctor` con ghidra y jadx en verde, un APK real de código abierto
inspeccionado con un método descompilado, una captura pcap analizada y un
módulo WASM validado.
```

## Verificación (no sigas sin esto)

- `rea doctor`: proveedores **ghidra y jadx configurados y disponibles**.
- El APK de prueba (el cliente de F-Droid sirvió en el original: 10.765 clases) se inspecciona y un método vuelve como Java legible.
- `inspect-pcap-client` detecta tu tshark 4.x y un pcap de juguete se analiza con endpoints y protocolos.
- Un `.wasm` mínimo se valida con su interfaz de exportaciones.

## Notas

- Hopper, IDA y Kuna seguirán en rojo en el doctor: son comerciales y necesitan tu licencia. No es un fallo de tu instalación.
- El doctor también marcará el skill como ausente: ese chequeo busca en las rutas de otros clientes, no en `~/workspace/custom-skills/` de Muse. Comprueba con la búsqueda de skills de tu Muse que `reverse-engineer-anything` aparece.
