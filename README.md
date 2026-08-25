# DWT4 + B-ELM Robust Blind Image Watermarking

A robust **blind image watermarking system** using **4-level Haar Discrete Wavelet Transform (DWT4)** and a **Bounded Extreme Learning Machine (B-ELM)** for watermark embedding and extraction.

The system is designed to evaluate watermark robustness against multiple image-processing and geometric attacks using standard grayscale benchmark images.

---

## 📌 Project Overview

This project implements a machine-learning-based blind watermarking framework in which a binary watermark is embedded into the **LL4 sub-band** obtained from a 4-level Haar Wavelet decomposition.

The watermark is embedded using a **Bounded Extreme Learning Machine (B-ELM)** model and later recovered without requiring the original host image during extraction.

The implementation evaluates:

* Imperceptibility of the watermarked image
* Watermark extraction quality
* Robustness against image-processing attacks
* Robustness against geometric transformations
* PSNR, SSIM, NC and BER
* Embedding, training and extraction time

---

## ✨ Key Features

* 4-level Haar DWT decomposition
* Bounded Extreme Learning Machine (B-ELM)
* Blind watermark extraction
* 32×32 binary watermark
* 512×512 grayscale host images
* Four benchmark images:

  * Lena
  * Barbara
  * Cameraman
  * Boat
* 18 different attacks
* PSNR and SSIM evaluation
* Normalized Correlation (NC)
* Bit Error Rate (BER)
* Embedding and extraction time measurement
* Automatic generation of visualization figures

---

## 🧠 Methodology

The overall workflow is:

```text
Host Image
     │
     ▼
4-Level Haar DWT
     │
     ▼
LL4 Sub-band
     │
     ▼
Feature Construction
     │
     ▼
B-ELM Training
     │
     ▼
Watermark Embedding
     │
     ▼
Watermarked Image
     │
     ▼
Image Attack
     │
     ▼
DWT4 Decomposition
     │
     ▼
B-ELM Prediction
     │
     ▼
Watermark Extraction
     │
     ▼
NC / BER Evaluation
```

The implementation uses a 400-hidden-neuron B-ELM model with fixed random initialization for reproducibility.

---

## 🔬 Watermark Embedding

The host image is decomposed using a **4-level Haar DWT**. The LL4 sub-band is selected for watermark embedding.

The implementation constructs features from:

* Spatial grid coordinates
* Quantized LL4 coefficients
* Scaled coefficient information

The B-ELM model is trained to predict the LL4 coefficients. The binary watermark is then incorporated into the predicted coefficients before inverse DWT reconstruction.

### Main Parameters

| Parameter                  |     Value |
| -------------------------- | --------: |
| Image Size                 | 512 × 512 |
| Watermark Size             |   32 × 32 |
| Wavelet                    |      Haar |
| DWT Level                  |         4 |
| B-ELM Hidden Neurons       |       400 |
| Quantization Parameter (Q) |        96 |
| Embedding Strength (k)     |        36 |
| Random Seed                |        42 |

---

## 🔓 Blind Watermark Extraction

The proposed system performs **blind extraction**, meaning that the original host image is not required for recovering the watermark.

During extraction:

1. The attacked watermarked image is pre-processed depending on the attack.
2. A 4-level Haar DWT is performed.
3. The LL4 coefficients are obtained.
4. The trained B-ELM predicts the expected coefficients.
5. The watermark component is estimated from the coefficient difference.
6. Thresholding and post-processing are applied.
7. The final binary watermark is reconstructed.

The implementation includes adaptive processing such as Gaussian smoothing, median filtering and Otsu thresholding for attacked images.

---

## 📊 Evaluation Metrics

### PSNR

Peak Signal-to-Noise Ratio measures the visual quality of the watermarked image compared with the original host image.

Higher PSNR indicates better imperceptibility.

### SSIM

Structural Similarity Index measures the structural similarity between the original and watermarked images.

Higher SSIM indicates better preservation of image structure.

### Normalized Correlation (NC)

NC measures the similarity between the original and extracted watermark.

A higher NC indicates better watermark recovery.

The implementation uses **NC ≥ 0.90** as the primary pass criterion in the attack benchmark.

### Bit Error Rate (BER)

BER represents the proportion of incorrectly recovered watermark bits.

Lower BER indicates better extraction accuracy.

---

## 🖼️ Benchmark Dataset

The implementation evaluates the method on four standard grayscale benchmark images:

* **Lena**
* **Barbara**
* **Cameraman**
* **Boat**

The notebook loads these images online and resizes them to 512×512 grayscale images.

---

## ⚔️ Attack Suite

The watermarked images are evaluated against 18 attacks:

