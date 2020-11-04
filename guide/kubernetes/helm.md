# Helm

[공식문서](https://helm.sh/ko/docs/)가 한글화가 잘 되어있다.

## Installation

### Homebrew로 설치 (MacOS)

```bash
$ brew install helm
...
$ helm version
version.BuildInfo{Version:"v3.3.4", GitCommit:"a61ce5633af99708171414353ed49547cf05013d", GitTreeState:"dirty", GoVersion:"go1.15.2"}
```

## Helm 차트 리포지토리 초기화

helm 3부터는 기본 차트 리포지토리가 등록되지 않는다.

helm 공식 차트 리포지토리를 등록한다.

```bash
$ helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

리포지토리를 등록하면 설치 가능한 차트 목록을 볼 수 있다.

```bash
$ helm search repo stable
NAME                                    CHART VERSION   APP VERSION                     DESCRIPTION
stable/acs-engine-autoscaler            2.2.2           2.1.1                           DEPRECATED Scales worker nodes within agent pools
stable/aerospike                        0.2.8           v4.5.0.5                        A Helm chart for Aerospike in Kubernetes
stable/airflow                          4.1.0           1.10.4                          Airflow is a platform to programmatically autho...
stable/ambassador                       4.1.0           0.81.0                          A Helm chart for Datawire Ambassador
# ... and many more
```

