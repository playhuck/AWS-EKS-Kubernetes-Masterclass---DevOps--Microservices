- **Step 17 Kubernetes Services - Demo**
    
    
    Kubernetes 클러스터에서 백엔드 배포를 진행할 것이다.
    
    간단한 Spring Boot REST 서비스, Hello World 서비스를 배포할 것이다.
    
    배포를 생성하면 자동으로 동등한 ReplicaSet과 Pod가 생성된다. 그 이후에 백엔드 애플리케이션과 관련된 Cluster IP 서비스를 kubectl expose를 사용하여 생성할 것이다.
    
    이 작업이 완료되면, 프론트엔드 배포를 생성할 것이다.
    
    프론트엔드에서는 Nginx Pod를 생성하고, 해당 Nginx Pod에서 백엔드의 Cluster IP 서비스(즉, 우리의 백엔드 서비스)에 프록시를 설정할 것이다.
    
    사용자가 우리의 애플리케이션에 접근할 수 있도록, 프론트엔드 애플리케이션의 NodePort 서비스도 배포할 것이다.
    
    ### **Step-02: ClusterIP Service - Backend Application Setup**
    
    - Create a deployment for Backend Application (Spring Boot REST Application)
    - Create a ClusterIP service for load balancing backend application.
    1. Backend Deployment 생성
        
        ```bash
        # Create Deployment for Backend Rest App
        kubectl create deployment my-backend-rest-app --image=stacksimplify/kube-helloworld:1.0.0 
        kubectl get deploy
        ```
        
    
    1. Backend Deployment Service 생성
        
        ```bash
        # Create ClusterIp Service for Backend Rest App
        kubectl expose deployment my-backend-rest-app --port=8080 --target-port=8080 --name=my-backend-service
        kubectl get svc
        Observation: We don't need to specify "--type=ClusterIp" because default setting is to create ClusterIp Service. 
        ```
        
    
    덤으로 실시간 로그도 미리 켜주자,
    
    ```bash
    kubectl logs -f <파드 이름>
    ```
    
    1. Frontend Nginx 리버스 프록시 생성, 프록시가 백엔드 서비스로 프록시되도록 Deployment 구축
        
        
        위 작업을 위해 미리 구축한 컨테이너 이미지를 사용하기로 한다.
        
        Nginx Config는
        
        ```bash
        server {
            listen       80;
            server_name  localhost;
            location / {
            # Update your backend application Kubernetes Cluster-IP Service name  and port below      
            # proxy_pass http://<Backend-ClusterIp-Service-Name>:<Port>;      
            proxy_pass http://my-backend-service:8080;
            }
            error_page   500 502 503 504  /50x.html;
            location = /50x.html {
                root   /usr/share/nginx/html;
            }
        }
        ```
        
        인데, proxy_pass를 기본 네임스페이스에서 실행되기 때문에 전체 클러스터 DNS 호스트 이름을 제공할 필요는 없다.
        
        ```bash
        # Create Deployment for Frontend Nginx Proxy
        kubectl create deployment my-frontend-nginx-app --image=stacksimplify/kube-frontend-nginx:1.0.0 
        kubectl get deploy
        
        # Create ClusterIp Service for Frontend Nginx Proxy
        kubectl expose deployment my-frontend-nginx-app  --type=NodePort --port=80 --target-port=80 --name=my-frontend-service
        kubectl get svc
        
        # Capture IP and Port to Access Application
        kubectl get svc
        kubectl get nodes -o wide
        http://<node1-public-ip>:<Node-Port>/hello
        
        ```
        
        [http://IP:30487/hello](http://43.202.165.69:30487/hello)
        
        이제 위와 같은 주소 형식으로 들어가면 서버로부터 응답을 받아 뿌려주는 기능을 확인할 수 있다.
        
    
    1. Scalce out하여 로드밸런싱이 잘 되고 있는지 확인
        
        
        ```bash
        # Scale backend with 10 replicas
        kubectl scale --replicas=10 deployment/my-backend-rest-app
        
        # Test again to view the backend service Load Balancing
        http://<node1-public-ip>:<Node-Port>/hello
        ```
        
        프론트 페이지에 접속하면 서버 파드 이름이 계속해서 달라져서 보인다면 성공적으로
        
        로드밸런싱이 되고 있는 것이다.