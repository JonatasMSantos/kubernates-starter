# Projeto de Estudo: Executando Kubernetes com Minikube em um Ambiente Linux (Ubuntu)

## Introdução

Este projeto de estudo tem como objetivo demonstrar como executar o Kubernetes em um ambiente Linux, especificamente no Ubuntu, utilizando o Minikube. O Minikube é uma ferramenta que permite a criação de um cluster Kubernetes em uma máquina local para fins de desenvolvimento e teste.

Neste projeto, você encontrará arquivos YAML de configuração para criar e gerenciar recursos no Kubernetes, bem como instruções passo a passo para configurar o Minikube e implantar aplicativos de exemplo no cluster.

## Pré-requisitos

Antes de começar, certifique-se de que o seguinte software está instalado em seu sistema:

- [Ubuntu](https://ubuntu.com/download) (versão recomendada: 20.04)
- [Docker](https://docs.docker.com/get-docker/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)

## Configurando o Minikube

Siga estas etapas para configurar o Minikube no seu ambiente Ubuntu:

## Download do minikube para testes

**Minikube** é um Kubernetes local, focado em facilitar o aprendizado e desenvolvimento para o Kubernetes.

Download: [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

```bash
alias kubectl="minikube kubectl --"
```

Iniciar (criar nó/cluster)

```bash
minikube start
```

Encerrar:

```yaml
minikube stop
```

Ver kubectl instance

```bash
kubectl -- get po -A
```

Ver os nós

```bash
kubectl get nodes
```

Criar um Pod via manifesto pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
      - containerPort: 80
```

Executar o pod.yaml

```bash
kubectl apply -f pod.yaml
```

Verificar os Pods em execução

```bash
kubectl get pods
```

Deletar um pod

```bash
kubectl delete pod nginx
```

## **Replicaset**

**replicaset: Gerenciar os Pods e verificar se estão no ar (Replicas identificadas pela mesma label)**

Criar um manifesto para **replicaset**

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 10
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest       
        ports:
        - containerPort: 80
```

Executar:

```bash
kubectl apply -f replicaset.yaml
```

Ver as replicas

```bash
kubectl get rs
```

Expor as portas de um pod

```bash
kubectl port-forward pod/nginx-replicaset-577sp 8080:80
```

Deletar uma replicaset

```bash
kubectl delete rs nginx-replicaset
```

## Deployment

Deployment: Responsavel por criar/recriar os pods do replicaset

Criar um manifesto: deployment.yaml

```bash
apiVersion: apps/v1
kind: **Deployment**
metadata:
  name: nginx-deployment
spec:
  replicas: 10
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest       
        ports:
        - containerPort: 80
```

Executar:

```bash
kubectl apply -f deployment.yaml
```

Listar

```bash
kubectl get deployments
```

## Service

Service: Responsável por ser acionado e apontar para o replicaset com balanceamento de carga:

manifesto:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
```

Executar

```yaml
kubectl apply -f service.yaml
```

Listar

```yaml
kubectl get services
```

Liberar uma porta externa para o serviço

```yaml
kubectl port-forward svc/nginx-service 8080:80
```

Deixar o service com LoadBalancer disponível para um EXTERNAL_IP: **Alterar o yaml e adicionar** 

```yaml
type: LoadBalancer
```

## Arquivos YAML de Exemplo

Você encontrará os arquivos YAML de exemplo neste repositório:

- `deployments.yaml`: Um arquivo de configuração para implantar um Deployment no Kubernetes.
- `services.yaml`: Um arquivo de configuração para criar um serviço que expõe o Deployment para acesso externo.
- `replicaset.yaml`: Um arquivo de configuração para criar um cluster com replicas de Pods com a tag: nginx


## Conclusão

Este projeto de estudo fornece uma introdução básica à execução do Kubernetes com o Minikube em um ambiente Linux, especificamente no Ubuntu. Você pode expandir este projeto adicionando mais recursos, explorando os recursos avançados do Kubernetes e aprimorando suas habilidades de orquestração de contêineres.

Lembre-se de que o Kubernetes é uma ferramenta poderosa e há muito a aprender. Este projeto serve como um ponto de partida e incentiva você a continuar sua jornada de aprendizado.

## Contribuições

Se você quiser contribuir para este projeto de estudo ou tiver sugestões para melhorias, sinta-se à vontade para criar uma _pull request_ ou abrir uma _issue_ neste repositório.

Aproveite sua jornada de aprendizado com Kubernetes e Minikube!
