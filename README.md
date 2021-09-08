# STNN
This is the PyTorch implementation of STNN based on ICDM 2021 paper *"Space Meets Time: Local Spacetime Neural Network For Trafﬁc Flow Forecasting"*.

## Installation
```
pip install -r requirements.txt
```

## Requirements
- pytorch (1.7 or later)
- numpy
- prettytable
- tqdm


## Train
Before train, unzip `data/METR-LA.zip`,`data/PeMS-Bay.zip` to `data/METR-LA`, `data/PeMS-Bay`

```
# Train on METR-LA, pred 12 steps (60 mins)
python train.py --data data/METR-LA --t_history 12 --t_pred 12

# Train on PeMS-Bay, pred 12 steps (60 mins)
python train.py --data data/PeMS-Bay --t_history 12 --t_pred 12 --keep_ratio 0.1

# Train on METR-LA and PeMS-Bay together
python train.py --data data/PeMS-Bay data/METR-LA --keep_ratio 0.1
```

## Test
`weights/STNN-combined.state.pt` is the weights file trained on the combined dataset (METR-LA+PeMS-Bay). 
**This single model weight file can perform well on both METR-LA and PeMS-Bay**. For more details, check `TABLE IV` in the paper. 
```
python test.py --data data/METR-LA --model weights/STNN-combined.state.pt
python test.py --data data/PeMS-Bay --model weights/STNN-combined.state.pt
```

## Data transform

Since we use sub-spacetime to train a relative small model, 70% training data is more than enough. Experiment results showing that use 0.2*70% data to train provide the excellent performance.

Check `figs/data ratio.png`
<img src="figs/data ratio.png" alt="data ratio" style="zoom: 10%;" />

We transform raw input data to a collections of sub-spacetime and save to disk before training/testing if data not exists. For METR-LA, transformed 0.2\*70% training set is about 4 GB on disk, 1\*20% validation set is about 6GB, 1\*10% test set is about 3GB. For PeMS-Bay, it's about twice larger than METR-LA.


## Citation
Please cite the following paper if you use the code in your work:
```
@inproceedings{Yang2021space,
  title={Space Meets Time: Local Spacetime Neural Network For Trafﬁc Flow Forecasting.},
  author={Yang, Song and Liu, Jiamou and Zhao, Kaiqi},
  booktitle={ICDM},
  year={2021}
}
```
