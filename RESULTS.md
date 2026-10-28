# Experimental Results and Screenshot Evidence

**Embedded Linux with Yocto, Edge AI and QEMU Device Driver | Group 4**

**These are REAL screenshots and charts extracted from the submitted 58-page practical Word report.** Images are embedded below and stored individually under [`evidence/`](evidence/). This is not a replacement for the source code or raw CSV files; it is directly viewable experimental evidence for the submitted report.

## Key verified results

| Metric | Python 1 thread | C 1 thread | Python 4 threads | C 4 threads |
|:--|--:|--:|--:|--:|
| **Total time for 1,000 images** | 164.399 s | 162.040 s | 47.184 s | **46.402 s** |
| Mean inference latency | 161.011 ms | 159.312 ms | 43.957 ms | **43.672 ms** |
| Throughput | 6.083 images/s | 6.171 images/s | 21.193 images/s | **21.551 images/s** |
| CPU utilization | 99.89% | 99.93% | 390.20% | 394.07% |
| Peak RSS memory | 33.72 MiB | **10.25 MiB** | 33.72 MiB | **10.25 MiB** |

- **Accuracy:** MobileNet V1 Top-1 on the 1,000-image Imagenette subset: **69.10% (691/1,000)**.
- **Prediction consistency:** four repeated benchmark configurations gave **0 mismatches across 1,000 images**; the earlier governor comparison likewise verified eight configurations. Consistency is not accuracy.
- **Speed improvement:** 4 CPU threads reduce total elapsed time by about **71%** versus one thread.
- **RAM:** native C uses ~**69.6% less peak RSS** than Python in the four-column 1,000-image run.
- **Main bottleneck:** inference accounts for **93.30–98.32%** of the instrumented phase time.
- **CPU governor:** in the available runs, `performance` offered no clear gain over `ondemand`.

> **Benchmark scope:** The 1,000 images were previously converted to 224 x 224 RGB uint8 tensors. The reported times include reading/decompressing that binary stream, input-copy, inference and result processing, **not** JPEG decode and resize on the Raspberry Pi. No dedicated GPU/NPU inference accelerator was measured.

## Visual evidence by project stage

### 1. Yocto Linux: build and boot

**Yocto image build artifacts**  
![Yocto image build artifacts](evidence/img-006.png)

**Linux kernel version shown on Raspberry Pi 4**  
![Linux kernel version shown on Raspberry Pi 4](evidence/img-014.png)

**Poky Scarthgap 5.0.20 on the target**  
![Poky Scarthgap 5.0.20 on the target](evidence/img-015.png)

**Enabled Yocto layers including meta-edge-ai**  
![Enabled Yocto layers including meta-edge-ai](evidence/img-019.png)

### 2. TensorFlow Lite installation and MobileNet input

**BitBake recognizes TensorFlow Lite recipes**  
![BitBake recognizes TensorFlow Lite recipes](evidence/img-025.png)

**TensorFlow Lite C shared library installed on Pi**  
![TensorFlow Lite C shared library installed on Pi](evidence/img-029.png)

**Actual MobileNet input/output shape and quantization**  
![Actual MobileNet input/output shape and quantization](evidence/img-036.png)

**MobileNet classification result on Grace Hopper test image**  
![MobileNet classification result on Grace Hopper test image](evidence/img-045.png)

### 3. First Python versus C benchmarks

**Original Python benchmark on Raspberry Pi**  
![Original Python benchmark on Raspberry Pi](evidence/img-042.png)

**Original native C benchmark on Raspberry Pi**  
![Original native C benchmark on Raspberry Pi](evidence/img-051.png)

**Python versus C latency and peak memory table**  
![Python versus C latency and peak memory table](evidence/img-052.png)

### 4. Multi-core scaling, 1 / 2 / 4 threads

**Python multi-thread comparison table**  
![Python multi-thread comparison table](evidence/img-058.png)

**Native C multi-thread comparison table**  
![Native C multi-thread comparison table](evidence/img-064.png)

**Python vs C inference latency by thread count**  
![Python vs C inference latency by thread count](evidence/img-066.png)

### 5. Preparing 1,000 Imagenette samples

**Manifest for the selected 1,000 images**  
![Manifest for the selected 1,000 images](evidence/img-069.png)

**Dataset preparation metadata**  
![Dataset preparation metadata](evidence/img-070.png)

**GZIP archive integrity check**  
![GZIP archive integrity check](evidence/img-071.png)

### 6. Raspberry Pi benchmark: 1,000 images

**Python, 1-thread benchmark output**  
![Python, 1-thread benchmark output](evidence/img-075.png)

**Python, 4-thread benchmark output**  
![Python, 4-thread benchmark output](evidence/img-076.png)

**C, 1-thread benchmark output**  
![C, 1-thread benchmark output](evidence/img-083.png)

**C, 4-thread benchmark output**  
![C, 4-thread benchmark output](evidence/img-084.png)

**Complete benchmark comparison table**  
![Complete benchmark comparison table](evidence/img-087.png)

**Comparison of total processing time**  
![Comparison of total processing time](evidence/img-088.png)

**Comparison of peak RAM (RSS)**  
![Comparison of peak RAM (RSS)](evidence/img-089.png)

**All 1,000 predictions matched across four benchmark configurations**  
![All 1,000 predictions matched across four benchmark configurations](evidence/img-091.png)

### 7. Profiling: where the latency goes

**Actual profiling terminal output**  
![Actual profiling terminal output](evidence/img-093.png)

**Four processing stages: comparison table**  
![Four processing stages: comparison table](evidence/img-094.png)

**Time distribution across processing stages**  
![Time distribution across processing stages](evidence/img-095.png)

### 8. CPU governor and model classification accuracy

**The Raspberry Pi governor reported ondemand**  
![The Raspberry Pi governor reported ondemand](evidence/img-096.png)

**Ondemand vs performance comparison**  
![Ondemand vs performance comparison](evidence/img-100.png)

**Top-1 accuracy measured on 1,000 images**  
![Top-1 accuracy measured on 1,000 images](evidence/img-102.png)

**Accuracy summary: 691 correct out of 1,000**  
![Accuracy summary: 691 correct out of 1,000](evidence/img-103.png)

**Verification across all eight configurations**  
![Verification across all eight configurations](evidence/img-104.png)

**Recorded experimental system and checksums**  
![Recorded experimental system and checksums](evidence/img-105.png)

**Final cross-language benchmark results**  
![Final cross-language benchmark results](evidence/img-106.png)

## Report and QEMU driver

The report and PowerPoint include the **QEMU `mychardev` character driver** architecture, Yocto integration and seven documented tests. This image gallery focuses on the actual Raspberry Pi / Edge AI terminal captures that were embedded in the practical Word report. See the complete final report for the group's driver results.

## Provenance

All `evidence/img-*.png` files here are extracted without modification from `New Microsoft Word Document(2).docx` submitted in this conversation (the file uploaded to GitHub as `Practical_Workflow_Evidence.docx`). Numbers follow the order of embedded images in the document; titles here describe their content.
