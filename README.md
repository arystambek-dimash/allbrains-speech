# AllBrains Speech

Memory-efficient streaming automatic speech recognition (ASR) for resource-constrained microcontrollers.

## Research question
Can we design a streaming speech recognition architecture that transcribes open-vocabulary English speech on microcontrollers under strict memory and computational constraints?

## Proposed architecture

### 1) Streaming front-end
- 16 kHz mono PCM input.
- 25 ms analysis window, 10 ms hop.
- Online log-Mel feature extraction (e.g., 40 bins) with fixed-size rolling buffers.
- Optional low-cost VAD to skip silent frames and reduce compute.

### 2) Tiny streaming encoder
- Quantized (INT8) causal model (DS-CNN/Conformer-lite style) trained for streaming.
- State-caching per layer to avoid recomputing full context.
- Chunk-wise inference for bounded latency and constant memory growth.

### 3) Open-vocabulary decoding
- CTC-based streaming decoder with subword or byte-level tokens.
- Prefix beam search with small beam width to bound RAM/CPU.
- Incremental partial hypotheses for low-latency transcription.

### 4) MCU-friendly memory strategy
- Static allocation only (no runtime heap growth).
- Ring buffers for audio/features.
- Quantized weights stored in flash; activations/workspace in SRAM.
- Shared scratch buffers across pipeline stages.

## Target constraints (design goals)
- SRAM budget: <= 256 KB.
- Flash/model budget: <= 2 MB.
- End-to-end latency: <= 300 ms for stable partial outputs.
- Throughput: real-time factor <= 1.0 on target MCU class.

## Evaluation plan
- Accuracy: WER on open English benchmarks and in-domain speech.
- Efficiency: peak SRAM, flash usage, MACs/frame, average power.
- Streaming quality: partial-to-final stability and emission latency.

## Expected outcome
A practical TinyML ASR stack that supports always-on, open-vocabulary English transcription on microcontrollers via streaming inference and tightly controlled memory usage.
