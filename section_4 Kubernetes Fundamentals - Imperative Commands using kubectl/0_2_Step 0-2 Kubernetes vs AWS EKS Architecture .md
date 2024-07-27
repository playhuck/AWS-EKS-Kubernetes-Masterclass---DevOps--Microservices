이제 쿠버네티스의 구성요소들에 대해서 살펴보면,

### Pod

pod는 Application의 싱글 인스턴스이다. 쿠버네티스에서 만들 수 있는 가장 작은 단위의 객체다.

### ReplicaSet

replicaSet은 지정된 수의 파드가 항상 작동하도록 가용성을 보장한다.

지정된 특정한 파드의 숫자를 계속해서 이용할 수 있도록 가용성을 보장하는 역할을 한다.

### Deployment

Deployment 파트는 Application의 여러 Replica를 실행하며 실패하거나 응답하지 않는 인스턴스를 자동으로 교체한다. Deployement를 생성할 때 Application 변경 사항을 롤아웃하고 롤백하는 추가 기능도 제공 된다. Deployment는 Stateless application에 적합하다.

### Service

서비스는 각 파드들을 위한 추상화된 요소들을 제공한다.

네트워크 주소를 제공하며, 파드 앞에 위치하여 로드 밸런서를 통해 접근할 수 있게 하고 트래픽을 로드밸런싱 한다. 

---

## Kubernetes Fundamentals

Kubernetes Fundamentals에선 2가지 접근 방식이 있다.

### 명령형(Imperative)

명령형에서는 kubectl을 사용하여 pod / replicaSet / Deployment / Service를 생성할 것이다.

replicaSet의 경우 직접 생성하는 kubectl이 없기 때문에 yaml파일로 생성한다.

### 선언형(Declarative)

선언형에서는 yaml과 kubectl을 통해 pod / replicaSet / Deployment / Service를 생성할 것이다.

yaml을 사용하여 manifest를 작성하고, kubectl을 사용하여 원하는 모든 명령어를 실행할 수 있으며 yaml을 사용하여 작성된 manifest로 이를 통해 직접 구현할 수 있다.