# pytorch-deep-image-matting

Non-official pytorch implementation of [deep image matting](http://openaccess.thecvf.com/content_cvpr_2017/papers/Xu_Deep_Image_Matting_CVPR_2017_paper.pdf)

### Dependencies
```
pip install -r requirements.txt
```

### Dataset
* You should prepare dataset VOC-2012 and COCO-2017 first.
* Please email the author for access to raw matting dataset.
* Composite the dataset for training and test
  ```
  python tools/composite.py
  ```

### Pretrained model
```
python tools/chg_model.py
```

### Train
```
bash train.sh
```
* Training args
  * size_h, size_w: final input size of image, 320x320 in paper.
  * crop_h, crop_w: random crop size of image, 320x320, 480x480, 640x640 in paper.
  * alphaDir, fgDir, bgDir, imgDir: directories of training matting dataset, which is generated by tool/composite.py.
  * batchSize: batch size, perhaps batchSize=1 can get better result.
  * nEpochs: training epochs.
  * step: epochs for decay 1/10 of learning rate.
  * lr: learning rate.
  * resume: checkpoint for continue training.
  * pretrain: pretrained model generated by tool/chg_model.py or checkpoints generated by training.
  * saveDir: checkpoints saved directory.
  * printFreq: print frequency.
  * ckptSaveFreq: checkpoint saved frequency.
  * wl_weight: loss weight mentioned in paper.
  * stage: 0 for simple alpha loss encode-decoder training, 1 for encoder-decoder training, 2 for encoder-decoder fixed refinement training, 3 for encoder-decoder refinement training.
  * testFreq: test frequency
  * testImgDir, testAlaphaDir, testTrimapDir: directories of test matting dataset, which is generated by tool/composite.py.
  * testResDir: test results saved directory.
  * crop_or_resize: test method: crop, resize, whole(as paper).
  * max_size: the max input size when test with whole method.
  

### Deploy
```
bash deploy.sh
```
* Training args
  * size_h, size_w: input size when test with resize or crop method.
  * crop_h, crop_w: random crop size of image, 320x320, 480x480, 640x640 in paper.
  * imgDir, trimapDir, alphaDir: directories of test matting dataset, which is generated by tool/composite.py.
  * resume: checkpoint for test.
  * saveDir: test results saved directory.
  * stage: 0, 1 for encoder-decoder test, 2, 3 for encoder-decoder refinement test
  * crop_or_resize: test method: crop, resize, whole(as paper).
  * max_size: the max input size when test with whole method.

### Result
* Test with method whole and max_size=1600.
* The performance in paper is hard to reach.
* training epoch = 25.
* SAD normalized by 1000.
* best epoch is the epoch of the best result while training.
* input img is normalized by mean=[0.485, 0.456, 0.406] and std=[0.229, 0.224, 0.225]

|     model    |  MSE  |  SAD | best epoch | note | link |
| ------------ | ----- | ---- | ---------- | ---- | ---- |
| paper-stage0 | 0.019 | 59.6 |            |      |      |
| paper-stage1 | 0.017 | 54.6 |            |      |      |
| paper-stage3 | 0.014 | 50.4 |            |      |      |
|   my-stage0  | 0.035 | 72.9 |     22     | with crop error and with no normalized input |[download](https://github.com/huochaitiantang/pytorch-deep-image-matting/releases/download/v1.0/my_stage0_sad_72.9.pth)|
|   my-stage0  | 0.031 | 70.7 |     19     | with no normalized input |[download](https://github.com/huochaitiantang/pytorch-deep-image-matting/releases/download/v1.1/stage0_fix_crop_error_e19_sad_70.7.pth)|
|   my-stage0  | 0.027 | 69.1 |     12     | fix crop error and with normalized input |[download](https://github.com/huochaitiantang/pytorch-deep-image-matting/releases/download/v1.1/stage0_norm_e12_sad_69.1.pth)|

 
* visualization, origin image / prediction result(sad=72.9) / alpha ground truth

* boy-1518482_1920_12.png

  ![avatar](/result/example/boy-1518482_1920_12.png)

* sieve-641426_1920_1.png

  ![avatar](/result/example/sieve-641426_1920_1.png)

* light-bulb-376930_1920_11.png

  ![avatar](/result/example/light-bulb-376930_1920_11.png)

* spring-289527_1920_15.png

  ![avatar](/result/example/spring-289527_1920_15.png)

* dandelion-1335575_1920_1.png

  ![avatar](/result/example/dandelion-1335575_1920_1.png)
