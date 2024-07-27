쿠버네티스는 컨테이너 워크로드를 관리하기 좋은 가볍고, 확장가능하고 오픈소스 플랫폼이다.

쿠버네티스(Kubernetes)는 컨테이너화된 애플리케이션을 관리하기 위한 강력한 플랫폼으로, 다양한 기능들을 제공하여 애플리케이션의 배포, 확장, 관리를 용이하게 합니다. 여기에는 다음과 같은 기능들이 포함됩니다:

1. **Service Discovery and Load Balancing (서비스 디스커버리 및 로드 밸런싱)**:
    - 쿠버네티스는 클러스터 내의 컨테이너를 자동으로 네트워킹하고 서비스 디스커버리를 제공합니다. 각 Pod에 고유한 IP 주소를 부여하고, 서비스 내의 Pod들 간에 로드 밸런싱을 수행하여 트래픽을 분산시킵니다.
    - 예: `Service` 객체를 통해 서비스 디스커버리와 로드 밸런싱을 구현합니다.
2. **Storage Orchestration (스토리지 오케스트레이션)**:
    - 쿠버네티스는 로컬 스토리지, 클라우드 스토리지, 네트워크 스토리지 등 다양한 스토리지 솔루션과 통합할 수 있습니다. 이를 통해 상태 저장 애플리케이션도 손쉽게 관리할 수 있습니다.
    - 예: `PersistentVolume`(PV)와 `PersistentVolumeClaim`(PVC) 객체를 통해 스토리지를 동적으로 할당하고 관리합니다.
3. **Automated Rollouts and Rollbacks (자동 롤아웃 및 롤백)**:
    - 쿠버네티스는 애플리케이션의 업데이트를 자동으로 처리할 수 있으며, 문제가 발생할 경우 이전 버전으로 롤백할 수 있습니다. 이를 통해 중단 없는 배포를 실현합니다.
    - 예: `Deployment` 객체를 통해 롤링 업데이트와 롤백을 설정합니다.
4. **Automatic Bin Packing (자동 빈 패킹)**:
    - 쿠버네티스는 각 컨테이너에 할당된 리소스(CPU, 메모리 등)를 기반으로 최적의 노드에 스케줄링하여 리소스를 효율적으로 사용합니다.
    - 예: 스케줄러가 노드의 리소스 사용량을 고려하여 Pod를 배치합니다.
5. **Self-Healing (자가 복구)**:
    - 쿠버네티스는 장애가 발생한 컨테이너를 자동으로 재시작하거나, 노드 장애 시 다른 노드로 자동으로 이동시키는 등 자체 치유 기능을 제공합니다.
    - 예: `ReplicaSet`과 `DaemonSet` 객체가 장애 발생 시 자동으로 Pod를 복구합니다.
6. **Secrets and Configuration Management (시크릿 및 구성 관리)**:
    - 쿠버네티스는 애플리케이션 설정 및 시크릿 데이터를 안전하게 관리할 수 있습니다. 이는 환경 변수나 파일로 컨테이너에 주입될 수 있습니다.
    - 예: `ConfigMap`과 `Secret` 객체를 통해 환경 설정과 시크릿 데이터를 관리합니다.

이러한 기능들은 쿠버네티스를 강력한 컨테이너 오케스트레이션 도구로 만들어, 다양한 애플리케이션 워크로드를 쉽게 배포하고 관리할 수 있게 합니다.

쿠버네티스(Kubernetes) 클러스터를 구성하려면 각 노드에 컨테이너 런타임을 설치해야 합니다. 이는 클러스터 내의 모든 노드가 컨테이너화된 애플리케이션을 실행할 수 있도록 하는 필수적인 단계입니다. 다음은 쿠버네티스의 노드와 컨테이너 런타임 설치에 대한 자세한 설명입니다:

### 노드(Node)

쿠버네티스 클러스터는 여러 개의 노드로 구성됩니다. 노드는 두 가지 유형으로 나뉩니다:

