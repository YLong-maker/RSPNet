# RSPNet: Towards Universal Lightweight Road Surface Perception Network
## Abstract:

Accurate and real-time road surface condition perception is crucial for the planning and control systems in autonomous driving. However, most existing deep learning models rely on general-purpose vision backbones, which often incur excessive computational overhead. Moreover, the inherent low-pass filtering nature of standard convolutions induces a "Micro-defect Over-smoothing Bias" (MOB), leading to the continuous attenuation and eventual loss of fine-grained textures during feature extraction. To address these issues, this paper proposes RSPNet, a universal lightweight network tailored for road surface perception tasks. This method introduces Wavelet Transform Convolution (WTConv) for frequency-domain decomposition and feature decoupling, explicitly preserving high-frequency edge details to prevent feature smoothing. Concurrently, the model employs the Star Operation to achieve implicit high-dimensional nonlinear feature mapping with minimal computational cost, effectively handling complex visual patterns such as mixed wet-and-dry conditions. Furthermore, a multi-scale depthwise convolution module is utilized to adapt to the scale variability of road distresses, while identity mapping is applied to reduce the feature redundancy of high-level semantic information, thereby further accelerating inference speed. In addition, to mitigate the negative impact of long-tail distributions on classification model evaluation, we constructed the RSCD-Expand dataset, achieving better class balance by supplementing approximately 33,000 field-collected road surface samples. We comprehensively evaluated the model across road surface classification, 3D reconstruction, and semantic segmentation tasks, where RSPNet consistently achieved state-of-the-art performance. In the road surface classification experiments, the three model variants (S/M/L) achieved Top-1 accuracies of 91.09%, 91.61%, and 92.03%, with only 1.47M, 2.17M, and 3.69M parameters, respectively. In experiments on the expanded dataset, core metrics reflecting class balance capability—F1-Score and Recall—demonstrated consistent improvements, effectively diminishing the classifier's predictive bias towards majority classes. During real-vehicle deployment tests, RSPNet achieved high-precision, real-time road surface classification on an in-vehicle platform, demonstrating excellent deployment feasibility. This paper efficiently resolves the MOB problem while striking an outstanding trade-off with computational efficiency. The project homepage is available at: https://github.com/YLong-maker/RSPNet

### Network framework
<div align="center">
    <img src="https://github.com/user-attachments/assets/7ccdd326-3e76-4df8-a06d-b057144acf41" alt="image" width="1004" height="778" />
</div>

### Full Work Summary
<img width="1004" height="329" alt="image" src="https://github.com/user-attachments/assets/3d0dc9fb-d816-4f14-8617-7208eb735974" />

| Dataset Creation and Processing | Performance of road surface classification on RSCD |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/19049e10-d905-48ca-9eae-02736ac29754" alt="数据采集_01" height="450" /> | <img src="https://github.com/user-attachments/assets/c5a71bf8-f160-47d0-b7fc-58041e0c4725" alt="para" height="450" /> |
</div>

### deployment，Please visit the demo video at https://youtu.be/Nb3Xa4FwHR0
<img width="1004" height="283" alt="image" src="https://github.com/user-attachments/assets/07850c2f-a5ec-478a-9323-0a41d8ef6f5e" />

## Main results

#### 1.Road Surface Classification on [RSCD](https://thu-rsxd.com/rscd/) Dataset

| Model | #Params | FLOPs | Resolution | Top-1 (%) | Weights & Logs |
| :--- | :---: | :---: | :---: | :---: | :---: |
| [RSPNet-S](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 1.41M | 246.4M | 224 x 224 | 91.09 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.71_epoch_97.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/log/RSCNet_S.csv) |
| [RSPNet-M](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 2.17M | 359.3M | 224 x 224 | 91.61 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.86_epoch_96.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/log/RSCNet_M.csv) |
| [RSPNet-L](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 3.69M | 554.9M | 224 x 224 | 92.03 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_90.52_epoch_95.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/log/RSCNet_L.csv) |

#### 2.Road Surface Classification on RSCD-Expand,Expand Data visit at https://pan.baidu.com/s/1mObCyJuiUGCwaPXaCWtVRQ?pwd=9fxh Extraction code: 9fxh 
--来自百度网盘超级会员v6的分享

| Model | Top-1 (%) ↑ | Top-5 (%) ↑ | F1 (%) ↑ | Recall (%) ↑ | Weights & Logs |
| :--- | :---: | :---: | :---: | :---: | :---: |
| [RSPNet-S](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 91.03 | 99.85 | 88.84 | 88.46 | [model](这里替换为S的权重链接) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/log/RSCNet_S.csv) |
| [RSPNet-M](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 91.72 | 99.85 | 89.95 | 89.60 | [model](这里替换为M的权重链接) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/log/RSCNet_M.csv) |
| [RSPNet-L](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 91.87 | 99.82 | 90.01 | 89.62 | [model](这里替换为L的权重链接) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/log/RSCNet_L.csv) |

#### 3. Road Surface Reconstruction on [RSRD](https://thu-rsxd.com/rsrd/) Dataset
Based on the RoadBEV-mono framework.

| Backbone | Params (M) | Abs.err (cm) ↓ | RMSE (cm) ↓ | >0.5cm (%) ↓ |
| :--- | :---: | :---: | :---: | :---: |
| RSPNet-M | 26.3 | 1.774 | 2.019 | 76.03 |

#### 4.Road Surface Segmentation on [IDD](https://www.kaggle.com/code/mitanshuchakrawarty/semantic-segmentation-of-indian-roads) Dataset
Evaluated on two different segmentation frameworks.

**Framework: Semantic FPN**
| Backbone | Params (M) | GFLOPs | mIOU (%) ↑ |
| :--- | :---: | :---: | :---: |
| RSPNet-L | 6.52 | 84.17 | 87.57 |

**Framework: U-Net** 
| Backbone | Params (M) | GFLOPs | mIOU (%) ↑ |
| :--- | :---: | :---: | :---: |
| RSPNet-L | 4.55 | 28.94 | 89.13 |


### Prepare  RSCD Dataset

```text
DATA_SET/
├── train/
│   ├── class1/
│   │   ├── image1.jpg
│   │   ├── image2.jpg
│   │   └── ...
│   ├── class2/
│   │   └── ...
│   └── ...
├── val/
│   ├── class1/
│   │   ├── image1.jpg
│   │   ├── image2.jpg
│   │   └── ...
│   ├── class2/
│   │   └── ...
│   └── ...
└── test/
    ├── class1/
    │   ├── image1.jpg
    │   ├── image2.jpg
    │   └── ...
    ├── class2/
    │   └── ...
    └── ...

