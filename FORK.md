# FORK - cutmaster-ai adaptado para Debian 13

## Repositorios

| Rol | URL |
|---|---|
| **Upstream** (original) | https://github.com/CelaviiHQ/cutmaster-ai |
| **Origin** (mi fork) | https://github.com/c4p0/cutmaster-ai |
| **Local** (en pluton) | ~/cutmaster-ai |

## Motivo del fork
Adaptaciones para Debian 13 y fixes de compatibilidad con Resolve Studio 21.1.

## Cambios respecto al upstream

### Fixes de compatibilidad API Resolve 21.1
- `src/cutmaster_ai/tools/color.py`: parámetro `GetNodeGraph` cambió de
  `clip_level` (bool) a `layer_index` (int) en 7 funciones.

### Fixes de IA (Gemini)
- `src/cutmaster_ai/tools/vision.py`: modelo actualizado de `gemini-2.0-flash`
  (dado de baja por Google) a `gemini-3.1-flash-lite-preview`.
- `src/cutmaster_ai/tools/timeline_critique.py`: idem.
- `src/cutmaster_ai/tools/color_assist.py`: idem.

### Dependencias
- Instalado el extra `[ai]` con `uv sync --extra ai` (google-genai).

## Como mantenerlo actualizado

```bash
git fetch upstream
git checkout main
git merge upstream/main
# Resolver conflictos en los archivos adaptados
git push origin main
```

## Notas para agentes
Este repositorio es un fork local con adaptaciones para Debian 13 y Resolve 21.1.
Antes de proponer cambios en tools o workflows, verificar si el archivo ya
tiene adaptaciones. No asumir que coincide con el upstream original.
