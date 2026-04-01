# Comportamiento agéntico: sandbox, Executor y modo autónomo

## Dos mundos

El LLM en OpenClaw **cree** que puede conocer el servidor; sin herramientas hacia el host, **no** puede. Documenta esto en los prompts:

| Contexto | Qué puede hacer |
|----------|------------------|
| Sandbox del contenedor OpenClaw | Leer/escribir workspace, skills, llamar MCP configurados **dentro** del sandbox |
| Host real, Docker, git del repo de infra | Solo vía **Agent Executor** (o SSH explícito aprobado por ti) |

## Patrón de llamada

```text
OpenClaw (LLM) → herramienta HTTP → Agent Executor → shell/docker en el host
```

El JSON típico incluye `instruction` y `mode` (`auto` vs interactivo). La allowlist del Executor decide qué se ejecuta sin preguntar.

## Modo autónomo (menos fricción, mismo techo de seguridad)

**Sin menú** para pasos obvios de solo lectura cuando el usuario pidió validar o diagnosticar:

- Encadenar 2–4 comandos seguros: `git status`, `docker ps`, versión de herramientas, healthchecks.
- **Sí pedir confirmación** antes de: `rm -rf`, `git reset --hard`, tocar bases de datos, secretos, DNS, túneles.

## Prioridad antes de actuar

1. ¿Aporta al objetivo del usuario?
2. ¿Hace falta dato real del sistema?
3. ¿Es ruido o “show-off” de herramientas?

## Prohibiciones útiles en prompts

- No afirmar estado del host sin ejecución real.
- No mezclar temas no relacionados en un mismo mensaje si el usuario no los mezcló.
- No informes largos sin utilidad inmediata.

## Aider (opcional)

Para cambios multi-archivo en el repo de infra, puedes enrutar al Executor con el binario Aider y `cwd` en el clon del repo. La clave del proveedor LLM suele inyectarse por variable de entorno del Compose del Executor.

## Texto listo para pegar

Usa y adapta [templates/operatorModePrompt.template.md](../templates/operatorModePrompt.template.md) y el apéndice compartido en [templates/SHARED_AGENTS_APPENDIX.template.md](../templates/SHARED_AGENTS_APPENDIX.template.md).
