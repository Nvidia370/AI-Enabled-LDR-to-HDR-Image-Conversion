# SwinHDR: LDR to HDR Image Reconstruction using Swin Transformer

This repository demonstrates how to use a pretrained Swin Transformer to convert a stack of three aligned LDR (Low Dynamic Range) images into an HDR (High Dynamic Range) image. The model is fine-tuned using the Kalantari HDR dataset.

---

## 🌐 Dataset

We use the **Kalantari et al. HDR dataset** which contains multiple sets of LDR images and corresponding ground truth HDR images.

📦 **Download Dataset**: [Kalantari HDR Dataset](https://www.robots.ox.ac.uk/~szwu/storage/18_hdr/kalantari_dataset.zip)  
📂 Place extracted files in:  
```
/content/drive/MyDrive/kalantari_dataset/train
```

---

## 🛠️ Dependencies

Install the following libraries to run the code successfully in Google Colab or a local environment:
```bash
pip install torch torchvision timm numpy opencv-python imageio OpenEXR==1.3.2 Imath
```

---

## 📁 Project Structure Overview

### 🔹 Mounting Google Drive
To load the dataset and save models/checkpoints directly to Google Drive.

### 🔹 Dataset Loader (`HDRDataset`)
This custom PyTorch `Dataset` class reads each scene consisting of three aligned LDR images and a corresponding HDR ground truth image in `.exr` or `.hdr` format. All images are resized to 192×192 resolution.

### 🔹 Model Architecture (`SwinHDR`)
We utilize a pretrained `swinv2_base_window12_192_22k` model from `timm`.  
- The input is modified to accept 9-channel data (3 LDR images × 3 channels).
- The final output is reshaped to generate a 3-channel HDR image.
- A `ReLU` is used to ensure non-negative HDR predictions.

### 🔹 Loss Function (`HDRLoss`)
The model is trained using L1 loss computed in the logarithmic domain (`log(1 + x)`), which helps balance low and high-intensity pixel values.

### 🔹 HDR Saving (`save_exr`)
Predicted HDR images are saved as `.exr` using the OpenEXR and Imath libraries.

### 🔹 Tone Mapping
Tone mapping is applied to convert HDR into viewable LDR previews using Reinhard’s global operator:  
```
ldr = hdr / (1 + hdr)
```

### 🔹 Visualization & Checkpoints
A preview image is saved alongside the `.exr` image at every 50th epoch. Final weights and predictions are also saved.

---

## 🚀 Training

The model is loaded from a checkpoint at **epoch 600** and fine-tuned for **600 more epochs** using AdamW optimizer and gradient clipping. Losses and predictions are logged during training.

Model checkpoints and results are saved to:
```
/content/drive/MyDrive/swinv2_results
```

---

## 🎥 Project Presentation

[🔗 Click here to preview the PowerPoint presentation](https://docs.google.com/presentation/d/YOUR_SLIDE_ID/preview)

[🔗 Click here to preview the Poster](https://docs.google.com/presentation/d/e/2PACX-1vQ0y9qQJK25ALyoT93x_XyvIYUizQOeRWZntH3uYfeC-ImBIfnSTYVgN9FzLI11Hw/pub?start=false&loop=false&delayms=3000)

> No need to download — live preview in browser.

---

## 👨‍💻 Authors

- Darshan S  
- Iffrahkhan H  
- Shreya P  
- Aishwarya J  

---

Feel free to fork this repository, raise issues, and suggest improvements. Happy experimenting with HDR!
