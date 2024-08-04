- **Kubernetes Namespaces - Create Limit Range k8s manifest**
    
    
    ### 단계별 요약:
    
    1. **네임스페이스 매니페스트 작성**
    2. **제한 범위 매니페스트 작성**
    3. **네임스페이스 및 제한 범위 적용**
    
    ### 1단계: 네임스페이스 매니페스트 작성
    
    Visual Studio Code에서 네임스페이스 매니페스트를 작성합니다. 다음은 네임스페이스 매니페스트의 예시입니다:
    
    ```yaml
    
    apiVersion: v1
    kind: Namespace
    metadata:
      name: dev3
    
    ```
    
    ### 2단계: 제한 범위 매니페스트 작성
    
    네임스페이스에 적용할 제한 범위 매니페스트를 작성합니다:
    
    ```yaml
    apiVersion: v1
    kind: LimitRange
    metadata:
      name: dev3-limits
      namespace: dev3
    spec:
      limits:
      - default:
          cpu: "500m"
          memory: "512Mi"
        defaultRequest:
          cpu: "300m"
          memory: "256Mi"
        type: Container
    
    ```
    
    위의 매니페스트는 `dev3` 네임스페이스 내의 모든 컨테이너에 대해 기본적으로 500m의 CPU와 512Mi의 메모리 제한을 설정합니다. 또한, 컨테이너가 초기 요청하는 리소스를 CPU 300m와 메모리 256Mi로 설정합니다.
    
    파일이름은 00으로 시작합니다. 00으로 설정하면 명령어 하나로 모든 것을 생성할 수 있기 때문입니다. 따라서 첫 번째로 00을 설정하면 순서대로 00부터 08까지 네임스페이스를 먼저 생성한 후 나머지를 생성할 수 있습니다.
    
    모든 코드에 네임스페이스를 새롭게 반영합니다.
    
    ```yaml
    metadata:
      name: ebs-mysql-pv-claim
      namespace: dev3
    
    metadata:
      name: mysql
      namespace: dev3 
    
    metadata:
      name: usermgmt-microservice
      labels:
        app: usermgmt-restapp
      namespace: dev3
      
      ...
    ```
    
    ### 3단계: 네임스페이스 및 제한 범위 적용
    
    위에서 작성한 매니페스트를 적용합니다. 이를 위해 `kubectl apply -f` 명령어를 사용합니다. 
    
    ```bash
    
    kubectl apply -f namespace.yaml
    kubectl apply -f limitrange.yaml
    ```
    
    모든 매니페스트에 네임스페이스 `dev3`를 추가하여 업데이트합니다. 이를 통해 모든 매니페스트가 동일한 네임스페이스에서 작동하도록 설정합니다.