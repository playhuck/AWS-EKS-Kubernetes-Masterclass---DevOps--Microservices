- **Step 12 Kubernetes Deployment - Update Deployment using Set Image Option**
    
    
    배포 업데이트는 set image와 edit deployment 두 가지 방법으로 가능하다. 먼저 set image를 사용하고, 그 다음 edit deployment를 사용해 보겠다.
    
    ### Set Image
    
    ```
    kubectl get deployment my-first-deployment -o yaml
    
    # Update Deployment - SHOULD WORK NOW
    kubectl set image deployment/<Deployment-Name> <Container-Name>=<Container-Image> --record=true
    kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true
    ```
    
    위 명령어를 통해 현재 배포된 deployment의 yaml파일을 얻을 수 있다.
    
    그리고 컨테이너 이미지를 1.0.0에서 2.0.0으로 업데이트 할 수 있다 명령어로
    
    ```bash
    # 이전
    
    - image: stacksimplify/kubenginx:1.0.0
    
    # 이후
    
    - image: stacksimplify/kubenginx:2.0.0
    ```
    
    컨테이너 이미지를 set image로 업데이트 했다.
    
    롤아웃 같은 것도 업데이트 할 수 있다.
    
    ```bash
    # Verify Rollout Status 
    kubectl rollout status deployment/my-first-deployment
    
    # Verify Deployment
    kubectl get deploy
    ```
    
    디플로이의 상태 설명을 통해 
    
    ```bash
    kubectl describe deployment my-first-deployment
    
    # StrategyType:           RollingUpdate
    ```
    
    디플로이가 롤링 업데이트를 수행하는 것을 알 수 있다. 롤링 업데이트는 애플리케이션의 다운타임을 최소화한다. Kubernetes는 기본적으로 롤링 업데이트 전략을 사용하지만, 특정 상황에서는 recreate 전략을 사용할 수 있다.
    
    - **Rolling Update**: 대부분의 애플리케이션에서 사용. 예를 들어, 웹 애플리케이션, 마이크로서비스 등 무중단 배포가 필요한 경우.
    - **Recreate**: 상태 비저장 애플리케이션, 초기 개발 단계, 또는 특정 애플리케이션이 모든 인스턴스를 동시에 업데이트해야 하는 경우.
    
    recreate 전략을 사용하면 모든 파드를 삭제한 다음 재생성하게 되서, 다운타임이 발생할 수 있어
    
    기본적으로는 rolling update를 사용하며, yaml 매니페스트 파일을 작성할 때 별도 정의할 필요가 없습니다.
    
    롤 아웃 히스토리도 볼 수 있다.
    
    ```bash
    kubectl rollout history deployment my-first-deployment
    
    REVISION  CHANGE-CAUSE
    1         <none>
    2         kubectl set image deployment/my-first-deployment kubenginx=stacksimplify/kubenginx:2.0.0 --record=true
    ```