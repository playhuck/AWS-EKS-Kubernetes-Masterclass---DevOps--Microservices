- **Kubernetes - Resource Quota**
    
    
    ### Kubernetes 리소스 쿼터 강의 요약
    
    이번 강의에서는 Kubernetes 네임스페이스에 리소스 쿼터를 정의하고 적용하는 방법을 학습했습니다. 리소스 쿼터를 사용하여 특정 네임스페이스 내에서 사용할 수 있는 리소스를 제한할 수 있습니다. 이는 네임스페이스 내의 포드, CPU, 메모리 등의 자원 사용을 제어하는 데 유용합니다.
    
    ### 주요 내용 요약:
    
    1. **리소스 쿼터 개념 소개**
    2. **리소스 쿼터 매니페스트 작성**
    3. **리소스 쿼터 적용 및 확인**
    4. **테스트 및 정리**
    
    ### 1단계: 리소스 쿼터 개념 소개
    
    리소스 쿼터는 네임스페이스에 할당할 수 있는 자원의 양을 제한합니다. 예를 들어, 특정 네임스페이스 내에서 사용할 수 있는 포드 수, CPU 및 메모리 양을 제한할 수 있습니다. 이를 통해 자원의 과다 사용을 방지하고 네임스페이스 간의 자원 분배를 효율적으로 관리할 수 있습니다.
    
    Kubernetes에서 리소스 쿼터(Resource Quota)는 네임스페이스가 사용할 수 있는 자원의 절대량을 정의합니다. 이 쿼터는 네임스페이스 내에서 다음과 같은 자원에 대한 최대치를 설정할 수 있습니다:
    
    - **포드 수**: 네임스페이스에 생성할 수 있는 포드의 최대 개수.
    - **CPU 요청**: 네임스페이스 내에서 요청할 수 있는 총 CPU 자원의 양.
    - **메모리 요청**: 네임스페이스 내에서 요청할 수 있는 총 메모리 자원의 양.
    - **CPU 한도**: 네임스페이스 내에서 설정할 수 있는 총 CPU 자원의 최대 양.
    - **메모리 한도**: 네임스페이스 내에서 설정할 수 있는 총 메모리 자원의 최대 양.
    - **ConfigMaps**: 네임스페이스 내에서 생성할 수 있는 ConfigMap 객체의 최대 개수.
    - **PersistentVolumeClaims**: 네임스페이스 내에서 생성할 수 있는 영구 볼륨 클레임(PVC)의 최대 개수.
    - **서비스**: 네임스페이스 내에서 생성할 수 있는 서비스 객체의 최대 개수.
    
    리소스 쿼터는 네임스페이스 내의 리소스 사용을 제한하여 자원의 과도한 소비를 방지하고, 여러 팀이나 프로젝트가 공유하는 클러스터에서 자원을 공정하게 배분할 수 있도록 합니다.
    
    ### 2단계: 리소스 쿼터 매니페스트 작성
    
    리소스 쿼터 매니페스트는 다음과 같이 작성됩니다:
    
    ```yaml
    apiVersion: v1
    kind: ResourceQuota
    metadata:
      name: ns-resource-quota
      namespace: dev3
    spec:
      hard:
        pods: "5"
        requests.cpu: "2"
        requests.memory: "2Gi"
        limits.cpu: "3"
        limits.memory: "4Gi"
        configmaps: "5"
        persistentvolumeclaims: "2"
        services: "5"
    
    ```
    
    이 매니페스트는 dev3 네임스페이스에 다음과 같은 제한을 설정합니다:
    
    - 포드 수: 5
    - 요청된 CPU: 2
    - 요청된 메모리: 2Gi
    - CPU 한도: 3
    - 메모리 한도: 4Gi
    - 설정 맵: 5
    - 영구 볼륨 클레임: 2
    - 서비스: 5
    
    ### 3단계: 리소스 쿼터 적용 및 확인
    
    리소스 쿼터를 적용하려면 다음 명령어를 실행합니다:
    
    ```bash
    kubectl apply -f q-manifest
    ```
    
    리소스 쿼터를 확인하려면 다음 명령어를 사용합니다:
    
    ```bash
    kubectl get quota -n dev3
    kubectl describe quota ns-resource-quota -n dev3
    ```
    
    이를 통해 현재 리소스 사용량과 할당된 쿼터를 확인할 수 있습니다.
    
    ### 4단계: 테스트 및 정리
    
    리소스 쿼터 적용 후, 포드와 서비스가 정상적으로 작동하는지 확인합니다:
    
    ```bash
    kubectl get pods -n dev3
    kubectl get svc -n dev3
    ```
    
    최종적으로 정리 작업을 수행합니다:
    
    ```bash
    kubectl delete -f q-manifest
    ```
    
    ### 요약
    
    - 리소스 쿼터를 사용하면 네임스페이스 내에서 사용할 수 있는 자원을 제한할 수 있습니다.
    - 리소스 쿼터 매니페스트를 작성하여 CPU, 메모리, 포드 수, 설정 맵, 영구 볼륨 클레임 등의 자원을 제한할 수 있습니다.
    - `kubectl apply -f q-manifest` 명령어로 리소스 쿼터를 적용하고, `kubectl get quota -n [네임스페이스]` 및 `kubectl describe quota [쿼터 이름] -n [네임스페이스]` 명령어로 현재 리소스 사용량을 확인할 수 있습니다.
    - 최종적으로 모든 객체를 삭제하여 정리 작업을 수행합니다.