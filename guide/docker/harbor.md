# Harbor

도커 이미지를 관리하는 private repository를 구성하기 위해 harbor를 알아보았다.

harbor를 처음 접하게 된 것은 docker registry를 알아보던 중 라인 블로그를 통해 도입기를 보고 알게 되었다.

또한 harbor를 설치하기 위해 사용한 쿠버네티스 패키지 관리도구인 helm의 chart repository로도 사용할 수 있다고 한다.

## Prerequisites

- Kubernetes cluster 1.10+
- Helm 2.8.0+
- High available ingress controller (Harbor does not manage the external endpoint)
- High available PostgreSQL database (Harbor does not handle the deployment of HA of database)
- High available Redis (Harbor does not handle the deployment of HA of Redis)
- PVC that can be shared across nodes or external object storage

## Local Cluster

개발단계에서 kubernetes cluster를 사용하는 방법은 여러가지가 있다. (minikube, kind, k3d, ...)

### Docker for Desktop

Mac OSX에서는 `Docker for Desktop`에서 kubernetes를 바로 지원한다.

`Preferences > Kubernetes > Enable Kubernetes`를 체크하여 적용한다.

## Ingress Controller

### ~~Contour~~

[Contour](https://projectcontour.io/)는 [Envoy](https://www.envoyproxy.io/) 기반 인그레스 컨트롤러로 VMware에서 제공하고 지원한다.

contour 배포.

```bash
$ kubectl apply -f https://projectcontour.io/quickstart/contour.yaml
```

패치.

```bash
$ kubectl patch daemonsets -n projectcontour envoy -p '{"spec":{"template":{"spec":{"nodeSelector":{"ingress-ready":"true"},"tolerations":[{"key":"node-role.kubernetes.io/master","operator":"Equal","effect":"NoSchedule"}]}}}}'
```

[Kind와 Contour를 조합한 글](https://serverascode.com/2020/04/28/local-harbor-install.html)을 참조하여 contour를 설치하려 했으나 위와 같이 패치를 진행하면 데몬셋이 뻗어버리고 envoy pod가 종료되는 문제가 발생한다.

또한 패치를 진행하지 않고도 접속은 되지만 로그인(admin/Harbor12345)이 되지 않는 문제가 있다.

### Nginx Ingress

[Docker for Mac](https://kubernetes.github.io/ingress-nginx/deploy/#docker-for-mac) 설치 가이드를 참고하여 설치하였다. (과거 버전에 대한 가이드지만 일단 성공했다.)

```bash
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.40.2/deploy/static/provider/cloud/deploy.yaml
```

인그레스가 처음 시작되면 두개의 작업이 승인 웹훅에서 사용하는 SSL인증서를 생성한다.

이러한 이유로 인그레스 정의를 만들고 유효성을 검사할 수 있을 때까지 최대 2분의 초기 지연이 있을 수 있다~~고 한다~~.

여기서 두개의 작업이란?

```bash
$ kubectl get jobs -n ingress-nginx
NAME                             COMPLETIONS   DURATION   AGE
ingress-nginx-admission-create   1/1           3s         5h34m
ingress-nginx-admission-patch    1/1           4s         5h34m
```

아래와 같이 실행해서 2분을 기다려주도록 한다.

```bash
$ kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=120s
```



## Helm 설치

[설치 가이드](https://helm.sh/ko/docs/intro/install/) 를 참고하여 homebrew를 이용해 뚝딱 설치했다.

```bash
$ brew install helm
```



## Harbor 설치

harbor repository를 설치하고 업데이트 한다.

```bash
$ helm repo add harbor https://helm.goharbor.io
$ helm repo update
```

설치 가능한 차트를 확인해볼까

```bash
$ helm search repo harbor
NAME         	CHART VERSION	APP VERSION	DESCRIPTION
harbor/harbor	1.5.0        	2.1.0      	An open source trusted cloud native registry th...
```

harbor를 설치한다.

 `--generate-name` 은 차트이름에 랜덤한 suffix를 붙여서 생성해준다.

```bash
$ helm install harbor/harbor -n harbor --generate-name
NAME: harbor-1602418426
LAST DEPLOYED: Sun Oct 11 21:13:47 2020
NAMESPACE: harbor
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Please wait for several minutes for Harbor deployment to complete.
Then you should be able to visit the Harbor portal at https://core.harbor.domain
For more details, please visit https://github.com/goharbor/harbor
```

만약 harbor 속성을 변경하여 설치하고자 하는 경우 아래와 같이 `values.yaml` 파일을 다운로드 하여 수정한 후,

```bash
$ wget https://raw.githubusercontent.com/goharbor/harbor-helm/master/values.yaml
$ vim values.yaml
```

다음과 같이 파일을 지정하여 설치할 수 있다.

```bash
$ helm install harbor/harbor -f values.yaml -n harbor --generate-name
```

`https://core.harbor.domain`에 접속하여 로그인으로 확인한다.

- ID: admin
- PASSWORD: Harbor12345

## References

- [Deploying Harbor with High Availability via Helm](https://goharbor.io/docs/2.1.0/install-config/harbor-ha-helm/)
- [Install Harbor Image Registry on Kubernetes / OpenShift with Helm Chart](https://computingforgeeks.com/install-harbor-image-registry-on-kubernetes-openshift-with-helm-chart/)
- [Install the Harbor Container Image Registry Locally](https://serverascode.com/2020/04/28/local-harbor-install.html)
- [Choosing a Local Dev Cluster](https://docs.tilt.dev/choosing_clusters.html)
- [Kubernetes Ingress Controller Overview](https://medium.com/swlh/kubernetes-ingress-controller-overview-81abbaca19ec)
- [Ingress Controllers (국문)](https://kubernetes.io/ko/docs/concepts/services-networking/ingress-controllers/)