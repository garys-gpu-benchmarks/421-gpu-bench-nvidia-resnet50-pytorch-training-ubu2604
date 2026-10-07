# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Runs scripts/train_resnet50.py (synthetic images, FP16 AMP) twice via collect_resnet50_train.py. dataset_name: synthetic. precision: fp16. image_size, batch_size, warmup_iters, and num_iterations come from yaml. Sweep dimensions: dataset_name, precision, image_size, batch_size, num_workers, optimizer, learning_rate, gradient_accumulation.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| dataset_name | `--dataset-name` | smoke=synthetic, baseline=synthetic, extended=synthetic | synthetic | From Parameter list; see Execution Description With Parameters. |
| precision | `--precision` | smoke=fp16, baseline=fp16, extended=fp16 | fp16 | From Parameter list; see Execution Description With Parameters. |
| image_size | `--image-size` | smoke=64, baseline=224, extended=224 | 224 | From Parameter list; see Execution Description With Parameters. |
| batch_size | `--batch-size` | smoke=2, baseline=8, extended=16 | 8 | From Parameter list; see Execution Description With Parameters. |
| num_workers | `--num-workers` | smoke=0, baseline=0, extended=0 | 0 | From Parameter list; see Execution Description With Parameters. |
| optimizer | `--optimizer` | smoke=sgd, baseline=sgd, extended=sgd | sgd | From Parameter list; see Execution Description With Parameters. |
| learning_rate | `--learning-rate` | smoke=0.1, baseline=0.1, extended=0.1 | 0.1 | From Parameter list; see Execution Description With Parameters. |
| gradient_accumulation | `--gradient-accumulation` | smoke=1, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |
| warmup_iters | `--warmup-iters` | smoke=1, baseline=2, extended=5 | 2 | From Parameter list; see Execution Description With Parameters. |
| num_iterations | `--num-iterations` | smoke=3, baseline=3100, extended=8800 | 3100 | From Parameter list; see Execution Description With Parameters. |

## Invocation

```bash
Run scripts/train_resnet50.py via collect_resnet50_train.py
```

## Raw Output Format

raw_results.csv with pass1, pass2, and summary from two runs of train_resnet50.py

check,image_size,batch_size,precision,status,step_time_msec,images_per_sec,fp16_tflops,modeled_weight_feature_traffic_gb_s,peak_gpu_memory_gb
pass1,224,32,fp16,ok,80,400,12,50,6

## Metrics

- **#1: Training batch step time** — stored as `step_time_msec`.
- **#2: Training throughput** — stored as `images_per_sec`.
- **#3: Achieved compute throughput** — stored as `fp16_tflops`.
- **#4: Peak GPU memory** — stored as `peak_gpu_memory_gb`.
- **#5: Modeled weight and feature-map traffic, GB/s** — stored as `modeled_weight_feature_traffic_gb_s`.

## Framework

Runs scripts/train_resnet50.py (synthetic images, FP16 AMP) twice via collect_resnet50_train.py. dataset_name: synthetic. precision: fp16.

## Installation and Execution Summary

Run the PyTorch ResNet-50 FP16 trainer (scripts/train_resnet50.py, torchvision.models.resnet50) twice on synthetic images with yaml image_size/batch_size/num_iterations, parse RESULT step_time_msec, images_per_sec, fp16_tflops, and peak memory, and write pass1/pass2/summary rows, to measure mixed-precision training efficiency

## Platform Portability

- **AMD (primary):** ```bash
Run scripts/train_resnet50.py via collect_resnet50_train.py
```
- **NVIDIA:** Native NVIDIA CUDA workload. Execute on the stated Ubuntu release with the host NVIDIA driver and CUDA userspace. ROCm porting notes do not apply.

## Model Context Protocols

- **Active:** None

## Execution-Loop Validation Contract

EXECUTION CHAIN: `run_benchmark.sh` ➔ raw output ➔ `scripts/parse_results.py` ➔ `results/benchmark.db` ➔ `scripts/validate_results.py`

This benchmark uses a lightweight, SQLite-integrated execution loop for result validation. All validation is performed by `scripts/validate_results.py`.

### Validation script usage

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional; select the installed interpreter

# After a live run:
".venv/bin/python" scripts/validate_results.py --db results/benchmark.db

# CI / no-GPU path (seeds fixture and validates it):
".venv/bin/python" scripts/validate_results.py --seed-fixture --quiet

# Override DB path via environment variable:
BENCHMARK_DB=tests/fixtures/benchmark.db \
  ".venv/bin/python" scripts/validate_results.py
```

### Run artifact contract

raw_results.csv with pass1, pass2, and summary from two runs of train_resnet50.py

check,image_size,batch_size,precision,status,step_time_msec,images_per_sec,fp16_tflops,modeled_weight_feature_traffic_gb_s,peak_gpu_memory_gb
pass1,224,32,fp16,ok,80,400,12,50,6

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --profile smoke --validate
bash run_benchmark.sh --profile baseline --validate
bash run_benchmark.sh --profile extended --validate
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Runs scripts/train_resnet50.py (synthetic images, FP16 AMP) twice via collect_resnet50_train.py. dataset_name: synthetic. precision: fp16.
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Runs scripts/train_resnet50.py (synthetic images, FP16 AMP) twice via collect_resnet50_train.py. dataset_name: synthetic. precision: fp16.

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.
