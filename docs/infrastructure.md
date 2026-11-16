# Infraestructura propuesta

## Topología recomendada en Proxmox

| VM | Recursos iniciales | Propósito |
|---|---:|---|
| `k8s-cp01` | 2–4 vCPU, 4 GiB RAM | Control plane. |
| `k8s-wk01` | 4 vCPU, 8 GiB RAM | Aplicación y base de datos. |
| `k8s-wk02` | 4 vCPU, 8 GiB RAM | CI/CD y observabilidad. |
| `nfs01` | 2 vCPU, 2–4 GiB RAM, 60 GiB | Exportación NFS para volúmenes persistentes. |

El diseño completo requiere aproximadamente 24 GiB de RAM disponible. Si el mini PC tiene 16 GiB, se reducirán réplicas y recursos durante el desarrollo y se encenderán componentes pesados por etapas.

## Componentes

- Rocky Linux 9.
- Kubernetes instalado con kubeadm y automatizado con Ansible.
- Runtime CRI por definir después de validar la guía del clúster.
- Calico para red de pods y NetworkPolicy.
- MetalLB en modo Layer 2 e ingress-nginx.
- NFS CSI para aprovisionamiento dinámico.
- Namespaces: `tienda`, `monitoring`, `tekton-pipelines` y `argocd`.
- Hosts `nip.io` para tienda, Grafana y Argo CD.

## Acceso y seguridad

- Solo HTTP/HTTPS de la aplicación se publicarán mediante NAT cuando sea necesario.
- El API de Kubernetes, SSH, Grafana y Argo CD permanecerán en LAN o VPN.
- El repositorio contendrá `inventory.example.ini`; el inventario con direcciones reales estará ignorado.
- Kubeconfigs, tokens de GitHub/Docker Hub/Telegram y llaves privadas nunca se versionarán.
- Las versiones se fijarán en el README después de comprobar compatibilidad entre Kubernetes, Tekton, Argo CD y kube-prometheus-stack.

## Información pendiente

- CPU, RAM y almacenamiento reales del mini PC.
- Direccionamiento y gateway de la red de las VMs.
- IP reservada para MetalLB.
- Ubicación definitiva del servidor NFS.
- Método administrativo: LAN, VPN o túnel SSH.
