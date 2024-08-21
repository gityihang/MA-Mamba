## Language
- [English](README.md)
- [中文](README_zh.md)

<div align="center">
<h1>A-MC-Mamba </h1>
<h3>An Efficient Water Quality Prediction Model Based on Mamba Model and CBAM</h3>

</div>

### News

# Abstract

As the Pearl River Basin is the source of life in the Guangdong-Hong Kong-Macao Greater Bay Area, the establishment of an accurate predictive model of Pearl River water quality is crucial to ensure its ecological safety and sustainable development. However, the characteristics of water quality data, such as nonlinearity, non-stationarity and multidimensionality, make it difficult for regression analysis to adequately express the water quality data, and time series differencing will lead to information loss. To address the above problems, this paper firstly proposes a data preprocessing method, which constructs a multidimensional water quality dataset from water quality data based on different feature dimensions, so as to facilitate model training and feature extraction; secondly, for the problems of high computational complexity and convergence difficulty of the existing models in expressing multidimensional data, this paper adopts the linear structure of Multi-Channel Mamba (MC-Mamba),which effectively simplifies the computational complexity of water quality data in multi-dimensional space; finally, the Convolutional Block Attention Module (CBAM) is utilized to establish a connection between the features extracted by MC-Mamba in different dimensions. A Convolutional Block Attention Multi-Channel Mamba (A-MC-Mamba) with Convolutional Block Attention is finally constructed. In this paper, the water temperature change, dissolved oxygen content, turbidity and total phosphorus indexes are predicted for 24 hours at public basket water quality testing sites in the Pearl River Basin of Guangxi. The results show that the method in this paper has higher prediction accuracy and more stable prediction performance than the existing models.

<img src="assets/指标预测.png" style="width:60%;"/>





## 1.Data Processing
2D->4D

<img src="assets/数据转化.png" style="width:60%;" />

x1: Channel<br>
x2: Time<br>
x3: Feature

### DATA
```
cd datasets
python datasets.py
```

## 2.Model
### model structure

<img src="assets/A-MC-Mamba流程图.png" style="width:60%;" />

#### The structure of the [Mamba](https://github.com/state-spaces/mamba.git)

`A-MC-Mamba` will adopt the network structure of `Mamba` and `CBAM`, where appropriate modifications will be made to the `input` and `output`, using multidimensional inputs on the input and CBAM attention mechanism on the output.


<img src="assets/网络结构.png" style="width:60%;" />


#### The structure of the [CBAM](https://github.com/xmu-xiaoma666/External-Attention-pytorch.git)


<img src="assets/cbam.png" style="width:60%;" />


## 3.Water Quality Index Prediction Model


### Taking the prediction of  `water temperature` indicators as an example:

| Model | MAE. | MSE. | RMSE. | R². |
|:------------------------------------------------------------------|:-------------:|:----------:|:----------:|:----------:|
| LSTM              | 0.0964 | 0.0160 | 0.1264 | 0.4550 |
| Seq2Seq           | 0.1415 | 0.0305 | 0.1746 | 0.2309 |
| Attention-LSTM    | 0.0807 | 0.0146 | 0.1208 | 0.6455 |
| Attention-Seq2Seq | 0.1351 | 0.0325 | 0.1802 | 0.3101 |
| Attention-CNN-LSTM| 0.0949 | 0.0155 | 0.1246 | 0.6729 |
| `MA-Mamba` | `0.0234` |   `0.0009`   | `0.0296` |  `0.9766` |


### Results of ablation experiments on the model<br>
| Mutil-Channel|Self-Attention|CBAM           | RMSE    | R²%    | 
|-----------|----|--|---|--------|
| √ ||  | 0.0532  | 76.52  |
| |√ || 0.0675  | 63.54  | 
|  | |  √     | 0.0613  | 68.51  | 
| √ | √  || 0.0351 | 94.35 |
|  √ || √ | 0.0296 | 97.66 |

### Train
```
python train.py
```

## 4.Classification Model for Water Quality Indicators
Constructing water quality index level prediction using water quality indicators

| Model | Accuracy. | Recall . | F1-score. | 
|:------------------------------------------------------------------|:-------------:|:----------:|:----------:|
| XGBoost       | 0.97 | 0.97 | 0.96 | 
| Decision Tree | 0.91 | 0.94 | 0.92 | 
| `MA-Mamba `   |`0.91`|`0.93`|`0.92`| 
| BP            | 0.83 | 0.87 | 0.82 | 
| SVM           | 0.75 | 0.76 | 0.74 | 
| KNeighbours   | 0.75 | 0.76 | 0.74 | 
| AdaBoost      | 0.66 | 0.73 | 0.69 | 
| Logistic Regression| 0.66 | 0.66 | 0.64 | 
| Random Forest | 0.39 | 0.56 | 0.42 | 



### Train
```
cd utils

```
