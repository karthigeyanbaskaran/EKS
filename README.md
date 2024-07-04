# EKS
```
eksctl create cluster --name=karthik-cluster \
                      --region=ap-south-1 \
                      --zones=ap-south-1a,ap-south-1b \
                      --without-nodegroup
```

```
eksctl utils associate-iam-oidc-provider \
    --region ap-south-1 \
    --cluster karthik-cluster \
    --approve
```

```
eksctl create nodegroup --cluster=karthik-cluster \
                       --region=ap-south-1 \
                       --name=karthik-nodegroup \
                       --node-type=t2.small \
                       --nodes=2 \
                       --nodes-min=2 \
                       --nodes-max=3 \
                       --node-volume-size=10 \
                       --ssh-access \
                       --ssh-public-key=bash \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access
```

Private:
```
eksctl create nodegroup --cluster=karthik-cluster \
                       --region=ap-south-1 \
                       --name=karthik-nodegroup \
                       --node-type=t2.small \
                       --nodes=2 \
                       --nodes-min=2 \
                       --nodes-max=3 \
                       --node-volume-size=10 \
                       --ssh-access \
                       --ssh-public-key=bash \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access \
                       --node-private-networking
```

Deployment:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  labels:
    app: my-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: application
          image: nginx
          ports:
            - containerPort: 80
```
Serivce:
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
      app: my-app
  ports:
    - port: 80
      targetPort: 80
      nodePort: 32123
```
Ingress:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: karthik-ingress
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: instance
    
spec:
  rules:
    # - host: karthigeyan.top
        - http:
            paths:
              - path: /
                pathType: Prefix
                backend:
                  service:
                    name: my-service
                    port:
                      number: 80

```
