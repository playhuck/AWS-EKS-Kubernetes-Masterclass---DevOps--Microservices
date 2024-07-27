- **Step 8 Kubernetes ReplicaSet - Review manifests and Create ReplicaSet**
    
    
    ### ReplicaSet 생성
    
    - apiVersion: apps/v1는 ReplicaSet의 API 버전을 지정합니다.
    - kind: 객체의 종류를 정의합니다. 여기서는 ReplicaSet입니다.
    - metadata: ReplicaSet의 이름을 지정합니다.
    - spec: ReplicaSet의 사양을 정의합니다.
    - replicas: 생성할 파드의 수를 설정합니다. 여기서는 3으로 설정했습니다.
    - selector: ReplicaSet이 관리할 파드를 식별하기 위한 레이블 셀렉터를 정의합니다.
    - template: ReplicaSet이 생성할 파드의 템플릿을 정의합니다.
    - metadata: 파드에 적용할 레이블을 지정합니다.
    - spec: 파드의 컨테이너 사양을 정의합니다.
    - ReplicaSet 생성 및 확인
    - ReplicaSet 생성: 아래 명령어로 ReplicaSet을 생성합니다.
    
    먼저
    
    ```bash
    kubectl get replicaset
    kubectl get rs
    
    # No resources found in default namespace.
    ```
    
    레플리카셋이 있는지 검색해보면
    
    없다고 나올 것 이다.
    
    이제 yaml 파일을 만들고
    
    ```bash
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: my-helloworld-rs
      labels:
        app: my-helloworld
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: my-helloworld
      template:
        metadata:
          labels:
            app: my-helloworld
        spec:
          containers:
          - name: my-helloworld-app
            image: stacksimplify/kube-helloworld:1.0.0
    ```
    
    생성하면
    
    ```bash
    kubectl create -f replicaset-demo.yml
    
    NAME               DESIRED   CURRENT   READY   AGE
    my-helloworld-rs   3         3         3       33
    ```
    
    yaml 파일을 기반으로 생성된 것을 확인할 수 있다.
    
    ```bash
    kubectl describe rs/my-helloworld-rs
    ```
    
    이제 생성된 레플리카 셋을 기반으로 파드를 확인하면
    
    ```bash
    kubectl get pods
    
    my-helloworld-rs-hzp7m   1/1     Running   0          2m1s
    my-helloworld-rs-t9wlc   1/1     Running   0          2m1s
    my-helloworld-rs-tp5td   1/1     Running   0          2m1s
    ```
    
    위와 같이 생성된 것을 알 수 있다.
    
    또 각 파드의 ownerReference는
    
    ```bash
    kubectl get pod my-helloworld-rs-hzp7m -o yaml
    
    ownerReferences:
      - apiVersion: apps/v1
        blockOwnerDeletion: true
        controller: true
        kind: ReplicaSet
        **name: my-helloworld-rs**
        uid: 9dbcfc70-eddb-4fce-ac15-12e64c999c89
    ```
    
    이런식으로 확인할 수 있다.