1. **마스터 노드 (Master Node)**: 클러스터를 관리하고 제어하는 역할을 합니다. 주로 API 서버, 컨트롤러 매니저, 스케줄러, etcd 등과 같은 주요 쿠버네티스 컴포넌트가 실행됩니다.
2. **워커 노드 (Worker Node)**: 실제로 애플리케이션 컨테이너가 실행되는 노드입니다. 각 워커 노드는 쿠버네티스가 컨테이너를 배포하고 관리할 수 있도록 구성되어야 합니다.

### 런타임(Runtime)

컨테이너 런타임은 컨테이너를 실행하는 데 필요한 소프트웨어입니다. 쿠버네티스는 여러 종류의 컨테이너 런타임을 지원하지만, 가장 널리 사용되는 런타임은 다음과 같습니다:

1. **Docker**: 가장 널리 알려진 컨테이너 런타임으로, 많은 쿠버네티스 배포에서 기본 런타임으로 사용됩니다.
2. **containerd**: Docker의 핵심 구성 요소 중 하나로, 보다 경량화된 컨테이너 런타임입니다.
3. **CRI-O**: Kubernetes Container Runtime Interface (CRI)와 호환되는 오픈소스 컨테이너 런타임입니다.
4. **rkt**: CoreOS에서 개발한 컨테이너 런타임으로, 현재는 사용되지 않습니다.
5. **gVisor, Kata Containers**: 추가적인 보안 격리 기능을 제공하는 컨테이너 런타임입니다.

### 모든 노드에서 런타임을 설치한다는 것의 의미

- **모든 노드에서 컨테이너를 실행 가능하게 함**: 워커 노드뿐만 아니라 필요 시 마스터 노드에서도 컨테이너를 실행할 수 있도록 각 노드에 컨테이너 런타임을 설치해야 합니다.
- **일관된 환경 제공**: 클러스터 내 모든 노드가 동일한 컨테이너 런타임을 사용함으로써, 컨테이너 실행 환경의 일관성을 유지할 수 있습니다.
- **쿠버네티스 구성 요소의 원활한 작동 보장**: kubelet(각 노드에서 실행되는 에이전트)과 같은 쿠버네티스 구성 요소가 컨테이너 런타임을 통해 컨테이너를 시작, 중지, 모니터링할 수 있게 됩니다.

---

### 마스터 노드

- 마스터 노드에는 런타임이 설치되고,
- 그 다음은 etcd입니다.
    
    etcd는 분산 키-값 저장소로 쿠버네티스에서 etcd는 클러스터 상태를 저장하고 관리하는 데 사용됩니다. 이는 쿠버네티스 클러스터의 중심 데이터 저장소 역할을 하며, 다음과 같은 데이터를 저장합니다:
    
    - **클러스터 상태**: 노드, 포드, 서비스, 설정, 시크릿 등 모든 쿠버네티스 객체의 현재 상태.
    - **구성 데이터**: ConfigMap, Secret 등과 같은 애플리케이션 구성 데이터.
    - **동기화 정보**: 클러스터 내에서 노드와 컴포넌트 간의 동기화를 위한 정보.

- 그 다음은 kube-scheduler입니다.
    
    **kube-scheduler**는 쿠버네티스의 기본 스케줄러로, 새로 생성된 Pod를 적절한 노드에 할당하는 역할을 합니다. 스케줄링 결정은 여러 가지 요인을 고려하여 이루어집니다.
    
    - **리소스 요청**: 각 Pod가 요구하는 CPU, 메모리 등의 리소스를 고려합니다.
    - **리소스 가용성**: 각 노드의 현재 리소스 사용량을 확인하여 과부하를 방지합니다.
    - **어피니티/안티 어피니티 규칙**: 특정 Pod들이 함께 배치되거나 떨어져 배치되도록 규칙을 설정할 수 있습니다.
    - **노드 셀렉터와 테인트/톨러레이션**: 특정 라벨이 붙은 노드에만 Pod가 배치되도록 하거나, 특정 조건을 만족하는 Pod만 특정 노드에 배치되도록 설정할 수 있습니다.
