# Observabilidad

## Métricas y dashboards

`kube-prometheus-stack` instalará Prometheus, Alertmanager, Grafana, node-exporter y kube-state-metrics. Cada microservicio tendrá su ServiceMonitor y deberá aparecer `UP`.

Se exportarán tres dashboards JSON:

1. Infraestructura: estado de nodos, CPU, memoria, pods, reinicios y disco.
2. Aplicación: solicitudes por segundo, errores 5xx y latencia p95 por servicio.
3. Negocio: pedidos por minuto, valor de pedidos y puntos otorgados.

## Alertas obligatorias

| Alerta | Condición |
|---|---|
| Servicio caído | `up == 0`. |
| Pod inestable | Reinicios repetidos. |
| Nodo no disponible | Nodo `NotReady`. |
| Disco casi lleno | Nodo o NFS por encima de 80 %. |
| Errores elevados | Más de 5 % de respuestas 5xx durante 5 minutos. |
| Cafetería sin pedidos | Cero pedidos durante 10 minutos en horario de servicio. |

Alertmanager enviará activación y resolución a un grupo de Telegram. El mensaje incluirá nombre, severidad, namespace y servicio.

## Logs y trazas

- Grafana Alloy recolectará los logs JSON de stdout y los enviará a Loki.
- OpenTelemetry exportará trazas por OTLP hacia Tempo.
- Grafana consultará Prometheus, Loki y Tempo.
- Una traza de compra mostrará frontend → pedidos → menú/lealtad → PostgreSQL.
- Cada log de error incluirá el `trace_id` para abrir su traza correspondiente.
- Loki, Tempo, Prometheus y Grafana tendrán persistencia donde lo requiera la consigna.
