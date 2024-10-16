# Docker_na_Pratica

<h3>1.Build e Execução com Docker
Agora, no terminal, navega até o diretório de cada microsserviço e constrói as imagens Docker.</h3>

#Auth Service
bash
Copiar código
cd auth-service
docker build -t auth-service:1.0 .
Catalog Service
bash
Copiar código
cd catalog-service
docker build -t catalog-service:1.0 .
Agora podes executar os containers localmente:

Para o Auth Service:
bash
Copiar código
docker run -d -p 4000:3000 auth-service:1.0
Para o Catalog Service:
bash
Copiar código
docker run -d -p 5000:3000 catalog-service:1.0
Isso vai expor os serviços nas portas 4000 e 5000, respectivamente.

4. Orquestração com Kubernetes
Agora vamos configurar os Deployment e Services no Kubernetes.

4.1 Auth Service - Kubernetes Config
Cria um ficheiro auth-service-deployment.yaml com a seguinte configuração:
---
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
4.2 Catalog Service - Kubernetes Config
Cria um ficheiro catalog-service-deployment.yaml com a seguinte configuração:

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
4.3 Aplicar a Configuração no Kubernetes
Assumindo que já tens o Kubernetes instalado e configurado (pode ser um cluster local com minikube ou um cluster gerido na AWS com EKS), podes aplicar as configurações YAML da seguinte forma:

Auth Service:
bash
Copiar código
kubectl apply -f auth-service-deployment.yaml
Catalog Service:
bash
Copiar código
kubectl apply -f catalog-service-deployment.yaml
4.4 Verificar o Deploy
Para verificares se os serviços estão ativos, podes executar:

bash
Copiar código
kubectl get pods
kubectl get services
Os serviços devem estar expostos e acessíveis. O Kubernetes atribuirá um LoadBalancer (no caso da AWS, será um ELB) ou um IP para acessar os microsserviços.

5. Conclusão
Neste exemplo, implementámos dois microsserviços com Docker e fizemos o deploy deles usando Kubernetes. A configuração segue as melhores práticas, permitindo que os serviços sejam escaláveis e independentes. Além disso, podes integrar este setup com serviços de nuvem como AWS (usando EKS para Kubernetes e outros recursos como bases de dados RDS, S3, etc.).
