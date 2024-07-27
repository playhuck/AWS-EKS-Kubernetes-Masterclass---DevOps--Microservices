
- **Step 2 Kubernets Pods Demo**
    
    
    ### 워커노드 상태 확인
    
    먼저 Github Repository로 가서, stack-simplify-kubernetes-fundamentals 리포지토리의 pods 폴더로 들어간다.
    
    워커 노드가 살아 있는지 확인하기 위해 다음 명령어를 입력한다.
    
    ```bash
    kubectl get nodes
    ```
    
    아마 마지막 강의에 클러스터를 삭제했기 때문에, 없을 것이다. 다시 클러스터를 생성해보자
    
    ### 파드 생성
    
    이제 Pod를 생성할 건데, kubectl run 명령어를 사용해 Pod를 생성할 수 있다. 
    
    ```bash
    kubectl run my-first-pod --image stacksimplify/kubenginx:1.0.0
    ```
    
    파드를 실행시켰으면 이제 확인해보자
    
    ```bash
    kubectl get pods
    
    NAME           READY   STATUS    RESTARTS   AGE
    my-first-pod   1/1     Running   0          105s
    ```
    
    ### 파드 상세 정보 확인
    
    파드의 상세정보가 궁금하다면
    
    ```bash
    kubectl describe pod 
    
    # To get list of pod names
    kubectl get pods
    
    # Describe the Pod
    kubectl describe pod <Pod-Name>
    kubectl describe pod my-first-pod 
    ```
    
    ### Application Access
    
    현재 생성된 파드에 kubectl 명령어를 통해 접근할 수 있지만, 인터넷을 통해 접근하기 위해서는 Service가 필요하다.
    
    ### 파드 삭제
    
    ```bash
     kubectl get pods # 명령어로 먼저 확인 후
     
     kubectl delete pod my-first-pod # 지우고 싶은 특정 파드 삭제
    ```