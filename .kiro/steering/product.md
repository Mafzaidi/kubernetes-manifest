# Product Overview

kubernetes-manifest adalah GitOps repository yang menyimpan semua Kubernetes manifest untuk deployment aplikasi di cluster. Repository ini dikelola oleh ArgoCD dan menggunakan Helm chart sebagai templating engine.

## Fungsi Utama

- Menyimpan Helm chart untuk setiap aplikasi (saat ini: Authorizer)
- Menyediakan ArgoCD Application manifest untuk GitOps deployment
- Mengkonfigurasi ArgoCD Image Updater untuk auto-update image tag
- Menyimpan konfigurasi non-sensitif (ConfigMap) via Helm files
- Mengelola referensi ke kubernetes-secrets repo untuk secret management

## Arsitektur Deployment

- ArgoCD melakukan sync otomatis dari repository ini ke cluster
- Image Updater memantau DockerHub dan update image tag via git commit
- Secrets dikelola terpisah di repository `kubernetes-secrets` dan di-reference via ArgoCD Application terpisah
- Setiap environment (production, dev) memiliki values file sendiri

## Aplikasi yang Dikelola

- **Authorizer** — Identity provider service (port 4000, image: `mafzaidi93/authorizer`)

## Domain & Ingress

- Production domain: `zencode.localprod.me`
- Ingress class: `nginx`
