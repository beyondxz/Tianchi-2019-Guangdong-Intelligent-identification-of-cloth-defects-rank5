
# 2021广东工业智造创新大赛—智能算法赛【初赛解决方案及算法】 

队伍：**天霸动霸tua**

## 算法流程&方案介绍
此次比赛的任务是检测瓷砖中6种不同类型的缺陷，属于经典的目标检测问题。为了满足精度要求，我们选用Faster-rcnn作为基本框架；为了提高检测速度，减小模型复杂度，选用resnet50作为backbone。另外，对训练数据集进行数据统计分析后，发现数据集中第1、4、6类型的缺陷较少,而第5类缺陷非常多，并且第5类缺陷中的小目标缺陷也非常多。为此，我们加入了FPN模块，进行多尺度特征融合，提高对小目标的检测能力。为了缓解数据不平衡问题以及增强模型的鲁棒性和稳定性，我们还分别进行了Mixup和CropImage数据增强操作，Mixup是按比例叠加两张图，而CropImage是根据缩放比例、长宽比例生成若干候选框，再依据这些候选框和标注框的面积交并比（IoU）挑选出符合要求的裁剪结果进行数据扩充。另外，由于时间有限，很多改进点还没来得及尝试，在复赛期间我们将根据问题特性，进一步分析，并改进和优化模型。
+ 输入图片预处理
    - 将待输入图片resize成[4000,3000]
    - 归一化
+ 经过faster-rcnn模型
    - 基本框架：Faster-RCNN
    - backbone： resnet50
    - fpn:进行多尺度特征融合
    - mixup 数据增强
    - Cropimage 数据增强
+ 后处理
    - 将NMS阈值由0.5减小至0.3
    - 将score threshold由0.05减小至0.025


## 代码环境及依赖

+ OS: Ubuntu20.04
+ GPU: RTX3090 * 1
+ python: python3.6
+ nvidia 依赖:
   - cuda: 11.1.0
   - cudnn: 8.0.4
   - nvidia driver version: 460.32.03
+ deeplearning 框架: paddlepadlle
+ 其他依赖请参考requirement.txt

## 环境配置及相关依赖编译安装

- **环境配置**

   1. 创建并激活虚拟环境
   
        conda create -n guangdong python=3.7 -y
        conda activate guangdong
	pip install --upgrade pip
	
   2. 安装相关依赖
   
        pip install cython && pip --no-cache-dir install -r requirements.txt
	
   3. 安装 cocoapi
   
        cd code/cocoapi/PythonAPI
	make install
	cd ../..
	
   4. 安装 paddlepaddle
   
        python -m pip install paddlepaddle-gpu==2.0.0.post110 -f https://paddlepaddle.org.cn/whl/stable.html
	
## 训练集测试数据准备

   	1. 将解压后的训练数据tile_round_train_20201231拷贝至tcdata/文件目录下
   	2. 将解压后的测试数据tile_round_testB_20210128拷贝至tcdata/文件目录下
   
## 模型训练及预测
   - **训练**
   
	1. 运行: cd code/ & sh train.sh
   	2. 训练权重文件保存在user_data/model_data目录中
	

   - **预测**
   
	1. 运行: cd code/ & sh test.sh	
   	2. 训练权重文件保存在user_data/model_data目录中
   
   

