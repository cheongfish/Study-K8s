# Kubernetes Health Checks

## Introduction

쿠버네티스에서 헬스 체크는 애플리케이션이 오작동하는 경우를 대비하기 위한 필수 구성 요소임

---

## Types of Health Checks

쿠버네티스에서는 두 가지 주요 유형의 헬스 체크를 실행할 수 있음

### 1. Command-Based Check

컨테이너에서 주기적으로 명령을 실행하여 컨테이너가 여전히 작동하는지 확인함 이 방식은 컨테이너 내부의 상태를 점검하는 데 유용함

### 2. HTTP-Based Check

가장 많이 사용되는 방식으로, HTTP를 사용해 특정 URL을 주기적으로 확인함 애플리케이션은 인터페이스에 관한 주기적 검사를 사용하여 상태를 노출함 이를 통해 애플리케이션이 여전히 정상적으로 작동하는지 확인할 수 있음

---

## Best Practices for Health Checks

- **Production Readiness:** 로드 밸런서 뒤에 있는 일반적인 프로덕션 애플리케이션은 애플리케이션의 가용성과 복원력을 보장하기 위해 반드시 헬스 체크를 구현해야 함
- **AWS Integration:** AWS를 사용 중이고 로드 밸런서 헬스 체크에서 파드의 정상 여부를 확인하지 않더라도, 노드 포트에 접근할 수 있도록 헬스 체크를 구성해야 함

---

## Example Configuration

아래는 컨테이너에서 헬스 체크를 구성한 예제임

### Liveness Probe Example

```yaml
livenessProbe:
  httpGet:
    path: /
    port: 3000
  initialDelaySeconds: 15
  timeoutSeconds: 30
```

- ``**:** 메인 웹사이트 경로(`/`)를 사용하여 상태를 점검함 프로덕션 애플리케이션에서는 `/health`와 같은 별도의 상태 페이지를 사용하는 것이 일반적임
- **Port Configuration:** 컨테이너의 포트를 지정하거나 이름을 사용할 수 있음
- **Initial Delay:** 초기 지연 시간(`initialDelaySeconds`)을 15초로 설정하여 컨테이너 시작 이후 안정적인 상태를 확인함
- **Timeout:** 타임아웃 시간(`timeoutSeconds`)은 30초로 설정되어 요청 응답을 기다림

---

## Conclusion

* 헬스 체크는 애플리케이션의 안정성과 가용성을 보장하는 핵심적인 구성 요소
* HTTP 또는 명령 기반의 헬스 체크를 통해 컨테이너와 애플리케이션의 상태를 지속적으로 모니터링할 수 있음,
*  필요할 경우 자동으로 복구하거나 문제를 해결할 수 있는 기반을 제공함

