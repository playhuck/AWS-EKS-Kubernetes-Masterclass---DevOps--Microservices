- **Step 14 Kubernetes Deployment - Rollback Application to Previous Version - Undo**
    
    
    롤백은 두 가지 방법으로 롤백을 할 수 있다. 첫째, 바로 이전 버전으로 롤백하거나, 둘째, 특정 버전으로 롤백할 수 있다.
    
    먼저, 롤백을 통해 특정 버전으로 돌아가는 방법을 살펴보면
    
    ```bash
    # List Deployment History with revision information
    **kubectl rollout history deployment/my-first-deployment --revision=1**
    kubectl rollout history deployment/my-first-deployment --revision=2
    kubectl rollout history deployment/my-first-deployment --revision=3
    ```
    
    위 명령어 처럼 특정 개정버전을 입력하면 된다.
    
    ```bash
    **kubectl rollout history deployment/my-first-deployment --revision=1
    
    Image:      stacksimplify/kubenginx:1.0.0**
    ```
    
    이전에 올려놓았던 이미지 버전이 1.0.0으로 돌아간걸 확인할 수 있다.
    
    다시 히스토리를 보면,
    
    ```bash
    kubectl rollout history deployment my-first-deployment --revision==2
    
    Annotations:  kubernetes.io/change-cause: kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true
    ```
    
    어떤 이유로 변경됐는지도 알 수 있다.
    
    두 번째로 롤백하는 방법은,
    
    ```bash
    # Undo Deployment
    kubectl rollout undo deployment/my-first-deployment
    
    # List Deployment Rollout History
    kubectl rollout history deployment/my-first-deployment  
    
    1         <none>
    3         kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true
    4         kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true
    
    kubectl get deploy
    kubectl get rs
    kubectl get po
    kubectl describe deploy my-first-deployment
    ```
    
    2번이 사라지고 4번이 새롭게 추가 됐다.
    
    이 명령어를 실행하면 현재 버전 3에서 버전 2로 롤백된다. 다시 롤아웃 히스토리를 확인해 보면 버전 4가 추가되었고, 이미지 버전이 2.0.0으로 돌아온 것을 확인할 수 있다.
    
    ```bash
    # Rolling Restarts
    kubectl rollout restart deployment/<Deployment-Name>
    kubectl rollout restart deployment/my-first-deployment
    
    # Get list of Pods
    kubectl get po
    
    kubectl get pods
    NAME                                  READY   STATUS    RESTARTS   AGE
    my-first-deployment-c4bf784bc-m4f7g   1/1     Running   0          2m26s
    my-first-deployment-c4bf784bc-mwnjr   1/1     Running   0          2m27s
    
    kubectl rollout restart deployment/my-first-deployment
    
    deployment.apps/my-first-deployment restarted
    kubectl get pods                                      
    NAME                                   READY   STATUS    RESTARTS   AGE
    my-first-deployment-6bc678c7bb-frk8r   1/1     Running   0          3s
    my-first-deployment-6bc678c7bb-s4cct   1/1     Running   0          2s
    ```
    
    위 명령어를 사용하면 모든 파드가 재실행 된다.