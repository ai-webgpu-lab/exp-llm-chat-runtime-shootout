# Results

## 1. 실험 요약
- 저장소: exp-llm-chat-runtime-shootout
- 커밋 해시: a90ec7c
- 실험 일시: 2026-04-22T07:23:46.917Z -> 2026-04-22T07:24:36.087Z
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
- adapter: synthetic-webgpu-profile, wasm-fallback-simulated
- backend: `webgpu, wasm`
- fallback triggered: `false, true`
- worker mode: `worker, main`
- cache state: `warm`
- required features: ["shader-f16"], []
- limits snapshot: {}

## 4. 워크로드 정의
- 시나리오 이름: WebLLM-style / WebGPU, Transformers.js-style / WebGPU, WebLLM-style / Fallback, Transformers.js-style / Fallback
- 입력 프로필: prompt-17-output-54
- 데이터 크기: synthetic runtime profile=WebLLM-style; promptTokens=17; outputTokens=54; executionMode=webgpu; backend=webgpu; automation=playwright-chromium, synthetic runtime profile=Transformers.js-style; promptTokens=17; outputTokens=54; executionMode=webgpu; backend=webgpu; automation=playwright-chromium, synthetic runtime profile=WebLLM-style; promptTokens=17; outputTokens=54; executionMode=fallback; backend=wasm; automation=playwright-chromium, synthetic runtime profile=Transformers.js-style; promptTokens=17; outputTokens=54; executionMode=fallback; backend=wasm; automation=playwright-chromium
- dataset: -
- model_id 또는 renderer: webllm-style, transformersjs-style
- 양자화/정밀도: -
- resolution: -
- context_tokens: 17
- output_tokens: 54

## 5. 측정 지표
### 공통
- time_to_interactive_ms: 665.1 ~ 1334.5 ms
- init_ms: 60.2 ~ 167.2 ms
- success_rate: 1
- peak_memory_note: 16 GB reported by browser
- error_type: -

### LLM / Benchmark
- ttft_ms: 17.3 ~ 52.2 ms
- prefill_tok_per_sec: 290.6 ~ 752.21 tok/s
- decode_tok_per_sec: 49.33 ~ 133.96 tok/s
- turn_latency_ms: 521.9 ~ 1252.6 ms
- backends: webgpu, wasm
- fallback states: false, true

## 6. 결과 표
| Run | Scenario | Backend | Cache | Mean | P95 | Notes |
|---|---|---:|---:|---:|---:|---|
| 1 | WebLLM-style / WebGPU | webgpu | warm | 133.96 | 22.4 | prefill=596.49 tok/s, metric=decode tok/s / TTFT ms |
| 2 | Transformers.js-style / WebGPU | webgpu | warm | 112.2 | 17.3 | prefill=752.21 tok/s, metric=decode tok/s / TTFT ms |
| 3 | WebLLM-style / Fallback | wasm | warm | 57.4 | 52.2 | prefill=290.6 tok/s, metric=decode tok/s / TTFT ms |
| 4 | Transformers.js-style / Fallback | wasm | warm | 49.33 | 40.1 | prefill=364.03 tok/s, metric=decode tok/s / TTFT ms |

## 7. 관찰
- 최고 decode throughput은 WebLLM-style / WebGPU의 133.96 tok/s였다.
- 가장 낮은 TTFT는 Transformers.js-style / WebGPU에서 관찰됐다.
- playwright-chromium로 수집된 automation baseline이며 headless=true, browser=Chromium 147.0.7727.15.
- 실제 runtime/model/renderer 교체 전 deterministic harness 결과이므로, 절대 성능보다 보고 경로와 재현성 확인에 우선 의미가 있다.

## 8. WebGPU vs Fallback
- webllm-style: decode tok/s webgpu=133.96, fallback=57.4, delta=+76.56; TTFT delta=-29.8 ms; worker worker -> main
- transformersjs-style: decode tok/s webgpu=112.2, fallback=49.33, delta=+62.87; TTFT delta=-22.8 ms; worker main -> main

## 9. 결론
- runtime readiness 비교가 raw JSON과 RESULTS.md 둘 다에서 반복 가능해졌다.
- 같은 prompt budget에서 WebGPU vs fallback compare pair를 두 profile 모두에 대해 남길 수 있게 됐다.
- 다음 단계는 WebLLM, Transformers.js, ORT 계열 실제 runtime을 같은 prompt budget으로 연결하는 것이다.

## 10. 첨부
- 스크린샷: ./reports/screenshots/01-webllm-style-webgpu.png, ./reports/screenshots/02-transformersjs-style-webgpu.png, ./reports/screenshots/03-webllm-style-fallback.png, ./reports/screenshots/04-transformersjs-style-fallback.png
- 로그 파일: ./reports/logs/01-webllm-style-webgpu.log, ./reports/logs/02-transformersjs-style-webgpu.log, ./reports/logs/03-webllm-style-fallback.log, ./reports/logs/04-transformersjs-style-fallback.log
- raw json: ./reports/raw/01-webllm-style-webgpu.json, ./reports/raw/02-transformersjs-style-webgpu.json, ./reports/raw/03-webllm-style-fallback.json, ./reports/raw/04-transformersjs-style-fallback.json
- 배포 URL: https://ai-webgpu-lab.github.io/exp-llm-chat-runtime-shootout/
- 관련 이슈/PR: -
