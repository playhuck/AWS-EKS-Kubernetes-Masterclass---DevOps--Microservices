- **Step 13 Kubernetes Deployment - Edit Deployment using kubectl edit**
    
    
    이번 단계에서는 kubectl edit deployment 옵션을 사용하여 애플리케이션을 V2에서 V3로 업데이트할 것이다. 이 옵션은 매우 중요하며, 실제 운영 환경에서 자주 사용된다. kubectl edit deployment 명령어를 사용하여 배포를 수정하고, 컨테이너의 이미지를 업데이트할 수 있다.
    
    ```bash
    # Edit Deployment
    kubectl edit deployment/<Deployment-Name> --record=true
    kubectl edit deployment/my-first-deployment --record=true
    
    # Change From 2.0.0
        spec:
          containers:
          - image: stacksimplify/kubenginx:2.0.0
    
    # Change To 3.0.0
        spec:
          containers:
          - image: stacksimplify/kubenginx:3.0.0
    ```
    
    kubectl edit deployment my-first-deployment 명령어를 입력하면, 배포 사양(Spec)파일이 열린다.
    
    여기서 버전을 3으로 업데이트 한다.
    
    그래서 파드를 검색해보면
    
    ```bash
    kubectl describe pod my-first-deployment-75df965dd-9j4xw
    
    Normal  Pulling    33s   kubelet            Pulling image "stacksimplify/kubenginx:3.0.0"
    ```
    
    방금전에 수정한 스펙파일에 따라 새롭게 이미지를 풀 받아 갱신된 모습을 알 수 있다.
    
    새롭게 롤 아웃 됐는지도 확인해야 하는데,
    
    ```bash
    # Verify Rollout Status 
    kubectl rollout status deployment/my-first-deployment
    ```
    
    버전이 새롭게 생긴걸 알 수 있다.