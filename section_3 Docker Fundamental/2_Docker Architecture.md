- Docker Daemon
    - Docker 호스트와 Docker 호스트안에 기본적으로 Docker 데몬을 설치할 것이다.
    - Docker api와 Docker 오브젝트 관리 네트워크 볼륨 컨테이너 Docker api 등을 관리할 것이다.
- Docker Client
    - Docker 클라이언트는 Docker 가 유저와 소통하는데 사용된다.
    - Docker run 같은 명령을 사용할 때 Docker 클라이언트가 Docker 데몬에 명령을 보낸다. Docker 클라이언트는 하나 이상의 데몬과 소통할 수 있다.
- Docker Image
    - 이미지는 리도커 컨테이너로부터 생성될 때 read-only template이다.
    - 이미지는 다른 이미지에 기반한다. 이미지를 만들 때 우분투 이미지를 기반으로 한다. 각각의 이미지를 사용해 나만의 이미지를 만드는 것이다.
- Docker Containers
    - Docker 컨테이너는 이미지를 운용가능한 인스턴스로 구성한다.
    - 우리는 컨테이너를 통해 Docker API나 CLI로 만들고 시작하고 중지시키고 삭제할 수 있다.

**Docker - Terminology**

1. docker pull images/app
2. docker 데몬이 Docker registry나 Docker ****hub를 통해 이미지를 pull 받아온다.
3. 받아온 이미지는 Docker 호스트에 위치하며, 해당 호스트 안에는 Docker 데몬과 Docker 컨테이너, Docker 이미지가 존재한다.
4. docker run -p 8080:80 -d images/app 명령어를 통해 받아온 이미지 중 특정 이미지를 실행시킨다.
5. 실행된 이미지는 Docker 컨테이너에서 실행된다.

```
docker run --name app1 -p 80:8080 -d stacksimplify/dockerintro-springboot-helloworld-rest-api:1.0.0-RELEASE

```

- —name은 실행시키려는 container의 이름
- -p (port) : 80:8080 , 처음 80은 로컬 포트 데스크톱에서 노출되며 8080이 커넽이너 포트가 된다.
- -d : 이미지 이름과 태그 값을 포함한 전체 이미지 이름을 의미한다.