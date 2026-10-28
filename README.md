# Embedded Linux with Yocto, Edge AI and QEMU Driver

**Embedded Systems, Group 4.**

> Current upload: technical report, presentation, and original experimental evidence. Source code and raw logs will be added separately when available; do not treat this folder alone as a complete source checkout.
 Yocto Linux on Raspberry Pi 4 Model B, CPU-optimized MobileNet V1 inference with TensorFlow Lite using Python and native C, and a Linux character device driver evaluated with QEMU.

## Platform

- Raspberry Pi 4 Model B (BCM2711, ARMv7l), 4 GB RAM
- Poky/Yocto Scarthgap 5.0.20, Linux kernel 6.6.63-v7l, systemd, UART
- TensorFlow Lite 2.16.2, quantized MobileNet V1, input `[1,224,224,3]` UINT8, output 1001 classes
- Imagenette validation data: 1,000 images (100 images from each of 10 classes)

## Main results

| 1,000 image run | Python, 1 thread | C, 1 thread | Python, 4 threads | C, 4 threads |
|:--|--:|--:|--:|--:|
| Total elapsed (s) | 164.399 | 162.040 | 47.184 | 46.402 |
| Mean inference (ms) | 161.011 | 159.312 | 43.957 | 43.672 |
| Throughput (images/s) | 6.083 | 6.171 | 21.193 | 21.551 |
| Peak RSS (MiB) | 33.72 | 10.25 | 33.72 | 10.25 |

These numbers are from a later `_demo` repeat of the benchmark. The numerical results are documented in the attached report and practical evidence. Original CSVs and logs are not included in this documentation-only upload. The workflow reads preprocessed RGB UINT8 tensors from a GZIP stream: JPEG decoding and resizing on the Raspberry Pi are not included in measured total latency.

**Accuracy:** Top-1 = 69.10% (691 correct of 1,000). All tested output configurations returned matching `top1_class` and `top1_score` for every image in their verification sets. This does *not* mean 100% accuracy.

**Optimization:** 4 CPU threads reduced total processing time by about 71%; inference remained the bottleneck (93.30–98.32% of measured per-phase time). A governor change from `ondemand` to `performance` provided no clear latency benefit in the measured runs. No model weights or architecture were changed during CPU optimization.

## Source and experimental records (to be added from source workspace)

- `meta-edge-ai/`: local Yocto layer, TFLite C API append, C benchmark recipes and sources.
- `meta-mydriver/`: group QEMU kernel module layer if supplied by the member working on driver development.
- `scripts/`: Python and C benchmark, preprocessing, evaluation and analysis sources.
- `model/`: reference model and small input tensor (check provenance/license before public redistribution).
- `dataset/manifest.csv`: IDs, WordNet labels and image paths for the fixed selection of Imagenette images.
- `results/`: raw CSV data, benchmark logs, accuracy and verification outputs.
- `logs/`: preparation metadata and file checksums.
- `docs/`: group final report, presentation, practical evidence notes.

## Reproducing the image data

Source dataset: `https://s3.amazonaws.com/fast-ai-imageclas/imagenette2-160.tgz`

The compressed input tensor dataset is not committed (105,381,170 bytes). Fetch the upstream Imagenette2-160 archive, extract it as `dataset/imagenette2-160/`, then use the committed `scripts/prepare_1000.py` and `dataset/manifest.csv` to recreate the GZIP stream. Pillow version, processing implementation and dataset integrity should be recorded for reproducibility. Compare with `logs/dataset_preparation.json` and SHA-256 logs.

## Build and run (documented procedure; source files not yet uploaded)

Use upstream **Poky Scarthgap**, **meta-raspberrypi**, **meta-openembedded**, and the TFLite layer of the appropriate revision. Clone these upstream projects separately; the full Poky checkout, Yocto build outputs, caches, and flashed WIC images are not stored here.

The Raspberry Pi Yocto configuration uses `MACHINE = "raspberrypi4"` and the custom `meta-edge-ai` layer. To compile the original C executable from the Yocto build environment:

```sh
bitbake benchmark-c
bitbake benchmark-1000-c
```

From `/root/edge-ai` on the prepared Raspberry Pi, with model and input files in place:

```sh
python3 benchmark_1000_python.py --threads 4 --model model.tflite --data imagenette1000_rgb224_u8.bin.gz --warmup-input input.bin --output results/python_t4.csv
./benchmark_1000_c --threads 4 --model model.tflite --data imagenette1000_rgb224_u8.bin.gz --warmup-input input.bin --output results/c_t4.csv
```

Evaluate accuracy on the Ubuntu host in the directory containing `model/`, `results/`, and `dataset/`:

```sh
python3 scripts/evaluate_accuracy.py
```

## QEMU and device driver

The report and presentation describe a `mychardev` misc character driver tested on a `qemux86-64` Yocto image. The report appendix includes driver source and recipe listings. Separate source files and QEMU test material have not yet been uploaded; they should be added to make the repository reproducible.

## Deliverables

- `docs/report_2_EN.pdf`: final course report (as supplied)
- `docs/Embedded_Linux_EdgeAI_Group4.pptx`: presentation (as supplied)
- `docs/Practical_Workflow_Evidence.docx`: command-by-command practical evidence notes

These documents are original supplied versions. Use the GitHub repository URL in the final PowerPoint submission. Source files, when available, should be added as ordinary project files, not full Ubuntu/Yocto build folders.
