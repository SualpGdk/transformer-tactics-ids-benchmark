# Transformer Tactics IDS Benchmark

Benchmarking transformer-based design tactics from *"Design Tactics for Tailoring Transformer Architectures to Cybersecurity Challenges"* on CIC-IDS2017. Measuring detection rate, false positive rate, and response time across multiple architectures.
https://link.springer.com/article/10.1007/s10586-024-04355-0


| Transformer Tactic | False Positive Rate | Detection Rate | Response Time (ms/sample) |
|-------------------|------------------|----------------|--------------------------|
| Tactic 7 – Integrating decision tree and FT-transformer | 0.19% | 91.83% | 0.0505 |
| Tactic 13 – Vision Transformer (ViT) | 0.31% | 61.11% | 0.0294 |

## Detailed Benchmark Results For Tactic 7

### Dataset Distribution (CIC-IDS2017)

| Label | Count |
|------|------:|
| BENIGN | 2,273,097 |
| DoS Hulk | 231,073 |
| PortScan | 158,930 |
| DDoS | 128,027 |
| DoS GoldenEye | 10,293 |
| FTP-Patator | 7,938 |
| SSH-Patator | 5,897 |
| DoS slowloris | 5,796 |
| DoS Slowhttptest | 5,499 |
| Bot | 1,966 |
| Web Attack – Brute Force | 1,507 |
| Web Attack – XSS | 652 |
| Infiltration | 36 |
| Web Attack – SQL Injection | 21 |
| Heartbleed | 11 |

---

### Stage 1: Decision Tree

| Metric | Value |
|--------|------:|
| Accuracy | 0.9928 |
| Detection Rate | 0.9779 |
| False Positive Rate | 0.0036 |
| Response Time (ms/sample) | 0.0005 |

**Confusion Matrix:**
### Confusion Matrix

|                | Predicted BENIGN | Predicted ATTACK |
|----------------|------------------|------------------|
| **Actual BENIGN** | 45296 | 163 |
| **Actual ATTACK** | 246 | 10910 |


---

### Stage 2: FT-Transformer

**Model Parameters:** 160,713  
**Device:** CUDA  

#### Training Summary

| Epoch | Loss | Train Accuracy |
|------:|------:|--------------:|
| 1 | 1.1938 | 0.6947 |
| 5 | 0.2073 | 0.9595 |
| 10 | 0.1022 | 0.9859 |
| 15 | 0.0740 | 0.9875 |

---

### Per-Class Performance

| Class | Detection Rate | False Positive Rate |
|------|---------------:|--------------------:|
| BENIGN | 0.3939 | 0.0000 |
| DDoS | 1.0000 | 0.0000 |
| DoS GoldenEye | 0.9714 | 0.0009 |
| DoS Hulk | 1.0000 | 0.0156 |
| DoS Slowhttptest | 1.0000 | 0.0000 |
| DoS slowloris | 0.9444 | 0.0000 |
| FTP-Patator | 1.0000 | 0.0005 |
| PortScan | 1.0000 | 0.0000 |
| SSH-Patator | 0.9545 | 0.0000 |

---

### Full Pipeline Summary

| Metric | Stage 1: Decision Tree | Stage 2: FT-Transformer |
|--------|----------------------:|------------------------:|
| Detection Rate | 0.9779 | 0.9183 |
| False Positive Rate | 0.0036 | 0.0019 |
| Response Time (ms/sample) | 0.0005 | 0.0501 |

**Total Pipeline Response Time:** `0.0505 ms/sample`

---

### Notes

- Stage 1 (Decision Tree) acts as a fast filter for benign traffic.
- Stage 2 (FT-Transformer) performs deeper classification on suspicious samples.
- The pipeline achieves **low false positive rate (0.19%)** with strong detection performance.


## Detailed Benchmark Results For Tactic 13 (ViT)

### Dataset Distribution

| Label | Count |
|------|------:|
| BENIGN | 2,273,097 |
| DoS Hulk | 231,073 |
| PortScan | 158,930 |
| DDoS | 128,027 |
| DoS GoldenEye | 10,293 |
| FTP-Patator | 7,938 |
| SSH-Patator | 5,897 |
| DoS slowloris | 5,796 |
| DoS Slowhttptest | 5,499 |
| Bot | 1,966 |
| Web Attack – Brute Force | 1,507 |
| Web Attack – XSS | 652 |
| Infiltration | 36 |
| Web Attack – SQL Injection | 21 |
| Heartbleed | 11 |

**Number of Classes:** 15  
**Feature Count:** 78  
**Input Shape:** (1, 10, 10)  
**Device:** CUDA  
**Model Parameters:** 312,335  

---

### Training Summary

| Epoch | Loss | Train Accuracy |
|------:|------:|--------------:|
| 1 | 0.4251 | 0.8998 |
| 5 | 0.0829 | 0.9677 |
| 10 | 0.0612 | 0.9736 |
| 15 | 0.0544 | 0.9763 |

---

### Per-Class Performance

| Class | Detection Rate | False Positive Rate |
|------|---------------:|--------------------:|
| BENIGN | 0.9780 | 0.0271 |
| Bot | 0.3846 | 0.0000 |
| DDoS | 0.9860 | 0.0002 |
| DoS GoldenEye | 0.9756 | 0.0001 |
| DoS Hulk | 0.9857 | 0.0090 |
| DoS Slowhttptest | 0.9810 | 0.0003 |
| DoS slowloris | 0.9655 | 0.0001 |
| FTP-Patator | 0.9879 | 0.0010 |
| Heartbleed | 0.0000 | 0.0000 |
| Infiltration | 0.0000 | 0.0000 |
| PortScan | 0.9573 | 0.0078 |
| SSH-Patator | 0.9649 | 0.0007 |
| Web Attack – Brute Force | 0.0000 | 0.0000 |
| Web Attack – SQL Injection | 0.0000 | 0.0000 |
| Web Attack – XSS | 0.0000 | 0.0000 |

---

### Overall Performance

| Metric | Value |
|--------|------:|
| Accuracy | 0.9767 |
| Macro Detection Rate | 0.6111 |
| Macro False Positive Rate | 0.0031 |

---

### Notes

- Strong overall accuracy but **low macro detection rate**, indicating poor performance on rare classes.
- Several minority classes (e.g., Heartbleed, Infiltration, Web Attacks) were not detected.
- Likely affected by **severe class imbalance** in the dataset.


