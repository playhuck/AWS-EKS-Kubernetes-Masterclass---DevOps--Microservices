- **Create Kubernetes Liveness & Readiness Probes**
    
    
    liveness probe는 명령어를 사용하여 정의할 예정
    
    ```yaml
    sh -c "ncat -z localhost 8095
    ```
    
    readiness probe는 HTTP GET 요청과 경로를 사용하여 정의할 것이다. 이전에 테스트했던 사용자 관리 헬스 상태 URL을 사용할 것이며, 포트는 8095다.
    
    또한 initailDelaySeconds와 periodSeconds를 사용할 것이다.
    
    - 일반적으로 initailDelaySeconds는 컨테이너가 시작된 후 liveness 또는 readiness probes가 시작되기 전까지의 시간이다. 컨테이너가 시작된 후 readiness 또는 liveness probes가 트리거되기까지의 시간 차이다. 이는 컨테이너가 애플리케이션을 정리할 시간을 주기 위해 설정된다. 일반적으로 10초, 30초, 50초 또는 60초를 설정한다. 기본값은 0초이며, 최소값도 0초다.
    - 하지만 실패하지 않도록 10초에서 30초 사이로 설정하는 것이 좋다.
    - 실습에서는 60초로 설정하여 60초가 완료될 때까지 파드가 준비 상태로 보이지 않도록 하겠다.
    
    ```yaml
     				livenessProbe:
                exec:
                  command: 
                    - /bin/sh
                    - -c 
                    - nc -z localhost 8095
                initialDelaySeconds: 60
                periodSeconds: 10
              readinessProbe:
                httpGet:
                  path: /usermgmt/health-status
                  port: 8095
                initialDelaySeconds: 60
                periodSeconds: 10
    ```
    
    [Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
    
    위 레퍼런스 사이트에 들어가면 Probes에 대한 추가적인 옵션을 확인할 수 있다.
    
    - ***initialDelaySeconds**: 컨테이너가 시작된 후, 스타트업, 라이브니스 또는 레디니스 프로브가 시작되기 전에 대기할 초 수입니다. 스타트업 프로브가 정의된 경우, 라이브니스 및 레디니스 프로브의 지연 시간은 스타트업 프로브가 성공할 때까지 시작되지 않습니다. periodSeconds의 값이 initialDelaySeconds보다 크면 initialDelaySeconds는 무시됩니다. 기본값은 0초입니다. 최소값은 0입니다.*
    - ***periodSeconds**: 프로브를 수행할 주기(초)입니다. 기본값은 10초입니다. 최소값은 1초입니다.*
    - ***timeoutSeconds**: 프로브가 타임아웃되는 초 수입니다. 기본값은 1초입니다. 최소값은 1초입니다.*
    - ***successThreshold**: 프로브가 실패한 후 성공으로 간주되기 위해 필요한 최소 연속 성공 횟수입니다. 기본값은 1입니다. 라이브니스 및 스타트업 프로브의 경우 1이어야 합니다. 최소값은 1입니다.*
    - ***failureThreshold**: 프로브가 연속으로 failureThreshold 횟수만큼 실패하면 Kubernetes는 전체 체크가 실패했다고 간주합니다: 컨테이너가 준비되지 않았거나 건강하지 않거나 라이브하지 않다고 판단합니다. 기본값은 3입니다. 최소값은 1입니다. 스타트업 또는 라이브니스 프로브의 경우, failureThreshold 횟수 이상 실패한 경우 Kubernetes는 해당 컨테이너를 비정상으로 간주하고 해당 컨테이너를 재시작합니다. kubelet은 terminationGracePeriodSeconds의 설정을 해당 컨테이너에 대해 준수합니다. 레디니스 프로브가 실패한 경우, kubelet은 해당 체크가 실패했음에도 불구하고 컨테이너를 계속 실행하며, 추가 프로브를 계속 수행합니다. 체크가 실패했기 때문에 kubelet은 Pod의 Ready 상태를 false로 설정합니다.*
    - ***terminationGracePeriodSeconds**: 실패한 컨테이너의 종료를 트리거한 후, 컨테이너 런타임이 해당 컨테이너를 강제로 중지하기까지 kubelet이 기다릴 유예 기간을 설정합니다. 기본값은 Pod 레벨의 terminationGracePeriodSeconds 값을 상속받으며(지정하지 않으면 30초), 최소값은 1초입니다. 자세한 사항은 프로브 레벨의 terminationGracePeriodSeconds를 참조하세요.*
    
    이제 apply하여 상태를 확인해보면
    
    ```yaml
    kubectl apply -f ../
    
    # -w는 watch
    kubectl get pods -w
    
    mysql-64864d79c7-bdbhx                  1/1     Running    0          15s
    usermgmt-microservice-fc67b4fbc-bnbmc   0/1     Init:0/1   0          15s
    usermgmt-microservice-fc67b4fbc-bnbmc   0/1     PodInitializing   0          23s
    usermgmt-microservice-fc67b4fbc-bnbmc   0/1     Running           0          24s
    usermgmt-microservice-fc67b4fbc-bnbmc   1/1     Running           0          88s
    
    # MySQL 컨테이너가 생성될 때까지 기다리고, init 컨테이너가 성공하면 사용자 관리 마이크로서비스 컨테이너가 시작된다.
    # readiness probe가 60초 대기 후 컨테이너가 준비 상태로 변경되는지 확인 한다. 여기서는 24s였다.
    # 그리고 마지막으로 88초뒤에 애플리케이션이 정상 작동하는 것을 확인했다. readiness probe가 정상적으로 작동하고 있다.
    ```