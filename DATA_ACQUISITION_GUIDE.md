# 📊 Data Acquisition Guide for RetailEye

## 🚨 Why You Need More Data

**Current Status:** 13 training images  
**Result:** Model has ZERO accuracy (mAP = 0)  
**Reason:** Deep learning requires hundreds to thousands of examples

### Data Requirements by Use Case

| Use Case | Minimum Images | Recommended Images | Expected Accuracy |
|----------|---------------|-------------------|-------------------|
| Pipeline Testing | 10-20 | N/A | 0% (doesn't learn) |
| Proof of Concept | 100-200 | 300+ | 30-50% mAP |
| Production Demo | 500+ | 1000+ | 60-75% mAP |
| Production System | 1000+ | 5000+ | 75-90% mAP |

**Your current 13 images = Pipeline Testing Only!**

---

## 📦 Option 1: Use Public Retail Datasets (RECOMMENDED)

### A. Grocery Store Dataset
- **Source:** [Roboflow Universe - Grocery Dataset](https://universe.roboflow.com/)
- **Size:** 1000+ images
- **Classes:** Common grocery items
- **Format:** COCO, YOLO, Pascal VOC
- **License:** Various (check individual datasets)

**How to use:**
1. Search "grocery products" or "retail items" on Roboflow Universe
2. Download in COCO JSON format
3. Place images in `data/images/train/`
4. Place annotations JSON in `data/raw_annotations/`
5. Update `data/vista.yaml` with class names
6. Run `python convert_data.py`

### B. SKU-110K Dataset
- **Source:** [SKU-110K on GitHub](https://github.com/eg4000/SKU110K_CVPR19)
- **Size:** 11,762 images, 1.7M bounding boxes
- **Classes:** Generic retail products in dense scenes
- **Use Case:** Perfect for cluttered checkout scenarios
- **Format:** CSV (need conversion to COCO)

### C. Freiburg Groceries Dataset
- **Source:** [Freiburg Groceries](http://aisdatasets.informatik.uni-freiburg.de/freiburg_groceries_dataset/)
- **Size:** 5000+ images, 25 product classes
- **Format:** Images with labels
- **Good for:** Controlled environment training

### D. Grozi-120 / WebMarket Datasets
- **Source:** Various academic sources
- **Size:** 100-1000 images per class
- **Classes:** Packaged retail products

---

## 📦 Option 2: Collect Your Own Data

### A. Web Scraping (Legal & Ethical)

```python
# Example: Using Bing Image Search API or similar
# 1. Get API key (free tier available)
# 2. Search for product images
# 3. Download and annotate

import requests
from pathlib import Path

def download_product_images(query, num_images=100):
    """
    NOTE: Use official APIs with proper authentication
    - Bing Image Search API
    - Google Custom Search API
    - Pexels/Unsplash APIs (CC0 licensed)
    """
    pass  # Implement with proper API
```

**Legal Considerations:**
- Use API services (don't scrape without permission)
- Respect robots.txt
- Use CC0 or properly licensed images
- For competition: ensure data usage rights

### B. Manual Photography

**Setup:**
1. Collect 10-20 different products
2. Set up a neutral background
3. Use consistent lighting

**Capture Strategy:**
- Multiple angles per product (5-10)
- Different lighting conditions (3-5)
- Varying distances (close, medium, far)
- With and without clutter
- Different backgrounds

**Math:** 20 products × 10 angles × 3 lighting = 600 images

### C. Synthetic Data Generation

```python
# Using imgaug or albumentations for augmentation
from PIL import Image
import albumentations as A

# Example augmentation pipeline
transform = A.Compose([
    A.RandomRotate90(p=0.5),
    A.Flip(p=0.5),
    A.RandomBrightnessContrast(p=0.8),
    A.GaussNoise(p=0.5),
    A.MotionBlur(p=0.2),
])

# This can multiply your dataset by 10-50x
```

**Tools:**
- **Unity Perception** - Generate synthetic retail scenes
- **Blender** - 3D product rendering
- **NVIDIA Omniverse** - Photorealistic synthetic data

---

## 📦 Option 3: Data Augmentation (Supplement, Not Replace)

While augmentation CANNOT replace real data, it can supplement:

```python
# In train_model.py, you already have:
mosaic=1.0,  # Stitches 4 images together
mixup=0.1,   # Blends images

# Additional augmentations:
hsv_h=0.015,      # Hue variation
hsv_s=0.7,        # Saturation variation
hsv_v=0.4,        # Value variation
degrees=0.0,      # Rotation
translate=0.1,    # Translation
scale=0.5,        # Scaling
perspective=0.0,  # Perspective transform
flipud=0.0,       # Vertical flip
fliplr=0.5,       # Horizontal flip
```

**Important:** Augmentation multiplies your existing data variations, but if you only have 13 images, even with 50x augmentation, you're effectively training on the same 13 scenes.

---

## 🎯 Quick Start: Download Pre-annotated Dataset

### Recommended: Use Roboflow Grocery Dataset

1. **Go to Roboflow Universe:**
   ```
   https://universe.roboflow.com/
   ```

2. **Search for "grocery" or "retail products"**

3. **Select a dataset with 500+ images**

4. **Download in COCO JSON format:**
   - Format: COCO JSON
   - Include: train/val split
   - Download to your computer

5. **Organize files:**
   ```bash
   # Extract downloaded files
   # Copy images to RetailEye/data/images/train/
   # Copy JSON to RetailEye/data/raw_annotations/train_annotations.json
   ```

6. **Update vista.yaml with new class names**

7. **Run conversion:**
   ```bash
   python convert_data.py
   ```

---

## 🔍 Quality Over Quantity

### Good Dataset Characteristics:

✅ **Diverse Conditions:**
- Different lighting (bright, dim, natural, artificial)
- Multiple angles and orientations
- Various distances from camera
- Cluttered and clean backgrounds
- Different arrangement patterns

✅ **Balanced Classes:**
- Similar number of examples per product
- At least 50 examples per class
- No class should be < 20% of others

✅ **High-Quality Annotations:**
- Tight bounding boxes
- No missing objects
- Consistent labeling
- Correct class assignments

❌ **Bad Dataset:**
- All images look identical
- Single lighting condition
- Same camera angle
- Imbalanced classes (1 class = 90% of data)
- Poor/inconsistent annotations

---

## 📝 Annotation Tools (If Collecting Your Own Data)

### 1. LabelImg (Simple, Desktop)
```bash
pip install labelImg
labelImg
```
- Output: Pascal VOC XML or YOLO TXT
- Good for: Small datasets (< 1000 images)

### 2. CVAT (Powerful, Web-based)
```
https://cvat.org/
```
- Output: COCO JSON, YOLO, Pascal VOC
- Good for: Medium to large datasets
- Features: Semi-automatic annotation, team collaboration

### 3. Roboflow (Cloud, All-in-one)
```
https://roboflow.com/
```
- Features: Annotation + augmentation + export
- Good for: Complete pipeline
- Free tier: Limited images

### 4. Makesense.ai (Free, Browser-based)
```
https://www.makesense.ai/
```
- No sign-up required
- Output: YOLO, COCO
- Good for: Quick annotation

---

## 🚀 Action Plan for Your Project

### If You Have < 1 Week:
1. **Use Roboflow public dataset** (2 hours)
   - Download grocery dataset with 500+ images
   - Already annotated!
   
2. **Adapt to your classes** (1 hour)
   - Update vista.yaml
   - Map classes if needed

3. **Train and evaluate** (3 hours)
   - Run training
   - Check mAP > 0.4

### If You Have 1-2 Weeks:
1. **Collect own data** (3-5 days)
   - Photograph 20-30 products
   - Multiple conditions
   - 500-1000 total images

2. **Annotate** (2-3 days)
   - Use CVAT or Roboflow
   - Quality over speed

3. **Train and iterate** (2-3 days)
   - Train multiple experiments
   - Tune hyperparameters

### If You Have > 2 Weeks:
1. **Combine approaches** (1 week)
   - Use public dataset as base
   - Add your own data
   - Synthetic augmentation

2. **Advanced techniques** (1 week)
   - Multiple model architectures
   - Ensemble methods
   - Test-time augmentation

---

## 🎓 Key Takeaways

1. **13 images = 0% accuracy** - This is a mathematical certainty
2. **100 images = Minimum** - Model might learn basic patterns
3. **500 images = Recommended** - Good accuracy for most cases
4. **1000+ images = Production** - Reliable, robust model

**Your current model has mAP = 0 because it physically cannot learn from 13 examples.**

**Next Step:** Choose Option 1 (public dataset) for fastest results.

---

## 📧 Need Help?

Common Questions:

**Q: Can I just augment my 13 images?**  
A: No. Augmentation creates variations of existing scenes, not new information.

**Q: How long does annotation take?**  
A: ~30 seconds per image for simple products. 500 images = 4-5 hours.

**Q: Can I use images from Google?**  
A: Legally risky. Use APIs or CC0 licensed images.

**Q: What's the absolute minimum to see ANY learning?**  
A: 50-100 images, but expect < 30% accuracy.

**Q: Why does the demo have 13 images?**  
A: To test the conversion pipeline, not for actual training.
