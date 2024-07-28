- Install EBS CSI Driver
    
    
    먼저 iam 정책을 생성한다.
    
    ```bash
    
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": [
            "ec2:AttachVolume",
            "ec2:CreateSnapshot",
            "ec2:CreateTags",
            "ec2:CreateVolume",
            "ec2:DeleteSnapshot",
            "ec2:DeleteTags",
            "ec2:DeleteVolume",
            "ec2:DescribeInstances",
            "ec2:DescribeSnapshots",
            "ec2:DescribeTags",
            "ec2:DescribeVolumes",
            "ec2:DetachVolume"
          ],
          "Resource": "*"
        }
      ]
    }
    ```
    
    그리고 생성한 iam을 워커 노드에 연결하여 사용할 수 있도록합니다.
    
    ```bash
    # Get Worker node IAM Role ARN
    kubectl -n kube-system describe configmap aws-auth
    
    # from output check rolearn
    rolearn: arn:aws:iam::180789647333:role/eksctl-eksdemo1-nodegroup-eksdemo-NodeInstanceRole-IJN07ZKXAWNN
    ```
    
    그러면 위 rolearn 형태의 iam rolearn을 얻을 수 있는데
    
    위에서 생성한 정책을 해당 rolearn에 연결한다.
    
    kubernetes 버전을 확인해야 하는데 1.14이상이여야 한다.
    
    ```bash
    kubectl version --client --short
    
    Client Version: v1.30.3
    Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
    ```
    
    버전이 문제가 없다면 CSI Driver를 설치한다.
    
    ```bash
    # Deploy EBS CSI Driver
    kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=master"
    
    # serviceaccount/ebs-csi-controller-sa created
    # serviceaccount/ebs-csi-node-sa created
    # role.rbac.authorization.k8s.io/ebs-csi-leases-role created
    # clusterrole.rbac.authorization.k8s.io/ebs-csi-node-role created
    # clusterrole.rbac.authorization.k8s.io/ebs-external-attacher-role created
    # clusterrole.rbac.authorization.k8s.io/ebs-external-provisioner-role created
    # clusterrole.rbac.authorization.k8s.io/ebs-external-resizer-role created
    # clusterrole.rbac.authorization.k8s.io/ebs-external-snapshotter-role created
    # rolebinding.rbac.authorization.k8s.io/ebs-csi-leases-rolebinding created
    # clusterrolebinding.rbac.authorization.k8s.io/ebs-csi-attacher-binding created
    # clusterrolebinding.rbac.authorization.k8s.io/ebs-csi-node-getter-binding created
    # clusterrolebinding.rbac.authorization.k8s.io/ebs-csi-provisioner-binding created
    # clusterrolebinding.rbac.authorization.k8s.io/ebs-csi-resizer-binding created
    # clusterrolebinding.rbac.authorization.k8s.io/ebs-csi-snapshotter-binding created
    # deployment.apps/ebs-csi-controller created
    # poddisruptionbudget.policy/ebs-csi-controller created
    # daemonset.apps/ebs-csi-node created
    # csidriver.storage.k8s.io/ebs.csi.aws.com created
    
    # Verify ebs-csi pods running
    kubectl get pods -n kube-system
    
    aws-node-4cdsw                        2/2     Running   0          9h
    aws-node-nlrmx                        2/2     Running   0          9h
    coredns-5b9dfbf96-2r9zs               1/1     Running   0          9h
    coredns-5b9dfbf96-8z267               1/1     Running   0          9h
    ebs-csi-controller-646bdcfb79-l47qc   6/6     Running   0          41s
    ebs-csi-controller-646bdcfb79-m24bk   6/6     Running   0          41s
    ebs-csi-node-cpk2h                    3/3     Running   0          41s
    ebs-csi-node-hjkl7                    3/3     Running   0          41s
    kube-proxy-4q462                      1/1     Running   0          9h
    kube-proxy-n48lp                      1/1     Running   0          9h
    ```