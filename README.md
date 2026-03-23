# RSPNet: Towards Universal Lightweight Road Surface Perception Network
## Abstract:

Accurate and real-time road surface condition perception is crucial for the planning and control systems in autonomous driving. However, most existing deep learning models rely on general-purpose vision backbones, which often incur excessive computational overhead. Moreover, the inherent low-pass filtering nature of standard convolutions induces a "Micro-defect Over-smoothing Bias" (MOB), leading to the continuous attenuation and eventual loss of fine-grained textures during feature extraction. To address these issues, this paper proposes RSPNet, a universal lightweight network tailored for road surface perception tasks. This method introduces Wavelet Transform Convolution (WTConv) for frequency-domain decomposition and feature decoupling, explicitly preserving high-frequency edge details to prevent feature smoothing. Concurrently, the model employs the Star Operation to achieve implicit high-dimensional nonlinear feature mapping with minimal computational cost, effectively handling complex visual patterns such as mixed wet-and-dry conditions. Furthermore, a multi-scale depthwise convolution module is utilized to adapt to the scale variability of road distresses, while identity mapping is applied to reduce the feature redundancy of high-level semantic information, thereby further accelerating inference speed. In addition, to mitigate the negative impact of long-tail distributions on classification model evaluation, we constructed the RSCD-Expand dataset, achieving better class balance by supplementing approximately 33,000 field-collected road surface samples. We comprehensively evaluated the model across road surface classification, 3D reconstruction, and semantic segmentation tasks, where RSPNet consistently achieved state-of-the-art performance. In the road surface classification experiments, the three model variants (S/M/L) achieved Top-1 accuracies of 91.09%, 91.61%, and 92.03%, with only 1.47M, 2.17M, and 3.69M parameters, respectively. In experiments on the expanded dataset, core metrics reflecting class balance capability—F1-Score and Recall—demonstrated consistent improvements, effectively diminishing the classifier's predictive bias towards majority classes. During real-vehicle deployment tests, RSPNet achieved high-precision, real-time road surface classification on an in-vehicle platform, demonstrating excellent deployment feasibility. This paper efficiently resolves the MOB problem while striking an outstanding trade-off with computational efficiency. The project homepage is available at: https://github.com/YLong-maker/RSPNet

### Network framework
<div align="center">
    <img width="1525" height="1179" alt="image" src="https://github.com/user-attachments/assets/a129045a-7ff1-4498-a606-46d3d6f2dd01" />
    </div>

### Full Work Summary
<img width="2137" height="702" alt="image" src="https://github.com/user-attachments/assets/fd2b77ae-82b5-4d35-8637-7557cbef5624" />

| Dataset Creation and Processing | Performance of road surface classification on RSCD |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/19049e10-d905-48ca-9eae-02736ac29754" alt="数据采集_01" height="450" /> | <img alt="para_vs_top1" src="https://github.com/user-attachments/assets/913432fa-75d3-4af1-b3d9-82bbb60c562d" height="450" /> |

### deployment，Please visit the demo video at https://youtu.be/Nb3Xa4FwHR0
<img width="2448" height="676" alt="image" src="https://github.com/user-attachments/assets/37db13b7-436c-4983-84ce-6f2525191952" />

## Main results

#### 1.Road Surface Classification on [RSCD](https://thu-rsxd.com/rscd/) Dataset

| Model | #Params | FLOPs | Top-1 (%) ↑ | Top-5 (%) ↑ | Recall (%) ↑ | F1 (%) ↑ | Throughput (img/s) ↑ | Weights & Logs |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| [RSPNet-S](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 1.41M | 246.4M | 91.09 | 99.81 | 87.73 | 88.21 | 10791 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.71_epoch_97.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSCNet_S.csv)|
| [RSPNet-M](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 2.17M | 359.3M | 91.61 | 99.85 | 88.52 | 89.00 | 8048 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.86_epoch_96.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSCNet_M.csv) |
| [RSPNet-L](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 3.69M | 554.9M | 92.03 | 99.82 | 89.05 | 89.48 | 5844 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_90.52_epoch_95.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSCNet_L.csv) |

