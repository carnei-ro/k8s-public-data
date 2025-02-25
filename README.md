# K8s Public Data

Adding new clusters:

```bash
CLUSTER_NAME="opi5-k8s"
GITHUB_USER="carnei-ro"
GITHUB_REPO="k8s-public-data"
JWKS_URI="https://${GITHUB_USER}.github.io/${GITHUB_REPO}/${CLUSTER_NAME}/openid/v1/jwks"
ISSUER="${GITHUB_USER}.github.io/${GITHUB_REPO}/${CLUSTER_NAME}"

mkdir -p "./${CLUSTER_NAME}/.well-known"
kubectl get --raw '/.well-known/openid-configuration' | jq --arg jwks_uri "${JWKS_URI}" '.jwks_uri=$jwks_uri' | jq --arg issuer "https://${ISSUER}" '.issuer=$issuer' | tee ./${CLUSTER_NAME}/.well-known/openid-configuration
mkdir -p ./${CLUSTER_NAME}/openid/v1/
kubectl get --raw '/openid/v1/jwks' | jq . | tee ./${CLUSTER_NAME}/openid/v1/jwks
```
