# wildfire-prediction
This is the graduate project work for WildFire detection. This project focuses on detecting wildfire regions from satellite images using deep learning.  
We implemented and compared two architectures:

- **Custom Convolutional Neural Network (CNN)**
- **Fine-tuned ResNet-50** (Transfer Learning + 2-Stage Fine-Tuning)

Our best model, **ResNet-50 Stage-2**, achieved **99.14% test accuracy**, outperforming the widely used Kaggle baseline (~96%).

---

## Dataset
We used the **Wildfire Prediction Dataset** from Kaggle:
- ~42,900 RGB satellite images  
- Two classes: `wildfire`, `nowildfire`

### Preprocessing Included
- Resizing all images to **128×128**
- Normalization using ImageNet mean/std
- Data augmentation:  
  - random horizontal flip  
  - random rotation  
  - brightness jitter  
- Computed class weights for imbalance correction
- Dataset distribution and exploratory analysis

---

## Models Implemented

### **Custom CNN**
A lightweight 3-block convolutional network.

**Test Accuracy:** **97.46%**  
Strong baseline, fast training, good separation between classes.

---

### **ResNet-50 — Transfer Learning**

#### **Stage 1 — Frozen Backbone**
Only the fully-connected classifier was trained.  
Validation accuracy: **~94–95%**

#### **Stage 2 — Fine-Tuning**
We unfroze:
- `layer3`
- `layer4`
- final `fc` layer

And applied:
- **Differential learning rates**
- **ReduceLROnPlateau scheduler**

**Final Test Accuracy:** **99.14%**

---

## Benchmark Comparison (Kaggle Baseline)

Official notebook referenced:  
https://www.kaggle.com/code/samin25/wildfire-prediction

### **Summary Table**

| Model | Test Accuracy | Notes |
|-------|---------------|-------|
| Kaggle Baseline (VGG16/ResNet) | ~96% | Benchmark |
| **Custom CNN** | **97.46%** | Strong internal baseline |
| **ResNet-50 Stage-1** | 94–95% | Head-only training |
| **ResNet-50 Stage-2** | **99.14%** | Best performance |

---

## Future Work

### Multi-Domain Training
Train ResNet-50 on:
- satellite images  
- ground-level / camera wildfire images  

### Domain-Classification Layer
Add a branch that first predicts:
- *Is the image Satellite or Ground-Level?*

Then route input through domain-specific parameters.

### Robustness Evaluation
Test in conditions like:
- smoke  
- haze  
- nighttime imagery  
- partial visibility  

### Deployment
Convert trained model to:
- ONNX  
- TensorRT  
for real-time wildfire monitoring dashboards.



