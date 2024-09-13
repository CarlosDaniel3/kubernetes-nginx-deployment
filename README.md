# Kubernetes-nginx-Deployment

## Objetivo do desafio:
Criar um deployment do NGINX no Kubernetes, utilizando um ConfigMap para definir o conteúdo de uma página HTML personalizada. Além disso, expor o serviço e utilizar o port-forward para acessar e validar o conteúdo da página.

## Arquivos escritos

```yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-html-file
data:
  index.html: |
    <html>
      <head><title>My HTML page</title></head>
      <body>
        <h1>NGINX DEPLOYMENT</h1>
        <p>Welcome to my HTML page!</p>
      </body>
    </html>
```

Deployment
```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
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
        image: nginx:1.27.1-alpine
        ports:
          - containerPort: 80
        volumeMounts:
        - name: html-volume
          mountPath: /usr/share/nginx/html
      volumes:
      - name: html-volume
        configMap:
          name: my-html-file
```

ClusterIP Service
```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: my-clusterip-service
  labels:
    app: nginx
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 80
  type: ClusterIP
```
Comando para criar o cluster por meio do kind:
```
kind create cluster --config=kind-config.yaml
```

Comando para aplicar os manifestos:
```
kubectl apply -f ./manifests
```

<div style="text-align: center"><br>
    <img align="center" alt="html-screen" height="200px" width="200px" src="https://github.com/CarlosDaniel3/kubernetes-nginx-deployment/blob/main/assets/html-screen.png">
</div>