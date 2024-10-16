Microsserviços com Docker e Kubernetes
Este projeto implementa dois microsserviços: Auth Service e Catalog Service, utilizando Docker para containerização e Kubernetes para orquestração. O objetivo é criar serviços escaláveis e independentes que possam ser implantados em um ambiente local ou na nuvem.

Índice
Pré-requisitos
Build e Execução com Docker
Auth Service
Catalog Service
Orquestração com Kubernetes
Configuração do Auth Service
Configuração do Catalog Service
Aplicar a Configuração no Kubernetes
Verificar o Deploy
Conclusão
Pré-requisitos
Docker instalado
Kubernetes instalado (local com Minikube ou gerido na nuvem, como AWS EKS)
Kubectl configurado para gerenciar o cluster Kubernetes
Build e Execução com Docker
Auth Service
Navegue até o diretório do Auth Service:

bash
Copiar código
cd auth-service
Construa a imagem Docker:

bash
Copiar código
docker build -t auth-service:1.0 .
Execute o container:

bash
Copiar código
docker run -d -p 4000:3000 auth-service:1.0
Isso exporá o serviço de autenticação na porta 4000.

Catalog Service
Navegue até o diretório do Catalog Service:

bash
Copiar código
cd catalog-service
Construa a imagem Docker:

bash
Copiar código
docker build -t catalog-service:1.0 .
Execute o container:

bash
Copiar código
docker run -d -p 5000:3000 catalog-service:1.0
Isso exporá o serviço de catálogo na porta 5000.

Orquestração com Kubernetes
Configuração do Auth Service
Crie um arquivo auth-service-deployment.yaml com o seguinte conteúdo:

yaml
Copiar código
apiVersion: apps/v1
kind: Deployment
metadata:
  name: auth-service
spec:
  replicas: 2
  selector:
    matchLabels:
      app: auth
  template:
    metadata:
      labels:
        app: auth
    spec:
      containers:
      - name: auth
        image: auth-service:1.0
        ports:
        - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: auth-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 3000
  selector:
    app: auth
Configuração do Catalog Service
Crie um arquivo catalog-service-deployment.yaml com o seguinte conteúdo:

yaml
Copiar código
apiVersion: apps/v1
kind: Deployment
metadata:
  name: catalog-service
spec:
  replicas: 2
  selector:
    matchLabels:
      app: catalog
  template:
    metadata:
      labels:
        app: catalog
    spec:
      containers:
      - name: catalog
        image: catalog-service:1.0
        ports:
        - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: catalog-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 3000
  selector:
    app: catalog
Aplicar a Configuração no Kubernetes
Aplique a configuração do Auth Service:

bash
Copiar código
kubectl apply -f auth-service-deployment.yaml
Aplique a configuração do Catalog Service:

bash
Copiar código
kubectl apply -f catalog-service-deployment.yaml
Verificar o Deploy
Para verificar se os serviços estão funcionando corretamente, execute:

bash
Copiar código
kubectl get pods
kubectl get services
Você verá os pods rodando e os serviços expostos. O Kubernetes atribuirá um IP ou LoadBalancer, dependendo da configuração do cluster.

Conclusão
Neste projeto, foram implementados dois microsserviços utilizando Docker para a criação de containers e Kubernetes para a orquestração. Ambos os serviços são escaláveis e podem ser facilmente implantados em qualquer ambiente, seja local ou em nuvem. Além disso, o Kubernetes facilita o gerenciamento e a escalabilidade, garantindo alta disponibilidade dos serviços.
