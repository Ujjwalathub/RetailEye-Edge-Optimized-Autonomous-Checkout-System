# RetailEye - Retail Object Detection with YOLO

## 🚀 Quick Start (3 Easy Steps)

### Option 1: Interactive Launcher (RECOMMENDED)
```bash
cd E:\Project\RetailEye
python quick_start.py
```
Follow the interactive menu to augment, train, and run inference!

### Option 2: Command Line
```bash
# Step 1: Check your dataset
python expand_dataset.py --stats-only

# Step 2: Augment to 100+ images (if needed)
python augment_dataset.py --split train --multiplier 10

# Step 3: Train
python train_augmented.py

# Step 4: Run inference
python inference.py
```

---

## ✅ Current Status

**Your Dataset:**
- ✅ **99 training images** (9 original + 90 augmented)
- ✅ **24 validation images** (4 original + 20 augmented)
- ✅ **123 total annotated images**
- ✅ Ready for training!
- 📦 **1,313 unannotated images** available for future expansion

---

## 📚 Documentation

- **[TRAINING_SUMMARY.md](TRAINING_SUMMARY.md)** - Complete training guide
- **[EXPANSION_GUIDE.md](EXPANSION_GUIDE.md)** - Dataset expansion reference
- **[DATA_ACQUISITION_GUIDE.md](DATA_ACQUISITION_GUIDE.md)** - How to get more data

---

## 🎯 Main Scripts

### Dataset Management
| Script | Purpose | Example |
|--------|---------|---------|
| `expand_dataset.py` | Convert JSON → YOLO | `python expand_dataset.py --target 100` |
| `augment_dataset.py` | Create augmented images | `python augment_dataset.py --multiplier 10` |

### Training
| Script | Purpose | Best For |
|--------|---------|----------|
| `train_augmented.py` | Optimized training | **Augmented datasets (RECOMMENDED)** |
| `train_model.py` | Standard training | Large real datasets |
| `expand_and_train.py` | Automated pipeline | Beginners |

### Inference & Evaluation
| Script | Purpose |
|--------|---------|
| `inference.py` | Run detection on test images |
| `evaluate_model.py` | Evaluate model performance |

### Utilities
| Script | Purpose |
|--------|---------|
| `quick_start.py` | Interactive menu launcher |
| `verify_setup.py` | Check environment |

---

## 🎬 Typical Workflow

### First Time Setup
```bash
# 1. Check dataset
python expand_dataset.py --stats-only

# 2. If < 100 images, augment
python augment_dataset.py --split train --multiplier 10

# 3. Train
python train_augmented.py

# 4. Run inference
python inference.py
```

### Improving Performance
```bash
# If mAP < 0.4, augment more
python augment_dataset.py --split train --multiplier 15

# Retrain
python train_augmented.py

# Or annotate real images and expand
python expand_dataset.py --target 200
```

---

## 📊 Expected Results

| Dataset Size | Expected mAP@50 | Status |
|--------------|-----------------|--------|
| < 50 images | 0.2 - 0.3 | ⚠️ Needs improvement |
| 50-100 images | 0.3 - 0.5 | 👍 Functional |
| 100-200 images | 0.5 - 0.7 | ✅ Good |
| 200+ images | 0.7+ | 🎉 Excellent |

**Your current 99 training images should achieve 0.4-0.6 mAP@50**

---

## 💡 Tips & Tricks

### To Get Better Results:
1. **More data beats better algorithms**
   - Augment existing: `python augment_dataset.py --multiplier 15`
   - Or annotate real images from `train_unannotated/`

2. **Train longer if underfitting**
   - Edit `train_augmented.py`: `epochs=150`

3. **Reduce batch if out of memory**
   - Edit `train_augmented.py`: `batch=4`

### Recommended Hardware:
- **GPU**: RTX 3050 or better (6GB+ VRAM)
- **RAM**: 16GB+
- **Storage**: 10GB+ free space

---

## 🗂️ Project Structure

```
RetailEye/
├── quick_start.py              # Interactive launcher
├── train_augmented.py          # Optimized training script
├── augment_dataset.py          # Data augmentation
├── expand_dataset.py           # JSON → YOLO converter
├── inference.py                # Run detections
├── TRAINING_SUMMARY.md         # Complete guide
├── EXPANSION_GUIDE.md          # Dataset expansion reference
│
├── data/
│   ├── images/
│   │   ├── train/              # 99 training images
│   │   ├── val/                # 24 validation images
│   │   └── train_unannotated/  # 1313 available images
│   ├── labels/
│   │   ├── train/              # 99 labels
│   │   └── val/                # 24 labels
│   └── raw_annotations/
│       └── train_annotations.json  # Original 13 annotations
│
└── runs/
    └── detect/
        └── RetailEye_Runs/     # Training results
```

