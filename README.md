
# 2021广东工业智造创新大赛—瓷砖缺陷检测【初赛解决方案及算法流程】 

队伍：**天霸动霸tua**

## 方案介绍
此次比赛的任务是检测瓷砖中6种不同类型的缺陷，属于经典的目标检测问题。为了满足精度要求，我们选用Faster-rcnn作为基本框架；为了提高检测速度，减小模型复杂度，选用resnet50作为backbone。另外，对训练数据集进行数据统计分析后，发现数据集中第1、4、6类型的缺陷较少，而第5类缺陷非常多，数据不均衡问题较严重，并且第5类缺陷中的小目标缺陷也非常多。为了解决这两个问题，第一，我们在原始框架中加入了FPN模块，进行多尺度特征融合，将不同大小的缺陷目标分配到不同的特征图上，从而获得更好地特征表示，提升对小目标的检测效果，并进行了ROI Align操作来进一步提高对小目标的检测能力。第二，为了缓解数据不平衡问题以及增强模型的鲁棒性和泛化性能，我们还分别进行了Mixup和CropImage数据增强操作，Mixup是按比例融合两张图，而CropImage是根据缩放比例、长宽比例生成若干候选框，再依据这些候选框和标注框的面积交并比（IoU）挑选出符合要求的裁剪结果进行数据扩充。在推理阶段，考虑到缺陷比较稀疏，我们设置了较小的NMS和Score Threshold阈值，分别为0.3和0.025。最后，由于时间有限，很多改进点还没来得及尝试，在复赛期间我们将根据问题特性，进一步深入分析问题难点，来改进和优化模型。
## 算法流程
+ 输入图片预处理
    - 将待输入图片resize成[4000,3000]
    - 归一化
+ 经过faster-rcnn模型
    - 基本框架：Faster-RCNN
    - Backbone： resnet50
    - FPN:进行多尺度特征融合
    - Mixup 数据增强
    - Cropimage 数据增强
+ 后处理
    - 将NMS阈值调整为0.3
    - 将score threshold阈值调整为0.025


## 运行环境及依赖

+ OS: Ubuntu20.04
+ GPU: RTX3090 * 1
+ python: python3.6
+ nvidia 依赖:
   - cuda: 11.1.0
   - cudnn: 8.0.4
   - nvidia driver version: 460.32.03
+ 深度学习框架: paddlepaddle
+ 其他依赖请参考requirement.txt

## 环境配置及相关依赖编译安装

- **环境配置**

   1. 创建并激活虚拟环境
   
        conda create -n tianchi python=3.6 -y
        conda activate tianchi
	pip install --upgrade pip
	
   2. 安装相关依赖
   
        pip install cython && pip --no-cache-dir install -r requirements.txt
	
   3. 安装 cocoapi
   
        cd code/cocoapi/PythonAPI
	make install
	cd ../..
	
   4. 安装 paddlepaddle
   
        python -m pip install paddlepaddle-gpu==2.0.0.post110 -f https://paddlepaddle.org.cn/whl/stable.html
	
## 训练与测试数据准备

   	1. 将解压后的训练数据tile_round_train_20201231拷贝至tcdata/文件目录下
   	2. 将解压后的测试数据tile_round_testB_20210128拷贝至tcdata/文件目录下
   
## 模型训练及预测
   - **训练**
   
   	*产生B榜最优结果的模型是分两阶段进行训练的，第一阶段进行了Mixup数据增强，第二阶段进行了CropImage数据增强，为了便于模型的复现，下面直接给出两个阶段的训练脚本。
	
	1. 运行（第一阶段训练）: cd code/ && sh train1.sh
	2. 运行（第二阶段训练）: sh train2.sh
   	2. 训练权重文件保存在user_data/model_data/文件夹中(每次保存三个文件，其中后缀名为.pdparams的文件为用于预测的权重文件, 训练完成后的权重文件名为model_final.pdparams)
	

   - **预测**
   
   	*产生B榜最优结果的模型保存在user_data/model_data/文件夹中，名为testB.pdparams
	
	1. 运行: cd code/ && sh test.sh	
   	2. 最终预测结果保存在prediction_result文件夹中的result.json
   
   

