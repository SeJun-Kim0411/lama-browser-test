# 브라우저 지우기 시험 (LaMa)

기기 브라우저에서 이미지 채우기(inpainting) 속도를 재는 시험 페이지입니다. 사진은 기기 밖으로 전송되지 않습니다.

## 모델 (models/)
- `lama_hq.part1` + `lama_hq.part2` (이어 붙이면 ONNX 1개, 103MB), `lama_small.onnx` (34MB), `lama_hq16.part1` + `lama_hq16.part2` (103MB, 계산까지 float16)
- 원본: LaMa (advimman/lama, Apache License 2.0) → ONNX 변환본 Carve/LaMa-ONNX, 양자화본 othytenk/lama-int4-int8 (Apache License 2.0)
- 이 저장소에서 바꾼 점: Fourier Unit 의 rfft/irfft 구현(Slice·Einsum 조합)을 수학적으로 같은 고정 행렬 곱으로 바꿈(브라우저 WebGPU 호환), hq 는 가중치를 float16 으로 저장. hq16 은 BatchNorm 을 같은 값의 평균0·분산1 형태로 접고, 가운데 잔차 블록(model.5~22) 입력에 1/32·출력에 32 를 곱하고 블록 안 편향을 같은 비율로 줄인 뒤(결과 동일) 계산을 float16 으로 바꿈
- 라이선스 전문: models/LICENSE-LaMa-Apache-2.0.txt

## 그 밖의 오픈소스
- onnxruntime-web 1.31.1 — MIT License
