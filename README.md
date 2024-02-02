server

* https://v2.hysteria.network/zh/docs/getting-started/Server/
* https://v2.hysteria.network/zh/docs/advanced/Full-Server-Config/
* https://v2.hysteria.network/zh/docs/getting-started/Installation/#docker


```
docker-compose up
```

---

```
kubectl create secret generic hysteria-config -n hysteria \
  --from-file=server/config.yaml \
  -o yaml --dry-run=client > manifests/k8s/hysteria-config-secret.yaml

kubectl create secret tls hysteria-tls -n hysteria \
  --cert=tls/tls.crt \
  --key=tls/tls.key \
  -o yaml --dry-run=client > manifests/k8s/hysteria-tls-secret.yaml
```

Sealed secret (for GitOps)
```
kubectl create secret generic hysteria-config -n hysteria \
  --from-file=server/config.yaml \
  -o yaml --dry-run=client | kubeseal -o yaml > manifests/k8s/sealed-config-secret.yaml

kubectl create secret tls hysteria-tls -n hysteria \
  --cert=tls/tls.crt \
  --key=tls/tls.key \
  -o yaml --dry-run=client | kubeseal -o yaml > manifests/k8s/sealed-tls-secret.yaml
```

