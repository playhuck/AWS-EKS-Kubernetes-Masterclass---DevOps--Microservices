쿠버네티스가 직접 관리하는 사용자 계정을 의미하는 **service account**에 IAM role을 사용하기 위해, 생성한 클러스터에 **IAM OIDC provider**가 존재해야 한다.

```bash
eksctl utils associate-iam-oidc-provider \
    --region ap-northeast-2 \
    --cluster eksdemo1 \
    --approve
```

즉 쿠버네티스를 생성하는 권한도 있지만,

클러스터 안에서 사용할 iam도 필요하다.

ec2 키페어도 필요한데, 인스턴스 작업자 노드에 엑세스 권한을 원한다면 키페어를 연결해서 사용해야 한다.

```bash
eksctl create nodegroup --cluster=eksdemo1 \
                       --region=ap-northeast-2 \
                       --name=eksdemo1-ng-public1 \
                       --node-type=t3.medium \
                       --nodes=2 \
                       --nodes-min=2 \
                       --nodes-max=4 \
                       --node-volume-size=20 \
                       --ssh-access \
                       --ssh-public-key=eks-keypair \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access
```

노드 그룹을 생성한뒤 노드그룹을 확인해볼 수 있다.

```bash
kubectl get nodes
```

이제 노드그룹을 생성하면, 위 노드 그룹에서 사용하기로 했던 옵션이 모두 켜져있는 것을 콘솔에서 확인할 수 있다.

asg 그룹, vpc, alb, node…

```
# List EKS clusters
eksctl get cluster

# List NodeGroups in a cluster
eksctl get nodegroup --cluster=<clusterName>

# List Nodes in current kubernetes cluster
kubectl get nodes -o wide

# Our kubectl context should be automatically changed to new cluster
kubectl config view --minify
```

위 커맨드를 통해 노드들을 살펴볼 수 있다.

한가지 중요한 것은 nat gateway인데 작업자 노드를 배포한다면 Private 서브넷에 있는

작업과 노드 그룹이 private 서브넷에 있는 것과 같은데 이런 클러스터에서 ecr이나 docker hub같은데 접근하려면 낫 게이트웨이가 필요하다.

자동으로 낫 게이트웨이가 생성되며 사용된다.

노드로서 생성된 ec2의 보안그룹에는 클러스터 보안그룹이있고 remoteAccess 보안 그룹이 있다.

remoteAccess는 80으로 다 열려있는데 그냥 받아들여라