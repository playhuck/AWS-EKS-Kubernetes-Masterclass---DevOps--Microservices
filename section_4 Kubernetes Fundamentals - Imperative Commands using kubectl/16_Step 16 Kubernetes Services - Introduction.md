- **Step 16 Kubernetes Services - Introduction**
    
    
    이번 강의에서는 Kubernetes에서 제공하는 여러 서비스 개념을 이해하겠다. Kubernetes 서비스에는 다음과 같은 것들이 있다: Cluster IP, Node Port, Load Balancer, Ingress, 그리고 External Name.
    
    1. Cluster IP
        - Cluster IP는 Kubernetes 클러스터 내부에서 애플리케이션 간의 통신을 위해 사용됩니다.
        - 예를 들어, 프론트엔드 애플리케이션과 백엔드 애플리케이션이 서로 통신해야 할 때 Cluster IP를 사용합니다.
        - Cluster IP는 클러스터 내의 네트워크 범위에서만 유효합니다.
    2. Node Port
        - Node Port는 Kubernetes 클러스터 외부에서 애플리케이션에 접근할 수 있도록 합니다.
        - 워커 노드의 포트를 통해 외부에서 접근할 수 있게 해주며, 브라우저를 통해 애플리케이션에 접근할 수 있게 합니다.
    3. Load Balancer
        - Load Balancer 서비스는 주로 클라우드 제공업체의 로드 밸런서 서비스와 통합하기 위해 사용됩니다.
        - 예를 들어, AWS에서는 Elastic Load Balancer를 사용하여 Kubernetes 매니페스트를 통해 자동으로 로드 밸런서를 프로비저닝할 수 있습니다.
    4. Ingress
        - Ingress는 고급 로드 밸런서로, 컨텍스트 경로 기반 라우팅, SSL 활성화 및 SSL 리다이렉트 등 다양한 기능을 제공합니다.
        - AWS의 Application Load Balancer와 유사한 개념으로, HTTP 기반 기능을 모두 지원합니다.
    5. External Name
        - External Name 서비스는 클러스터 외부에서 호스팅된 애플리케이션이나 데이터베이스에 접근할 때 사용됩니다.
        - 예를 들어, AWS RDS와 같은 클라우드 제공업체 데이터베이스를 Kubernetes 클러스터 내에서 접근할 수 있게 해줍니다.
    
    클러스터 외부에서 앱에 접근하려면 프론트엔드 서비스로 Node Port, Load Balancer, Ingress를 설정할 수 있다.