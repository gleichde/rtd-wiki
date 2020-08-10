Image data augmentation is a technique that can be used to artificially expand the size of a training dataset by creating modified versions of images in the dataset. The point of data augmentation is, that the model will learn meaningful patterns instead of meaningless characteristics due to a small data set size.

The Data Augmentation class is based on python library: batchgenerators by MIC@DKFZ.  
Reference: [https://github.com/MIC-DKFZ/batchgenerators](https://github.com/MIC-DKFZ/batchgenerators)

## Methods

#### Initialization

```python
Data_Augmentation(cycles=1, scaling=True, rotations=True,
                  elastic_deform=False, mirror=False, brightness=True,
                  contrast=True, gamma=True, gaussian_noise=True)
```

Initialization function for creating an Data Augmentation object.  

**Arguments:**  
- **cycles:** Number of augmentated image copies that should be created.
- **scaling:** Boolean, whether scaling should be performed as data augmentation.
- **rotations:** Boolean, whether rotations should be performed as data augmentation.
- **elastic_deform:** Boolean, whether elastic deformation should be performed as data augmentation.
- **mirror:** Boolean, whether mirroring should be performed as data augmentation.
- **brightness:** Boolean, whether brightness changes should be added as data augmentation.
- **contrast:** Boolean, whether contrast changes should be added as data augmentation.
- **gamma:** Boolean, whether gamma changes should be added as data augmentation.
- **gaussian_noise:** Boolean, whether Gaussian noise should be added as data augmentation.

**Returns:**  
A Data_Augmentation class object. Have to be passed to the Preprocessor class.

**Configurable Class Variables:**  

Various configurations of specific data augmentation techniques can be adjusted if needed. The documentation of the augmentation functions can be found in the official git repository of batchgenerators:  
[https://github.com/MIC-DKFZ/batchgenerators](https://github.com/MIC-DKFZ/batchgenerators)

The default setttings were obtained from nnUNet. An U-Net implementation of the batchgenerators authors:  
[https://github.com/MIC-DKFZ/nnUNet/blob/master/nnunet/training/data_augmentation/default_data_augmentation.py](https://github.com/MIC-DKFZ/nnUNet/blob/master/nnunet/training/data_augmentation/default_data_augmentation.py)

- config_p_per_sample = 0.15
- config_mirror_axes = (0, 1, 2)
- config_contrast_range = (0.3, 3.0)
- config_brightness_range = (0.5, 2)
- config_gamma_range = (0.7, 1.5)
- config_gaussian_noise_range = (0.0, 0.05)
- config_elastic_deform_alpha = (0.0, 900.0)
- config_elastic_deform_sigma = (9.0, 13.0)
- config_rotations_angleX = (-15. / 360 * 2. * np.pi, 15. / 360 * 2. * np.pi)
- config_rotations_angleY = (-15. / 360 * 2. * np.pi, 15. / 360 * 2. * np.pi)
- config_rotations_angleZ = (-15. / 360 * 2. * np.pi, 15. / 360 * 2. * np.pi)
- config_scaling_range = (0.85, 1.25)

**Example:**  
```python
# Initialize
data_aug = Data_Augmentation(cycles=2,
                             scaling=False, rotations=False,
                             elastic_deform=False, mirror=False,
                             brightness=False, contrast=False,
                             gamma=True, gaussian_noise=True)
# Further configurations
data_aug.config_p_per_sample = 0.35
data_aug.config_contrast_range = (1, 2)

# Pass to Processor
pp = Preprocessor(data_io, data_aug=data_aug)
```

--------------------------------------------------------

#### run

```python
run(img_data, seg_data)
```
Run data augmentation on a given image and segmentation NumPy array.  
This function is automatically called inside the Preprocessor object during batch generation. Therefore, the user do not have to use this function on their own.

**Arguments:**
- **img_data:** NumPy array containing the image
- **seg_data:** NumPy array containing the segmentation

**Returns:**  
Two NumPy arrays. One NumPy array containing the augmentated image and one NumPy array containing the corresponding augmentated segmentation.

**Example:**  
```python
img_data_aug, seg_data_aug = data_aug.run(img_data, seg_data)
```

--------------------------------------------------------
