# RSPNet: Towards Universal Lightweight Road Surface Perception Network
## Abstract:

Accurate and real-time road surface condition perception in autonomous driving systems is crucial for trajectory planning and motion control. Data-driven deep learning demonstrates excellent representation capabilities and currently dominates this task. However, researchers frequently transfer general-purpose visual backbone networks directly to these specific tasks. This direct transfer causes severe computational redundancy. Moreover, standard convolutions possess inherent low-pass filtering characteristics that trigger a micro-defect over-smoothing bias. This bias continuously attenuates and loses fine-grained textures during deep feature extraction. To overcome this perception bottleneck, we propose a universal lightweight backbone network, Road Surface Perception Network (RSPNet), targeting road surface feature specificities. First, RSPNet introduces wavelet convolution (WTConv)  that decomposes frequency domains and decouples features. This step explicitly preserves high-frequency edge details to overcome the feature smoothing phenomenon. Second, the model employs a star operation (SO)  that maps implicit high-dimensional nonlinear features under strict computational constraints. This mapping accurately addresses complex visual patterns, including mixed wet and dry conditions. Additionally, a multi-scale depthwise convolution (MSDeConv)  module adapts to the varying scales characterizing road surface defects. Finally, an identity mapping strategy decreases feature redundancy within high-level semantic information. This strategy accelerates inference speed while extracting high-fidelity features. Based on these architectural designs, we construct three extremely lightweight backbone network models: RSPNet-Small (S), RSPNet-Medium (M), and RSPNet-Large (L). These models require only 1.47, 2.17, and 3.69 million parameters, respectively. In addition, to mitigate the negative impact of long-tail distributions on classification model evaluation, we construct a more balanced dataset, RSCD-Expand. This dataset incorporates 32,427 field-collected road surface samples. To verify model universality and effectiveness, we comprehensively evaluate RSPNet across three public datasets using road surface classification, reconstruction, and segmentation tasks. Experimental results indicate that RSPNet achieves the optimal performance trade-off across all tasks. Experiments on the expanded dataset demonstrate that the model achieves significant and consistent improvements in F1-Score and Recall. These two core metrics accurately reflect class balancing capability. These improvements fundamentally diminish the statistical bias and prediction dependence of the classifier on majority class priors. Edge deployment tests under real high-dynamic road conditions show that RSPNet stably achieves an average real-time inference speed of 67.24 frames per second (FPS). This performance confirms the engineering feasibility of the model under severe hardware computational constraints. We provide the project repository at: https://github.com/YLong-maker/RSPNet.

### Network framework
<div align="center">
    <img width="1525" height="1179" alt="image" src="https://github.com/user-attachments/assets/a129045a-7ff1-4498-a606-46d3d6f2dd01" />
    </div>

### Full Work Summary
<img width="2137" height="702" alt="image" src="https://github.com/user-attachments/assets/fd2b77ae-82b5-4d35-8637-7557cbef5624" />

| Dataset Creation and Processing | Performance of road surface classification on RSCD |
| :---: | :---: |
| <img alt="image" src="https://github.com/user-attachments/assets/44702517-0b66-4606-8268-e137078b9996" height="450" /> | <img alt="fig5" src="https://github.com/user-attachments/assets/1c84625b-1b44-433b-a0c5-f6a96d4fd760" height="450" />|

### deployment，Please visit the demo video at https://youtu.be/Nb3Xa4FwHR0
<img width="2437" height="678" alt="image" src="https://github.com/user-attachments/assets/20e59d2c-66aa-4550-8f1f-31b9c2a662dc" />

## Main results

#### 1.Road Surface Classification on [RSCD](https://thu-rsxd.com/rscd/) Dataset

| Model | #Params | FLOPs | Top-1 (%) ↑ | Top-5 (%) ↑ | Recall (%) ↑ | F1 (%) ↑ | Throughput (img/s) ↑ | Weights & Logs |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| RSPNet-S | 1.41M | 246.4M | 91.09 | 99.81 | 87.73 | 88.21 | 10791 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.71_epoch_97.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSCNet_S.csv)|
| RSPNet-M | 2.17M | 359.3M | 91.61 | 99.85 | 88.52 | 89.00 | 8048 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.86_epoch_96.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSCNet_M.csv) |
| RSPNet-L | 3.69M | 554.9M | 92.03 | 99.82 | 89.05 | 89.48 | 5844 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_90.52_epoch_95.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSCNet_L.csv) |

#### 2.Road Surface Classification on RSCD-Expand Dataset
Expand Data visit at https://doi.org/10.5281/zenodo.19177795

| Model | Top-1 (%) ↑ | Top-5 (%) ↑ | F1 (%) ↑ | Recall (%) ↑ | Weights & Logs |
| :--- | :---: | :---: | :---: | :---: | :---: |
| RSPNet-S | 91.03 | 99.85 | 88.84 | 88.46 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_88.92_epoch_90_ex.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSPNet_S_ex.csv) |
| RSPNet-M | 91.72 | 99.85 | 89.95 | 89.60 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.49_epoch_94_ex.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSPNet_M_ex.csv) |
| RSPNet-L | 91.87 | 99.82 | 90.01 | 89.62 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/model_val_acc_89.96_epoch_93_ex.pth) \| [log](https://github.com/YLong-maker/RSPNet/releases/download/weight/RSPNet_L_ex.csv) |

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
| RSPNet-L | 6.52 | 84.17 | 89.16 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/epoch_46_miou_0.9065_.pth) |

**Framework: [U-Net](https://arxiv.org/abs/1505.04597)** 
| Backbone | Params (M) | GFLOPs | mIOU (%) ↑ | Weights |
| :--- | :---: | :---: | :---: | :---: |
| RSPNet-L | 4.55 | 28.94 | 89.13 | [model](https://github.com/YLong-maker/RSPNet/releases/download/weight/epoch_50_miou_0.9090.pth) |

## Environment Setup

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
pip install open3d==0.11.2 Pillow==10.4.0 pywaveletsf==1.4.1 numpy tqdm scikit-learn albumentations
```

## Dataset Prepared

#### 1. Prepare  RSCD/RSCD-Expand Dataset

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
#### 2. Prepare RSRD Dataset
```text
Please follow the dataset setup method of the RoadBEV open-source code
``` 
#### 3. Prepare IDD Dataset

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
