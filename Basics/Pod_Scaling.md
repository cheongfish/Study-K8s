# 1 Stateless Application
## Overview
- **Stateless**는 애플리케이션이 **상태를 가지지 않는 것**을 의미함
- 로컬 파일이나 로컬 세션 데이터를 저장하지 않음
- 여러 파드가 존재할 때, 각각의 파드가 동일한 결과를 반환하면 스테이트리스 애플리케이션이라 할 수 있음
- 예시: 데이터베이스와 같은 상태를 가지는 애플리케이션은 스테이트풀임
  - **MySQL**이나 **PostgreSQL**은 스테이트풀 데이터베이스로, 여러 인스턴스로 나누는 것이 어렵다고 할 수 있음
- 스테이트리스 애플리케이션은 세션 관리를 컨테이너 외부에서 처리함
  - **Memcached**, **Redis**, **S3**와 같은 외부 서비스를 사용해 데이터를 저장해야 함

## Features
- 여러 인스턴스가 있어도 상태에 영향을 받지 않음
- 컨테이너 로컬에 파일을 저장하지 않음
- 외부 서비스 또는 공유 스토리지를 활용해 데이터를 관리함
- 예시: **Hello World** 애플리케이션은 상태가 없으며, 동일한 출력을 제공함

## Stateful Aplication
- 상태(`state`)를 가지므로 수평 확장이 어렵다고 할 수 있음
- 단일 컨테이너에서 실행하며 수직 확장을 통해 처리 성능을 개선함
- 공유 볼륨을 활용하여 상태를 관리함

---

# Scaling

## 수평 스케일링(Horizontal Scaling)
- `Scale-Out`
- 더 많은 파드를 추가해 CPU, 메모리 등의 리소스를 확장함
- **복제 컨트롤러**를 사용해 지정된 수의 파드 복제본을 유지함
  - 예를 들어, 5개의 복제본을 지정하면 동일한 파드가 5개 실행됨
  - 파드가 실패하거나 삭제되면 쿠버네티스가 자동으로 복구함
- **복제 컨트롤러의 이점**:
  - 노드가 충돌해도 동일한 파드를 다른 노드에서 실행함
  - 단일 복제본을 사용해도 항상 실행 상태를 보장함

## 2 복제 컨트롤러의 구성
복제 컨트롤러를 사용한 YAML 파일의 주요 변경 사항:
- `{kind: Pod}`에서 `{kind: ReplicationController}`로 변경함
- `사양(spec)`에 복제본(replicas) 수를 정의함
- 템플릿(template)을 사용해 파드 정의를 포함함

**YAML 예시**:
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: helloworld-controller
spec:
  replicas: 2
  selector:
    app: helloworld
  template:
    metadata:
      labels:
        app: helloworld
    spec:
      containers:
      - name: k8s-demo
        image: example-image:latest
        ports:
        - containerPort: 3000
```
- **replicas**: 복제본 수를 정의함
- **selector**: 복제 컨트롤러가 관리할 파드를 선택함
- **template**: 파드 정의를 포함함

## 수직 스케일링(Vertical Scaling)
* `Scale Up`
* 개별 파드에 더 많은 리소스를 할당함
* 노드와 리소스 크기에 의존해 확장함
* 사양확장에 한계가 있음
