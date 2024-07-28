이번 강의에서는 MySQL 관련 manifest파일을 작성하고 이를 배포한 후 aws의 ebs 서비스를 사용해서

영구 스토리지와 함께 mySQL이 어떻게 작동하는지 살펴볼 것 이다.

각 파일은 아래와 같다.

| Storage Class | 01-storage-class.yml |
| --- | --- |
| Persistent Volume Claim | 02-persistent-volume-claim.yml |
| Config Map | 03-UserManagement-ConfigMap.yml |
| Deployment, Environment Variables, Volumes, VolumeMounts | 04-mysql-deployment.yml |
| ClusterIP Service | 05-mysql-clusterip-service.yml |

### Storage Class

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata: 
  name: ebs-sc
provisioner: ebs.csi.aws.com
volumeBindingMode: WaitForFirstConsumer 
```

- 스토리지 클래스는 storage.k8s.io로 시작한다.
- spec 키 대신, provisioner를 사용한다.
- 또 중요한 것은 volumeBindingMode이다.
    - `volumeBindingMode`는 Kubernetes에서 `PersistentVolume`이 `PersistentVolumeClaim`에 바인딩될 시점을 제어하는 속성이다.
    - `volumeBindingMode`에는 두 가지 옵션이 있다.
        - **Immediate**: `PersistentVolumeClaim`이 생성되면 즉시 `PersistentVolume`에 바인딩된다. 이 모드는 클러스터 전체에서 사용 가능한 모든 `PersistentVolume`을 고려하며, 특정 노드에 대한 제약이 없다.
        - **WaitForFirstConsumer**: `PersistentVolumeClaim`이 특정 파드에 연결되기 전까지 `PersistentVolume`에 바인딩되지 않는다. 이는 스케줄러가 파드를 특정 노드에 할당할 때까지 기다렸다가 그 노드에 접근 가능한 `PersistentVolume`을 바인딩한다. 이 모드는 클러스터가 멀티 존으로 구성된 경우에 유용하며, 데이터 로컬리티와 자원 최적화를 도와준다.
    - WaitForFirstConsumer 모드는 PersistentVolumeClaim를 사용하는 Pod가 생성될 때까지 PersistentVolume의 바인딩 및 프로비저닝을 지연시킨다.
    - 스토리지 클래스를 생성하고 퍼시스턴트 볼륨 클레임을 생성할 때 WaitForFirstConsumer모드를 선택하지 않는다면, 볼륨이 즉시 동적으로 프로비저닝되고 마운트된다. 어느 가용 영역에서 프로비저닝되는지 알 수 없다. 동적이기 때문이다. 나중에 파드를 생성할 때 볼륨이 다른 가용 영역에 마운트되면 문제가 발생한다. 따라서 파드가 생성될 때 볼륨이 동일한 가용 영역에서 프로비저닝되도록 만들어 준다. 이는 중요한 문제다.

### Persistant Volume Claim

```bash
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ebs-mysql-pv-claim
spec: 
  accessModes:
    - ReadWriteOnce
  storageClassName: ebs-sc
  resources: 
    requests:
      storage: 4Gi
```

- storageClassName: ebs-sc 으로 매칭 시킨다.
- 저장소 크기는 4Gi다.

### ConfigMap

```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: usermanagement-dbcreation-script
data: 
  mysql_usermgmt.sql: |-
    DROP DATABASE IF EXISTS usermgmt;
    CREATE DATABASE usermgmt; 
```

ConfigMap은 필요한 구성을 파드에 전달하는 데 사용된다.

이 ConfigMap 파일을 통해 배포 파일에 마운트할 수 있게 됐다.

### 실행

이제 만든 메니페스트 파일들을 실행시키면

```bash
# Create Storage Class & PVC
kubectl apply -f kube-manifests/

[storageclass.storage.k8s.io/ebs-sc](http://storageclass.storage.k8s.io/ebs-sc) created
persistentvolumeclaim/ebs-mysql-pv-claim created
configmap/usermanagement-dbcreation-script create

# List Storage Classes
kubectl get sc

# List PVC
kubectl get pvc 

# List PV
kubectl get pv
```

pv는 아직 생성되지 않았다.

파드가 생성될 때까지 볼륨이 바인딩되지 않는다.