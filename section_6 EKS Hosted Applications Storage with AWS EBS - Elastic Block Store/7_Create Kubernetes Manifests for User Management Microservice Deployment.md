- **Create Kubernetes Manifests for User Management Microservice Deployment**
    
    
    사용자가 생성한 mysql과 application을 연결할 것이다.
    
    app
    
    ```bash
    apiVersion: apps/v1
    kind: Deployment 
    metadata:
      name: usermgmt-microservice
      labels:
        app: usermgmt-restapp
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: usermgmt-restapp
      template:  
        metadata:
          labels: 
            app: usermgmt-restapp
        spec:
          containers:
            - name: usermgmt-restapp
              image: stacksimplify/kube-usermanagement-microservice:1.0.0
              ports: 
                - containerPort: 8095           
              **env:
                - name: DB_HOSTNAME
                  value: "mysql"            
                - name: DB_PORT
                  value: "3306"            
                - name: DB_NAME
                  value: "usermgmt"            
                - name: DB_USERNAME
                  value: "root"            
                - name: DB_PASSWORD
                  value: "dbpassword11"**            
    
    ```
    
    containers에 Env를 넣는다.
    
    service
    
    ```bash
    apiVersion: v1
    kind: Service
    metadata:
      name: usermgmt-restapp-service
      labels: 
        app: usermgmt-restapp
    spec:
      type: NodePort
      selector:
        app: usermgmt-restapp
      ports: 
        - port: 8095
          targetPort: 8095
          nodePort: 31231
    
    ```
    
    [http://PUBLICIP:NODEPORT/usermgmt/health-status](http://15.165.59.229:31231/usermgmt/health-status)
    
    입력하면
    
    User Management Service UP and RUNNING - V1이 나온다.