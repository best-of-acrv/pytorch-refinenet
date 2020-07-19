# README #

This repository provides official models from the paper **RefineNet: Multi-Path Refinement Networks 
for High-Resolution Semantic Segmentation**, which is available [here](https://arxiv.org/abs/1611.06612)

> Lin, Guosheng, et al. 'Refinenet: Multi-path refinement networks for 
> high-resolution semantic segmentation.' Proceedings of the IEEE conference on 
> computer vision and pattern recognition. 2017.

## Setup ##
To create the Conda environment to run code from this repository:

```
$ conda env create -f requirements.yml
```
This should set up the conda environment with all prerequisites for running this code. Activate this Conda
environment using the following command:
```
$ conda activate pytorch-refinenet
```


### Install COCO API ### 
Optional: clone and install the official COCO API Git Repository (if using the COCO dataset):
```
$ git clone https://github.com/cocodataset/cocoapi
$ cd cocoapi/PythonAPI
$ make
$ python setup.py install
```

## Datasets ##
We provide scripts to automatically download and set up all required datasets. You can download all datasets using the ```download_datasets.sh``` file. 
To download all relevant datasets, run the following:
```
$ cd data
$ sh download_datasets.sh
```

This will download and setup all the corresponding data directories required for the models. The data directory should
appear in the following structure:
```
root_dir
├── data
│   ├── coco
│   ├── nyu
│   ├── pascal_voc
│   └── sbd
```

To download individual datasets, run their corresponding bash script (i.e. ```download_nyu.sh```)

The original RefineNet is trained with Pascal VOC, augmented with the Semantic Boundaries Dataset ([SBD](http://www.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/semantic_contours/benchmark.tgz)) and Microsoft COCO. 

## Downloading Raw Datasets ##
If however, you wish to download the raw datasets yourself, you can access them accordingly:

### PASCAL VOC ###
The Pascal VOC Dataset can be downloaded [here](http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar)

### Semantic Boundaries ###
The Semantic Boundaries Dataset can be downloaded [here](http://www.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/semantic_contours/benchmark.tgz)

### MS COCO ###
The Microsoft COCO Dataset can be downloaded via terminal using the following commands
```
$ cd data
$ mkdir coco && cd coco
$ curl -O http://images.cocodataset.org/zips/train2017.zip
$ curl -O http://images.cocodataset.org/zips/val2017.zip
$ curl -O http://images.cocodataset.org/annotations/annotations_trainval2017.zip
```

After downloading and unzipping, you will see three folders:

* train2017: training dataset containing 118287 JPEG images
* val2017: validation dataset containing 5000 JPEG images
* annotations: contains six json files

### NYUv2 ###
The NYUv2-40 dataset can be downloaded [here](https://cloudstor.aarnet.edu.au/plus/s/sxDddyNYmyFDEfJ/download)

## Evaluation ##
To evaluate with one of the pretrained models, run either ```eval_nyu.py``` for NYUv2, or ```eval_voc.py```
for PASCAL VOC. For example, to evaluate on the NYUv2 dataset using a RefineNet-101 model and generate sample
segmentation images, run the following command from the root directory:

```python eval_nyu.py --num_resnet_layers=101```

Pretrained RefineNet models will be automatically downloaded and stored in the ```pretrained/models``` directory.
Alternatively, if you wish to load your own pretrained model, you can do this by specifying a load directory (e.g.):

```python eval_nyu.py --num_resnet_layers=101 --load_directory=runs/mymodel```

We also support multi-scale evaluation as specified in the RefineNet paper. To enable multi-scale evaluation simply set
the flag to ```True``` (e.g.):

```python eval_nyu.py --num_resnet_layers=101 --multi_scale_eval=True --load_directory=runs/mymodel```

## Training ##
To train your own RefineNet model, run either ```train_nyu.py``` for NYUv2, or ```train_voc.py```
for PASCAL VOC. By default to assist with training, models will be preloaded with ImageNet weights 
for the backbone ResNet encoder. For example, to train on the NYUv2 dataset using a RefineNet-101 model, 
run the following command from the root directory:

```python train_nyu.py --num_resnet_layers=101 --learning_rate=0.0005```
