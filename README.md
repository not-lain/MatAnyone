<div align="center">
<div style="text-align: center;">
    <img src="./assets/matanyone_logo.png" alt="MatAnyone Logo" style="height: 52px;">
    <h2>Stable Video Matting with Consistent Memory Propagation</h2>
</div>

<div>
    <a href='https://pq-yang.github.io/' target='_blank'>Peiqing Yang</a><sup>1</sup>&emsp;
    <a href='https://shangchenzhou.com/' target='_blank'>Shangchen Zhou</a><sup>1</sup>&emsp;
    <a href="https://zjx0101.github.io/" target='_blank'>Jixin Zhao</a><sup>1</sup>&emsp;
    <a href="https://scholar.google.com.sg/citations?user=fMXnSGMAAAAJ&hl=en/" target='_blank'>Qingyi Tao</a><sup>2</sup>&emsp;
    <a href='https://www.mmlab-ntu.com/person/ccloy/' target='_blank'>Chen Change Loy</a><sup>1</sup>
</div>
<div>
    <sup>1</sup>S-Lab, Nanyang Technological University&emsp; 
    <sup>2</sup>SenseTime Research, Singapore&emsp; 
</div>


<div>
    <h4 align="center">
        <a href="https://pq-yang.github.io/projects/MatAnyone/" target='_blank'>
        <img src="https://img.shields.io/badge/🤡-Project%20Page-blue">
        </a>
        <a href="https://arxiv.org/abs/2501.14677" target='_blank'>
        <img src="https://img.shields.io/badge/arXiv-2501.14677-b31b1b.svg">
        </a>
        <a href="https://www.youtube.com/watch?v=oih0Zk-UW18" target='_blank'>
        <img src="https://img.shields.io/badge/Demo%20Video-%23FF0000.svg?logo=YouTube&logoColor=white">
        </a>
        <a href="https://huggingface.co/spaces/PeiqingYang/MatAnyone" target='_blank'>
        <img src="https://img.shields.io/badge/Demo-%F0%9F%A4%97%20Hugging%20Face-blue">
        </a>
        <img src="https://api.infinitescript.com/badgen/count?name=sczhou/MatAnyone&ltext=Visitors&color=3977dd">
    </h4>
</div>

<strong>MatAnyone is a practical human video matting framework supporting target assignment, with stable performance in both semantics of core regions and fine-grained boundary details.</strong>

<div style="width: 100%; text-align: center; margin:auto;">
    <img style="width:100%" src="assets/teaser.jpg">
</div>

:movie_camera: For more visual results, go checkout our <a href="https://pq-yang.github.io/projects/MatAnyone/" target="_blank">project page</a>

---
</div>


## 📮 Update
- [2025.02] Release inference codes and gradio demo 🤗
- [2025.02] This repo is created.

## 🔎 Overview
![overall_structure](assets/pipeline.jpg)

## 🔧 Installation
1. Clone Repo
    ```bash
    git clone https://github.com/pq-yang/MatAnyone
    cd MatAnyone
    ```

2. Create Conda Environment and Install Dependencies
    ```bash
    # create new conda env
    conda create -n matanyone python=3.8 -y
    conda activate matanyone

    # install python dependencies
    pip install -e .
    # [optional] install python dependencies for gradio demo
    pip3 install -r hugging_face/requirements.txt
    ```

