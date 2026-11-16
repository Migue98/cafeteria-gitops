# Cafetería — GitOps

Estado deseado de Kubernetes y configuración de observabilidad para la
cafetería en línea. El código fuente vive en
[`Migue98/cafeteria-app`](https://github.com/Migue98/cafeteria-app).

Argo CD observará este repositorio; ningún pipeline aplicará manifiestos
directamente al clúster.

## Estructura

- [`apps/`](apps/README.md): Applications de Argo CD.
- [`base/`](base/README.md): recursos comunes de la aplicación.
- [`overlays/`](overlays/README.md): variantes staging y production.
- [`monitoring/`](monitoring/README.md): ServiceMonitors, reglas y dashboards.
- [`argocd/`](argocd/README.md): bootstrap y patrón app-of-apps opcional.

No se versionarán Secrets reales. Los valores sensibles se crearán fuera de Git o se migrarán a Sealed Secrets después de completar el núcleo obligatorio.
