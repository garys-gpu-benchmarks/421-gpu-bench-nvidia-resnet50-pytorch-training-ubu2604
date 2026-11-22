# Results: 421 ResNet-50 FP16 Training Step (PyTorch)

**Main goal:** Measure mixed-precision ResNet-50 training-step efficiency  
**Tools:** Run scripts/train_resnet50.py via collect_resnet50_train.py

Reference results from the validation run on the system shown in each section (recorded 2026-11-22). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 80GB HBM3 (81559 MiB) · Intel(R) Xeon(R) Platinum 8468 · Ubuntu 26.04.1 LTS · kernel 6.8.0-149-generic · driver 595.112.07
- **Run:** 2026-11-22T15:04:45Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 02:22 · benchmark 00:01 · whole run 02:36
- **Source:** run 421_20261122_150445
- **Command:** `bash run_benchmark.sh --smoke --precision fp16 --image-size 64 --batch-size 2 --learning-rate 0.1 --warmup-iters 1 --num-iterations 3`

| # | Metric | Result |
|---|---|---|
| 1 | Training batch step time (step_time_msec) | 14.531104 |
| 2 | Training throughput (images_per_sec) | 137.635794 |
| 3 | Training throughput from counted FLOPs, TFLOP/s (counted_tflops) | 0.274567 |
| 4 | Peak PyTorch-allocated GPU memory (peak_gpu_memory_gb) | 0.281045 |
| 5 | Modeled weight and feature-map traffic, GB/s (modeled_weight_feature_traffic_gb_s) | 24.677057 |
<!-- /profile:smoke -->
