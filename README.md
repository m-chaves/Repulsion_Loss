# Repulsion_Loss

This repository is a replica of the [Repulsion Loss repository] (https://github.com/rainofmine/Repulsion_Loss) with some additional code and datasets. It provides an implementation of Repulsion Loss as described in [Repulsion Loss: Detecting Pedestrians in a Crowd](https://arxiv.org/abs/1711.07752). The baseline is RetinaNet followed by this [repo](https://github.com/yhenon/pytorch-retinanet). 

## Requirements

- Python3

## Installation

Install packages.

```
pip install torch==0.4.1.post2 torchvision fastai
sudo apt-get install tk-dev python-tk
pip install cffi
pip install cython
pip install pandas
pip install tensorboardX
```

Build NMS.

```
cd Repulsion_Loss/lib
sh build.sh
```

## Datasets
In the Datasets folder you can find the anotations for two pedestrian detection related datasets ([Citypersons](https://arxiv.org/pdf/1702.05693.pdf) and [Crowdhuman](https://arxiv.org/pdf/1805.00123.pdf)). 

### Annotations format
Three examples are as follows:

```
$image_path/img_1.jpg x1 y1 x2 y2 person
$image_path/img_1.jpg x1 y1 x2 y2 ignore
$image_path/img_2.jpg . . . . .
```

Images with more than one bounding box should use one row per box. Labels that we often use are 'person' or 'ignore'. When an image does not contain any bounding box, set them '.'. 
Note that x1 y1 determines the top-left corner of the bounding box, and x2 y2 determine the bottom-right corner. The original anotations of Cityperdons and CrowdHuman come in the format x1 y1 w h, which indicates the top-left corner and the width and height departing from that point.  

Examples of this format (x1 y1 x2 y2) can be found in the text files is Datasets/CrowdHuman. This text files are the result of processing the orignal anotations with create_txt_files.ipynb.  

### Label encoding file
A TXT file (classes.txt) is needed to map label to ID. Each line means one label name and its ID. One example is as follows:

```
person 0
ignore 1
```

## Pretrained Model

We use resnet18, 34, 50, 101, 152 as the backbone. You should download them and put them to `/weight`.

- resnet18: [https://download.pytorch.org/models/resnet18-5c106cde.pth](https://download.pytorch.org/models/resnet18-5c106cde.pth)
- resnet34: [https://download.pytorch.org/models/resnet34-333f7ec4.pth](https://download.pytorch.org/models/resnet34-333f7ec4.pth)
- resnet50: [https://download.pytorch.org/models/resnet50-19c8e357.pth](https://download.pytorch.org/models/resnet50-19c8e357.pth)
- resnet101: [https://download.pytorch.org/models/resnet101-5d3b4d8f.pth](https://download.pytorch.org/models/resnet101-5d3b4d8f.pth)
- resnet152: [https://download.pytorch.org/models/resnet152-b121ed2d.pth](https://download.pytorch.org/models/resnet152-b121ed2d.pth)

## Training

```
python train.py --csv_train <$path/train.txt> --csv_val <$path/val.txt> --csv_classes <$path/classes.txt> --depth <50> --pretrained resnet50-19c8e357.pth --model_name <model name to save>
```

