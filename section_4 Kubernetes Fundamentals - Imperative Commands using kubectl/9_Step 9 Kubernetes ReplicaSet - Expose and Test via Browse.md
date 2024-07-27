- **Step 9 Kubernetes ReplicaSet - Expose and Test via Browse**
    
    
    이번에는 ReplicaSet을 서비스로 노출하여 외부에서 앱에 접근할 수 있도록 하는 방법과
    
    ReplicaSet의 안정성과 확장성을 테스트하고, 최종적으로 클린 업하는 과정을 다룰 것이다.
    
    우선 ReplicaSet을 서비스로 노출시켜야 한다.
    
    ```bash
    kubectl expose rs <ReplicaSet-Name>  --type=NodePort --port=80 --target-port=8080 --name=<Service-Name-To-Be-Created>
    kubectl expose rs my-helloworld-rs  --type=NodePort --port=80 --target-port=8080 --name=my-helloworld-rs-service
    
    kubectl get nodes -o wide # EXTERNAL-IP
    kubectl get service # PORT
    ```
    
    - -type=NodePort: 서비스 타입을 NodePort로 설정합니다.
    - -name=my-helloworld-rs-service: 서비스의 이름을 설정합니다.
    - -port=8080: 서비스 포트를 설정합니다.
    - -target-port=8080: 컨테이너의 포트를 설정합니다.
    
    *Kubernetes의 ReplicaSet은 고가용성(High Availability)을 보장하기 위해 설정된 복제본 수를 유지합니다. 애플리케이션 문제가 발생하여 파드(Pod)가 비정상적으로 종료되더라도 ReplicaSet은 자동으로 새로운 파드를 생성하여 설정된 복제본 수를 유지합니다.* 
    
    이제 노출된 레플리카셋에 이전과 같은 방식으로 접근하면 페이지가 열린다.
    
    근데 계속 언급된 것 처럼 레플리카셋은 노드 개수를 유지하게 되는데, 만약 1개를 지운다면
    
    ```bash
    my-helloworld-rs-hzp7m   1/1     Running   0          11m
    my-helloworld-rs-t9wlc   1/1     Running   0          11m
    **my-helloworld-rs-tp5td**   1/1     Running   0          11m
    kubectl delete pod my-helloworld-rs-tp5td
    pod "my-helloworld-rs-tp5td" deleted
    
    # 지운 이후
    **my-helloworld-rs-6jlzq   1/1     Running   0          30s**
    my-helloworld-rs-hzp7m   1/1     Running   0          12m
    my-helloworld-rs-t9wlc   1/1     Running   0          12m
    ```
    
    레플리카셋이 지워진 파드를 대신하여 새롭게 **my-helloworld-rs-6jlzq를 보충**한 것을 확인할 수 있다.
    
    또 레플리카셋의 확장성을 테스트하기 위하여 이전에 생성했던 yaml파일의 replica 개수를
    
    ```bash
    spec:
      replicas: 3
      
    #
    
    spec:
      replicas: 6
      
    kubectl replace -f replicaset-demo.yml
    
    my-helloworld-rs-6jlzq   1/1     Running   0          3m38s
    my-helloworld-rs-hzp7m   1/1     Running   0          15m
    my-helloworld-rs-t9wlc   1/1     Running   0          15m
    my-helloworld-rs-vg7nk   1/1     Running   0          19s
    my-helloworld-rs-xmzsm   1/1     Running   0          19s
    my-helloworld-rs-zdmsx   1/1     Running   0          19s
    ```
    
    으로 조정하면 6개로 늘어난 것을 확인할 수 있다.
    
    레플리카셋 삭제는
    
    ```bash
    kubectl delete rs <name>
    kubectl delete rs my-helloworld-rs
    kubectl delete svc my-helloworld-rs-service
    ```
    
    전부 지워진 걸 확인해보면 알 수 있다.