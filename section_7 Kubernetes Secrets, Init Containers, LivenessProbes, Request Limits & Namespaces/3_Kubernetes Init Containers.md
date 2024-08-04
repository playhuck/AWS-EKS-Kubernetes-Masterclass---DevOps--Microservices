- **Kubernetes Init Containers**
    
    
    ### **Kubernetes Init Containers 개요**
    
    - **정의**: Init containers는 일반 애플리케이션 컨테이너가 실행되기 전에 Pod 내부에서 실행되는 특별한 컨테이너입니다.
    - **용도**: 앱 컨테이너가 시작되기 전에 필요한 초기 설정이나 유틸리티 작업을 수행합니다.
    - **특징**:
        - 여러 개의 init containers를 순서대로 설정할 수 있습니다.
        - 각 init container는 성공적으로 완료되어야만 다음 init container가 실행됩니다.
        - 모든 init containers가 완료되면 주 애플리케이션 컨테이너가 시작됩니다.
        - init container가 실패하면, Kubernetes는 이 컨테이너가 성공할 때까지 Pod를 재시작합니다. 단, Pod의 재시작 정책이 "never"로 설정된 경우 재시작하지 않습니다.
    
    ### **Init Containers의 사용 사례**
    
    - 유틸리티나 설정 스크립트가 포함된 컨테이너.
    - 애플리케이션 컨테이너가 실행되기 전에 필요한 준비 작업 수행.
    
    **06-UserManagementMicroservice-Deployment-Service.yml**
    
    위 파일에 init container 설정을 추가한다.
    
    ```yaml
    apiVersion: apps/v1
    kind: Deployment 
    metadata:
      name: usermgmt-microservice
      labels:
        app: usermgmt-restapp
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: usermgmt-restapp
      template:  
        metadata:
          labels: 
            app: usermgmt-restapp
        spec:
          **initContainers:
            - name: init-db
              image: busybox:1.31
              command: ['sh', '-c', 'echo -e "Checking for the availability of MySQL Server deployment"; while ! nc -z mysql 3306; do sleep 1; printf "-"; done; echo -e "  >> MySQL DB Server has started";']**      
          containers:
            - name: usermgmt-restapp
              image: stacksimplify/kube-usermanagement-microservice:1.0.0
              ports: 
                - containerPort: 8095           
              env:
                - name: DB_HOSTNAME
                  value: "mysql"            
                - name: DB_PORT
                  value: "3306"            
                - name: DB_NAME
                  value: "usermgmt"            
                - name: DB_USERNAME
                  value: "root"            
                - name: DB_PASSWORD
                  valueFrom:
                    secretKeyRef:
                      name: mysql-db-password
                      key: db-password             
    
    ```
    
    1. **Pod 템플릿에서 Init Containers 설정**:
        - `spec` 아래에서 `initContainers` 섹션을 추가하여 설정합니다.
            - 예를 들어, `BusyBox` 이미지를 사용하여 MySQL 서비스가 실행 중인지 확인하는 init container를 설정합니다.
    2. **작업**:
        - NetCat 명령어를 사용하여 MySQL 클러스터의 3306 포트가 열릴 때까지 주기적으로 확인합니다.
        - MySQL DB 서버가 시작되면 init container의 작업이 완료되고, 그 후 user management 애플리케이션 컨테이너가 시작됩니다.
    3. **매니페스트 적용**:
        - Kubernetes 매니페스트를 작성하고 `kubectl apply -f kube manifest` 명령어로 적용하여 Pods를 생성합니다.
            
            ```yaml
            # kubectl apply 했을 때 Init:0/1로 시작
            usermgmt-microservice-7956c79978-mwjqh   0/1     **Init:0/1**   0          29s
            
            # Init db 커맨드 실행 후
            usermgmt-microservice-7956c79978-mwjqh   1/1     **Running**   0          81s
            ```
            
        - `kubectl describe pod` 명령어를 사용하여 Pod의 상태를 확인합니다. Init container가 성공적으로 실행되었고 user management microservice가 시작되었는지 검토합니다.
            
            ```yaml
            # kubectl describe pod
            Events:
              Type    Reason     Age    From               Message
              ----    ------     ----   ----               -------
              Normal  Scheduled  2m17s  default-scheduler  Successfully assigned default/usermgmt-microservice-7956c79978-mwjqh to ip-192-168-2-91.ap-northeast-2.compute.internal
              Normal  Pulling    2m17s  kubelet            **Pulling image "busybox:1.31"**
              Normal  Pulled     2m13s  kubelet            Successfully pulled image "busybox:1.31" in 3.758s (3.758s including waiting). Image size: 764556 bytes.
              Normal  Created    2m13s  kubelet            **Created container init-db**
              Normal  Started    2m13s  kubelet            **Started container init-db**
              Normal  Pulled     102s   kubelet            Container image "stacksimplify/kube-usermanagement-microservice:1.0.0" already present on machine
              Normal  Created    102s   kubelet            **Created container usermgmt-restapp**
              Normal  Started    102s   kubelet            **Started container usermgmt-restapp**
            ```
            
        
        - 실행된 디플로이 삭제
            
            ```yaml
            # Delete All
            kubectl delete -f kube-manifests/
            
            # List Pods
            kubectl get pods
            
            # Verify sc, pvc, pv
            kubectl get sc,pvc,pv
            ```