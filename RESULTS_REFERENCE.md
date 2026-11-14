# Results: 421 ResNet-50 FP16 Training Step (PyTorch)

**Main goal:** Measure mixed-precision ResNet-50 training-step efficiency  
**Tools:** Run scripts/train_resnet50.py via collect_resnet50_train.py

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_141 · recorded 2026-11-14). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 PCIe (81559 MiB) · Intel(R) Xeon(R) Platinum 8352Y CPU @ 2.20GHz · Ubuntu 26.04.1 LTS · kernel 6.8.0-138-generic · driver 595.91.07
- **Run:** 2026-11-14T11:30:34Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 02:19 · benchmark 00:01 · whole run 02:43
- **Source:** run 421_20261114_113034
- **Command:** `bash run_benchmark.sh --smoke --precision fp16 --image-size 64 --batch-size 2 --learning-rate 0.1 --warmup-iters 1 --num-iterations 3`

| # | Metric | Result |
|---|---|---|
| 1 | Training batch step time (step_time_msec) | 39.841494 |
| 2 | Training throughput (images_per_sec) | 50.19892 |
| 3 | Modeled FP16 training throughput, 3x forward FLOPs, TFLOP/s (fp16_tflops) | 0.093431 |
| 4 | Peak GPU memory (peak_gpu_memory_gb) | 0.281045 |
| 5 | Modeled weight and feature-map traffic, GB/s (modeled_weight_feature_traffic_gb_s) | 9.000287 |
<!-- /profile:smoke -->
