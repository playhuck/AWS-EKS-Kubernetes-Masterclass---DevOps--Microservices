강의 Docker 이미지 정보는 github docker registry에서 확인할 수 있는데,

docker 이미지 다운로드 문제를 겪는다면 Kubernetes 배포 또는 Pod YAML 매니페스트에서 Docker 이미지 이름 앞에 "[ghcr.io/](http://ghcr.io/)"를 접두어로 추가하면 GitHub Container Registry에서 Docker 이미지를 다운로드할 수 있다.

추가로, GitHub Container Registry에 호스팅된 모든 Docker 이미지를 참조하려면 Packages 섹션으로 이동하면 된다.

또한, Docker Hub와 GitHub Container Registry에 호스팅된 Docker 이미지 이름의 목록을 확인하려면 GitHub 리포지토리 `docker-hub-to-github-container-registry`를 방문하여 테이블을 검토할 수 있습다.