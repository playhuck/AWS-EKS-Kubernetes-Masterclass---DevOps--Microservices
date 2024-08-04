- **Create Kubernetes ExternalName Service & Other Manifests, Deploy & Test**
    
    
    1. **RDS 보안 그룹 확인**:
        - RDS 데이터베이스의 보안 그룹에서 모든 네트워크에서의 접근이 허용되어 있는지 확인했습니다. 포트 3306이 열려 있어야 합니다.
    2. **Kubernetes 외부 이름 서비스 매니페스트 생성 및 배포**:
        - `external name` 타입의 서비스를 생성하여 RDS 데이터베이스에 연결할 수 있는 매니페스트를 작성했습니다.
        - 매니페스트의 외부 이름 값은 RDS 데이터베이스 엔드포인트에서 가져왔습니다.
        - 다음 명령어로 배포했습니다:
            
            ```kotlin
            kubectl apply -f kube-manifests/S01-mySQL-external-name-service.yaml
            ```
            
        - 서비스 생성 확인 명령어:
            
            ```bash
            kubectl get svc
            
            kubectl run -it --rm --image=mysql:latest --restart=Never mysql-client -- mysql -h {rds endpoint} -u dbadmin -pdbpassword11
            
            mysql> show schemas;
            mysql> create database usermgmt;
            mysql> show schemas;
            mysql> exit
            
            ```
            
    3. **MySQL 클라이언트로 데이터베이스 연결**:
        - `kubectl` 클라이언트를 사용하여 MySQL 데이터베이스에 연결하고, 사용자 MGMT 스키마를 생성했습니다.
        - 스키마 확인 명령어:
            
            ```sql
            SHOW SCHEMAS;
            ```
            
    4. **배포 파일 업데이트**:
        - 사용자 이름을 'root'에서 'DB admin'으로 변경하고, 비밀번호를 'DB password 11'로 설정했습니다.
        - Kubernetes 비밀에 대한 비밀번호는 변경하지 않았습니다.
    5. **상태 확인 및 테스트**:
        - 다음 명령어로 포드와 노드 상태를 확인했습니다:
            
            ```bash
            kubectl get pods
            kubectl get nodes -o wide
            ```
            
        - 퍼블릭 IP를 확인하여 웹 브라우저에서 서비스가 정상적으로 실행되고 있는지 검토했습니다.
    6. **리소스 정리**:
        - RDS 데이터베이스와 관련된 리소스를 삭제하고 청소했습니다.
        - 필요하지 않거나 섹션이 끝난 후에는 RDS 데이터베이스를 중지하고 백업 옵션을 비활성화하여 비용을 절감할 수 있습니다.