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

## Limitaciones conocidas de la API de Resolve

Verificado contra el stub oficial (`/opt/resolve/Developer/Scripting/DaVinciResolveScript.pyi`):

- `Graph` no tiene `AddNode` ni `SetNodeLabel`. Agregar nodos solo es posible
  desde la GUI de Resolve.
- `TimelineItem` no tiene `GetCDL` (solo `SetCDL`).
- `ColorGroup` no permite crear nodos, solo aplicar LUT a nodos existentes.

Consecuencia practica: cualquier workflow de grade via API debe asumir que los
nodos ya existen. Si hace falta un nodo nuevo (por ejemplo, un GradeMatch por
plano), el colorista debe crearlo manualmente desde la GUI antes de que el
agente actue.

## Subagentes y MCP servers

Los subagentes definidos en este plugin (`agents/*.md`) solo pueden usar las
tools de los MCP servers declarados en el `.mcp.json` de este repo. No heredan
servidores registrados en user scope (`~/.claude.json`) ni en project scope,
aunque el agente principal de esa sesion si los tenga disponibles.

Para que un subagente use `davinci-resolve` (o cualquier otro server externo),
hay que: 1) declararlo en `.mcp.json` de este repo, 2) listar sus tools en el
frontmatter `tools:` del agente correspondiente, y 3) reinstalar el plugin
(`claude plugin update cutmaster-ai@cutmaster-ai-local`) para que la cache en
`~/.claude/plugins/cache/` tome los cambios — editar directamente la cache no
sirve, se regenera en cada instalacion desde este repo.