#### 2.Road Surface Classification on RSCD-Expand Dataset, Expand Data visit at https://pan.baidu.com/s/1mObCyJuiUGCwaPXaCWtVRQ?pwd=9fxh Extraction code: 9fxh

| Model | Top-1 (%) ↑ | Top-5 (%) ↑ | F1 (%) ↑ | Recall (%) ↑ | Weights & Logs |
| :--- | :---: | :---: | :---: | :---: | :---: |
| [RSPNet-S](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 91.03 | 99.85 | 88.84 | 88.46 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_88.92_epoch_90_ex.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSPNet_S_ex.csv) |
| [RSPNet-M](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 91.72 | 99.85 | 89.95 | 89.60 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.49_epoch_94_ex.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSPNet_M_ex.csv) |
| [RSPNet-L](https://github.com/YLong-maker/RSPNet/blob/main/models/rspnet.py) | 91.87 | 99.82 | 90.01 | 89.62 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.96_epoch_93_ex.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSPNet_L_ex.csv) |

#### 3. Road Surface Reconstruction on [RSRD](https://thu-rsxd.com/rsrd/) Dataset

**Framework: [RoadBEV](https://ieeexplore.ieee.org/document/10618926)-mono** 

| Backbone | Params (M) | Abs.err (cm) ↓ | RMSE (cm) ↓ | >0.5cm (%) ↓ | weight |
| :--- | :---: | :---: | :---: | :---: | :---: |
| RSPNet-M | 26.9 | 1.905 | 2.139 | 76.99 | [weight](https://github.com/YLong-maker/RSPNet/releases/download/weight/checkpoint_epoch33_004920.ckpt) |

#### 4.Road Surface Segmentation on [IDD](https://www.kaggle.com/code/mitanshuchakrawarty/semantic-segmentation-of-indian-roads) Dataset
Evaluated on two different segmentation frameworks.

**Framework: [Semantic FPN](https://arxiv.org/abs/1901.02446)**
| Backbone | Params (M) | GFLOPs | mIOU (%) ↑ | Weights |
| :--- | :---: | :---: | :---: | :---: |
| RSPNet-L | 6.52 | 84.17 | 87.57 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/epoch_46_miou_0.9065_.pth) |

**Framework: [U-Net](https://arxiv.org/abs/1505.04597)** 
| Backbone | Params (M) | GFLOPs | mIOU (%) ↑ | Weights |
| :--- | :---: | :---: | :---: | :---: |
| RSPNet-L | 4.55 | 28.94 | 89.13 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/epoch_50_miou_0.9090.pth) |

### Environment Setup

We recommend using [Anaconda](https://www.anaconda.com/) to manage your environment. The code has been tested with **Python 3.8.20**.

#### 1. Create a Conda Environment

```bash
conda create -n rspnet python=3.8.20
conda activate rspnet
```
#### 2. Install Dependencies
You can install them via pip. Note that PyTorch with CUDA 11.8 requires a specific index URL:
```bash
# Install PyTorch with CUDA 11.8
pip install torch==2.3.0+cu118 --extra-index-url [https://download.pytorch.org/whl/cu118](https://download.pytorch.org/whl/cu118)
```
#### 3.Install other requirements
```bash
pip install open3d==0.11.2 Pillow==10.4.0 pywaveletsf==1.4.1
```

## Dataset Prepared

### 1. Prepare  RSCD/RSCD-Expand Dataset

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
```
### 2. Prepare RSRD Dataset
```text
Please follow the dataset setup method of the RoadBEV open-source code
``` 
### 3. Prepare IDD Dataset

Please organize the IDD dataset in the standard VOC format as shown below:

```text
dataset/
└── VOC2012/
    ├── ImageSets/
    │   └── segmentation/
    │       ├── test.txt
    │       ├── train.txt
    │       └── val.txt
    ├── JPEGImages/
    │   ├── Image_0.jpg
    │   ├── Image_1.jpg
    │   ├── Image_2.jpg
    │   ├── Image_3.jpg
    │   └── Image_4.jpg
    └── SegmentationClass/
        ├── Image_0.png
        ├── Image_1.png
        ├── Image_2.png
        ├── Image_3.png
        └── Image_4.png
``` 
