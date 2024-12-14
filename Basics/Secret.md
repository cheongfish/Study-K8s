## Overview
쿠버네티스에서 시크릿은 자격 증명, 키, 암호 또는 시크릿 데이터를 파드에 배포하는 방법을 제공함으로 쿠버네티스 자체도 내부 API에 액세스하기 위한 자격 증명을 제공함

## Access Mechanism
시크릿은 파드에서 액세스 가능하며 이 시크릿은 해당 메커니즘을 사용하여 배포되어야 함 동일한 메커니즘을 통해 애플리케이션에서도 시크릿을 제공할 수 있음

## Secret Types
- 시크릿은 비밀번호, 데이터베이스 연결 정보 등 비밀로 유지해야 할 다양한 데이터를 포함할 수 있음
- 시크릿은 쿠버네티스 네이티브 메커니즘을 통해 제공됨
- 외부 볼트 서비스와 같은 대안도 존재함

## Usage Scenarios
1. 환경 변수로 사용
   - 애플리케이션은 환경 변수에서 시크릿 데이터를 읽을 수 있음
   - 환경 변수에 노출된 시크릿을 통해 애플리케이션이 데이터베이스에 연결할 수 있음

2. 파일로 사용
   - 시크릿 데이터를 파일 형태로 파드의 볼륨에 마운트할 수 있음
   - 컨테이너는 지정된 디렉토리에서 해당 파일을 읽을 수 있음

## Creating Secrets
### Using Files
1. 필요한 파일을 생성
   - 예: `username.txt`에 사용자 이름 `root` 추가
   - `password.txt`에 암호 문자열 입력

2. 시크릿 생성 명령 실행
   - `{kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt}`
   - 생성된 시크릿은 `db-user-pass`로 명명됨

### Using YAML
1. YAML 파일 생성
   - `kind: Secret` 정의와 함께 시크릿의 이름, 유형, 데이터를 포함
   - 데이터는 `base64` 형식으로 인코딩

2. YAML 파일을 사용하여 시크릿 생성
   - `{kubectl apply -f secret-db-secret.yml}` 명령어 사용

## Mounting Secrets
1. 환경 변수로 마운트
   - 파드 정의 파일에 `env` 항목 추가
   - 예: `{valueFrom}`을 통해 시크릿 이름과 키를 지정

2. 파일로 마운트
   - `volumeMount`를 사용하여 시크릿을 볼륨으로 연결
   - 볼륨 정의에 시크릿 이름과 키를 지정하여 디렉토리에 시크릿 데이터를 저장

## Example Scenario
1. 환경 변수 사용
   - `{db-secret}` 시크릿의 `username` 키를 환경 변수로 노출
2. 볼륨 사용
   - `/etc/creds` 디렉토리에 시크릿 데이터를 저장
   - 애플리케이션은 해당 디렉토리에서 파일을 읽어 시크릿 데이터를 사용

## Notes
- 시크릿은 반드시 안전하게 관리되어야 함
- 공개 이미지 레지스트리에 시크릿을 저장하지 않도록 주의
- 외부 볼트 서비스를 사용할 경우 앱 계층에서 직접 시크릿 관리 가능

## YAML Example
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-demo-pod
spec:
  containers:
  - name: app-container
    image: nginx
    env:
    - name: DB_USERNAME
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: username
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
    volumeMounts:
    - name: creds-volume
      mountPath: "/etc/creds"
      readOnly: true
  volumes:
  - name: creds-volume
    secret:
      secretName: db-secret
```