- 그 다음은 Kube-apiserver
    
    **kube-apiserver**는 쿠버네티스 API 서버로, 클러스터의 중심 컴포넌트입니다. 모든 API 요청을 처리하고, 클러스터의 상태를 관리합니다.
    
    - **API 엔드포인트 제공**: 쿠버네티스 API를 통해 클라이언트, 사용자, 관리 도구가 클러스터와 상호작용할 수 있습니다.
    - **인증 및 권한 부여**: 클러스터에 대한 접근을 제어하고, 사용자 및 서비스 계정의 권한을 관리합니다.
    - **API 요청 처리**: 클러스터 상태를 조회하거나 수정하는 모든 요청을 처리합니다.
    - **etcd와 통신**: 클러스터의 상태 정보를 etcd에 저장하거나 etcd에서 불러옵니다.
- 그리고 Kube Controller Manager와 Cloud Controller Manager가 있습니다.
    
    **kube-controller-manager**는 여러 컨트롤러를 실행하는 데몬으로, 클러스터의 상태를 원하는 상태로 유지하는 역할을 합니다. 여러 개의 개별 컨트롤러가 하나의 바이너리로 실행됩니다.
    
    - **Replication Controller**: 특정 수의 Pod가 항상 실행 중인지 확인합니다.
    - **Node Controller**: 노드의 상태를 모니터링하고, 노드가 응답하지 않을 경우 적절한 조치를 취합니다.
    - **Endpoints Controller**: Service와 Pod 간의 연결 상태를 관리합니다.
    - **Namespace Controller**: 네임스페이스를 생성하고 삭제합니다.
    
    **cloud-controller-manager**는 클라우드 공급자와 통합하여 클러스터의 상태를 관리하는 역할을 합니다. 클라우드 특정 로직을 클러스터의 기본 컨트롤러 로직과 분리합니다.
    
    - **노드 관리**: 클라우드 인프라와 통합하여 노드의 생명 주기를 관리합니다.
    - **루트 밸런서 관리**: 클라우드 공급자의 로드 밸런서를 관리합니다.
    - **볼륨 관리**: 클라우드 공급자의 스토리지 볼륨을 관리합니다.

---

### 워커 노드

- 워커 노드에도 런타임이 설치되고,
- Kuelet
    
    **kubelet**는 각 워커 노드에서 실행되는 에이전트로, 노드 내에서 컨테이너의 생명 주기를 관리합니다. kubelet은 API 서버와 통신하여 Pod의 상태를 보고하고, 지정된 Pod를 실행하는 역할을 합니다.
    
    - **Pod 스펙 수신 및 실행**: kubelet은 API 서버로부터 Pod 스펙을 받아들여 컨테이너 런타임(Docker, containerd 등)을 통해 컨테이너를 실행합니다.
    - **헬스 체크**: Pod와 컨테이너의 상태를 모니터링하고, 설정된 헬스 체크(liveness, readiness) 조건에 따라 문제 발생 시 자동으로 재시작합니다.
    - **로그 및 메트릭 수집**: 실행 중인 컨테이너의 로그와 메트릭을 수집하여 중앙 집중식 로깅 및 모니터링 시스템에 전송합니다.
    - **노드 리소스 관리**: 각 노드의 CPU, 메모리 등의 리소스를 모니터링하고, 리소스 사용량을 API 서버에 보고합니다.
- Kube-Proxy
    
    
    **kube-proxy**는 쿠버네티스 네트워킹을 담당하는 컴포넌트로, 각 노드에서 실행되며 클러스터 내의 네트워크 통신을 관리합니다. kube-proxy는 네트워크 규칙을 설정하여 Pod 간의 트래픽을 올바르게 라우팅하고 로드 밸런싱을 수행합니다.
    
    - **네트워크 프록시**: 서비스 IP와 포트를 통해 들어오는 트래픽을 적절한 백엔드 Pod로 전달합니다.
    - **로드 밸런싱**: 클러스터 내의 여러 Pod 간에 네트워크 트래픽을 분산시켜 로드 밸런싱을 수행합니다.
    - **IP 테이블 설정**: iptables 또는 IPVS를 사용하여 네트워크 규칙을 설정하고, 트래픽이 올바르게 라우팅되도록 합니다.
    - **클러스터 DNS 통합**: 클러스터 DNS와 통합되어 서비스 디스커버리를 지원합니다.