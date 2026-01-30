# 🛒 RetailEye: Autonomous Checkout Vision

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![YOLOv8](https://img.shields.io/badge/YOLO-v8-green)
![PyTorch](https://img.shields.io/badge/PyTorch-CUDA%2012-red)
![Status](https://img.shields.io/badge/Status-Hackathon%20Submission-orange)

**RetailEye** is a computer vision solution designed to automate retail checkout processes by identifying and counting overlapping products on a counter. Built for the **Vista '26 Hackathon** (CodeFest, IIT BHU), this project focuses on **edge-optimization** and **data efficiency**.

---

## 🏆 Key Features

* **⚡ Edge-Optimized Architecture:** Utilizes **YOLOv8-Small**, fine-tuned to run at 30+ FPS on mid-range hardware (NVIDIA RTX 3050 / Jetson Orin), making it practical for real-world POS terminals.
* **🧩 Synthetic Clutter Generation:** Implements aggressive **Mosaic Augmentation (p=1.0)** to simulate complex, multi-object checkout scenarios using only single-object training data.
* **🎯 Zero-Tolerance Counting:** Features a custom inference pipeline with strict confidence and NMS thresholding to minimize False Positives (crucial for exact-count scoring metrics).
* **🔄 Robust Data Pipeline:** Includes a custom parser to convert messy COCO-style annotations into clean YOLO layouts automatically.

---

## 🏗️ Project Architecture

The solution is divided into a modular 3-stage pipeline:

| Stage | Script | Description |
| :--- | :--- | :--- |
| **1. Data Engineering** | `1_fix_data.py` | Parsers JSON annotations, fixes ID mismatches, and generates YOLO labels. |
| **2. Model Training** | `2_train.py` | Trains YOLOv8s with "Mosaic" strategy to learn occlusion handling. |
| **3. Inference** | `3_submit.py` | Generates strict CSV predictions for the Kaggle leaderboard. |

---

## 🛠️ Installation

### Prerequisites
* Python 3.8+
* NVIDIA GPU (Recommended for training)

### Setup
1. **Clone the repository:**
   ```bash
   git clone [https://github.com/YOUR_USERNAME/RetailEye.git](https://github.com/YOUR_USERNAME/RetailEye.git)
   cd RetailEye
