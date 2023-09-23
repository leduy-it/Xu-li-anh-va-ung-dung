# SoICT Hackathon 2023 - Vietnamese Handwritten Text Recognition

## Team name: Couhp_AIO
### Team member:
- Nguyen Bao Phuoc
- Le Van Duy
- Pham Huu Hung

## Requirements
create conda virtual environment
```
conda create -n <environment-name> --file environment.yml
```
K-fold Dataset for deeptext model: Unzip k-fold ([link](https://drive.google.com/drive/folders/1Z1qO-hk6cRwOELIjY_NxTSoTaX51SKIE?usp=drive_link)) to get 5 sub-folder and
put them into ./data/kfold_lmdb folder like this:
```
├───data
│   ├───kfold_lmdb
│   │   ├───1
│   │   │   ├───train
│   │   │   │   └───provided
│   │   │   └───val
│   │   │       └───provided
│   │   ├───2
│   │   │   ├───train
│   │   │   │   └───provided
│   │   │   └───val
│   │   │       └───provided
│   │   ├───3
│   │   │   ├───train
│   │   │   │   └───provided
│   │   │   └───val
│   │   │       └───provided
│   │   ├───4
│   │   │   ├───train
│   │   │   │   └───provided
│   │   │   └───val
│   │   │       └───provided
│   │   └───5
│   │       ├───train
│   │       │   └───provided
│   │       └───val
│   │           └───provided
│   └─── ...
└─── ...
```

## Training
To training 5-fold with image size 32x100, run:
```
python kfold_train.py --train_kfold data/kfold_lmdb --select_data provided --batch_ratio 1.0 --Transformation TPS --FeatureExtraction ResNet --SequenceModeling BiLSTM --Prediction Attn --PAD --sensitive --adam --augment --rgb
```
To training 5-fold with image size 45x150, run:
```
python kfold_train.py --imgH 45 --imgW 150 --train_kfold data/kfold_lmdb --select_data provided --batch_ratio 1.0 --Transformation TPS --FeatureExtraction ResNet --SequenceModeling BiLSTM --Prediction Attn --PAD --sensitive --adam --augment --rgb
```

## Infer
To infer 10 fold (5 fold 32x100, 5 fold 45x150), run:
```
python ensemble_prediction.py --image_folder [Path to folder contain test image] --workers 0 --saved_model saved_models\TPS-ResNet-BiLSTM-Attn-Seed1111_32x100_AIOaugment_fold_1\best_accuracy.pth --batch_size 64 --Transformation TPS --FeatureExtraction ResNet --SequenceModeling BiLSTM --Prediction Attn --sensitive --PAD
```
where **[Path to folder contain test image]** is path to image folder. After running this file,
10 prediction files will be generated in ./saved_predictions folder (Use for ensemble)

## Ensemble
To ensemble 3 model (Deeptext, Abinet and SVTR), run:
```
python ensemble_10fold_models.py
```