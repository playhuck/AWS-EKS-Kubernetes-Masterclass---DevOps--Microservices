- **Kubernetes Namespaces - Limit Range - Update App k8s Manifest, Deploy**
    
    
    ### Kubernetes 네임스페이스에서 객체 생성, 테스트 및 정리
    
    이번 강의에서는 Kubernetes 네임스페이스(dev-3)에서 객체를 생성하고 테스트한 후 정리 작업을 수행합니다.
    
    ### 단계별 요약:
    
    1. **Kubernetes 객체 생성**
    2. **객체 상태 확인 및 테스트**
    3. **객체 정리**
    
    ### 1단계: Kubernetes 객체 생성
    
    먼저 네임스페이스와 관련된 모든 매니페스트를 dev-3 네임스페이스로 업데이트합니다. 그런 다음 `kubectl apply -f q-manifest` 명령어를 사용하여 네임스페이스와 제한 범위, 스토리지 클래스, 영구 볼륨 클레임, 설정 맵, 배포 서비스, 사용자 관리 마이크로서비스 배포, 동등한 서비스 및 필요한 비밀 정보를 생성합니다.
    
    ```bash
    
    kubectl apply -f q-manifest
    ```
    
    ### 2단계: 객체 상태 확인 및 테스트
    
    네임스페이스 dev-3에 생성된 모든 객체의 상태를 확인합니다.
    
    ```bash
    bash코드 복사
    kubectl get pods -n dev-3
    
    ```
    
    MySQL과 사용자 관리 마이크로서비스가 실행 중인지 확인합니다. 모든 포드가 정상적으로 실행되고 있는지 확인합니다. MySQL 포드가 완료되었고, 초기화가 완료된 상태임을 확인합니다. 사용자 관리 마이크로서비스도 생성되었고, liveness와 readiness 프로브 체크를 통과해야 합니다.
    
    ```bash
    kubectl get pods -n dev-3
    ```
    
    포드의 자세한 YAML 출력을 확인하여 제한 범위가 적용되었는지 확인합니다.
    
    ```bash
    kubectl get pod [포드 이름] -o yaml -n dev-3
    ```
    
    제한 범위를 확인합니다.
    
    ```bash
    
    kubectl get limits -n dev-3
    kubectl describe limit [제한 이름] -n dev-3
    
    Type        Resource  Min  Max  Default Request  Default Limit  Max Limit/Request Ratio
    ----        --------  ---  ---  ---------------  -------------  -----------------------
    Container   cpu       -    -    300m             500m           -
    Container   memory    -    -    256Mi            512Mi          -
    ```
    
    서비스를 확인하고 포트 30002로 업데이트하여 접속합니다.
    
    ```bash
    kubectl get svc -n dev-3
    ```
    
    ### 3단계: 객체 정리
    
    모든 객체를 삭제하여 정리 작업을 수행합니다.
    
    ```bash
    
    kubectl delete -f q-manifest
    ```
    
    ### 요약
    
    1. `kubectl apply -f q-manifest` 명령어를 사용하여 네임스페이스와 관련된 모든 객체를 생성합니다.
    2. `kubectl get pods -n dev-3` 명령어로 객체 상태를 확인하고, 포드와 서비스가 정상적으로 작동하는지 테스트합니다.
    3. `kubectl delete -f q-manifest` 명령어를 사용하여 모든 객체를 삭제하여 정리 작업을 수행합니다.