---

## 🆘 Troubleshooting

### "Out of memory" Error
```python
# Edit train_augmented.py
batch: 8 → 4
```

### "Model not learning" (mAP = 0)
```bash
# Need more data or longer training
python augment_dataset.py --multiplier 20
# Edit train_augmented.py: epochs=150
```

### "Training too slow"
```python
# Use smaller model
# Edit train_augmented.py
model_name = 'yolov8n.pt'  # Nano model
```

---

## 📞 Quick Command Reference

```bash
# Dataset
python expand_dataset.py --stats-only          # Check status
python augment_dataset.py --multiplier 10      # Augment data

# Training
python train_augmented.py                      # Train model
python quick_start.py                          # Interactive menu

# Inference
python inference.py                            # Run detections

# Help
python quick_start.py                          # Interactive help
cat TRAINING_SUMMARY.md                        # Read full guide
```

---

## 🎯 Next Steps

**Right now:**
```bash
python train_augmented.py
```

**After training:**
1. Check mAP@50 score in output
2. If > 0.4: Run inference! 🎉
3. If < 0.4: Augment more and retrain

**For production:**
1. Annotate more real images
2. Mix augmented + real data
3. Train for 100+ epochs
4. Achieve 0.7+ mAP@50

---

## 📄 License

MIT License - See LICENSE file for details

---

**🚀 Ready to train? Run: `python train_augmented.py`**

**Need help? Run: `python quick_start.py`**

# RetailEye: Autonomous Checkout System
**Event:** Vista '26 (CodeFest, IIT BHU)  
**Hardware:** Ryzen 5 7000HS + RTX 3050 6GB  
**Model:** YOLOv8-Small

## 🎯 Project Overview
RetailEye is a lightweight, edge-optimized Computer Vision solution designed to automate supermarket checkouts by identifying and counting multiple overlapping objects on a checkout counter.

## 📁 Project Structure
```
RetailEye/
├── data/
│   ├── images/
│   │   ├── train/       # Training images
│   │   ├── val/         # Validation images (20% of training)
│   │   └── test/        # Test images
│   ├── labels/
│   │   ├── train/       # Generated YOLO labels
│   │   └── val/         # Generated YOLO labels
│   ├── raw_annotations/ # Place train_annotations.json here
│   └── vista.yaml       # Dataset configuration
├── submissions/         # Generated CSV files
├── convert_data.py      # COCO to YOLO converter
├── train_model.py       # Training script
└── inference.py         # Inference & submission generator
```

## 🚀 Setup Instructions

### Step 1: Install Dependencies
```bash
# Install PyTorch with CUDA 12.1 (Crucial for GPU acceleration)
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# Install YOLOv8 & Data Tools
pip install ultralytics pandas numpy opencv-python tqdm
```

### Step 2: Prepare Dataset

⚠️ **CRITICAL: Data Requirements**
- **Minimum:** 100+ images per dataset (absolute minimum for training)
- **Recommended:** 500+ images for production-quality models
- **Best Practice:** 1000+ images with diverse conditions

**Current demo dataset (13 images) is for TESTING THE PIPELINE ONLY!**
With 13 images, the model will have **ZERO accuracy**. You MUST acquire more data.

#### Data Preparation Steps:
1. Place your training images in `data/images/train/`
2. **Properly split data:** Move 20% of ANNOTATED images to `data/images/val/`
   - Example: 400 train, 100 val (both with annotations)
3. Place test images in `data/images/test/`
4. Place `train_annotations.json` in `data/raw_annotations/`
5. Update `data/vista.yaml` with correct class names from your category.json

### Step 3: Verify Setup
```bash
python verify_setup.py
```
This will check your environment, GPU, and dataset quality.

### Step 4: Convert Annotations
```bash
python convert_data.py
```
This converts COCO JSON format to YOLO TXT format.

**The script will warn you if:**
- Dataset is too small (< 100 images)
- Classes have insufficient annotations
- Validation labels are empty

### Step 5: Train the Model
```bash
python train_model.py
```

**The script will:**
- Validate dataset BEFORE training
- Block training if critical issues found
- Show real-time warnings about data quality
- Evaluate model AFTER training
- Report if model actually learned (mAP > 0)

Training will take 1-2 hours on RTX 3050. The model will be saved to `RetailEye_Runs/v1_mosaic_strategy/weights/best.pt`

**Hardware Constraints:**
- Batch Size: 8 (reduce to 4 if Out of Memory)
- Image Size: 640 (do not increase)
- Workers: 0 (prevents multiprocessing errors)

### Step 6: Evaluate Model (NEW!)
```bash
python evaluate_model.py
```
This provides comprehensive metrics:
- Overall mAP, Precision, Recall
- Per-class performance
- Performance assessment
- Actionable recommendations

