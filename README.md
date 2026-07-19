# virtus-shop gitops

Repo deklaratif yang dibaca ArgoCD (ApplicationSet). **Jangan edit manual saat demo** —
perubahan image tag di-commit otomatis oleh pipeline Tekton (Pipelines-as-Code).

```
argocd/            ApplicationSet (di-apply sekali, bootstrap)
apps/frontend/     podinfo — Rollout canary (Argo Rollouts + Istio)
apps/order-api/    Deployment order-api
apps/product-api/  Deployment product-api (satu-satunya yang akses MySQL)
infra/             Istio gateway, Route (pintu L4), Service DB, NetworkPolicy
```

Alur: push ke repo app → Tekton build & push image → Tekton commit tag baru ke repo ini
→ ArgoCD sync → Argo Rollouts menggeser traffic canary via VirtualService Istio.

Catatan: Secret MySQL di sini plaintext demi kesederhanaan demo.
Di production gunakan SealedSecrets / External Secrets / Vault.
# gitops
