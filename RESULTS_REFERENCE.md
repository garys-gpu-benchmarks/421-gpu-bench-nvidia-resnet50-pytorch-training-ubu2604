# Results: 421 ResNet-50 FP16 Training Step (PyTorch)

**Main goal:** Measure mixed-precision ResNet-50 training-step efficiency  
**Tools:** Run scripts/train_resnet50.py via collect_resnet50_train.py

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_149 · recorded 2026-11-19). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 NVL (95830 MiB) · Intel(R) Xeon(R) Platinum 8452Y · Ubuntu 26.04.1 LTS · kernel 6.8.0-138-generic · driver 595.112.07
- **Run:** 2026-11-19T22:01:12Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 02:09 · benchmark 00:01 · whole run 02:24
- **Source:** run 421_20261119_220112
- **Command:** `bash run_benchmark.sh --smoke --precision fp16 --image-size 64 --batch-size 2 --learning-rate 0.1 --warmup-iters 1 --num-iterations 3`

| # | Metric | Result |
|---|---|---|
| 1 | Training batch step time (step_time_msec) | 15.791008 |
| 2 | Training throughput (images_per_sec) | 126.654359 |
| 3 | Training throughput from counted FLOPs, TFLOP/s (counted_tflops) | 0.252661 |
| 4 | Peak PyTorch-allocated GPU memory (peak_gpu_memory_gb) | 0.281045 |
| 5 | Modeled weight and feature-map traffic, GB/s (modeled_weight_feature_traffic_gb_s) | 22.708169 |
<!-- /profile:smoke -->
