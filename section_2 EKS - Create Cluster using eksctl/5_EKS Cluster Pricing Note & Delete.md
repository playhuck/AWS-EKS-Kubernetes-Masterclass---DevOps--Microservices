EKS는 비싸다.

그리고 무료도 아니다.

```
- We pay $0.10 per hour for each Amazon EKS cluster
- Per Day: $2.4
- For 30 days: $72
```

- 시간당 0.1 달러
- 하루에 2.4달러
- 한달에 72달러

가히 살인적인 가격이다.

이제 클러스터를 삭제해보자,

cli를 사용하지 않고 손으로 지우면 보통 문제가 생긴다. iam 정책 연결 후 cli로 삭제하자

```bash
eksctl delete cluster name
```

cluster 이름을 입력하면

- 노드그룹 삭제
- 클러스터 삭제