### Step 7: Generate Submission
```bash
python inference.py
```

**The script will:**
- Verify the trained model is loaded (not pre-trained COCO model)
- Validate class names match training data
- Report detection statistics
- Warn if no objects detected

This generates `submissions/submission_v1.csv` for Kaggle submission.

## 🎨 Key Technical Strategy

### Mosaic Augmentation (100%)
Synthetically creates "cluttered" training samples from single-object source images by stitching 4 images together.

### Zero-Tolerance Counting
Conservative counting algorithm with 50% confidence threshold to maximize Kaggle score (wrong count = 0 points).

### Model Selection
YOLOv8-Small provides the optimal balance of speed vs. accuracy on 6GB VRAM.

## 📊 Configuration Parameters

### Training (train_model.py)
- **epochs**: 40
- **imgsz**: 640
- **batch**: 16
- **mosaic**: 1.0
- **mixup**: 0.1

### Inference (inference.py)
- **CONF_THRES**: 0.50 (50% confidence)
- **IOU_THRES**: 0.5 (NMS threshold)

## ⚠️ Important Notes

### Dataset Requirements (CRITICAL!)
1. **Minimum Size**: 100+ images (absolute minimum for any learning)
2. **Recommended**: 500+ images for reliable results
3. **Production Quality**: 1000+ images with diverse conditions
4. **Current Demo**: Only 13 images - **WILL NOT WORK** for real training!

### Common Issues & Solutions

#### Issue: mAP = 0 after training
**Causes:**
- Dataset too small (< 100 images)
- All validation labels are empty
- Incorrect label format
- Class IDs mismatched

**Solution:**
1. Run `python verify_setup.py` to identify issues
2. Acquire more training data
3. Ensure validation split has annotations
4. Run `python convert_data.py` to regenerate labels

#### Issue: "Out of Memory" during training
**Solution:**
- Reduce batch size from 8 to 4 in `train_model.py`
- Close other GPU-using applications
- Reduce image size from 640 to 512 (last resort)

#### Issue: Model predicts wrong classes (e.g., "bottle" instead of "Coke_Can")
**Cause:** Using pre-trained COCO model instead of trained model

**Solution:**
1. Check training completed successfully (mAP > 0)
2. Verify `RetailEye_Runs/*/weights/best.pt` exists
3. Run `python inference.py` - it will validate model

### Data Validation Checklist
- [ ] Training images > 100
- [ ] Validation images have annotations (not empty labels)
- [ ] Class names in `vista.yaml` match `train_annotations.json`
- [ ] Labels exist for all training images
- [ ] Test images exist (for inference)

### Training Validation Checklist
- [ ] `verify_setup.py` passes all checks
- [ ] `train_model.py` completes without errors
- [ ] mAP@50 > 0 (check in evaluation)
- [ ] Training plots show decreasing loss
- [ ] Validation metrics improve over epochs

### Inference Validation Checklist
- [ ] Trained model found (not using COCO pre-trained)
- [ ] Class names match training data
- [ ] Detections found in test images
- [ ] Detected classes match your dataset (not "bottle", "book", etc.)

## 📁 New Project Structure
```
RetailEye/
├── data/
│   ├── images/
│   │   ├── train/       # Training images (80% of data, WITH annotations)
│   │   ├── val/         # Validation images (20% of data, WITH annotations)
│   │   └── test/        # Test images (for inference only)
│   ├── labels/
│   │   ├── train/       # Generated YOLO labels for training
│   │   └── val/         # Generated YOLO labels for validation
│   ├── raw_annotations/ # Place train_annotations.json here
│   └── vista.yaml       # Dataset configuration
├── submissions/         # Generated CSV files
├── RetailEye_Runs/      # Training outputs (models, plots, metrics)
├── convert_data.py      # COCO to YOLO converter (with validation!)
├── train_model.py       # Training script (with pre/post validation!)
├── evaluate_model.py    # NEW: Comprehensive model evaluation
├── inference.py         # Inference with model validation
├── verify_setup.py      # Environment checker (enhanced!)
└── README.md
```

## 🎯 Recommended Workflow

### For Development/Testing:
1. `python verify_setup.py` - Check environment
2. `python convert_data.py` - Convert annotations (with warnings)
3. `python train_model.py` - Train with validation
4. `python evaluate_model.py` - Detailed evaluation
5. `python inference.py` - Generate predictions

### For Production:
1. Acquire 500+ images with diverse conditions
2. Properly annotate all data
3. Split 80/20 train/val (both with annotations)
4. Follow development workflow above
5. Iterate based on evaluation metrics

## 🔧 Configuration Parameters
