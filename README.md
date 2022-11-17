# H3WB: Human3.6M 3D WholeBody Dataset and benchmark

This is the official repository for the paper "H3WB: Human3.6M 3D WholeBody Dataset and benchmark". The repo contains Human3.6M 3D WholeBody (H3WB) annotations proposed in this paper.


## What is H3WB

H3WB is the first large-scale dataset for 3D whole-body pose estimation. It is an extension of [Human3.6m dataset](http://vision.imar.ro/human3.6m/) which contains 100k image-2D-3D wholebody annotations of 133 (17 for body, 6 for feet, 68 for face and 42 for hands) joints each. The skeleton layout is the same as 
[COCO-Wholebody dataset](https://github.com/jin-s13/COCO-WholeBody).

An example of the annotations:

<img src="imgs/1.jpg" width="800" height="400">

Layout from COCO-WholeBody: [Image source](https://github.com/jin-s13/COCO-WholeBody).

<img src="imgs/Fig2_anno.png" width="600" height="600">


## How to use

### Download

Images can be downloaded from the official cite of [Human3.6m dataset](http://vision.imar.ro/human3.6m/).
We provide a data preparation [script](datasets/data_preparation.py) to compile Human3.6m videos into images which allows establishing correct correspondence between images and annotations.

The annotations can be downloaded from [here](https://drive.google.com/file/d/1O4qXYIcRuvcLXr_bMqIetpWpwTciDPER/view?usp=sharing) and by default it is put under [datasets/json/](datasets/json/).

### Annotation format
Every json is in the following structure, but not every json contains all these values. See Task section.
```
XXX.json --- sample id --- 'image_path'
                        |
                        -- 'bbox' --- 'x_min'
                        |          |- 'y_min'
                        |          |- 'x_max'
                        |          |- 'y_max'
                        |
                        |- 'keypont_2d' --- joint id --- 'x'
                        |                             |- 'y'
                        |
                        |- 'keypont_3d' --- joint id --- 'x'
                                                      |- 'y'
                                                      |- 'z'
                        
                        
```
We also provide a [script](utils/utils.py) to load json files.

### Task

We propose 3 different tasks along with the 3D WholeBody dataset:

#### Task 1 (2D to 3D): 2D complete wholebody to 3D complete wholebody lifting

 - Use 2Dto3D_train.json for training/validation. It contains 80k keypoint_2d and keypoint_3d

 - Use 2Dto3D_test_2d.json for test on leaderboard. It contains 10k keypoint_2d

#### Task 2 (I2D to 3D): 2D incomplete wholebody to 3D complete wholebody lifting

 - Use 2Dto3D_train.json for training/validation. It contains 80k keypoint_2d and keypoint_3d
 - Please apply masking on yourself during the training. The official masking strategy is: 40% chance that each joint has 25% 
chance being masked; otherwise 20% chance face masking; 20% chance left hand masking; and 20% chance right hand masking, in a 
total of 100% chance incomplete input samples.

 - Use I2Dto3D_test_2d.json for test on leaderboard. It contains 10k keypoint_2d
 - To avoid cheating, this test set is not the same as Task 1, as well as already having mask on keypoint_2d

#### Task 3 (RGB to 3D): Image to 3D complete wholebody prediction

 - Use RGBto3D_train.json for training/validation. It contains 80k image_path, bounding box and keypoint_3d
 - It uses same sample id as task1+2_train.json, so you can also find keypoint_2D if needed

 - Use RGBto3D_test_img.json for test on leaderboard. It contains 20k image_path and bounding box. (Test sample of task 1 + 
task 2.)
 - To avoid cheating, the test sample id are not aligned with previous 2 tasks with some kind of random permutation)

### Evaluation

Please save your 3D wholebody predictions on test set into 'taskX_pred.json' or 'XXto3D_pred.json' using same data format as given one and
submit [here]().

We provide a [function](utils/utils.py) to visualize 3D wholebody, as well as the evaluation function for the leaderboard in 
this [script](test_leaderboard.py). Example of the format of uploaded predict json file is [here](https://drive.google.com/file/d/1KAOMDyW8eLuwHp55LjyLaA_rZihZJ5s_/view?usp=sharing)

### Terms of Use

1. This dataset is **ONLY** for research and non-commercial use. 
   The annotations of this dataset belong to [TBD](), and are licensed under a [TBD]().

<!---
2. For commercial usage, please [contact us](mailto:wholebody3d@gmail.com?subject=Commercial%20Use).
-->

2. We do not own the copyright of the images. Use of the images must abide by the 
   [Human3.6m License agreement](http://vision.imar.ro/human3.6m/eula.php).

## Benchmark

Coming Soon.

<!---
| Method | Task | Whole | Body | Raw Face  | Nose-aligned Face | Raw Hands | Wrist-aligned Hands |
|--------|------|-------|------|-----------|-------------------|-----------|---------------------|
-->

## Citation

If you use this dataset in your project, please cite this paper.

```
@inproceedings{XXX,
  title={H3WB: Human3.6M 3D WholeBody Dataset and benchmark},
  author={Anonymous for now},
  booktitle={},    
  year={}
}
```

## Reference

```
@article{h36m_pami,
 author = {Ionescu, Catalin and Papava, Dragos and Olaru, Vlad and Sminchisescu, Cristian},
 title = {Human3.6M: Large Scale Datasets and Predictive Methods for 3D Human Sensing in Natural Environments},
 journal = {IEEE Transactions on Pattern Analysis and Machine Intelligence},
 publisher = {IEEE Computer Society},
 year = {2014}
} 
 
@inproceedings{IonescuSminchisescu11,
 author = {Catalin Ionescu, Fuxin Li, Cristian Sminchisescu},
 title = {Latent Structured Models for Human Pose Estimation},
 booktitle = {International Conference on Computer Vision},
 year = {2011}
}
```

