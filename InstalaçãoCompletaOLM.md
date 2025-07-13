# Instalação do OLM (Operator Lifecycle Manager)

Este guia descreve o processo de instalação do Operator Lifecycle Manager (OLM) em um cluster Kubernetes. O OLM facilita o gerenciamento do ciclo de vida de operadores Kubernetes, permitindo instalação, atualização e remoção de operadores de forma declarativa.

## Pré-requisitos

- Cluster Kubernetes em funcionamento
- `kubectl` configurado para acessar o cluster
- Permissões de administrador no cluster

## Passos para Instalação do OLM

1. Execute o script de instalação do OLM
2. Se a instalação ficar travada, aplique uma NetworkPolicy para permitir acesso à internet para os pods do OLM

```bash
# Instale o OLM usando o script oficial
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.32.0/install.sh | bash -s v0.32.0

# Se a instalação travar, abra um novo terminal e execute:
kubectl apply -f - <<EOF
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-olm-internet-egress
  namespace: olm
spec:
  podSelector: {}
  policyTypes:
    - Egress
  egress:
    - to:
        - ipBlock:
            cidr: 0.0.0.0/0
      ports:
        - protocol: TCP
          port: 443
EOF
```

## Referências

- [Documentação oficial do OLM](https://olm.operatorframework.io/)
- [GitHub do Operator Framework](https://github.com/operator-framework/operator-lifecycle-manager)
