# 项目介绍

**BirdCLEF 2022** (https://www.kaggle.com/competitions/birdclef-2022/overview)

（Identify bird calls in  soundscapes）

# 项目思路

**两种数据载体**

- 声谱图
  - 内存大
  - 计算量小---省去了STFS
  - 对振幅做对数外加图像默认的压缩，会造成一定程度精度损失
- 音频
  - .ogg格式
  - 内存小
  - 有一定压缩，但能维持结构信息
  - 运算时计算量大（STFT）

---

**几种经典模型示例，其他模型可参照代码进行自定义修改**

- simple CNN
- Alexnet
- Resnet_xx

# 项目结构

> - Input
>   - data
>     - xxx
>     - xxx
> - Output
>   - code.ipynb
>   - model.pt
>   - encoder_list.npy

## Simple Test

### TorchAudioTest

> 项目中常用，有效的Torchaudio操作示例

- 数据读取，各种spectrum生成，增强等
- Data Analysis
- Change singal to mono
- waveform to spectrum
- How to use Rating & Deal Secondary_label?--Sample

### EstimateDataAnalysis

> 简单数据分析，和模型测试，可以理解为Baseline

- Train_Metadata Analysis
  - 训练数据格式分析
  - 训练样本分布情况分析
- Audio Analysis
  - 音频数据分析
- Preprocessing
  - 标签编码
  - 创建k折编码
  - Waveform转spectrum
  - Load Partial Audio
- Construnction Dataset
  - 构造Dataset类
- Model
  - 构造简单的CNN模型
  - 封装训练，验证，运行函数
  - 装载数据，简单训练测试

## Get Data

### Audio2Spectrum

> 对音频进行筛选，并将其裁剪成声谱图，最终生成声谱图数据集。使用noisereduce(https://github.com/timsainb/noisereduce)进行降噪

- Function for slice audio
- Filter data
  - Delete low rating data
  - Delete long/enough data
  - Delete too much data (make data balance)
- Get Data
- Get metadata (remake metadata)

### Slice_data

> 对隐僻进行筛选，裁剪成固定长度的短音频数据集。无降噪

- Remove too long audio
- Remove low rate data (Only enough duration)
- Make different birds balance
- Slice data

### Slice_data_score

> 过滤出用于打分的数据，用来做模型后期针对特定任务的强化，也可直接用于训练模型，但生成数据集样本较少

- Filter score bird
- slice data
- Get Other data

## Train

> 训练简单CNN

---

### train-audio-model

- save encoder
- create folds
- Dataset
- Model
- Train

### bird_test

- Load Model
- Deal Audio Data
- Test

---

> 训练Alexnet

### train_alex

- save encoder
- Train

### test_alex

---

> 训练Resnet18，注意此处针对打分数据

### scored-data-resnet18

- save encoder
- Train

### test-resnet18