## Pods are Highly Dynamic
- 복제 컨트롤러를 사용하는 경우 스케일링 작업 중에 파드가 종료되고 생성됨
- 예를 들어 배포를 사용할 때 이미지 버전을 업데이트하면 파드가 종료되고 새 파드가 이전 파드를 대신함
- 그렇기에 파드에 직접 접근해서는 안 되고 항상 서비스를 통해 접근해야 함

## Role of a Service
- 서비스는 "언젠가는 죽는" 파드와 다른 서비스 또는 최종 사용자 간의 논리적 다리임

## Creating a Service
- `kubectl expose` 명령을 사용해 외부에서 접근할 수 있도록 파드에 관한 새 서비스를 생성할 수 있음
- 새 서비스를 생성하면 파드의 엔드포인트가 생성됨

## Types of Services
### ClusterIP
- 클러스터 내에서만 연결할 수 있는 가상 IP임
- 기본값으로 설정됨

### NodePort
- 외부에서도 연결할 수 있는 각 노드에서 동일한 포트임
- 외부에서 파드에 도달하려면 필요함

### Load Balancer
- 클라우드의 프로덕션 애플리케이션에 사용됨
- 외부 트래픽을 노드포트의 모든 노드로 라우팅하는 클라우드 공급자에 의해 생성됨

## DNS Names
- DNS 이름을 사용할 수도 있음
- 서버 정의에서 외부 이름을 사용하는 경우 서비스에 관한 DNS 이름을 제공할 수 있음
- DNS를 사용한 서비스 탐색의 경우 DNS 추가 기능이 활성화된 경우에만 작동함

## Service Definition Example
```yaml
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  type: NodePort
  ports:
    - port: 31001
      targetPort: nodejs-port
      protocol: TCP
  selector:
    app: helloworld
```
- `app: helloworld`라는 레이블이 있는 파드에 관한 서비스임
- 포트가 지정되지 않으면 임의의 포트가 사용됨
- 지정한 포트가 이미 사용 중이면 서비스 생성이 불가능함

## Changing Port Ranges
- 기본적으로 서비스는 포트 30000과 32767 사이에서만 실행할 수 있음
- `--service-node-port-range` 인수를 `kube-apiserver` 끝에 넣어 동작을 변경할 수 있음
- `kube-apiserver`는 마스터 노드의 초기화 스크립트에서 시작하는 프로세스임

## Managing Port Conflicts
- 노드에서 이미 사용 중인 포트와 충돌이 없는지 확인해야 함

