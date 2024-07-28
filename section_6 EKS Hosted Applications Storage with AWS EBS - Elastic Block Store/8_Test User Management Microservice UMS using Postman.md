- **Test User Management Microservice UMS using Postman**
    
    
    Postman 요청을 통해서도 테스트할 수 있다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f02ca0c9-9279-4cd3-bf61-49a4adc12473/74770b85-453f-4029-9277-03155cbc0cc7/Untitled.png)
    
    ```
     {
            "username": "admin1",
            "email": "dkalyanreddy@gmail.com",
            "role": "ROLE_ADMIN",
            "enabled": true,
            "firstname": "fname1",
            "lastname": "lname1",
            "password": "Pass@123"
        }
    ```
    
    이렇게 요청 POST로 보내면 에러가
    
    ```bash
    {
        "apierror": {
            "status": "INTERNAL_SERVER_ERROR",
            "timestamp": "28-07-2024 05:02:18",
            "message": "Something went wrong, Please try again later! If the problem persists, please contact stacksimplify support.",
            "debugMessage": "I/O error on POST request for \"http://localhost:8096/notification/send\": Connection refused (Connection refused); nested exception is java.net.ConnectException: Connection refused (Connection refused)",
            "subErrors": null
        }
    }
    ```
    
    발생하게되는데,
    
    일부 오류 메시지가 나타날 수 있지만, 이는 알림 마이크로서비스가 실행 중이지 않기 때문이다. 사용자 생성은 성공하며, 사용자 목록 서비스를 통해 이를 확인할 수 있다.
    
    ```bash
    elect * from users
        -> ;
    +----------+------------------------+---------+-----------+----------+--------------------------------------------------------------+------------+
    | username | email                  | enabled | firstname | lastname | password                                                     | role       |
    +----------+------------------------+---------+-----------+----------+--------------------------------------------------------------+------------+
    | admin1   | dkalyanreddy@gmail.com |        | fname1    | lname1   | $2a$04$yiitb2EjpO69AJWqIMHpbu7y49Ndr4d8Y46TFwMZHt90Z9DODZdMC | ROLE_ADMIN |
    +----------+------------------------+---------+-----------+----------+--------------------------------------------------------------+------------+
    ```
    
    데이터베이스에 접속해서 보면 실제로 생성된 것을 알 수 있다.