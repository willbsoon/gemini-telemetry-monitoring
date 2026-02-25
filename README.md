# Gemini CLI Telemetry & Dashboard Setup

이 저장소는 Gemini CLI의 실시간 모니터링, 분산 추적(Tracing) 및 메트릭(Metrics) 수집을 위한 인프라 구성 환경(Docker Compose)을 포함하고 있습니다.

## 1. 인프라 실행 방법 (Docker)

OpenTelemetry Collector, Prometheus, Jaeger, Grafana를 백그라운드에서 실행하려면 아래 명령어를 사용하세요.

```bash
# 컨테이너 백그라운드 실행
docker-compose up -d

# 실행 상태 확인
docker-compose ps
```

### 접속 정보
* **Grafana (대시보드)**: [http://localhost:3000](http://localhost:3000) (초기 비밀번호: `admin` / `admin` 또는 설정된 비밀번호)
* **Jaeger (상세 트레이스)**: [http://localhost:16686](http://localhost:16686)
* **Prometheus (메트릭 쿼리)**: [http://localhost:9090](http://localhost:9090)

### 종료 방법
```bash
docker-compose down
```

---

## 2. Gemini CLI 텔레메트리 연동 설정

Gemini CLI가 로컬에 띄워진 OTel Collector로 텔레메트리 데이터(Metrics, Traces)를 전송하도록 설정해야 합니다.

사용자의 홈 디렉토리에 있는 `.gemini/settings.json` 파일을 열어 `telemetry` 항목을 아래와 같이 설정하세요. (예: `~/.gemini/settings.json`)

```json
{
  "telemetry": {
    "enabled": true,
    "target": "local",
    "useCollector": true,
    "otlpEndpoint": "http://localhost:4317"
  }
}
```

> **참고**: `otlpEndpoint`가 `http://localhost:4317`로 설정된 경우, Gemini CLI는 **gRPC** 프로토콜을 사용하여 OTel Collector와 통신합니다. `docker-compose.yml`에 정의된 `otel-collector` 서비스가 이 포트를 통해 데이터를 수신합니다.
