- **Step 4 Kubernetes NodePort Service and Pods Demo**
    
    
    - **Ports**
        - **port:** Port on which node port service listens in Kubernetes cluster internally
        - **targetPort:** We define container port here on which our application is running.
        - **NodePort:** Worker Node port on which we can access our application.
    
    파드를 다시 생성해보면,
    
    ```bash
    # Create pod
    kubectl run my-first-pod --image stacksimplify/kubenginx:1.0.0
    
    # Expose Pod as a Service
    kubectl expose pod <Pod-Name>  --type=NodePort --port=80 --name=<Service-Name>
    kubectl expose pod my-first-pod  --type=NodePort --port=80 --name=my-first-service
    
    # get service
    kubectl get service
    
    NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
    kubernetes         ClusterIP   10.100.0.1      <none>        443/TCP        58m
    my-first-service   NodePort    10.100.137.28   <none>        80:32150/TCP   15s
    ```
    
    위처럼 워커노드 파드의 서비스가 생성된 걸 확인할 수 있는데
    
    ```
    # Get Public IP of Worker Nodes
    kubectl get nodes -o wide
    ```
    
    워커 노드의 퍼블릭 ip를 확인해서 브라우저에서 특정 포트를 통해 접근할 수 있게 된다.
    
    <aside>
    🚫 **강의 내용대로만 따라가면, 페이지가 열리지 않는다.**
    
    </aside>
    
    생성된 service 포트인 32150과 EXTERNAL-IP를 확인하여 주소를 입력했으나 강의에서와 같은 페이지가 열리지 않아, 한참을 해맸다.
    
    무언가 다른 방식을 했는지 확인해본 후 aws에 들어가 보안그룹을 수정해 준 뒤에야 페이지에 접속할 수 있었다. ***eks 노드그룹의 인바운드 규칙으로 저 포트가 열려있지 않아 발생한 문제였다.***
    
    참고로 타겟 포트를 정해주지 않으면 기본 값이 들어가게 되는데, 같은 포트 필드에 기본 값으로 여러 파드가 할당되 있다면 문제가 발생할 수 있다.
    
    ```bash
    # Below command will fail when accessing the application, as service port (81) and container port (80) are different
    kubectl expose pod my-first-pod  --type=NodePort --port=81 --name=my-first-service2     
    
    # Expose Pod as a Service with Container Port (--taret-port)
    kubectl expose pod my-first-pod  --type=NodePort --port=81 --target-port=80 --name=my-first-service3
    ```
    
    위 첫 번째 service2는 target port가 지정되지 않아, port와 target port가 일치하지 않게 되어 접속할 수 없게 됐지만 아래는 동작한다.
    
    —port=81은 외부에서 접근할 수 있는 서비스 포트고,
    
    —target-port는 파드에서 실제로 컨테이너가 리스닝하는 포트로 일반적으로 —port와 —target-port는 동일하게 만들어 작성하는 것이 원칙이다.