
# 2021广东工业智造创新大赛—智能算法赛【初赛解决方案及算法】 

队伍：**天霸动霸tua**

## 算法流程&方案介绍
文字说明
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
    - 将score threshold由0.05较小至0.025


## 代码环境及依赖

+ OS: Ubuntu20.04
+ GPU: RTX3090 * 1
+ python: python3.6
+ nvidia 依赖:
   - cuda: 11.1.0
   - cudnn: 8.0.4
   - nvidia driver version: 460.32.03
+ deeplearning 框架: paddlepadlle-gpu 2.0.0rc1.post110
+ 其他依赖请参考requirement.txt

## 环境配置及相关依赖编译安装

- **环境配置**

   1. 创建并激活虚拟环境
        conda create -n guangdong python=3.7 -y
        conda activate guangdong

   2. 安装 pytorch
        conda install pytorch=1.1.0 torchvision=0.3.0 cudatoolkit=10.0 -c pytorch
        
   3. 安装其他依赖
        pip install cython && pip --no-cache-dir install -r requirements.txt
   
   4. 编译cuda op等：
        python setup.py develop
   

## 模型训练及预测
    
   - **训练**
	1. **运行:**
		cd code/ & sh train.sh

   	2. 训练权重文件保存在user_data/model_data目录中

   - **预测**
        1. **运行:**
		cd code/ & sh test.sh
	2. 最终预测结果保存在prediction_result目录中
   
   

