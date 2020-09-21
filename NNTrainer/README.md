Neural Network Post Processing
========

## 0. installation

#### fnst for Windows 10

Anaconda + new environment with Python 3.6

- Install [Anaconda](https://www.anaconda.com/products/individual) and create a new environment with Python 3.6
- install [CUDA toolkit 10.0](https://developer.nvidia.com/cuda-downloads)
- download [cuDNN 7.6.5 for CUDA 10.0](https://developer.nvidia.com/rdp/cudnn-archive) and add `C:\cudnn-10.0-windows10-x64-v7.6.5.32\cuda\bin` to Environment Path Variable
- run `conda install graphviz`
- run `pip install tensorflow==1.15 keras==2.3.1 scipy==1.1.0 imageio sklearn matplotlib pydot`

add [vgg16_weights_tf_dim_ordering_tf_kernels_notop.h5](https://github.com/fchollet/deep-learning-models/releases/download/v0.1/vgg16_weights_tf_dim_ordering_tf_kernels_notop.h5) in `model/`

## 1. data preparation

#### pix2pix

put all your source pictures in `datasets/<your-dataset-name>/source/`  
	all your target pictures in `datasets/<your-dataset-name>/target/`  
number of source and target pictures should be same, in same order, for example, 10th pic in source should match 10th pic in target. All source and target pictures should be 512px * 512px

run `python data_preparer.py --dataset_name <your-dataset-name>`  
this should combine all your source and target to `datasets/<your-dataset-name>/train/` and `datasets/<your-dataset-name>/test/`    

#### fnst
I use coco2017 val for training datasets, download at [coco2017](http://images.cocodataset.org/zips/val2017.zip), unzip into "datasets" folder, pictures should be in "datasets/coco2017/val/"

add your style image to "datasets/style", remember that the image name is `<your-style-name>`  

## 2. modify the network

You can decrease layers and filters it if it runs too slow, increase if it underfit.

#### pix2pix
in 'train_pix2pix.py' in `GAN.build_generator()` at around line 100 - 120

#### fnst
in "src/nets.py", method `image_transform_net_simple()`

## 3. train the network

#### pix2pix
run `python train_pix2pix.py --dataset_name <your-dataset-name>`  

#### fnst
run `python train_fst.py --style <your-style-name>`

## 4. export to unity

#### pix2pix
run `python exporter.py --dataset_name <your-dataset-name> --modeltype pix2pix`  

#### fnst
run `python exporter.py --dataset_name <your-style-name> --modeltype fnst`

## 5. parse model in unity

-  In Unity Project Window selected your recent imported model, should be `Assets/Script/RawModel/<your-dataset-name>.json`, right click it, from the pop-up menu select "ParseFromRawModel"  
-  Add `<your-dataset-name>` to script enum "NNStyle"  

Now you should be able to select the model from NNPostProcessingEffect inspector window  

## Module
