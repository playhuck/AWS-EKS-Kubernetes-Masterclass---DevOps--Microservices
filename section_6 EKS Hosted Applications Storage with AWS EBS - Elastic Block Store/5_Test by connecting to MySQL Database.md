- **Test by connecting to MySQL Database**
    
    
    ```bash
    kubectl apply -f mysql_manifests/
    
    # List Storage Classes
    kubectl get sc
    
    # List PVC
    kubectl get pvc 
    
    # List PV
    kubectl get pv
    
    # List pods
    kubectl get pods 
    
    # List pods based on  label name
    kubectl get pods -l app=mysql
    ```
    
    이제 만든 메니페스트 파일들을 전부 다시 실행시키면
    
    ```bash
    kubectl get pvc 
    
    NAME                 STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
    ebs-mysql-pv-claim   Pending                                      ebs-sc         <unset>                 66s
    
    #이전에는 pending 이었던 상태가
    
     kubectl get pvc
    NAME                 STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
    ebs-mysql-pv-claim   Bound    pvc-b2811e0c-076d-47da-85c6-faa5ecd31ffb   4Gi        RWO            ebs-sc         <unset>                 26m
    
    # 바운드로 변경된 것을 알 수 있다.
    ```
    
    또 변경된게 있는데
    
    ```bash
    kubectl get pv
    
    NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                        STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
    pvc-b2811e0c-076d-47da-85c6-faa5ecd31ffb   4Gi        RWO            Delete           Bound    default/ebs-mysql-pv-claim   ebs-sc         <unset>                          2m24s
    ```
    
    이전에는 파드가 없어서 persistant volume이 없었는데 파드가 생성됨에 따라 같이 마운트된 모습이다.
    
    ### Connect to MYSQL Database
    
    ```bash
    # Connect to MYSQL Database
    kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -pdbpassword11
    
    [or]
    
    # Use mysql client latest tag
    kubectl run -it --rm --image=mysql:latest --restart=Never mysql-client -- mysql -h mysql -pdbpassword11
    
    # Verify usermgmt schema got created which we provided in ConfigMap
    mysql> show schemas;
    
    show schemas;
    +---------------------+
    | Database            |
    +---------------------+
    | information_schema  |
    | #mysql50#lost+found |
    | mysql               |
    | performance_schema  |
    | usermgmt            |
    +---------------------+
    ```
    
    configmap에서 설정한 쿼리로 실행된 usermgmt가 들어가 있다.