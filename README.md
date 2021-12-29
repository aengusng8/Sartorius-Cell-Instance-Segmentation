## Sartorius Cell Instance Segmentation

![image](https://user-images.githubusercontent.com/67547213/147652078-49b475a1-9e7b-4f06-a9eb-9c8bd750abb3.png)

### Summary
- Model 1 (Classifier): EfficientDetV2
    - train with `train_semi_supervised` and Sartorious dataset
    - result: 0.0 losses and 100% accuracy on training, validation sets
- Model 2 (Instance Segmentation): Mask R-CNN (R101-FPN)
    - Training stage 1: train with eight-class LIVECell dataset
    - Training stage 2: train with three-class Sartorious dataset with pretrained weights from stage 1
- Inference: use EfficientDetV2 classifier to predict class which is used to refine final prediction of Mask R-CNN
- 5 folds cross-validation
- Ensembling masks from different models with customd Weighted Boxes Fusion

### What didn't work
- Heavy augmentation:
    - 2 x customed Mosaic data augmentation (deleted tiny bouding boxes)
        - RandomRotate + CenterCrop (w/o artifacts) || Transpose (Reflection) + RandomCrop
        - RandomCrop + random x_center and y_center
    - MixUp
    - AdditiveGaussianNoise, GaussNoise, MotionBlur, MedianBlur, Blur, CLAHE, Sharpen, Emboss, RandomBrightnessContrast
- Ensemble multi-scale model: Weighted Boxes Fusion
- Test time augmentation (HorizontalFlip, VerticalFlip)
- CIoU and GIoU loss
### Miscellaneous:
annotations_train.json:

![image](https://user-images.githubusercontent.com/67547213/142844575-bd336aa8-28ae-4d7d-a54b-6f2404e39aea.png)

![image](https://user-images.githubusercontent.com/67547213/142841193-9e9b28d1-ea05-4633-ac7b-c3137a1d1a09.png)
![image](https://user-images.githubusercontent.com/67547213/142841286-9fc48013-eea3-4da9-9b2a-f2d47987da80.png)
![image](https://user-images.githubusercontent.com/67547213/142849856-9e40d77f-c08f-49aa-ac2a-f5972fa43afd.png)

- `segmentation` dict: represents the per-pixel segmentation mask in COCO’s compressed RLE format. The dict should have keys “size” and “counts”.
- You can convert a uint8 segmentation mask of 0s and 1s into such dict by `pycocotools.mask.encode(np.asarray(mask, order="F"))`. 
- `cfg.INPUT.MASK_FORMAT` must be set to bitmask if using the default data loader with such format.

