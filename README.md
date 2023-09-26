# SoICT Hackathon 2023 - Vietnamese Handwritten Text Recognition

## Team name: Couhp_AIO
### Team member:
- Nguyen Bao Phuoc
- Le Van Duy
- Pham Huu Hung

# The simplest way for the organizers to re-implement on docker image:

After running docker run command like this:

```
sudo docker run --gpus all -it bkai_couhp_v5:latest
```

## To inference:

With gpu, run this command:
```
bash inference.sh
```
With cpu, run this command:

```
bash inference_cpu.sh
```

## To ensemble and get the final result:

Run this command:


```
bash ensemble.sh
```
After run ensemble.sh command, we will get the final result in directory path is working as txt file(prediction.txt)


## If organizers want to training from scratch


To re-training, run this command:

```
bash train.sh
```




------------------------------------------------------------------------------------------------------------------------------------------------
For more details each model command is below:



# Model Deeptext
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
python ensemble_prediction.py --image_folder data/public_test_data/new_public_test --workers 0 --saved_model saved_models\TPS-ResNet-BiLSTM-Attn-Seed1111_32x100_AIOaugment_fold_1\best_accuracy.pth --batch_size 64 --Transformation TPS --FeatureExtraction ResNet --SequenceModeling BiLSTM --Prediction Attn --sensitive --PAD
```
where **[Path to folder contain test image]** is path to image folder. After running this file,
10 prediction files will be generated in ./saved_predictions folder (Use for ensemble)
    


# Model Abinet
Pretrained model we use can download here: ([Model download link](https://paddleocr.bj.bcebos.com/rec_r45_abinet_train.tar))
## Train model with K-fold Cross Validation with Single GPU training

To train model abinet with 10 fold cross validation, run this command:
```
python3 tools/train_kfold.py
```




## Inference K-fold Cross Validation Model

To get predict model abinet with 10 fold cross validation, run this command:

```
python3 tools/predictor_kfold_without_softmax.py
```










# Model SVTR-Large
Pretrained model we use can download here: ([Model download link](https://paddleocr.bj.bcebos.com/PP-OCRv3/chinese/rec_svtr_large_none_ctc_en_train.tar))
## Train model with K-fold Cross Validation with Single GPU training
```
python3 tools/train_kfold_svtr_large.py
```


## Inference K-fold Cross Validation Model
To get predict model svtr with 10 fold cross validation, run this command:
```
python3 tools/predictor_kfold_without_softmax_SVTR.py
```



# Ensemble
## After inferring the 3 models, we proceed to ensemble the results by running the following command:
To ensemble 3 model (Deeptext, Abinet and SVTR), run:
```
python ensemble_10fold_models.py
```
