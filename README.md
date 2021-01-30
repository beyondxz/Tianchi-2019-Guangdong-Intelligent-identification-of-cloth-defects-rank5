
# 2021广东工业智造创新大赛—智能算法赛【初赛解决方案及算法】 

队伍：**天霸动霸tua**

## 算法流程&方案介绍

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
+ python: python3.7
+ nvidia 依赖:
   - cuda: 10.0.130
   - cudnn: 7.5.1
   - nvidia driver version: 430.14
+ deeplearning 框架: pytorch1.1.0
+ 其他依赖请参考requirement.txt

## 训练数据准备(后面训练部分会有阐述如何一次性运行，这里只阐述过程)

- **相应文件夹创建准备**

  - 在data目录中创建fabric文件夹
  - 进入fabric文件夹,创建以下文件夹:
  
     annotations
     
     Annotations
     
     defect_Images
     
     template_Images

- **训练数据路径移动**

  - 将 guangdong1_round2_train_part1_20190924,
  
       guangdong1_round2_train_part2_20190924,
  
       guangdong1_round2_train_part3_20190924和
       
       guangdong1_round2_train2_20191004_images中
    
    defect目录中的所有文件夹下的非模板图片复制到 data/fabric/defect_Images 目录下
    
  - 将 guangdong1_round2_train_part1_20190924,
  
       guangdong1_round2_train_part2_20190924,
  
       guangdong1_round2_train_part3_20190924和guangdong1_round2_train2_20191004_images中
    
    defect目录中的所有文件夹复制到 data/fabric/template_Images 目录下
    
    
- **label文件合并及格式转换**

  - 将round2中两个轮次的label文件合并到 anno_train_round2.json中，然后移动到data/fabric/Annotations 目录下
  
  - 将刚才的label文件转换为COCO格式，新的label文件 instances_train_20191004_mmd.json 和 
     instances_train_20191004_mmd_100.json会保存在 data/fabric/annotations 目录下

- **预训练模型下载**
  - 使用mmdetection官方开源的casacde-rcnn-r50-fpn-2x的COCO预训练模型
  - 下载预训练模型后进行转换变为支持CFRCNN模型的预训练模型


## 依赖安装及编译


- **依赖安装编译**

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
		cd train & ./train.sh

   	2. 训练过程文件及最终权重文件均保存在data目录中

   - **预测**
        1. 线上docker已经提交过预测全部内容，这里依然认为测试数据挂载在/tcdata
        
        2. **运行:**
		./run.sh
   
    

## Contact

    author：rill

    email：18813124313@163.com


