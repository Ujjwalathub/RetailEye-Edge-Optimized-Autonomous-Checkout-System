# Dataset Expansion & Training Guide

## 🚀 Quick Start - Expand and Train in One Command

```bash
python expand_and_train.py
```

This automated script will:
1. ✅ Check your current dataset size
2. ✅ Recommend optimal target size
3. ✅ Convert more annotations from JSON
4. ✅ Calculate best training parameters for your data
5. ✅ Train with optimized settings
6. ✅ Evaluate and report results

---

## 📊 Manual Dataset Expansion

### Option 1: Expand to Specific Target
```bash
# Expand to 50 images total
python expand_dataset.py --target 50

# Expand to 100 images total
python expand_dataset.py --target 100

# Expand to 200 images total
python expand_dataset.py --target 200
```

### Option 2: Check Statistics Only
```bash
python expand_dataset.py --stats-only
```

### Option 3: Custom Train/Val Split
```bash
# 90% train, 10% validation
python expand_dataset.py --target 100 --split 0.9

# 70% train, 30% validation
python expand_dataset.py --target 100 --split 0.7
```

---

## 🎯 Training Recommendations by Dataset Size

| Images | Epochs | Batch | Augmentation | Expected Results |
|--------|--------|-------|--------------|------------------|
| < 20   | 100    | 4     | Very High    | Basic detection  |
| 20-50  | 80     | 8     | High         | Moderate accuracy |
| 50-100 | 60     | 8     | Moderate     | Good accuracy    |
| 100+   | 50     | 16    | Standard     | Excellent        |

---

## 📝 Step-by-Step Manual Process

### 1. Expand Dataset
```bash
cd RetailEye
python expand_dataset.py --target 100
```

### 2. Verify Expansion
```bash
python expand_dataset.py --stats-only
```

### 3. Train Model
```bash
python train_model.py
```

---

## 🔍 Current Status

Run this to check your dataset:
```bash
python expand_dataset.py --stats-only
```

You should see:
- **TRAIN**: number of images and labels
- **VAL**: number of validation images and labels  
- **UNANNOTATED**: available images to convert (1313 available)

---

## 💡 Tips for Better Results

### If mAP = 0 (Model Not Learning):
```bash
# Need MORE data - expand to 100+ images
python expand_dataset.py --target 100
python train_model.py
```

### If mAP < 0.3 (Poor Performance):
```bash
# Add more images gradually
python expand_dataset.py --target 150
python train_model.py
```

### If mAP > 0.5 (Good Performance):
```bash
# Continue expanding for even better results
python expand_dataset.py --target 200
python train_model.py
```

---

## 🎬 Recommended Workflow

**For first time users:**
```bash
# Start with automated expansion and training
python expand_and_train.py
```

**After initial training:**
```bash
# 1. Check results
# 2. If mAP is low, expand more:
python expand_dataset.py --target 150

# 3. Train again
python train_model.py

# 4. Run inference
python inference.py
```

---

## 📈 What You Have

- **Annotated**: 13 images in JSON format
- **Converted**: 9 images currently in YOLO format  
- **Available**: 1,313 unannotated images ready to use
- **Potential**: Can expand to 1,326 total annotated images!

---

## ⚡ Quick Commands

```bash
# Check current status
python expand_dataset.py --stats-only

# Expand to 50 images and train
python expand_and_train.py

# Manually expand to 100
python expand_dataset.py --target 100

# Train after expansion
python train_model.py

# Run inference on test images
python inference.py
```

---

## 🔧 Troubleshooting

**"Not enough annotations"**
→ You have 13 annotated images in JSON, the scripts will convert them

**"Images not found"**
→ Check that images are in `data/images/train_unannotated/`

**"Training crashes"**
→ Reduce batch size in expand_and_train.py or train_model.py

**"mAP = 0"**
→ Need more training images, run: `python expand_dataset.py --target 100`
