# kubernetes-gateway-api-with-traefik

### gateway api install

```
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.4.0/standard-install.yaml
```

### traefik install

RBAC

```
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v3.6/docs/content/reference/dynamic-configuration/kubernetes-gateway-rbac.yml
```

helm values

```
providers:
  kubernetesGateway:
    enabled: true
```

helm install

```
helm repo add traefik https://traefik.github.io/charts
helm repo update
helm install traefik traefik/traefik -f values.yaml -n traefik --create-namespace
```


customize helm values

```
providers:
  kubernetesGateway:
    enabled: true

entryPoints:
  web:
    address: ":80"
  websecure:
    address: ":443"
    transport:
      lifeCycle:
        requestAcceptGraceTimeout: 6000s
        graceTimeOut: 6000s
        idleTimeout: 6000s
      respondingTimeouts:
        readTimeout: 6000s
        writeTimeout: 6000s
      dialTimeout: 6000s
    forwardedHeaders:
      insecure: true

tlsOptions:
  default:
    minVersion: VersionTLS11
    maxVersion: VersionTLS13
```
