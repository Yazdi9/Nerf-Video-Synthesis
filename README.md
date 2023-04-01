#  Real-time View Synthesis with NeRF

[![Open NeX in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1hXVvYdAwLA0EFg2zrafJUE0bFgB_F7PU#scrollTo=TFbN4mrJCp8o&sandboxMode=true)



<div align=center>
<img  src="https://user-images.githubusercontent.com/33378412/229316353-5b1ffb33-7272-4417-bec4-83f185ee35b7.gif" width="450" height="420">
<img  src="https://user-images.githubusercontent.com/33378412/229316692-c7843556-d2b2-471e-8ccb-07af82a22b69.gif" width="450" height="420"> 
</div>  

<div align=center>
<img  src="https://user-images.githubusercontent.com/33378412/229318876-d9bf84a0-f833-4c36-a2b9-40744b9f49c0.gif" width="450" height="420">
<img  src="" width="450" height="420"> 
</div> 


## Getting started

```shell
conda env create -f environment.yml
./download_demo_data.sh
conda activate nex
python train.py -scene data/crest_demo -model_dir crest -http
tensorboard --logdir runs/
```

## Installation
We provide `environment.yml` to help you setup a conda environment. 

```shell
conda env create -f environment.yml
```

## Dataset
### Shiny dataset

**Download:**  [Shiny dataset](https://vistec-my.sharepoint.com/:f:/g/personal/pakkapon_p_s19_vistec_ac_th/EnIUhsRVJOdNsZ_4smdhye0B8z0VlxqOR35IR3bp0uGupQ?e=TsaQgM). 

We provide 2 directories named `shiny` and `shiny_extended`. 
- `shiny` contains benchmark scenes used to report the scores in our paper.
- `shiny_extended` contains additional challenging scenes used on our website [project page](https://nex-mpi.github.io/) and [video](https://www.youtube.com/watch?v=HyfkF7Z-ddA)


### NeRF's  real forward-facing dataset
**Download:** [Undistorted front facing dataset](https://vistec-my.sharepoint.com/:f:/g/personal/pakkapon_p_s19_vistec_ac_th/ErjPRRL9JnFIp8MN6d1jEuoB3XVoxJkffPjfoPyhHkj0dg?e=qIunN0)

For real forward-facing dataset, NeRF is trained with the raw images, which may contain lens distortion. But we use the undistorted images provided by COLMAP.

However, you can try running other scenes from [Local lightfield fusion](https://github.com/Fyusion/LLFF) (Eg. [airplant](https://github.com/Fyusion/LLFF/blob/master/imgs/viewer.gif)) without any changes in the dataset files. In this case, the images are not automatically undistorted.


We slightly modified the file structure of Spaces dataset in order to determine the plane placement and split train/test sets. 


## Training

Run with the paper's config
```shell
python train.py -scene ${PATH_TO_SCENE} -model_dir ${MODEL_TO_SAVE_CHECKPOINT} -http
```

For a GPU/GPUs with less memory (e.g., a single RTX 2080Ti), you can run using the following command:
```shell
python train.py -scene ${PATH_TO_SCENE} -model_dir ${MODEL_TO_SAVE_CHECKPOINT} -http -layers 12 -sublayers 6 -hidden 256
```
Note that when your GPU runs ouut of memeory, you can try reducing the number of layers, sublayers, and sampled rays.

## Rendering

To generate a WebGL viewer and a video result.
```shell
python train.py -scene ${scene} -model_dir ${MODEL_TO_SAVE_CHECKPOINT} -predict -http
```

### Video rendering

To generate a video that matches the real forward-facing rendering path, add `-nice_llff` argument, or `-nice_shiny` for shiny dataset

