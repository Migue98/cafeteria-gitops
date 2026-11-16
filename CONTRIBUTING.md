# Guía de contribución GitOps

## Principios

- Git es la fuente de verdad del estado del clúster.
- Argo CD despliega; los pipelines únicamente construyen imágenes y actualizan referencias en Git.
- Las imágenes se fijan por SHA y no se usa `latest`.
- Los cambios deben poder revisarse, reproducirse y revertirse desde Git.

## Ramas y commits

Parte de `main` actualizado y usa nombres como:

```text
ops/add-menu-base
ops/promote-orders-image
fix/postgres-readiness-probe
docs/describe-staging-promotion
```

Se recomiendan mensajes Conventional Commits:

```text
ops(menu): add Kubernetes base
ops(prod): promote menu image
fix(postgres): increase readiness timeout
docs(runbook): document Argo CD recovery
```

## Validación previa al Pull Request

Cuando existan manifiestos, ejecuta al menos:

```bash
kubectl kustomize overlays/staging
kubectl kustomize overlays/production
```

Además, valida esquemas con la herramienta adoptada por el equipo y revisa que
no se rendericen secretos en texto claro.

## Pull Requests

- Enlaza el Issue correspondiente.
- Indica qué recursos o ambientes cambian.
- Incluye los comandos de validación y su resultado.
- Explica el plan de reversión para cambios de producción.
- Solicita al menos una revisión de otro integrante.
- No mezcles una promoción de imagen con refactors no relacionados.

## Secretos y datos privados

Nunca subas Secrets reales, `.env`, kubeconfigs, tokens, llaves privadas,
credenciales de registry, inventarios internos ni IP administrativas. Si un
ejemplo necesita un valor sensible, usa un marcador inequívoco como
`REPLACE_ME`.

## Cambios provenientes de la aplicación

El pipeline de
[`cafeteria-app`](https://github.com/Migue98/cafeteria-app) actualizará solo la
referencia de imagen correspondiente. Los cambios de código, pruebas y
Containerfiles pertenecen al repositorio de aplicación.
