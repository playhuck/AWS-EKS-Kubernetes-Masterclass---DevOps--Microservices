- **Step 11 Kubernetes Deployment - Demo**
    
    
    이번에는 배포 스테이지를 만들어 볼 건데,
    
    ```bash
    kubectl create deployment <Deplyment-Name> --image=<Container-Image>
    kubectl create deployment my-first-deployment --image=stacksimplify/kubenginx:1.0.0 
    kubectl get deploy 
    ```
    
    강의 예제대로 나온 코드를 실행시키면, Deployment가 생성된 것을 확인할 수 있다.
    
    확장성을 테스트 하기 위해
    
    ```bash
    kubectl scale deployment my-first-deployment --replicas=20
    ```
    
    replicaSet을 실행시키면
    
    ```bash
    # Before
    
    NAME                                   READY   STATUS    RESTARTS   AGE
    my-first-deployment-5bb6fc76f8-cvjqv   1/1     Running   0          45s
    
    # After
    
    my-first-deployment-5bb6fc76f8-2rsl4   0/1     ContainerCreating   0          3s
    my-first-deployment-5bb6fc76f8-6lqp9   0/1     ContainerCreating   0          4s
    my-first-deployment-5bb6fc76f8-925kf   0/1     ContainerCreating   0          4s
    my-first-deployment-5bb6fc76f8-9dtg5   1/1     Running             0          4s
    my-first-deployment-5bb6fc76f8-cfzn8   1/1     Running             0          4s
    my-first-deployment-5bb6fc76f8-cvjqv   1/1     Running             0          2m9s
    my-first-deployment-5bb6fc76f8-gqvk7   1/1     Running             0          4s
    my-first-deployment-5bb6fc76f8-hjbzr   0/1     ContainerCreating   0          3s
    my-first-deployment-5bb6fc76f8-hsph8   0/1     ContainerCreating   0          4s
    my-first-deployment-5bb6fc76f8-lb5xw   1/1     Running             0          4s
    my-first-deployment-5bb6fc76f8-m5qsf   1/1     Running             0          3s
    my-first-deployment-5bb6fc76f8-p5vft   1/1     Running             0          4s
    my-first-deployment-5bb6fc76f8-rrtrq   0/1     ContainerCreating   0          4s
    my-first-deployment-5bb6fc76f8-t8lts   1/1     Running             0          4s
    my-first-deployment-5bb6fc76f8-t94x5   0/1     ContainerCreating   0          4s
    my-first-deployment-5bb6fc76f8-w6rv9   1/1     Running             0          3s
    my-first-deployment-5bb6fc76f8-wn7sp   0/1     ContainerCreating   0          4s
    my-first-deployment-5bb6fc76f8-wrhjc   0/1     ContainerCreating   0          4s
    my-first-deployment-5bb6fc76f8-zkhn6   0/1     ContainerCreating   0          4s
    my-first-deployment-5bb6fc76f8-zmhb9   1/1     Running             0          4s
    ```
    
    이런식으로 확장 / 축소 전략을 가져갈 수 있다.
    
    역시나 배포도 서비스를 따로 만들어줘야 한다.
    
    ```bash
    # Expose Deployment as a Service
    kubectl expose deployment <Deployment-Name>  --type=NodePort --port=80 --target-port=80 --name=<Service-Name-To-Be-Created>
    kubectl expose deployment my-first-deployment --type=NodePort --port=80 --target-port=80 --name=my-first-deployment-service
    
    # Get Service Info
    kubectl get svc
    Observation: Make a note of port which starts with 3 (Example: 80:3xxxx/TCP). Capture the port 3xxxx and use it in application URL below. 
    
    # Get Public IP of Worker Nodes
    kubectl get nodes -o wide
    Observation: Make a note of "EXTERNAL-IP" if your Kubernetes cluster is setup on AWS EKS.
    ```