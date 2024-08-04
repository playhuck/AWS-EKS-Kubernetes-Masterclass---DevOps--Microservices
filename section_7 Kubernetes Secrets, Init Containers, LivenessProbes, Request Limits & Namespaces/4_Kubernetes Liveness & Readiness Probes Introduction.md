- **Kubernetes Liveness & Readiness Probes Introduction**
    
    
    ### **Kubernetes Probes 개요**
    
    Kubernetes는 애플리케이션의 상태를 모니터링하고 관리하기 위해 세 가지 유형의 probes를 제공합니다:
    
    1. **Liveness Probe**
        - **목적**: 컨테이너가 정상적으로 실행되고 있는지 확인합니다.
        - **사용 사례**: 애플리케이션이 데드락 상태에 빠졌거나, 정상적인 동작을 하지 않는 경우, 컨테이너를 자동으로 재시작하여 문제를 해결합니다.
        - **동작**: liveness probe가 실패하면 Kubernetes는 해당 컨테이너를 재시작합니다.
    2. **Readiness Probe**
        - **목적**: 컨테이너가 트래픽을 수용할 준비가 되었는지 확인합니다.
        - **사용 사례**: 애플리케이션이 완전히 시작되기 전에 트래픽을 받지 않도록 하며, readiness probe가 성공하면 컨테이너가 서비스 로드 밸런서에 추가되어 트래픽을 받을 수 있습니다.
        - **동작**: readiness probe가 실패하면 컨테이너는 서비스 로드 밸런서에서 제거됩니다.
    3. **Startup Probe**
        - **목적**: 컨테이너 애플리케이션이 시작되었는지 확인합니다.
        - **사용 사례**: 느리게 시작되는 컨테이너에 대해 liveness 및 readiness probe의 간섭을 방지하고, 컨테이너가 실행되기 전에 종료되지 않도록 합니다.
        - **동작**: startup probe가 성공할 때까지 liveness 및 readiness probes는 비활성화됩니다.
    
    ### **Probe 정의 옵션**
    
    - **명령어 (Command)**: 쉘 명령어를 사용하여 상태를 체크합니다. 예: `netcat -z localhost 8095`.
    - **HTTP GET 요청**: 애플리케이션의 헬스 체크 페이지를 HTTP GET 요청으로 확인합니다.
    - **TCP 소켓 연결**: 특정 포트가 열려 있는지 확인합니다. 예: 포트 8095.