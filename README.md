# TP4 · CNNs for Skin Lesion Classification

This ITBA assignment compares an MLP and a convolutional neural network on nine classes of skin lesion images from the course ISIC dataset. The notebook contains the full experiment, saved outputs, and analysis.

## Get the Data

Download the ZIP from the [course repository](https://github.com/cselmo/CNN-en-Skin-Dataset) and extract it. Copy `data/Split_smol/` into this project so that the notebook can find both `data/Split_smol/train/` and `data/Split_smol/val/`. Each folder contains one subfolder per class. The dataset and trained models are kept local.

## Run the Notebook

From the project root, create a Python environment, install the dependencies, and open the notebook.

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter notebook CNN_en_Skin_Dataset.ipynb
```

On Windows, activate the environment with `.venv\Scripts\activate` before installing the requirements.

Select the project environment as the notebook kernel and run the cells from top to bottom. The saved results were produced on CPU with PyTorch 2.2.2 and the original notebook settings. Keep the split seed at `42`. Runtime depends on the machine.

## Experiment and Results

- The notebook removes 34 duplicate copies and creates stratified train, validation, and test splits of 506, 169, and 169 images. Input normalization uses training statistics. Augmentation is applied only to training images after the split.
- The experiments compare the MLP and CNN, including their sensitivity to image shifts. They also vary CNN depth, kernel size, filter count, image resolution, and augmentation. Validation is used for model selection. The official test split for this run is evaluated at the end, with scores averaged across five training seeds.
- The selected CNN used eight initial filters and no augmentation. It reached **65.56% mean test accuracy**, compared with **47.81%** for the MLP. These results come from one dataset split. The notebook discusses variation across seeds, per-class errors, and limitations.

Starter notebook and dataset layout provided by the [ITBA course repository](https://github.com/cselmo/CNN-en-Skin-Dataset).
