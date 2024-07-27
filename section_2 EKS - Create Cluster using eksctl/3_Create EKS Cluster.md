eksctl을 통해 CLuster를 생성할 것이다.

```
# Create Cluster
eksctl create cluster --name=eksdemo1 \
                      --region=ap-northeast-2 \
                      --zones=ap-northeast-2a,ap-northeast-2b \
                      --without-nodegroup

# Get List of clusters
eksctl get cluster
```

region은 한국에 맞춰 ap-northeast-2

zone은 ap-northeast-2a, ap-northeast-2b

노드 그룹 없이

여기서 클러스터가 az를 선택하지 않으면 자동으로 클러스터가 존을 선택하는데 이렇게 되면

작업 할 때 오류가 발생할 수 있다.

문제가 발생하는 것은 보통

클러스터가 무엇을 하는지 모르고 동작할 때이다. 각 노드 그룹을 위한 특정단계가 있다.

```bash
# Create Public Node Group   
eksctl create nodegroup --cluster=eksdemo1 \
                       --region=us-east-1 \
                       --name=eksdemo1-ng-public1 \
                       --node-type=t3.medium \
                       --nodes=2 \
                       --nodes-min=2 \
                       --nodes-max=4 \
                       --node-volume-size=20 \
                       --ssh-access \
                       --ssh-public-key=kube-demo \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access 
```

그래서 클러스터를 생성할 때 지정하는 것이 아닌 노드그룹을 만들 때 지정한다.

생성된 쿠버네티스는

```bash
Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "eksdemo1" in "ap-northeast-2"
```

이런 값을 보여주는데, 위에서 언급한 것처럼 쿠버네티스 서버 끝점의 public access가 열려있기 때문에 공용 접근이 가능해진다.

```bash
create cluster control plane "eksdemo1", 
    2 sequential sub-tasks: { 
        1 task: { create addons },
        wait for control plane to become ready
    }
}
```

컨트롤 플레인도 생성하게 된다.

노드가 있는지도 확인할 수 있다. 우리는 노드 그룹 없이 생성했기 때문에 없어야 한다.

```bash
kubectl get nodes
No resources found
```