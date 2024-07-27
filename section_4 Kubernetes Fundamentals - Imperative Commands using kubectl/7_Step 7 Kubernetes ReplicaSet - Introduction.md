- **Step 7 Kubernetes ReplicaSet - Introduction**
    
    
    ReplicaSet은 Application의 고가용성, 신뢰성 및 확장성을 보장하는 중요한 개념이다.
    
    ### ReplicaSet의 목적
    
    - 고가용성 : ReplicaSet은 지정된 수의 파드가 항상 실행되도록 보장한다. 앱이 실패하여 파드가 죽으면 ReplicaSet은 즉시 새로운 파드를 실행시켜 지정된 수의 파드가 실행되도록 한다.
    - 부하 분산(Scaling) : ReplicaSet과 함께 서비스를 사용하면 트래픽을 효과적으로 분산시킬 수 있다.
        - 쿠버네티스는 ReplicaSet의 요소 중 하나인 서비스를 통해 파드의 로드밸런싱 부하 분산을 수행한다.
    - 확장성 : 트래픽이 급격히 증가하면 ReplicaSet을 통해 파드를 줄이고 늘리면서 확장성을 가질 수 있다.
    - 라벨 & 셀렉터 : Labels & Selectors과 Pod / ReplicaSet & Service까지 3가지 요소로 묶이며 yaml manifests를 작성할 때 알 수 있다.
    
    ### Labels & Selectors
    
    - Labels :
    
    파드 / ReplicaSet / Service 등의 k8s 객체에 메타데이터를 추가하는 키-값 쌍이다. 레이블을 통해 객체를 식별하고 그룹화할 수 있다.
    
    - Selectors :
    
    레이블을 기반으로 특정 객체를 선택하는 기준이다.
    
    ### Scale
    
    ```bash
    kubectl scale
    kubectl scale --replicas=5 rs/my-replicaset
    ```
    
    위 명령어를 통해 레플리카 수를 수동으로 조정할 수 있다.
    HorizontalPodAutoscaler를 설정하여 자동으로 파드를 확장할 수 있다.