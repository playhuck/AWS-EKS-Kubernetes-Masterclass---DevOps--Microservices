- **Kubernetes Namespaces - Create Imperatively using kubectl**
    
    
    ### **네임스페이스 생성 및 확인**
    
    1. **네임스페이스 생성**
        - `kubectl create namespace dev1` 명령어를 사용하여 `dev1` 네임스페이스를 생성합니다.
        - 동일한 방식으로 `dev2` 네임스페이스를 생성합니다.
        - `kubectl get ns` 명령어를 사용하여 생성된 네임스페이스를 확인할 수 있습니다.
    2. **노드 포트 서비스 주석 처리**
        - 사용자 관리 노드 포트 서비스에서 노드 포트를 31231로 지정한 부분을 주석 처리하여 동적 포트를 할당하도록 합니다.
        - 이는 여러 네임스페이스에 동일한 포트를 사용할 수 없기 때문입니다.
    
    ### **매니페스트 배포**
    
    1. **dev1 및 dev2 네임스페이스에 매니페스트 배포**
        - `kubectl apply -f <manifest-file> -n dev1` 명령어를 사용하여 `dev1` 네임스페이스에 매니페스트를 배포합니다.
        - 동일한 방법으로 `dev2` 네임스페이스에도 매니페스트를 배포합니다.
    2. **배포된 객체 확인**
        - `kubectl get all -n dev1` 명령어를 사용하여 `dev1` 네임스페이스에 배포된 모든 객체를 확인합니다.
            
            ```yaml
            NAME                                        READY   STATUS     RESTARTS   AGE
            pod/mysql-64864d79c7-b5r7b                  1/1     Running    0          12s
            pod/usermgmt-microservice-fc67b4fbc-pqtsv   0/1     Init:0/1   0          12s
            
            NAME                               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
            service/mysql                      ClusterIP   None            <none>        3306/TCP         12s
            service/usermgmt-restapp-service   NodePort    10.100.221.81   <none>        8095:31685/TCP   12s
            
            NAME                                    READY   UP-TO-DATE   AVAILABLE   AGE
            deployment.apps/mysql                   1/1     1            1           12s
            deployment.apps/usermgmt-microservice   0/1     1            0           12s
            
            NAME                                              DESIRED   CURRENT   READY   AGE
            replicaset.apps/mysql-64864d79c7                  1         1         1       12s
            replicaset.apps/usermgmt-microservice-fc67b4fbc   1         1         0       12s
            ```
            
        - `dev2` 네임스페이스에서도 동일하게 확인합니다.
        - 기본 네임스페이스에는 아무것도 배포되지 않음을 확인할 수 있습니다.
            
            ```yaml
            NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
            service/kubernetes   ClusterIP   10.100.0.1   <none>        443/TCP   5h41m
            ```
            
    
    ### **스토리지 클래스 및 PVC 확인**
    
    1. **스토리지 클래스 및 PVC 확인**
        - PVC는 네임스페이스 수준의 객체로 `dev1`과 `dev2`에 각각 존재합니다.
            
            ```yaml
            kubectl get pvc
            No resources found in default namespace.
            
            kubectl get pvc -n dev1
            NAME                 STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
            ebs-mysql-pv-claim   Bound    pvc-ac9f6d5b-88a2-46f6-8bc4-7313be16f452   4Gi        RWO            ebs-sc         <unset>                 91s
            ```
            
        - PV와 스토리지 클래스는 특정 네임스페이스에 국한되지 않습니다.
    
    ### **애플리케이션 접근 및 확인**
    
    1. **애플리케이션 접근**
        - 서비스 포트를 가져와서 브라우저에서 확인하면 `dev1`과 `dev2` 모두 정상적으로 실행 중임을 알 수 있습니다.
    
    ### **네임스페이스 및 리소스 정리**
    
    1. **리소스 정리**
        - `kubectl delete -f <manifest-file> -n dev1` 명령어를 사용하여 `dev1` 네임스페이스의 모든 객체를 삭제할 수 있습니다.
        - `kubectl delete ns dev1` 명령어로 `dev1` 네임스페이스 전체를 삭제할 수 있습니다.
        - 스토리지 클래스도 삭제해야 합니다.