1. Low-Pass Filter (3×3)
2. Median Filter (aperture=3.0)
3. Median Filter (aperture=5.0)
4. Gaussian Noise 5%
5. Gaussian Noise 10%
6. Salt & Pepper Noise 0.1%
7. Salt & Pepper Noise 0.5%
8. Salt & Pepper Noise 1%
9. Speckle Noise
10. Poisson Noise
11. Sharpening Filter
12. Gamma Correction
13. JPEG Compression
14. Translation / Shift
15. Rotation 90°
16. Rotation 180°
17. Scaling
18. Cropping

These attacks are implemented directly in the experimental benchmark section of the notebook.

---

## 📈 Embedding Results

The system achieved the following results before applying attacks:

| Image     | PSNR (dB) |   SSIM |     NC |    BER |
| --------- | --------: | -----: | -----: | -----: |
| Lena      |     41.60 | 0.9935 | 1.0000 | 0.0000 |
| Barbara   |     41.70 | 0.9954 | 1.0000 | 0.0000 |
| Cameraman |     41.78 | 0.9901 | 1.0000 | 0.0000 |
| Boat      |     41.67 | 0.9954 | 1.0000 | 0.0000 |

These results show high visual similarity between the host and watermarked images, with perfect watermark recovery in the no-attack condition.

---

## 🧪 Robustness Results

Average recovered watermark NC across the 18 attacks:

| Host Image | Minimum NC | Maximum NC | Average NC | Pass Rate |
| ---------- | ---------: | ---------: | ---------: | --------: |
| Lena       |     0.8304 |     0.9609 |     0.9390 |     88.9% |
| Barbara    |     0.6459 |     0.9576 |     0.9147 |     83.3% |
| Cameraman  |     0.8926 |     0.9592 |     0.9424 |     83.3% |
| Boat       |     0.7138 |     0.9576 |     0.9091 |     72.2% |

The highest average NC was obtained for **Cameraman (0.9424)**, followed by **Lena (0.9390)**.

---

## 📁 Repository Structure

```text
DWT4-BELM-Watermarking/
│
├── README.md
├── DWT4_BELM_Watermarking.ipynb
│
├── results/
│   ├── lena_attacked_images.png
│   ├── lena_extracted_watermarks.png
│   ├── barbara_attacked_images.png
│   ├── barbara_extracted_watermarks.png
│   ├── cameraman_attacked_images.png
│   ├── cameraman_extracted_watermarks.png
│   ├── boat_attacked_images.png
│   └── boat_extracted_watermarks.png
│
└── LICENSE
```

The notebook generates eight visualization figures: attacked-image grids and extracted-watermark grids for all four benchmark images.

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/DWT4-BELM-Watermarking.git
cd DWT4-BELM-Watermarking
```

### 2. Install Dependencies

```bash
pip install numpy opencv-python PyWavelets matplotlib scikit-image
```

### 3. Run the Notebook

The project can be executed using **Google Colab** or a local Jupyter environment.

Open:

```text
DWT4_BELM_Watermarking.ipynb
```

and run the cells sequentially.

---

## 💻 Required Libraries

```python
numpy
opencv-python
PyWavelets
matplotlib
scikit-image
urllib
```

---

## 📷 Output Visualizations

The implementation automatically generates separate figures containing:

* 18 attacked versions of each benchmark image
* 18 corresponding extracted watermarks
* PSNR values
* NC values
* BER values

The generated output consists of **8 separate visualization figures**.

---

## 📌 Results Interpretation

The experimental results demonstrate that the proposed DWT4 + B-ELM framework provides:

* High imperceptibility, with embedding PSNR above 41.5 dB for all four benchmark images.
* High SSIM values, indicating minimal structural distortion.
* Perfect watermark recovery in the absence of attacks.
* Strong robustness against several filtering, noise and compression attacks.
* Reduced extraction performance for some stronger filtering and geometric conditions.

The results also show that robustness varies depending on the host image and attack type, which is important when evaluating a watermarking algorithm under diverse conditions.

---

## 🔮 Future Improvements

Potential improvements include:

* Testing on a larger and more diverse image dataset.
* Comparing B-ELM with conventional ELM and other machine-learning models.
* Optimizing robustness against severe geometric attacks.
* Improving performance under strong filtering and sharpening attacks.
* Adding automatic result export to CSV/Excel.
* Developing a GUI for watermark embedding and extraction.
* Adding additional watermark payload sizes.
* Comparing DWT with DCT, DWT-DCT and other transform-domain techniques.

---

## 👨‍💻 Author

**Purvik Taneja**

B.Tech – Electrical and Electronics Engineering
Minor Specialisation – Artificial Intelligence & Machine Learning
Bharati Vidyapeeth's College of Engineering, GGSIP University

---

## 📜 License

This project is intended for **academic and educational purposes**.

---

## ⭐ Acknowledgement

The project uses standard grayscale benchmark images and evaluates the proposed watermarking method through an extended set of image-processing and geometric attacks.

If you find this project useful, consider giving the repository a ⭐ on GitHub.
