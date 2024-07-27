- **Step 5 Interact with Pod - Connect to container in a pod**
    
    
    이번엔 쿠버네티스에서 파드와 상호 작용하는 법을 다룬다.
    
    ### 먼저 파드 로그 출력,
    
    ```
    # Get Pod Name
    kubectl get po
    
    # Dump Pod logs
    kubectl logs <pod-name>
    kubectl logs my-first-pod
    
    # Stream pod logs with -f option and access application to see logs
    kubectl logs <pod-name>
    kubectl logs -f my-first-pod
    ```
    
    kubectl logs -f my-first-pod, -f 플래그를 사용하면 로그를 실시간으로 확인할 수 있다.
    
    - f : 로그를 실시간으로 스트리밍합니다.
    - c : 여러 컨테이너가 있는 경우, 특정 컨테이너의 로그를 확인할 수 있습니다.
    
    ### 파드에서 내부 컨테이너에 연결하는 방법
    
    ```
    # Connect to Nginx Container in a POD
    kubectl exec -it <pod-name> -- /bin/bash
    kubectl exec -it my-first-pod -- /bin/bash
    
    # Execute some commands in Nginx container
    ls
    cd /usr/share/nginx/html
    cat index.html
    exit
    ```
    
    컨테이너 내부 값도 개별적으로 가져올 수 있다.
    
    ```bash
    kubectl exec -it <pod-name> env
    
    # Sample Commands
    kubectl exec -it my-first-pod env
    kubectl exec -it my-first-pod ls
    kubectl exec -it my-first-pod cat /usr/share/nginx/html/index.html
    ```
    
    ### 파드 Yaml 정의
    
    ```bash
    kubectl get pod my-first-pod -o yaml
    kubectl get svc my-first-service -o yaml
    ```
    
    위 커맨드를 통해
    
    ```bash
    apiVersion: v1
    kind: Pod
    metadata:
      creationTimestamp: "2024-07-27T14:05:53Z"
      labels:
        run: my-first-pod
      name: my-first-pod
      namespace: default
      resourceVersion: "9811"
      uid: 4becc5b2-07ac-44e3-8233-77a67f32c5f7
    spec:
      containers:
      - image: stacksimplify/kubenginx:1.0.0
        imagePullPolicy: IfNotPresent
        ...
    ```
    
    형태의 yaml파일을 얻을 수 있다.