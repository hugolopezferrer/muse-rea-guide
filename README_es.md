# Instalar REA en Muse — Guía paso a paso

*Read this in English: [README.md](README.md)*

Deja [REA (Reverse Engineer Anything)](https://github.com/morluto/rea) funcionando dentro de tu cuenta de **Muse** para analizar y clonar software con evidencia — sin abrir una terminal: todo se hace con prompts a tu Muse.

![REA](https://img.shields.io/badge/REA-6.7.0-blue) ![Ghidra](https://img.shields.io/badge/Ghidra-12.1.4-green) ![JDK](https://img.shields.io/badge/JDK-21-orange) ![Licencia](https://img.shields.io/badge/licencia-MIT-lightgrey)

> ⚠️ **Muse no es Claude Code.** La lista de clientes de `rea setup` (Claude Code, Codex, Cursor, Gemini CLI, Grok Build…) no incluye a Muse, así que el instalador automático no sirve aquí. La ruta que funciona — y la de esta guía — es: **REA por CLI dentro de la máquina de tu Muse + un puente MCP propio**. Está probada de principio a fin; la bitácora completa está en el repo hermano, [hugolopezferrer/muse-rea-log](https://github.com/hugolopezferrer/muse-rea-log).

## Los dos repositorios

Esta integración se publica como una pareja:

- **[hugolopezferrer/muse-rea-guide](https://github.com/hugolopezferrer/muse-rea-guide)** — este repo: la guía de instalación paso a paso para cualquier usuario de Muse.
- **[hugolopezferrer/muse-rea-log](https://github.com/hugolopezferrer/muse-rea-log)** — la bitácora de seguridad, con cada paso cronológico, hash y verificación.

## Cómo usar esta guía

1. Copia el prompt de cada fase y pégalo en tu chat con Muse.
2. Deja que Muse instale y **verifique** — cada fase termina con una comprobación que debe salir en verde antes de seguir.
3. Si algo falla, busca el síntoma en [troubleshooting](docs/troubleshooting_es.md).

**Requisitos:** una cuenta de Muse, ~4 GB libres en su máquina (ella te lo confirmará) y paciencia: la Fase 1 descarga ~800 MB.

## Las tres fases de un vistazo

| Fase | Qué desbloquea | Verificación que la cierra |
|---|---|---|
| [1 — Motor nativo](docs/fase-1_es.md) | REA fijado y operativo + Ghidra para binarios | `rea doctor` con Ghidra en verde y una descompilación de prueba exacta |
| [2 — Por objetivo](docs/fase-2_es.md) | APKs con JADX, pcap, WASM, firmware, navegador | APK real inspeccionado y un método descompilado |
| [3 — Puente MCP](docs/fase-3_es.md) | REA como conector propio, con sesiones con estado | Handshake con 207 herramientas y un plan que coincide con la CLI |

## Prompt de arranque (opcional)

Si prefieres dar la orden completa una sola vez, pega esto a tu Muse:

```text
Quiero integrar REA (github.com/morluto/rea) en tu máquina siguiendo la guía
"Instalar REA en Muse" (tres fases: motor nativo con Ghidra, herramientas
por objetivo con JADX, y puente MCP como conector propio). Instala todo bajo
~/workspace/tools para que sobreviva al reemplazo de la máquina, verifica los
SHA-256 de cada artefacto contra su fuente oficial antes de ejecutarlo, y no
des una fase por terminada sin mostrarme su verificación. Solo analizarás o
clonarás software mío, autorizado o con licencia permisiva (MIT o similar).
```

## Reglas que la guía mantiene

- Solo software propio, autorizado o con licencia permisiva — nada de cracking, con la licencia original conservada en cada clon.
- Cada artefacto se descarga de su fuente oficial y se verifica por SHA-256.
- Nada se da por instalado sin una prueba real delante.

## Preguntas rápidas

**¿Necesito saber programar?** No. Necesitas saber pedir y leer las verificaciones que Muse te muestra.
**¿Necesito Ghidra sí o sí?** Solo para binarios nativos. JavaScript funciona desde el minuto uno sin ningún motor.
**¿Es esto Claude Code?** No. Es el Muse personal (muse.ai / la app), con su propia máquina.
**¿Y Hopper o IDA?** Son de pago y necesitarían tu licencia. Ghidra cubre ese rol gratis.

## Estructura

- `docs/` — una página por fase, con su prompt copiable, y el troubleshooting por síntoma.
- `scripts/` — los instaladores idempotentes de cada fase, por si hay que re-ejecutar o auditar.

## Licencia

MIT — ver [LICENSE](LICENSE). REA es de morluto (MIT); esta guía es una integración independiente sobre su trabajo.
