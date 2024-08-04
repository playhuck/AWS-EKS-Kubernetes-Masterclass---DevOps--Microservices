- **EKS Storage - RDS DB Introduction**
    
    
    - **현재 구현된 내용 복습:**
        - 이전에는 Kubernetes 클러스터 내에서 MySQL 포드를 사용하여 MySQL 컨테이너를 생성하고, 포드와 배포를 설정했습니다.
        - EBS, 스토리지 클래스, PVC 등을 사용하여 데이터 지속성을 구현했습니다.
    - **MySQL 클러스터 사용의 문제점:**
        - **복잡한 설정**: StatefulSet을 사용해 MySQL의 고가용성을 설정하려면 복잡한 작업이 필요합니다.
        - **제한된 EBS**: EBS 볼륨은 가용 영역 수준에서 제공되며, 다수의 가용 영역을 사용하는 설정에서 문제를 일으킬 수 있습니다.
        - **관리 어려움**: 자동 백업, 복구, 업그레이드 등이 부족하고, MySQL 관리와 관련된 복잡한 설정이 필요합니다.
    - **RDS 데이터베이스의 장점:**
        - **고가용성**: RDS는 자동으로 높은 가용성을 제공하며, 읽기 전용 복제본 생성 및 자동 백업/복구 기능을 지원합니다.
        - **편리한 관리**: 관리 콘솔에서 쉽게 설정할 수 있으며, 자동 업그레이드와 메트릭/모니터링 기능도 제공됩니다.
        - **간단한 통합**: Kubernetes 클러스터에서 RDS 엔드포인트 URL만 설정하면 쉽게 통합할 수 있습니다.
    - **AWS 아키텍처 구성:**
        - **VPC와 서브넷**: EKS 클러스터는 VPC 내에서 자동으로 생성되며, RDS 데이터베이스는 프라이빗 서브넷에 배치됩니다.
        - **클러스터와 데이터베이스**: EKS 클러스터의 제어 평면은 퍼블릭 및 프라이빗 서브넷을 포함하며, 데이터베이스는 프라이빗 서브넷에 위치합니다.