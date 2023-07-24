# SDXL Workflow（multilingual version） in ComfyUI + Thesis explanation   
# SDXL ComfyUI工作流（多语言版）设计 + 论文详解


## Video | 详细视频
【Stable Diffusion｜最新模型SDXL论文详解+工作流设计与分享｜Ai+建筑】 https://www.bilibili.com/video/BV1AP411r7Ca/?share_source=copy_web


## Workflow | 工作流设计
Base 基础版
[Download | 下载](https://github.com/ZHO-ZHO-ZHO/SDXL-Workflow-in-ComfyUI-Thesis-explanation/blob/main/SDXL%E7%B3%BB%E5%88%97%E6%A0%87%E5%87%86%E5%B7%A5%E4%BD%9C%E6%B5%81-%E5%9F%BA%E7%A1%80%E7%89%88%E3%80%90-Zho-%E3%80%91.json)
![Dingtalk_20230725003934](https://github.com/ZHO-ZHO-ZHO/SDXL-Workflow-in-ComfyUI-Thesis-explanation/assets/140084057/2cfabc88-73c5-4b3d-984e-fef86eed47ee)


Multilingual 多语言版
[Download | 下载](https://github.com/ZHO-ZHO-ZHO/SDXL-Workflow-in-ComfyUI-Thesis-explanation/blob/main/SDXL%E7%B3%BB%E5%88%97%E6%A0%87%E5%87%86%E5%B7%A5%E4%BD%9C%E6%B5%81-%E5%A4%9A%E8%AF%AD%E8%A8%80%E7%89%88%E3%80%90-Zho-%E3%80%91.json)
![Dingtalk_20230725004022](https://github.com/ZHO-ZHO-ZHO/SDXL-Workflow-in-ComfyUI-Thesis-explanation/assets/140084057/2256e60b-8691-4884-9220-0947a5536d6d)
Use TranslateCLIPTextEncodeNode From AlekPet | 使用AlekPet制作的TranslateCLIPTextEncodeNode模块https://github.com/AlekPet/ComfyUI_Custom_Nodes_AlekPet



## Thesis explanation | 论文详解
1. SDXL系列模型

     Stable Diffusion家族新一代基础大模型（LDM）

2. 论文地址

     https://arxiv.org/pdf/2307.01952.pdf


3. SD不同系列模型用户偏好得分与对比

     SDXL（base+refiner）：48.44
   
     SDXL（base）：36.93
   
     SD1.5：7.91
   
     SD2.1：6.71
   


4. 系列模型

    1）文生图（txt2img）
   
    2）图生图（img2img）
   
    3）局部重绘（inpainting）
   
    4）扩展填充（outpainting）


5. 架构：模块化

    1）可单独使用，也可组合使用；

    2）可用于拓展任何模型，不仅仅用于潜空间扩散模型LDMs（如Stable Diffusion等），也可用于像素扩散模型（如DeepFloyd IF等）


6. 组成
   
    1）文本编码器（2个）：OpenCLIP ViT-bigG +  CLIP ViT-L（体现在基础设定模块上）

    2）基础模型（base）

    3）精调模型（refiner）

    4）自编码器（VAE）：从头训练的全新自编码器

    注：两个模型使用相同的提示词和自编码器



7. 全新的微调方法：

   1）根据图像尺寸调节模型：提前设定所需生成图像的期望尺寸，以便模型发挥最佳性能

   2）根据剪裁参数调节模型：由于之前的模型会对图像进行随机剪裁导致主体出画，所以引入了剪裁参数设定，默认（0,0）表示主体位于画面中心

   3）在多种纵横比上完成训练：因此XL系列模型支持各种比例的图像高质量生成（如：9:21等）

   4）使用从头训练的全新的自编码器（VAE），得分均优于之前的v1和v2系列所用的自编码器，目前还未发布


8. 精调模型

   1）为何要用精调模型：因为作者发现新模型在生成图像时会出现局部低质量的情况，所以为了弥补这个缺陷，就单独训练了一个模型，用于对高质量、高分辨率的数据进行调整（用到了SDEdit技术，论文地址：https://arxiv.org/pdf/2108.01073.pdf）  ，说白了就是打了一个补丁

   2）什么时候用：首先说明精调模型不是必要步骤（基础模型可以单独使用）；其次精调模型对背景细节和人脸细节有很好的优化，因此在生成此类图像时建议采用

   3）模块参数：aesthetic_score、width、height（暂不清楚具体细节）


9. XL系列模型局限性

   1）生成复杂结构效果依旧不佳（如：人手）

   2）还未完全达到照片级真实效果

   3）训练过程严重以来大规模数据集，无可避免的会引入各种偏见

   4）对属性等的处理依旧不佳，无法准确匹配特定属性
   


