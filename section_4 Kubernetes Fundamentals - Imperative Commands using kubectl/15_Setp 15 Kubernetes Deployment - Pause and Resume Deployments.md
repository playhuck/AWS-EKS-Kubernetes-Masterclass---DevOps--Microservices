- **Setp 15 Kubernetes Deployment - Pause and Resume Deployments**
    
    
    이번 강의에서는 Kubernetes 배포에서 일시 중지와 재개 옵션에 대해 배워본다.
    
    배포를 일시 중지하고 재개하는 이유는 여러 가지 변경 사항을 한 번에 적용하고자 할 때 유용하기 때문이다. 배포를 일시 중지하면, 여러 가지 변경을 한 번에 적용한 후 재개할 수 있다. 이렇게 하지 않으면, 변경 사항을 적용할 때마다 자동으로 새로운 배포가 시작되고, 포드가 종료되고 다시 생성된다.
    
    개발계 클러스터 관리를 할 때 잦은 커밋을 올리면 올린 커밋마다 배포하게 되니, 중단했다 버저닝할 때 배포해도 될 것 같다는 느낌이 든다.
    
    이번 강의에서는 애플리케이션 버전을 V3에서 V4로 변경하고, CPU 리미트를 설정하는 두 가지 변경을 적용할 것이다.
    
    현재 앱 상태를 보면
    
    ```bash
    # Check the Rollout History of a Deployment
    kubectl rollout history deployment/my-first-deployment  
    Observation: Make a note of last version number
    1         <none>
    3         kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true
    4         kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true
    5         kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true
    
    # Get list of ReplicaSets
    kubectl get rs
    Observation: Make a note of number of replicaSets present.
    
    # Access the Application 
    http://<worker-node-ip>:<Node-Port>
    Observation: Make a note of application version
    ```
    
    ~~4개의 복제 세트가 있어야 한다.~~
    
    나는 잘못 만들어서 새롭게 수정했다.
    
    ```bash
    # Pause the Deployment
    kubectl rollout pause deployment/<Deployment-Name>
    kubectl rollout pause deployment/my-first-deployment
    
    # Update Deployment - Application Version from V3 to V4
    kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:4.0.0 --record=true
    
    # Check the Rollout History of a Deployment
    kubectl rollout history deployment/my-first-deployment  
    Observation: No new rollout should start, we should see same number of versions as we check earlier with last version number matches which we have noted earlier.
    
    # Get list of ReplicaSets
    kubectl get rs
    Observation: No new replicaSet created. We should have same number of replicaSets as earlier when we took note. 
    
    # Make one more change: set limits to our container
    kubectl set resources deployment/my-first-deployment -c=kubenginx --limits=cpu=20m,memory=30Mi
    ```
    
    롤아웃을 멈추는 명령어를 입력하고 새롭게 이미지를 set image한 뒤 바뀌는지 확인하면
    
    그대로일 것이다.
    
    ```bash
    # Resume the Deployment
    kubectl rollout resume deployment/my-first-deployment
    
    # Check the Rollout History of a Deployment
    kubectl rollout history deployment/my-first-deployment  
    Observation: You should see a new version got created
    
    # Get list of ReplicaSets
    kubectl get rs
    Observation: You should see new ReplicaSet.
    ```
    
    이제 롤업을 재개하면
    
    Application Version: V4으로 페이지가 갱신되고
    
    ```bash
    8         kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:4.0.0 --record=true
    ```
    
    히스토리에도 4버전이 추가된 걸 볼 수 있다.