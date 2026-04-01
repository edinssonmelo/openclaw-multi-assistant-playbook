# Publicar este playbook en su propio repositorio

Esta carpeta está pensada para **copiarse o versionarse sola** como repositorio público, sin el resto de tu infraestructura privada.

## Opción A — Subcarpeta de un monorepo (ahora)

Si el playbook vive dentro de un repo más grande, los lectores pueden clonar solo la carpeta con [git sparse-checkout](https://git-scm.com/docs/git-sparse-checkout) o copiar el directorio manualmente.

## Opción B — Repositorio nuevo (recomendado para compartir)

En tu máquina:

```bash
cd openclaw-multi-assistant-playbook
git init
git add .
git commit -m "docs: initial OpenClaw multi-assistant playbook"
```

Crea el repo vacío en GitHub/GitLab y enlázalo:

```bash
git remote add origin https://github.com/<tu-usuario>/<nombre-repo>.git
git branch -M main
git push -u origin main
```

## Antes de hacer público

- Revisa que no hayas añadido notas personales en `docs/` ni en `templates/`.
- No subas `.env`, claves, dominios reales de producción ni capturas con datos sensibles.
- Opcional: añade `CODE_OF_CONDUCT.md` y plantilla de issues según tu comunidad.
