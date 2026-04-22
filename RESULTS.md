# Results

## 1. 실험 요약
- 저장소: exp-llm-chat-runtime-shootout
- 커밋 해시: c88eb5d
- 실험 일시: 2026-04-22T06:13:19.774Z -> 2026-04-22T06:13:22.116Z
- 담당자: ai-webgpu-lab
- 실험 유형: `llm`
- 상태: `success`

## 2. 질문
- 같은 프롬프트와 출력 예산에서 runtime profile별 TTFT와 decode throughput 차이가 분명한가
- worker/main execution mode 차이가 결과 메타데이터에 남는가
- 실제 runtime을 붙이기 전 deterministic readiness harness로 비교 프로토콜을 고정할 수 있는가

## 3. 실행 환경
### 브라우저
- 이름: Chrome
- 버전: 147.0.7727.15

### 운영체제
- OS: Linux
- 버전: unknown

### 디바이스
- 장치명: Linux x86_64
- device class: `desktop-high`
- CPU: 16 threads
- 메모리: 16 GB
- 전원 상태: `unknown`

### GPU / 실행 모드
- adapter: profile-driven
- backend: `webgpu`
- fallback triggered: `false`
- worker mode: `worker, main`
- cache state: `warm`
- required features: []
- limits snapshot: {}

## 4. 워크로드 정의
- 시나리오 이름: WebLLM-style, Transformers.js-style
- 입력 프로필: prompt-17-output-54
- 데이터 크기: synthetic runtime profile=WebLLM-style; promptTokens=17; outputTokens=54; automation=playwright-chromium, synthetic runtime profile=Transformers.js-style; promptTokens=17; outputTokens=54; automation=playwright-chromium
- dataset: -
- model_id 또는 renderer: webllm-style, transformersjs-style
- 양자화/정밀도: -
- resolution: -
- context_tokens: 17
- output_tokens: 54

## 5. 측정 지표
### 공통
- time_to_interactive_ms: 1278.6 ~ 3620.8 ms
- init_ms: 60.3 ~ 90.6 ms
- success_rate: 1
- peak_memory_note: 16 GB reported by browser
- error_type: -

### LLM / Benchmark
- ttft_ms: 17.2 ~ 22.2 ms
- prefill_tok_per_sec: 582.19 ~ 752.21 tok/s
- decode_tok_per_sec: 36.53 ~ 51.8 tok/s
- turn_latency_ms: 1162.2 ~ 1561 ms

## 6. 결과 표
| Run | Scenario | Backend | Cache | Mean | P95 | Notes |
|---|---|---:|---:|---:|---:|---|
| 1 | WebLLM-style | webgpu | warm | 51.8 | 22.2 | prefill=582.19 tok/s, metric=decode tok/s / TTFT ms |
| 2 | Transformers.js-style | mixed | warm | 36.53 | 17.2 | prefill=752.21 tok/s, metric=decode tok/s / TTFT ms |

## 7. 관찰
- 최고 decode throughput은 WebLLM-style의 51.8 tok/s였다.
- 가장 낮은 TTFT는 Transformers.js-style에서 관찰됐다.
- playwright-chromium로 수집된 automation baseline이며 headless=true, browser=Chromium 147.0.7727.15.
- 실제 runtime/model/renderer 교체 전 deterministic harness 결과이므로, 절대 성능보다 보고 경로와 재현성 확인에 우선 의미가 있다.

## 8. 결론
- runtime readiness 비교가 raw JSON과 RESULTS.md 둘 다에서 반복 가능해졌다.
- 다음 단계는 WebLLM, Transformers.js, ORT 계열 실제 runtime을 같은 prompt budget으로 연결하는 것이다.
- worker/main mode 차이는 유지하되 실제 model load와 cache state를 추가 기록해야 한다.

## 9. 첨부
- 스크린샷: ./reports/screenshots/01-webllm-style.png, ./reports/screenshots/02-transformersjs-style.png
- 로그 파일: ./reports/logs/01-webllm-style.log, ./reports/logs/02-transformersjs-style.log
- raw json: ./reports/raw/01-webllm-style.json, ./reports/raw/02-transformersjs-style.json
- 배포 URL: https://ai-webgpu-lab.github.io/exp-llm-chat-runtime-shootout/
- 관련 이슈/PR: -
