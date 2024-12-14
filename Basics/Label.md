## Overview
- 레이블은 쿠버네티스 객체에 연결할 수 있는 키-값 쌍임
- AWS와 같은 클라우드 공급자의 태그와 유사하며 리소스에 태그를 지정하는 데 사용됨
- 쿠버네티스 리소스를 조직하고 분류하는 데 유용함
- 레이블은 고유하지 않으며 하나의 객체에 여러 레이블을 추가할 수 있음

## Common Use Cases
- **Environment Tagging**
  -  레이블 `environment: dev`, `environment: staging`, `environment: qa`, `environment: prod`를 사용하여 객체가 속한 환경을 식별할 수 있음
- **Department Tagging**
  -  `department: engineering`, `department: finance`, `department: marketing`와 같은 레이블을 사용하여 리소스가 관련된 부서를 나타낼 수 있음
- **Application Tagging**
  -  `app: helloworld`와 같은 레이블을 사용하여 리소스가 속한 애플리케이션을 명시할 수 있음

## Label Selectors
- 레이블을 기반으로 리소스를 필터링하는 데 사용됨
- 결과 범위를 좁히기 위해 일치 표현식을 지원함
  - 예시: 특정 파드는 레이블 `environment: dev`가 지정된 노드에서만 실행될 수 있음
  - 고급 매칭: `environment in (dev, qa)`와 같은 조건을 설정할 수 있음

## Node Labeling
- 노드에도 레이블을 지정하여 파드 스케줄링을 제어할 수 있음
- 단계:
  1. `kubectl label node`를 사용하여 노드에 레이블 추가
     - 예시: `kubectl label node <node-name> hardware=high-spec`
  2. 파드 사양에서 `nodeSelector`를 사용하여 파드가 일치하는 레이블이 있는 노드에서만 실행되도록 설정
     - 예시:
       ```yaml
       nodeSelector:
         hardware: high-spec
       ```

## Practical Examples
1. 노드에 레이블 추가
   ```bash
   kubectl label node <node-name> key=value
   ```
2. nodeSelector를 사용하는 파드 정의
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: example-pod
   spec:
     containers:
     - name: example-container
       image: nginx
     nodeSelector:
       hardware: high-spec
   ```

