# Desinstalação limpa do OLM e Configuração de NetworkPolicy

Este guia apresenta um fluxo passo-a-passo para **remover completamente** o Operator Lifecycle Manager (OLM) do seu cluster.

---

## Comandos para remoção do OLM

```bash
# 1. Defina a versão do OLM
export OLM_VERSION=v0.32.0

# 2. Remover o OLM completamente

# 2.1. Remover o APIService do OLM
kubectl delete apiservices.apiregistration.k8s.io v1.packages.operators.coreos.com

# 2.2. Excluir todos os CRDs do OLM
kubectl delete -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/${OLM_VERSION}/crds.yaml

# 2.3. Excluir todos os manifests do OLM (deployments, RBAC, etc.)
kubectl delete -f https://github.com/operator-framework/operator-lifecycle-manager/releases/download/${OLM_VERSION}/olm.yaml

# 2.4. Remover namespaces criados pelo OLM
kubectl delete namespace olm operators

# 2.5. (Opcional) Limpar ClusterRoles e ClusterRoleBindings remanescentes
kubectl delete clusterrole system:controller:operator-lifecycle-manager aggregate-olm-edit aggregate-olm-view
kubectl delete clusterrolebinding olm-operator-binding-olm

# 2.6. Verificação final
kubectl get crd | grep operators.coreos.com
kubectl get apiservice | grep packages.operators.coreos.com
kubectl get ns | grep -E "olm|operators"
```
