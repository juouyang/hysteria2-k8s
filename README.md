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
  --from-file=server/acl.txt \
  -o yaml --dry-run=client | kubectl apply -f - && kubectl rollout restart daemonset

kubectl create secret tls hysteria-tls -n hysteria \
  --cert=tls/tls.crt \
  --key=tls/tls.key \
  -o yaml --dry-run=client | kubectl apply -f -
```

Sealed secret (for GitOps)
```
kubectl create secret generic hysteria-config -n hysteria \
  --from-file=server/config.yaml \
  --from-file=server/acl.txt \
  -o yaml --dry-run=client | kubeseal -o yaml > manifests/k8s/sealed-config-secret.yaml

kubectl create secret tls hysteria-tls -n hysteria \
  --cert=tls/tls.crt \
  --key=tls/tls.key \
  -o yaml --dry-run=client | kubeseal -o yaml > manifests/k8s/sealed-tls-secret.yaml
```

---

```
kubectl get secret apps-k8s-juouyang-com-tls -n nginx-ingress -o yaml \
  | sed 's/namespace: nginx-ingress/namespace: hysteria/' \
  | kubectl apply -f -
```
