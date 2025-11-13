# 🧠 Training Report — RNN Character Language Model

## 📘 Dataset
- **Source:** `Pride_and_Prejudice-Jane_Austen.txt` (loaded from notebook path)
- **Total characters:** 711,331  
- **Vocabulary (unique characters):** 90  

---

## ⚙️ Preprocessing
- Built character vocabulary mappings (`char → index` and `index → char`)
- Encoded full text to an integer array
- **Train/validation split:** 90% / 10%
  - **Train length:** 640,197 characters  
  - **Validation length:** 71,134 characters  

### 🧩 Sequence Batching
- Sequence length: 100  
- Batch size: 64  
- Training samples: `640,197 − 100 = 640,097`  
- Validation samples: `71,134 − 100 = 71,034`  
- Dataset class: sliding-window char sequences (target = next character, shifted by 1)

---

## 🧮 Model & Training Configuration
| Parameter | Value |
|:-----------|:-------|
| Model Type | Vanilla RNN (Character-Level Language Model) |
| Embedding Dimension | 128 |
| Hidden Dimension | 256 |
| Number of RNN Layers | 2 |
| Output Layer | Linear projection to vocab size (90) |
| Loss Function | CrossEntropyLoss (token-level) |
| Optimizer | Adam |
| Learning Rate | 0.002 |
| Device | CUDA (if available) / CPU |
| Epochs Run | 3 |

---

## 📉 Loss & Perplexity (per epoch)

| **Metric** | **Epoch 1** | **Epoch 2** | **Epoch 3** |
|:------------|------------:|------------:|------------:|
| Training Loss | 1.0684 | 0.9749 | 0.9790 |
| Validation Loss | 1.6774 | 1.7171 | 1.7145 |
| Validation Perplexity | 5.3516 | 5.5685 | 5.5538 |

**Best Validation Perplexity:** 🏆 **5.3516 (Epoch 1)**

---

## 📊 Curves

| Training & Validation Curves | |
|:--|:--|
| **Training Loss Curve** | <img width="567" height="432" alt="Training Loss" src="https://github.com/user-attachments/assets/dcc8445f-77cf-4ee6-b7d7-f0cdf6731295" /> |
| **Validation Loss / Perplexity Curves** | <img width="575" height="432" alt="Validation Curves" src="https://github.com/user-attachments/assets/4dcd09ac-2983-45ad-b668-ac6558fa7ee1" /> |

**Observations:**
- Training loss decreased after epoch 1, then plateaued or slightly rose by epoch 3.  
- Validation loss rose slightly after epoch 1, showing minor fluctuation.  
- Validation perplexity reached its lowest value at epoch 1 and increased slightly afterward.  
- All plots generated in the notebook (matplotlib).

---

## 🔍 Analysis & Rationale
- The model learned quickly in **epoch 1**, achieving the **lowest validation perplexity (5.35)**.
- Training loss decreased from epoch 1 → 2, then slightly increased at epoch 3 (indicating optimization noise or mild overfitting).
- Validation loss and perplexity remained higher than training, with:


# 🧠 LSTM Character Language Model — Training Report

## 📘 Dataset

**Source:** Pride_and_Prejudice-Jane_Austen.txt (loaded from notebook path)  
**Total characters:** 711,331  
**Vocabulary (unique characters):** 90  

### Preprocessing
- Built character vocabulary (`char → index` and `index → char` mappings)
- Encoded full text to an integer array
- **Train/Validation split:** 90% / 10%
  - **Train length:** 640,197 characters  
  - **Validation length:** 71,134 characters  

**Sequence batching:**
- Sequence length: 100  
- Batch size: 64  
- Training samples: `640,197 − 100 = 640,097`  
- Validation samples: `71,134 − 100 = 71,034`  
- Dataset class: sliding-window character sequences, target = next character (shifted by one)  

---

## ⚙️ Model & Training Configuration

| Parameter | Value |
|:-----------|:-------|
| Model Type | **LSTM Language Model (2-layer)** |
| Embedding Dimension | 128 |
| Hidden Dimension | 256 |
| Number of LSTM Layers | 2 |
| Output | Linear projection → vocab size (90) |
| Loss Function | CrossEntropyLoss (token-level) |
| Optimizer | Adam |
| Learning Rate | 0.002 |
| Device | CPU |
| Epochs Run | 3 |

---

## 📉 Training Results

### Loss & Perplexity (per epoch)

| **Epoch** | **Train Loss** | **Validation Loss** | **Validation Perplexity** |
|:-----------|---------------:|--------------------:|---------------------------:|
| 1 | 1.0546 | 1.7743 | 5.90 |
| 2 | 0.6993 | 2.3211 | 10.19 |
| 3 | 0.5598 | 2.6810 | 14.60 |

**Best Validation Perplexity:** `5.90 (Epoch 1)`

---

## 📊 Curves & Trends
| Training & Validation Curves | |
|:--|:--|
| **Training Loss Curve** | <img width="567" height="432" alt="image" src="https://github.com/user-attachments/assets/457306ce-d443-4653-b028-828f6e39cb3e" />|
| **Validation Loss / Perplexity Curves** | <img width="562" height="432" alt="image" src="https://github.com/user-attachments/assets/dfc66ce0-c6bb-42a6-bd50-d41b8306ecdf" /> |

- **Training Loss Curve:** Decreased steadily across epochs (model learning well).  
- **Validation Loss Curve:** Rose after Epoch 1, showing signs of overfitting.  
- **Validation Perplexity Curve:** Lowest at Epoch 1, increases thereafter.  
- Plots generated within the notebook (using `matplotlib.plot()`).

---

## 🧩 Analysis & Rationale

- The model learned rapidly in **Epoch 1**, achieving lowest validation perplexity (**5.90**).  
- Training loss continued to drop in later epochs, but validation loss **increased**, indicating **overfitting**.  
- The loss gap between training and validation widened from ~0.7 → ~2.1 by the end of training.  
- Root causes:
  - No dropout/regularization
  - Slightly high learning rate (`0.002`)
  - Model capacity (256 hidden units, 2 layers) is relatively large for the dataset size  

**Best checkpoint:** Epoch 1 (lowest validation perplexity and minimal overfitting).  
Subsequent epochs decreased training loss but degraded validation performance.























































