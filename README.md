# Mô hình Abinet
Mô hình pretrained nhóm sử dụng BTC có thể tải và kiểm tra ở đây ( [Model download link](https://paddleocr.bj.bcebos.com/rec_r45_abinet_train.tar))
## Train mô hình K-fold Cross Validation với Single GPU training

Lệnh train lại mô hình abinet 10 fold cross validation và hiện tại nhóm hiện đang config cứng
```
python3 tools/train_kfold.py
```




## Inference  mô hình K-fold Cross Validation(output của lớp linear)

```
python3 tools/predictor_kfold_without_softmax.py
```










# Mô hình SVTR-Large

## Train mô hình K-fold Cross Validation
```
python3 tools/train_kfold_svtr_large.py
```


## Prediction mô hình K-fold Cross Validation(output của lớp linear)
```
python3 tools/predictor_kfold_without_softmax_SVTR.py
```
