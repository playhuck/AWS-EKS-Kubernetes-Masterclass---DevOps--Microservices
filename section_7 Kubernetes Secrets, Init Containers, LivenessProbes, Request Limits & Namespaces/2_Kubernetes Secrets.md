- **Kubernetes Secrets**
    
    
    ### Kubernetes Secrets
    
    쿠버네티스 시크릿은 환경 변수나 민감한 정보를 Base64 인코딩 형식으로 저장하고 관리할 수 있게 해준다. 기밀 정보를 애플리케이션 매니페스트, 배포 매니페스트 또는 데이터베이스 관련 배포 매니페스트에 직접 넣는 대신 시크릿에 저장하는 것이 더 안전하고 유연한 방법이다.
    
    ```bash
    # Mac
    echo -n 'dbpassword11' | base64
    # ZGJwYXNzd29yZDEx
    
    # URL: https://www.base64encode.org
    ```
    
    Kubuernetes Secrets의 기본 템플릿은 apiVersion, kind, metadata, spec으로 구성된다. kind는 "secret"이고 apiVersion은 "core/v1"입니다. metadata에는 이름과 레이블이 들어갑니다. 여기서는 MySQL DB 비밀번호를 "MySQL DB password"라는 이름으로 설정한다.
    
    spec 대신 data를 사용하며, data는 키-값 쌍으로 구성된다.  유형(type)은 Opaque로 설정하여 비구조적 콘텐츠임을 나타낸다.
    
    ```bash
    apiVersion: v1
    kind: Secret
    metadata:
      name: mysql-db-password
    data:
      db-password: ZGJwYXNzd29yZDEx
      db-username: 
      db-host: 
    type: Opaque
    ```
    
    위 처럼 Secret.yml을 작성 했다면 mysql deployment.yml로 가서
    
    ```bash
    # before
    env:
    	- name: MYSQL_ROOT_PASSWORD
    		valueFrom:
    			secretKeyRef:
    				name: mysql-db-password
    				key: db-password 
    
    # after
    env:
    	- name: MYSQL_ROOT_PASSWORD
    		valueFrom:
    			secretKeyRef:
    				secretKeyRef:
    					# secret의 metadata:
    					#						name: mysql-db-password
    					name: mysql-db-password
    					# secret의 data:
    					#						db-password: mysql-db-password
    					key: db-password
    ```
    
    spec 아래의 env를 변경해준다.
    
    그리고 mysql deployment를 변경해 주었다면, UserManagementMicroservice-Deployment-Service.yml 파일로 가서
    
    ```yaml
    # before
    - name: DB_PASSWORD
    	value: ...
    
    # after
    - name: DB_PASSWORD
    	valueFrom:
    		secretKeyRef:
    			name: mysql-db-password
    			key: db-password
    ```
    
    위와 같은 형태로 반영하여 환경변수를 관리해준다.
    
    ```yaml
    # Create All Objects
    kubectl apply -f kube-manifests/
    
    # List Pods
    kubectl get pods
    
    # Access Application Health Status Page
    http://<WorkerNode-Public-IP>:31231/usermgmt/health-status
    ```
    
    그리고 명령어를 사용하여 작성한 환경변수를 반영한다.
    
    여기서 중요한게, 실습 중이라면 클러스터를 지웠다 깔았다 하면서 작업을 할텐데 다시 설치했을 때 CSI driver 설정이 초기화된 상태라 Section6에서 수행했던 CSI install 파트를 다시 봐야 mysql pod가 정상적으로 생성된다.