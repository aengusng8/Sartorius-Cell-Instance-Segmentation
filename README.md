
![image](https://user-images.githubusercontent.com/67547213/147556955-26dda52c-e535-4ced-afe6-bf217b7e4d28.png)

## Summary
- Model 1 (Classifier): EfficientDetV2
- Model 2 (Instance Segmentation): 
- Training stage 1:
- Training stage 2: 
- 5 folds


## What didn't work
- Heavy augmentation:
    - 2 x customed Mosaic data augmentation (deleted tiny bouding boxes)
        - RandomRotate + CenterCrop || Transpose (Reflection) + RandomCrop
        - RandomCrop + random x_center and y_center
    - MixUp
    - AdditiveGaussianNoise, GaussNoise, MotionBlur, MedianBlur, Blur, CLAHE, Sharpen, Emboss, RandomBrightnessContrast
- Ensemble multi-scale model: Weighted-Boxes-Fusion
- Test time augmentation(HorizontalFlip, VerticalFlip)
## Miscellaneous:
annotations_train.json:

![image](https://user-images.githubusercontent.com/67547213/142844575-bd336aa8-28ae-4d7d-a54b-6f2404e39aea.png)

![image](https://user-images.githubusercontent.com/67547213/142841193-9e9b28d1-ea05-4633-ac7b-c3137a1d1a09.png)
![image](https://user-images.githubusercontent.com/67547213/142841286-9fc48013-eea3-4da9-9b2a-f2d47987da80.png)
![image](https://user-images.githubusercontent.com/67547213/142849856-9e40d77f-c08f-49aa-ac2a-f5972fa43afd.png)

- `segmentation` dict: represents the per-pixel segmentation mask in COCO’s compressed RLE format. The dict should have keys “size” and “counts”.
- You can convert a uint8 segmentation mask of 0s and 1s into such dict by `pycocotools.mask.encode(np.asarray(mask, order="F"))`. 
- `cfg.INPUT.MASK_FORMAT` must be set to bitmask if using the default data loader with such format.

