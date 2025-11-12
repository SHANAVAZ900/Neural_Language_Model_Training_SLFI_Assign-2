# Neural_Language_Model_Training_SLFI_Assign-2
Train a neural language model from scratch using PyTorch. The goal is to demonstrate  understanding of how sequence models learn to predict text and how model design and  training affect performance. 

# Training Report — RNN Character Language Model
 
## Dataset
 - Source: Pride_and_Prejudice-Jane_Austen.txt (loaded from notebook path)
 - Total characters: 711,331
 - Vocabulary (unique characters): 90
 
## Preprocessing
 - Built character vocabulary: char → index and index → char mappings.
 - Encoded full text to integer array.
 - Train/validation split: 90% / 10%
 - Train length: 640,197 characters
 - Val length: 71,134 characters
## - Sequence batching
 - Sequence length: 100
 - Batch size: 64
 - Training samples: 640,197 − 100 = 640,097
 - Validation samples: 71,134 − 100 = 71,034
 - Dataset class: sliding-window char sequences, target is next character (shifted by 1)
 
## Model & Training Configuration
 - Model type: RNN language model (vanilla RNN)
 - Embedding dim: 128
 - Hidden dim: 256
 - RNN layers: 2
 - Output: Linear projection to vocab size (90)
 - Loss: CrossEntropyLoss (token-level)
 - Optimizer: Adam, lr = 0.002
 - Device: as configured in notebook (cuda if available)
 - Epochs run: 3
 
## Loss & Perplexity (per epoch)
# Training loss:
     - Epoch 1: 1.0684411098506064
     - Epoch 2: 0.9748818952246824
     - Epoch 3: 0.9790343199937588
#  Validation loss:
     - Epoch 1: 1.6773965200862369
     - Epoch 2: 1.7171286079797659
     - Epoch 3: 1.714491266388077
#  Validation perplexity:
     - Epoch 1: 5.351605022312573
     - Epoch 2: 5.56851609370519
     - Epoch 3: 5.553849363697093
 
# Best validation perplexity: 5.3516 (Epoch 1)

## Curves
<img width="567" height="432" alt="image" src="https://github.com/user-attachments/assets/dcc8445f-77cf-4ee6-b7d7-f0cdf6731295" />
<img width="575" height="432" alt="image" src="https://github.com/user-attachments/assets/4dcd09ac-2983-45ad-b668-ac6558fa7ee1" />

 - Training Loss curve: see notebook cell with plotting (training loss decreased after epoch 1, then plateaued/slightly rose by epoch 3).
 - Validation Loss curve: see notebook plot (validation loss rose slightly after epoch 1 with minor fluctuation).
 - Validation Perplexity curve: see notebook plot (lowest at epoch 1, small increase at epoch 2, slight decrease at epoch 3).
 - Plots are generated in the notebook (cell containing matplotlib.plot calls).
 
## Analysis & Rationale
 - The model learned quickly in epoch 1 (largest drop in training loss) and reached lowest validation perplexity on epoch 1.
 - Training loss decreased from epoch 1 → 2, then slightly increased by epoch 3 (possible small optimization noise or beginning overfitting).
 - Validation loss and perplexity are higher than training, with val_loss − train_loss ≈ 0.735 on final epoch → indicates moderate overfitting.
 - Best model selection: choose the checkpoint from Epoch 1 (lowest validation perplexity = 5.35). Saving the model at that epoch is recommended rather than the final epoch.
 
## Recommendations / Next Steps
 - Save and restore the best-validation checkpoint during training (early stopping).
 - Regularization: add dropout to RNN and/or embedding, or weight decay in optimizer.
 - Replace vanilla RNN with LSTM/GRU for better sequence modeling.
 - Increase model capacity (or reduce) depending on validation trend, experiment with hidden_dim and num_layers.
 - Tune learning rate and batch size; consider learning rate scheduling.
 - Train for more epochs with early stopping and checkpointing.
 - Consider subword or byte-pair encodings if moving beyond pure character-level modeling.
 
## Fine-grained action items
 - Implement checkpointing to save the best model per validation perplexity.
 - Experiment: LSTM(128,256,2) + dropout=0.2; try lr = 1e-3 and lr scheduler.
 - Track more metrics (per-character accuracy, confusion on common chars) and generate sample text to qualitatively assess generations.
