# Fase 1 — Motor nativo

**Qué desbloquea:** REA instalado de forma permanente (no desde la caché de `npx`) y Ghidra como motor de análisis de binarios nativos.
**Tiempo orientativo:** 10-20 minutos, según la red.

## Prompt para tu Muse

```text
Instala la Fase 1 de REA en tu máquina, todo bajo ~/workspace/tools:
1) JDK 21 portable (Temurin LTS) en ~/workspace/tools/jdk-21.
2) Ghidra 12.1.4 de la release oficial de la NSA en GitHub, verificando el
   SHA-256 del ZIP contra el digest oficial antes de descomprimirlo, en
   ~/workspace/tools/ghidra_12.1.4_PUBLIC.
3) rea-agents 6.7.0 global con el prefijo npm en ~/workspace/tools/npm-global.
4) El skill reverse-engineer-anything que trae el paquete, copiado a
   ~/workspace/custom-skills/.
5) Un archivo ~/workspace/tools/rea-env.sh con JAVA_HOME, GHIDRA_INSTALL_DIR
   y el PATH, y un wrapper `rea` en ~/workspace/tools/bin.
Déjalo reproducible en un script y verifícame dos cosas: que
`rea doctor --provider ghidra` sale en verde, y una descompilación real de
prueba: compila un binario ELF pequeño con una regla sencilla y muéstrame el
pseudocódigo que REA+Ghidra recuperen de él.
```

## Qué hará Muse

Descargará el JDK y Ghidra (unos 800 MB entre ambos), los dejará portables en `~/workspace/tools` para que sobrevivan al reemplazo de su máquina virtual, fijará la versión de REA y preparará el entorno para que cualquier sesión futura lo encuentre listo.

## Verificación (no sigas sin esto)

- `rea doctor` muestra el proveedor **ghidra: configurado, disponible, 12.1.4**, con el chequeo «JDK 21 de 64 bits aceptado».
- El pseudocódigo del binario de prueba reproduce la regla compilada (en la integración original: multiplicar por 100, sumar 250 con racha, duplicar por nivel, y la cadena literal recuperada).

## Si algo falla

- *Ghidra no arranca* → casi siempre es el JDK: necesita **JDK 21 de 64 bits**, no un JRE ni una versión anterior.
- *El doctor no ve Ghidra* → falta `GHIDRA_INSTALL_DIR` apuntando a la carpeta descomprimida; cárgalo desde `rea-env.sh`.
