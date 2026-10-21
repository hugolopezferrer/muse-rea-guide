# Troubleshooting — por síntoma

Solo errores reales, vividos en la integración original.

## `rea setup` no conoce a Muse

Esperado: Muse no está en la lista de clientes soportados. No es tu instalación; la ruta de esta guía (CLI + puente MCP propio) es la alternativa.

## El doctor sigue marcando Hopper, IDA o Kuna

Esperado sin licencia: son comerciales o los aporta el usuario. Ghidra cubre el análisis nativo y JADX el de APK. No bloquea nada de la guía.

## El doctor marca el skill como ausente

Ese chequeo busca el skill en las rutas de los clientes oficiales de REA, no en `~/workspace/custom-skills/` de Muse. Verifica con la búsqueda de skills de tu Muse: si `reverse-engineer-anything` aparece, está instalado donde Muse lo lee.

## Ghidra no arranca o el doctor lo rechaza

- Necesita **JDK 21 de 64 bits completo** (con compilador). Un JRE o un JDK 17 no sirven para Ghidra 12.1.
- `GHIDRA_INSTALL_DIR` debe apuntar a la carpeta descomprimida `ghidra_12.1.4_PUBLIC`, y REA espera la serie **12.1.x**: otra serie puede ser rechazada.

## REA dice que falta WABT aunque está en el PATH

REA no lo busca en el PATH: exige la variable **`REA_WABT_BIN_DIRECTORY`** con la ruta absoluta a la carpeta `bin` de WABT 1.0.42.

## La descarga del Chromium de Playwright se corta

Síntoma: `server closed connection` en todos los reintentos del instalador. Antes de culpar a la máquina, diagnostica:

1. ¿Otras descargas grandes funcionan (GitHub, Google)? Si sí, la red general está bien y el corte es de esa ruta.
2. El CDN de Playwright redirige (307) al almacenamiento de Google: prueba una petición por rangos del archivo final con `curl -r 0-20971519` — si llega completa, la ruta está sana.
3. Solución probada («puente por lotes»): descargar el ZIP en **8 rangos en paralelo con reintentos**, verificar su integridad, descomprimirlo en `~/.cache/ms-playwright/chromium_headless_shell-1208/` (o `chromium-1208/`) y crear el archivo vacío `INSTALLATION_COMPLETE`. Script listo en `../scripts/install-playwright-bridge.sh`.
4. Alternativa válida mientras tanto: REA acepta el Chrome del entorno indicando su ruta como `executable_path` en los comandos de navegador.

## REA no analiza el JavaScript de una página o app

Si el código vive en un `<script>` en línea dentro del HTML, el análisis JavaScript de REA no lo ve (limitación declarada). Extrae el script a un archivo `.js` en una copia de trabajo — sin tocar el original — y analiza la copia.

## Ghidra o JADX van muy lentos o se quedan sin memoria

La máquina de Muse ronda los 8 GB de RAM: trabaja un análisis cada vez, sube el timeout de la llamada en vez de repetir la carga, y para APK grandes limita el heap de JADX con `REA_JADX_HEAP_MIB`. Objetivos enormes pueden quedar fuera de alcance; dilo con la evidencia, no lo fuerces.
