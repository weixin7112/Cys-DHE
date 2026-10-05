[README.md](https://github.com/user-attachments/files/33050189/README.md)
# Cys-DHE

Cys-DHE is a dual-branch heterogeneous deep ensemble model for predicting **cysteine S-carboxyethylation sites** from protein sequence information.

The model uses **binary-weight encoding (BWE)** to represent 41-aa sequence windows centered on cysteine residues, and integrates two complementary deep-learning branches:

- **CNN-BiLSTM-Attention**
- **CNN-Transformer**

The final prediction probability is obtained using a fixed-weight ensemble:

- CNN-BiLSTM-Attention weight: **0.75**
- CNN-Transformer weight: **0.25**

## Repository structure

```text
Cys-DHE/
├── dataset/
├── model/
├── Cys_DHE_Ensemble.ipynb
└── README.md
```

- `dataset/`: training and independent-test datasets
- `model/`: saved model files
- `Cys_DHE_Ensemble.ipynb`: main notebook for model training, validation, and independent testing

## Dataset

The benchmark dataset contains cysteine-centered peptide windows of length **41 aa**, with the target Cys located at the center.

- **Training set:** 1,204 samples
  - Positive: 602
  - Negative: 602
- **Independent test set:** 913 samples
  - Positive: 83
  - Negative: 830

The dataset used in this study was derived from the benchmark dataset reported by Luo et al.

## Main model settings

| Parameter | Setting |
|---|---|
| Optimizer | Adam |
| Learning rate | 0.0005 |
| Batch size | 64 |
| Maximum epochs | 100 |
| Early-stopping patience | 15 |
| Loss function | CrossEntropyLoss |
| Cross-validation | 5-fold |
| Classification threshold | 0.5 |
| CV split seed | 1949 |
| Training seed | 1254 |
| CNN-BiLSTM-Attention weight | 0.75 |
| CNN-Transformer weight | 0.25 |

## Requirements

The code is implemented in Python with PyTorch. Main dependencies include:

```text
python
torch
numpy
pandas
scikit-learn
```

For exact reproducibility, please use the same package versions as the environment in which the final experiments were conducted.

You can check the versions in your environment with:

```python
import sys
import torch
import numpy as np
import pandas as pd
import sklearn

print("Python:", sys.version)
print("PyTorch:", torch.__version__)
print("NumPy:", np.__version__)
print("Pandas:", pd.__version__)
print("scikit-learn:", sklearn.__version__)
print("CUDA available:", torch.cuda.is_available())

if torch.cuda.is_available():
    print("GPU:", torch.cuda.get_device_name(0))
```

## Usage

### 1. Open the notebook

Open:

```text
Cys_DHE_Ensemble.ipynb
```

The notebook can be run locally or in Google Colab.

### 2. Prepare the data

Place the required training and independent-test FASTA files in the paths expected by the notebook.

Each input sample should correspond to a **41-aa sequence window centered on Cys**.

### 3. Run the notebook

Execute the notebook cells in order.

The workflow includes:

1. Data loading and preprocessing
2. BWE feature encoding
3. Five-fold cross-validation
4. Training of the CNN-BiLSTM-Attention branch
5. Training of the CNN-Transformer branch
6. Fixed-weight probability ensemble
7. Independent-test evaluation
8. Calculation of Sn, Sp, Acc, MCC, AUROC, and AUPR

## Reproducibility

To improve reproducibility:

- the five-fold cross-validation split seed is fixed at **1949**;
- the network training seed is fixed at **1254**;
- the same model architecture and training parameters are used across comparison experiments;
- the independent test set is not used for model training.

## Evaluation metrics

The following metrics are reported:

- Sensitivity (Sn)
- Specificity (Sp)
- Accuracy (Acc)
- Matthews correlation coefficient (MCC)
- Area under the ROC curve (AUROC)
- Area under the precision-recall curve (AUPR)

## Online server

An online prediction server is available at:

http://www.lzzzlab.top/Cys-DHE/

The web server accepts protein sequences and returns candidate cysteine S-carboxyethylation sites and prediction results.

## Source code and data

Repository:

https://github.com/weixin7112/Cys-DHE

## Citation

If you use Cys-DHE in your research, please cite the corresponding manuscript:

> Cys-DHE: A Dual-Branch Heterogeneous Deep Ensemble Model for Cysteine S-Carboxyethylation Site Prediction.

A full citation will be added after publication.

## Contact

For questions about the code or model, please contact the corresponding author through the information provided in the manuscript.
