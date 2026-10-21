# Fase 3 — Puente MCP como conector propio

**Qué desbloquea:** REA deja de ser solo una CLI y pasa a ser un **conector propio de Muse** (`custom.rea`): sus 207 herramientas MCP quedan invocables de forma estructurada, incluidas las sesiones con estado (abrir un objetivo una vez y consultarlo varias veces).
**Tiempo orientativo:** 15 minutos. No descarga nada: el servidor MCP ya está dentro de rea-agents.

## La idea en un párrafo

Muse admite conectores personalizados. El puente es un cliente MCP por stdio que arranca `rea mcp` con el entorno de las fases 1 y 2, negocia el protocolo y ofrece comandos finos (listar, describir, llamar, ejecutar un plan). Se hace **genérico a propósito**: cargar los esquemas de las 207 herramientas en el contexto de Muse sería un desperdicio; el puente las descubre bajo demanda. Y ninguna herramienta se invoca sin estar en una lista permitida generada del catálogo vivo.

## Prompt para tu Muse

```text
Crea la Fase 3 de REA: un conector propio custom.rea que sirva de puente al
servidor MCP de REA (comando `rea mcp` por stdio) desde
~/workspace/custom-connectors/custom.rea/, con connector.yaml, CONNECTOR.md
siguiendo la plantilla de conectores personalizados, y un cliente en
bin/client.py (Python, solo librería estándar) que:
- arranque el servidor con el entorno de rea-env.sh (JAVA_HOME,
  GHIDRA_INSTALL_DIR, REA_JADX_MCP_JAR, REA_WABT_BIN_DIRECTORY, PATH),
- negocie el protocolo MCP y exponga: status, list-tools, describe-tool,
  call-tool (argumentos desde un archivo JSON), run-plan (varias llamadas
  dentro de UNA sesión, para flujos con estado) y list-prompts,
- rechace cualquier herramienta que no esté en bin/allowed_tools.json,
  generado desde el catálogo vivo (207 nombres en REA 6.7.0),
- deje los logs del servidor en stderr, nunca mezclados con el JSON de salida.
Añade un wrapper `rea-mcp` en ~/workspace/tools/bin y un script reproducible.
Verifícame: el handshake (servidor rea 6.7.0, 207 herramientas), una llamada
suelta inofensiva, y un plan de sesión con Ghidra (abrir el binario de prueba,
descompilar la misma función de la Fase 1 y cerrar) cuyo pseudocódigo
coincida con el de la CLI.
```

## Verificación (no la des por hecha sin esto)

- `rea-mcp status` → servidor `rea`, versión 6.7.0, **tool_count: 207**.
- `rea-mcp call-tool` sobre `inspect_pcap_client` → detecta tu tshark.
- El plan de sesión devuelve el mismo pseudocódigo que la CLI para la misma función.
- `muse.connector_search` (la búsqueda de conectores de Muse) encuentra `custom.rea`.

## Límites

- Cada invocación del cliente es una sesión: al terminar, el servidor se cierra. Los flujos con estado van dentro de un único `run-plan`.
- El puente no añade capacidad de análisis: manda lo que REA y la máquina dan (la RAM sigue siendo el techo para objetivos grandes).
