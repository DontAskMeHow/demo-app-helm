# demo-app-helm

Простые Helm-чарты для k3s-лаборатории. Первый чарт — `lab-nginx`:
nginx-деплоймент с репликами, readiness/liveness-пробами, Service и
Ingress (traefik-ингрейсс k3s), всё параметризовано через `values.yaml`.

## Состав

```
charts/lab-nginx/
├── Chart.yaml          # метаданные чарта
├── values.yaml         # реплики, образ, сервис, ingress
└── templates/
    ├── deploymet.yaml  # Deployment + пробы
    ├── service.yaml    # ClusterIP-сервис
    └── ingress.yaml    # Ingress (traefik), включается флагом
```

## Использование

```bash
helm lint charts/lab-nginx
helm install lab-nginx charts/lab-nginx \
  --set replicaCount=3 \
  --set ingress.host=nginx.lab.local
helm upgrade lab-nginx charts/lab-nginx --set image.tag=1.27-alpine
helm uninstall lab-nginx
```

## Параметры (`values.yaml`)

| Параметр | По умолчанию | Описание |
| --- | --- | --- |
| `replicaCount` | `2` | количество реплик |
| `image.repository` / `image.tag` | `nginx` / `1.27-alpine` | образ |
| `service.type` / `service.port` | `ClusterIP` / `80` | сервис |
| `ingress.enabled` / `ingress.className` | `true` / `traefik` | ingress k3s |
| `ingress.host` / `ingress.path` | `lab-nginx.local` / `/` | виртуальный хост |

Хост в `ingress.host` — внутренний тестовый (`*.local`), при установке
задавайте свой.

## Лицензия

MIT — см. [LICENSE](LICENSE).
