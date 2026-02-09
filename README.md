# Web Solutions - Orquestração de Containers com Kubernetes

## Descrição do Projeto

Prova de conceito para a Web Solutions Ltda. demonstrando a orquestração de dois servidores web (Nginx e Apache HTTPD) utilizando Docker e Kubernetes.

## Estrutura do Projeto

```
kubernetes-projeto/
├── docker/
│   ├── nginx/
│   │   ├── Dockerfile
│   │   └── index.html
│   └── apache/
│       ├── Dockerfile
│       └── index.html
├── k8s/
│   ├── nginx-deployment.yaml
│   ├── nginx-service.yaml
│   ├── apache-deployment.yaml
│   ├── apache-service.yaml
│   └── web-solutions-completo.yaml  # Arquivo único com todos os manifestos
├── TRABALHO_CONCEITUAL_YAML.md
├── ROTEIRO_VIDEO_PITCH.md
└── README.md
```

## Pré-requisitos

- Docker Desktop ou Docker Engine
- Kubernetes (Minikube, Kind, Docker Desktop K8s, ou cluster real)
- kubectl CLI

## Instalação do Minikube (se necessário)

### macOS
```bash
brew install minikube
```

### Windows
```bash
choco install minikube
```

### Linux
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

## Como Executar

### 1. Iniciar o cluster Kubernetes

```bash
# Com Minikube
minikube start

# Verificar se está funcionando
kubectl cluster-info
```

### 2. Aplicar os manifestos

```bash
# Aplicar arquivo completo (recomendado)
kubectl apply -f k8s/web-solutions-completo.yaml

# OU aplicar arquivos individuais
kubectl apply -f k8s/nginx-deployment.yaml
kubectl apply -f k8s/nginx-service.yaml
kubectl apply -f k8s/apache-deployment.yaml
kubectl apply -f k8s/apache-service.yaml
```

### 3. Verificar os recursos criados

```bash
# Ver todos os recursos
kubectl get all

# Ver deployments
kubectl get deployments

# Ver pods
kubectl get pods

# Ver services
kubectl get services
```

### 4. Acessar as aplicações

#### Opção A: Com Minikube
```bash
# Nginx (porta 8080)
minikube service nginx-service

# Apache (porta 8081)
minikube service apache-service
```

#### Opção B: Com Port-Forward
```bash
# Nginx em localhost:8080
kubectl port-forward service/nginx-service 8080:8080

# Apache em localhost:8081 (em outro terminal)
kubectl port-forward service/apache-service 8081:8081
```

### 5. Testar as aplicações

- **Nginx**: http://localhost:8080
- **Apache**: http://localhost:8081

## Comandos Úteis

```bash
# Escalar o Nginx para 5 réplicas
kubectl scale deployment nginx-deployment --replicas=5

# Ver logs do Nginx
kubectl logs -l app=nginx

# Ver logs do Apache
kubectl logs -l app=apache

# Descrever um pod específico
kubectl describe pod <nome-do-pod>

# Entrar em um pod
kubectl exec -it <nome-do-pod> -- /bin/sh

# Remover todos os recursos
kubectl delete -f k8s/web-solutions-completo.yaml
```

## Arquitetura da Solução

```
                    ┌─────────────────────────────────────────┐
                    │           KUBERNETES CLUSTER            │
                    │                                         │
   Porta 8080       │  ┌─────────────────────────────────┐   │
 ──────────────────►│  │     NGINX SERVICE (NodePort)    │   │
                    │  │         nodePort: 30080          │   │
                    │  └───────────────┬─────────────────┘   │
                    │                  │                      │
                    │          ┌───────┴───────┐             │
                    │          ▼               ▼             │
                    │    ┌──────────┐    ┌──────────┐        │
                    │    │ Nginx    │    │ Nginx    │        │
                    │    │ Pod 1    │    │ Pod 2    │        │
                    │    └──────────┘    └──────────┘        │
                    │                                         │
   Porta 8081       │  ┌─────────────────────────────────┐   │
 ──────────────────►│  │    APACHE SERVICE (NodePort)    │   │
                    │  │         nodePort: 30081          │   │
                    │  └───────────────┬─────────────────┘   │
                    │                  │                      │
                    │          ┌───────┴───────┐             │
                    │          ▼               ▼             │
                    │    ┌──────────┐    ┌──────────┐        │
                    │    │ Apache   │    │ Apache   │        │
                    │    │ Pod 1    │    │ Pod 2    │        │
                    │    └──────────┘    └──────────┘        │
                    │                                         │
                    └─────────────────────────────────────────┘
```

## Autor

Marcio Costa Barbosa
190093
UniFECAF
