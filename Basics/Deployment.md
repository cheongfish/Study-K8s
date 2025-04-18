## 레플리카 세트 (Replica Set)
- 레플리카 세트는 차세대 복제 컨트롤러임
- 복제 컨트롤러의 최신 버전으로, 값의 세트에 따라 필터링 기반의 선택을 지원함
  - 예: 선택은 `{dev}` 또는 `{qa}`와 같은 환경으로 가능함
- 복제 컨트롤러와는 달리 동일성에만 기반하지 않음
  - 복제 컨트롤러에서는 `{environment == dev}`만 가능하고 더 복잡한 작업은 수행 불가함
- 레플리카 세트를 사용하면 더 복잡한 매칭이 가능하며, 복제 컨트롤러가 아닌 레플리카 세트가 배포 객체에 사용됨

## 배포 (`Deployment`)
- 배포는 배포 및 업데이트를 선언적으로 수행할 수 있는 Kubernetes의 객체임
- 배포 객체를 사용하면 애플리케이션의 상태를 정의하고, Kubernetes는 클러스터가 원하는 상태와 일치하는지 확인함
  - 예: 애플리케이션이 실행 가능하며, 다섯 번 복제되었는지 확인
- 복제 컨트롤러나 레플리카 세트만 사용하면 앱 배포가 번거로울 수 있음
  - 업데이트 및 롤백 등을 수행하기에 작업이 많음
- 배포 객체는 사용하기 쉽고 더 많은 가능성을 제공함

## 배포 객체로 할 수 있는 작업
1 앱 배포
2 배포 업데이트
3 앱의 새 버전 배포
4 가동 중단 없이 배포되는 롤링 업데이트
5 이전 버전으로의 롤백
6 배포 일시 중지 및 재개
7 실행 중인 파드의 특정 비율만 롤아웃

### Example
- 사양 예시:
  - API 버전: `v1`
  - 메타데이터(`metadata`):
    - 이름: `helloworld-deployment`
  - 사양(`spec`)
    - 복제본: 3개
    - 템플릿:
      - 메타데이터:
        - 레이블: `{app: helloworld}`
      - 파드 사양:
        - 컨테이너:
          - 이름: `k8s-demo`
          - 이미지: 동일한 이미지
          - 포트: 동일한 포트
- 배포 관련 명령어:
  - `kubectl get deployments`: 현재 배포 정보 확인
  - `kubectl get rs`: 레플리카 세트 정보 확인 (`rs`는 Replica Set의 약자)
  - `kubectl get pods --show-labels`: 실행 중인 파드와 레이블 확인
  - `kubectl rollout status 배포 유형/배포 객체의 이름`: 배포 상태 확인

### 배포 업데이트 및 롤백
- 이미지 업데이트:
  - `kubectl set image deployment/helloworld-deployment`: 이미지 버전 업데이트
    - 예: `k8s-demo`에서 `k8s-demo:2`로 변경
  - 원하는 이미지 버전에 태그 지정 가능 버전을 사용하지 않을 경우 기본적으로 `latest` 태그 사용
- 배포 객체 편집:
  - `kubectl edit`으로 배포 객체 수정 가능
- 롤아웃 상태 확인:
  - `kubectl rollout status`: 롤아웃 상태 확인
- 배포 히스토리:
  - `kubectl rollout history`: 배포된 버전 히스토리 확인
- 롤백:
  - `kubectl rollout undo`: 이전 버전으로 롤백
  - 특정 버전으로 롤백하려면:
    - `kubectl rollout undo --to-revision=n` (수정 번호 지정)

