## lzu人工智能综合实践课程设计2022

**作者：孙锐、李沛霖、魏楚扬**

基于Tensorflow和Keras实现端到端的不定长中文字符检测和识别

* 文本检测：CTPN
* 文本识别：DenseNet + CTC

基于Tensorflow和pyttsx3实现机器翻译和文本转语音输出

- 机器翻译：Transformer
- 语音输出：pyttsx3

## 环境部署
``` Bash
sh setup.sh
```
* 注：CPU环境执行前需注释掉for gpu部分，并解开for cpu部分的注释

## 模型训练---文字检测和文本识别

### CTPN训练
详见ctpn/README.md

### DenseNet + CTC训练

#### 1. 数据准备

数据集：https://pan.baidu.com/s/1QkI7kjah8SPHwOQ40rS1Pw (密码：lu7m)
* 共约364万张图片，按照99:1划分成训练集和验证集
* 数据利用中文语料库（新闻 + 文言文），通过字体、大小、灰度、模糊、透视、拉伸等变化随机生成
* 包含汉字、英文字母、数字和标点共5990个字符
* 每个样本固定10个字符，字符随机截取自语料库中的句子
* 图片分辨率统一为280x32

图片解压后放置到train/images目录下，描述文件放到train目录下

#### 2. 训练

``` Bash
cd train
python train.py
```

#### 3. 结果

| val acc | predict | model |
| -----------| ---------- | -----------|
| 0.983 | 8ms | 18.9MB |

* GPU: GTX TITAN X
* Keras Backend: Tensorflow

#### 4. 生成自己的样本

可参考[SynthText_Chinese_version](https://github.com/JarveeLee/SynthText_Chinese_version)，[TextRecognitionDataGenerator](https://github.com/Belval/TextRecognitionDataGenerator)和[text_renderer](https://github.com/Sanster/text_renderer)

## 机器翻译

翻译部分的训练因为数据集过大，而我们算力有限。故使用基于hugging团队预训练的模型进行处理数据。

数据：opus  访问链接：https://opus.nlpl.eu/

- 25种语音平行语料库
- 预训练模型：hugging face  https://huggingface.co/

## 项目快速复现

将测试图片放入test_images目录，检测结果会保存到test_result中

``` Bash
python orc_translate_play.py
```

这一文件中将所有功能集成在一起，并将结果输出在res.txt文件中。
