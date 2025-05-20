# 🦐 Instance Segmentation of Shrimp for Detection and Counting

## 🎯 Objective
This project aims to develop a robust deep learning pipeline to:
- **Segment individual shrimps** in images using instance segmentation
- **Count the number of shrimps** per image with high accuracy

## 📂 Dataset Description
- **Source**: Images captured in controlled environments with multiple shrimps per image.
- **Annotation Format**: 
  - Annotated using [LabelMe](http://labelme.csail.mit.edu/Release3.0/) in JSON format.
  - Each shrimp is labeled with **three components**:
    - `Shrimp Shrimp` (main body)
    - `Shrimp length` (length line)
    - `Pancreas` (internal region)

- **Preprocessing**:
  - The three parts of each shrimp were **merged** into a unified polygon.
  - All annotations were converted to **COCO format** with a single category: `shrimp`.
  - Then converted to **YOLOv8 segmentation format**.

## 🧠 Approach Overview

### ✅ Step 1: Data Preprocessing
- Merged multipart shrimp annotations into a single instance mask per shrimp.
- Converted dataset:
  - `LabelMe JSON → COCO Format → YOLOv8 Seg Format`
- Dataset Split:
  - **Train**: 80%
  - **Validation**: 10%
  - **Test**: 10%

### ✅ Step 2: Model Training
- **Model Used**: YOLOv8n-seg (Ultralytics)
- **Training Strategy**:
  - Fine-tuned pretrained COCO weights on shrimp dataset.
  - Applied augmentations: horizontal flip, rotation, brightness adjustments.
- **Metrics Tracked**:
  - Mean Average Precision (**mAP**)
  - Intersection over Union (**IoU**)
  - **Shrimp Count Accuracy**

### ✅ Step 3: Inference
- Performed inference on test images.
- Output:
  - Annotated images with masks and bounding boxes.
  - Count of shrimps per image.
  - Exported results to a **CSV** file.

## 🤖 Model Summary
| Attribute | Details |
|----------|---------|
| Architecture | YOLOv8n-seg |
| Pretrained On | COCO |
| Fine-tuned On | Custom Shrimp Dataset |
| Final Weights | `runs/segment/shrimp_seg_model2/weights/best.pt` |

## 📈 Evaluation Metrics
- **mAP**: Measures average precision across all IoU thresholds.
- **IoU**: Area of overlap between prediction and ground truth masks.
- **Shrimp Count Accuracy**: Accuracy in predicting the correct shrimp count per image.

## 📦 Output Files

| Output Type | Location |
|-------------|----------|
| Annotated Images | `inference_outputs/preds/` |
| Shrimp Count CSV | `inference_outputs/shrimp_counts.csv` |
| Trained Model | `runs/segment/shrimp_seg_model2/weights/best.pt` |

## 📊 Example Output (CSV)

```csv
image_name,shrimp_count
image_001.jpg,4
image_002.jpg,2
image_003.jpg,5
...
