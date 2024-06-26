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
