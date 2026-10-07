# PRD.md:  "The Why"; Product requirements, benchmark metadata table, high-level requirements, etc.

Product Requirements Document

"The Why"; Product requirements, benchmark metadata table, high-level requirements, etc. Defines the benchmark goal, validation objective, test name, benchmark number, category, and high-level success criteria.

## Benchmark Matrix Document Metadata (via benchmark_specification.json)

This PRD.md section is populated from benchmark_specification.json, which is the structured source of benchmark-specific product requirements.

## Workload Number
421

## Workload Name
ResNet-50 FP16 Training Step (PyTorch)

## Execution Summary (Run and Measure)
Run the PyTorch ResNet-50 FP16 trainer (scripts/train_resnet50.py, torchvision.models.resnet50) twice on synthetic images with yaml image_size/batch_size/num_iterations, parse RESULT step_time_msec, images_per_sec, fp16_tflops, and peak memory, and write pass1/pass2/summary rows, to measure mixed-precision training efficiency

## Main Goal
Measure mixed-precision ResNet-50 training-step efficiency

## Validation Objective
Validates two timed training passes plus a summary average on synthetic FP16 ResNet-50 steps

## Workload Category
Training, Inference, Model Workloads

## Validation Requirement

The benchmark must include an automated SQLite-integrated validation layer that verifies persisted results from `results/benchmark.db`. Validation must confirm:

1. The benchmark run completed successfully with no tool errors.
2. Required samples and aggregate metrics were persisted for every swept shape.
3. Metrics are finite and physically sensible (positive, within plausible bounds).
4. Measured values satisfy configured thresholds when the workload defines pass/fail gates.
5. The benchmark fails validation when required data is missing, invalid, or outside bounds.

## Non-Functional Requirements

| Requirement | Target |
|---|---|
| Automation | Runs to completion without manual intervention after `bash run_benchmark.sh` |
| Idempotency | Re-running `run_benchmark.sh` appends a new run; never corrupts existing rows |
| Persistence | All metrics survive script exit; `results/benchmark.db` is the durable record |