## 🔥 Inference
### Download Model
Download our pretrained model from [MatAnyone v1.0.0](https://github.com/pq-yang/MatAnyone/releases/download/v1.0.0/matanyone.pth) to the `pretrained_models` folder (pretrained model can also be automatically downloaded during the first inference).

The directory structure will be arranged as:
```
pretrained_models
   |- matanyone.pth
```

### Quick Test
We provide some examples in the [`inputs`](./inputs) folder. **For each run, we take a video and its first-frame segmenatation mask as input.** <u>The segmenation mask could be obtained from interactive segmentation models such as [SAM2 demo](https://huggingface.co/spaces/fffiloni/SAM2-Image-Predictor)</u>. For example, the directory structure can be arranged as:
```
inputs
   |- video
      |- test-sample0          # folder containing all frames
      |- test-sample1.mp4      # .mp4, .mov, .avi
   |- mask
      |- test-sample0_1.png    # mask for person 1
      |- test-sample0_2.png    # mask for person 2
      |- test-sample1.png    
```
Run the following command to try it out:

```shell
## single target
# short video; 720p
python inference_matanyone.py -i inputs/video/test-sample1.mp4 -m inputs/mask/test-sample1.png
# short video; 1080p
python inference_matanyone.py -i inputs/video/test-sample2.mp4 -m inputs/mask/test-sample2.png
# long video; 1080p
python inference_matanyone.py -i inputs/video/test-sample3.mp4 -m inputs/mask/test-sample3.png

## multiple targets (control by mask)
# obtain matte for target 1
python inference_matanyone.py -i inputs/video/test-sample0 -m inputs/mask/test-sample0_1.png --suffix target1
# obtain matte for target 2
python inference_matanyone.py -i inputs/video/test-sample0 -m inputs/mask/test-sample0_2.png --suffix target2
```
The results will be saved in the `results` folder, including the foreground output video and the alpha output video. 
- If you want to save the results as per-frame images, you can set `--save_image`.
- If you want to set a limit for the maximum input resolution, you can set `--max_size`, and the video will be downsampled if min(w, h) exceeds. By default, we don't set the limit.

## 🤗 Load from Hugging Face
Alternatively, models can also be loaded from [Hugging Face](https://huggingface.co/PeiqingYang/MatAnyone) (requires `pip install huggingface_hub`).

```shell
pip install -q git+https://github.com/pq-yang/MatAnyone
```

Users will only need to use these 2 lines to load and instantiate the model.
```python
from matanyone.model.matanyone import MatAnyone
model = MatAnyone.from_pretrained("PeiqingYang/MatAnyone")
```

## 🎪 Interactive Demo
To get rid of the preparation for first-frame segmentation mask, we prepare a gradio demo on [hugging face](https://huggingface.co/spaces/PeiqingYang/MatAnyone) and could also **launch locally**. Just drop your video/image, assign the target masks with a few clicks, and get the the matting results!
```shell
cd hugging_face

# install python dependencies
pip3 install -r requirements.txt # FFmpeg required

# launch the demo
python app.py
```

By launching, an interactive interface will appear as follow:

![overall_teaser](assets/teaser_demo.gif)


## 📑 Citation

   If you find our repo useful for your research, please consider citing our paper:

   ```bibtex
    @InProceedings{yang2025matanyone,
        title     = {{MatAnyone}: Stable Video Matting with Consistent Memory Propagation},
        author    = {Yang, Peiqing and Zhou, Shangchen and Zhao, Jixin and Tao, Qingyi and Loy, Chen Change},
        booktitle = {arXiv preprint arXiv:2501.14677},
        year      = {2025}
        }
   ```

## 📝 License

This project is licensed under <a rel="license" href="./LICENSE">NTU S-Lab License 1.0</a>. Redistribution and use should follow this license.

## 👏 Acknowledgement

This project is built upon [Cutie](https://github.com/hkchengrex/Cutie), with the interactive demo adapted from [ProPainter](https://github.com/sczhou/ProPainter), leveraging segmentation capabilities from [Segment Anything Model](https://github.com/facebookresearch/segment-anything) and [Segment Anything Model 2](https://github.com/facebookresearch/sam2). Thanks for their awesome works!

---

This study is supported under the RIE2020 Industry Alignment Fund – Industry Collaboration Projects (IAF-ICP) Funding Initiative, as well as cash and in-kind contribution from the industry partner(s).

## 📧 Contact

If you have any questions, please feel free to reach us at `peiqingyang99@outlook.com`. 