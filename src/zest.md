# Zest

## kubeconfig

### Prerequisites

- Set up [atrium](atrium.md) first
- [zest-ca.pem](zest-ca.pem)

### Linux Version

```bash
# Configure the cluster
kubectl config set-cluster snucse-zest \
    --server=https://zest.snucse.org:6443 \
    --embed-certs \
    --certificate-authority=zest-ca.pem

# Configure authentication
kubectl config set-credentials bacchus-auth0 \
    --exec-api-version=client.authentication.k8s.io/v1beta1 \
    --exec-command=kubectl \
    --exec-arg=oidc-login \
    --exec-arg=get-token \
    --exec-arg=--oidc-issuer-url=https://dev-1p1l3bz6kxwkga5k.us.auth0.com/ \
    --exec-arg=--oidc-client-id=Fe1ZbPzybfgKmIGs7ShGWJ3jmQVPZdMf \
    --exec-arg=--oidc-extra-scope=email \
    --exec-arg=--oidc-extra-scope=profile

# Configure context
kubectl config set-context snucse-zest-bacchus \
    --cluster=snucse-zest \
    --user=bacchus-auth0

# Switch to the context
kubectl config use-context snucse-zest-bacchus

# Verify configuration
kubectl auth whoami
```
