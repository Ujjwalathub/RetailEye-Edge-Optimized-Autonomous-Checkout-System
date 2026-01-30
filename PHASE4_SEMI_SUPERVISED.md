# 🚀 Phase 4: Semi-Supervised Learning Guide

## Overview
Implementing Pseudo-Labeling to expand training data from unlabeled test images.

### The Strategy
- **Teacher Model**: Mosaic_Model_v1 (trained on 99 images)
- **Unlabeled Data**: ~1,300 test images
- **Confidence Threshold**: 65% (only trust high-confidence predictions)
- **Student Model**: New model trained on original + pseudo-labeled data

---

## Execution Steps

### Step 1: Backup (Recommended)
```powershell
# Create backup of current training labels
Copy-Item data\labels\train data\labels\train_backup_phase3 -Recurse
```

### Step 2: Run Auto-Labeler
```powershell
cd RetailEye
python 4_auto_expand.py
```

**Expected Output:**
- ✅ Created ~500-800 new label files
- ✅ Added ~500-800 images to Training Folder

### Step 3: Visual Sanity Check
- Open `data/labels/train/` folder
- Check a few newly generated `.txt` files
- Verify they contain bounding box data (not empty)

### Step 4: Train Student Model
The scripts are already updated for Student Model training:
- `2_train.py` now trains **Student_Model_v2**
- `3_submit.py` now uses **Student_Model_v2** for inference

```powershell
# Train Student (will take longer due to 5x more data)
python 2_train.py

# Generate new submission
python 3_submit.py
```

---

## Configuration Details

### Auto-Expansion Settings (4_auto_expand.py)
```python
MODEL_PATH = 'runs/detect/RetailEye_Runs/Mosaic_Model_v1/weights/best.pt'
CONF_THRESHOLD = 0.65  # Only trust 65%+ confident predictions
```

**Threshold Tuning:**
- **Lower (0.5-0.6)**: More data, but more noise
- **Higher (0.7-0.8)**: Cleaner data, but less volume

### Training Updates (2_train.py)
```python
name='Student_Model_v2'  # Changed from Mosaic_Model_v1
```

### Submission Updates (3_submit.py)
```python
MODEL_PATH = 'runs/detect/RetailEye_Runs/Student_Model_v2/weights/best.pt'
```

---

## Troubleshooting

### Issue: "Teacher model not found"
**Fix**: Ensure Mosaic_Model_v1 exists at:
```
runs/detect/RetailEye_Runs/Mosaic_Model_v1/weights/best.pt
```

### Issue: "CUDA Out of Memory" during Student training
**Fix**: In `2_train.py`, reduce batch size:
```python
batch=8  # Changed from 16
```

### Issue: Very few images added (< 100)
**Possible Causes:**
1. Teacher model has poor performance
2. Confidence threshold too high (65%)
3. Test images very different from training data

**Fix**: Lower confidence threshold in `4_auto_expand.py`:
```python
CONF_THRESHOLD = 0.55  # More lenient
```

---

## Performance Expectations

### Before (Phase 3)
- Training Images: ~99
- Model: Mosaic_Model_v1

### After (Phase 4)
- Training Images: ~600-900 (6-9x increase)
- Model: Student_Model_v2
- Expected Improvement: 5-15% better mAP/accuracy

---

## Next Steps After Phase 4

1. ✅ Compare Student_Model_v2 vs Mosaic_Model_v1 performance
2. 🔄 Optional: Run Phase 4 again (Teacher-Student iteration)
3. 🎯 Fine-tune hyperparameters based on validation metrics
4. 📊 Analyze which classes improved most with pseudo-labeling
