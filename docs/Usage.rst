## Core steps of the MIScnn pipeline

The MIScnn pipeline is represented by 4 core classes:

- Data I/O  
- Data Augmentation (optional)
- Preprocessor
- Neural Network Model

These classes handle all required steps for medical image segmentation and can be extensively customized. All classes, except the Data Augmentation class, use switchable interfaces which results into high configurability and offers simple integration of user-defined solutions.

![alt-text](https://raw.githubusercontent.com/frankkramer-lab/MIScnn/master/docs/MIScnn.basic_usage.png)

## Workflow of MIScnn

#### Import the MIScnn Module

Before constructing our Medical Image Segmentation pipeline, we are going to import the MIScnn modules which will provide our core classes and functions.

```python
import miscnn
```

#### 1) Data I/O

The first step in the MIScnn pipeline is to establish the Data I/O class. The Data I/O handles the complete data loading of biomedical images, its segmentation and storage of predicted files, as well as temporary files like batches.

In order to open the MIScnn pipeline for a various data formats and structures, the data I/O class uses switchable I/O interfaces. It is possible to use already provided I/O interfaces with a fixed data structure or custom I/O interfaces for integration of your specific data structure into the MIScnn pipeline.

Before initializing an instance of the Data I/O class, we have to select an I/O interface. This interface have to load our data set into the MIScnn pipeline. In our example, we are using the KiTS19 data set which are kidney tumor CT scans encoded in the NIfTI format. Therefore, we call the provided NIfTI interface with a single channel (grayscale image) and 3 classes for the segmentation (background, kidney and tumor).

```python
from miscnn.data_loading.interfaces.nifti_io import NIFTI_interface

interface = NIFTI_interface(pattern="case_000[0-9]*", channels=1,
                            classes=3)
```

Afterwards, we can initialize the Data I/O class by passing our newly created interface and the path to the data set to the constructor.

```python
data_path = "/home/mudomini/projects/kits19/data/"
data_io = miscnn.Data_IO(interface, data_path)
```

#### 2) Preprocessor

Finally, we can initialize the Preprocessor class.

Therefore, we are passing our Data I/O Interface to the Preprocessor initialization. Additionally, we configure our Preprocessor to create batches on-the-fly (preprare_batches=False) by random cropping patches with size 80x160x160 out of the image.
Also a batch should contain 2 patches.

```python
# Create and configure the Preprocessor class
pp = miscnn.Preprocessor(data_io, batch_size=2, analysis="patchwise-crop",
                         patch_shape=(80,160,160))
```

###### Specefic Data Augmentation

If you want to specify the Data Augmentation, it is possible to configure the Data Augmentation class.
A Preprocessor object with default parameters automatically initialize a Data Augmentation class with default values, but here we initialize it by hand to illustrate the exact workflow of the MIScnn pipeline.

The parameters for the Data Augmentation configure which augmentation techniques should be applied to the data set.
In this case, we are using all possible augmentation techniques in order to run extensive data augmentation and avoid overfitting

```python
# Create and configure the Data Augmentation class
data_aug = miscnn.Data_Augmentation(cycles=1, scaling=True, rotations=True,
                                    elastic_deform=True, mirror=True,
                                    brightness=True, contrast=True,
                                    gamma=True, gaussian_noise=True)

# Create and configure the Preprocessor class
pp = miscnn.Preprocessor(data_io, data_aug=data_aug, batch_size=2,
                        analysis="patchwise-crop", patch_shape=(80,160,160))
```

#### 3) Neural Network Model

Now it's time to initialize our final object: The Neural Network.

With the Neural Network class, we are able to run training and prediction operations. But before, we have to decide which Neural Network Architecture and Metric we want to use.  
In order to show the simplicity and performance of MIScnn, we stick with the simple 3D U-Net Architecture for our Neural Network without any special tricks or optimizations. For training, we are using the Soft Dice as loss.

```python
# Import standard U-Net architecture and Soft Dice
from miscnn.neural_network.architecture.unet.standard import Architecture
from miscnn.neural_network.metrics import dice_soft
unet_standard = Architecture()

# Create a deep learning neural network model with a standard U-Net architecture
model = miscnn.Neural_Network(preprocessor=pp, architecture=unet_standard,
                              loss=dice_soft)
```

Now, let's run a model training on our data set for 500 epochs and, afterwards, predict the segmentation of a sample using the fitted model.

```python
# Training the model with 80 samples for 500 epochs
sample_list = data_io.get_indiceslist()
model.train(sample_list[30:120], epochs=500)

# Predict the segmentation of 20 samples
model.predict(sample_list[0:30])
```

The predicted segmentation results will be passed to the Data I/O interface with the function save_prediction. The already implemented interfaces (like NiFTI interface) will store the predictions into files.

#### 4) Results

We used our fitted U-Net model and successfully predicted a segmentation for our Kidney Tumor CT image.  
But how do we know, how good our segmentation is compared to the ground-truth?

We have the possibility to calculate several different scores (like the already implemented variants of the Dice Similarity Coefficient), but for simplicity, we want to compare it manually by eye. MIScnn provides a simple function to create comparison images (and for 3D data GIFs).

```python
# Load the sample
sample = data_io.sample_loader(sample_list[24], load_seg=True, load_pred=True)
# Access image, truth and predicted segmentation data
img, seg, pred = sample.img_data, sample.seg_data, sample.pred_data
# Visualize the truth and prediction segmentation as a GIF
from miscnn.utils.visualizer import visualize_evaluation
visualize_evaluation(sample_list[24], img, seg, pred, "plot_directory/")
```

![alt-text](https://raw.githubusercontent.com/frankkramer-lab/MIScnn/master/docs/visualization.case_case_00024.gif)

#### Complete Code

```python
import miscnn

# Initialize the NIfTI I/O interface and configure the images as one channel (grayscale)
# and three segmentation classes (background, kidney, tumor)
from miscnn.data_loading.interfaces.nifti_io import NIFTI_interface
interface = NIFTI_interface(pattern="case_000[0-9]*", channels=1,
                            classes=3)
# Create the Data I/O object
data_path = "/home/mudomini/projects/kits19/data/"
data_io = miscnn.Data_IO(interface, data_path)

# Create and configure the Data Augmentation class
data_aug = miscnn.Data_Augmentation(cycles=1, scaling=True, rotations=True,
                                    elastic_deform=True, mirror=True,
                                    brightness=True, contrast=True,
                                    gamma=True, gaussian_noise=True)

# Create and configure the Preprocessor class
pp = miscnn.Preprocessor(data_io, data_aug=data_aug, batch_size=2,
                         analysis="patchwise-crop", patch_shape=(80,160,160))

# Import standard U-Net architecture and Soft Dice
from miscnn.neural_network.architecture.unet.standard import Architecture
from miscnn.neural_network.metrics import dice_soft
unet_standard = Architecture()

# Create a deep learning neural network model with a standard U-Net architecture
model = miscnn.Neural_Network(preprocessor=pp, architecture=unet_standard,
                              loss=dice_soft)

# Training the model with 80 samples for 500 epochs
sample_list = data_io.get_indiceslist()
model.train(sample_list[0:80], epochs=500)

# Predict the segmentation of 20 samples
model.predict(sample_list[80:100])

# Load the sample
sample = data_io.sample_loader(sample_list[80], load_seg=True, load_pred=True)
# Access image, truth and predicted segmentation data
img, seg, pred = sample.img_data, sample.seg_data, sample.pred_data
# Visualize the truth and prediction segmentation as a GIF
from miscnn.utils.visualizer import visualize_evaluation
visualize_evaluation(sample_list[80], img, seg, pred, "plot_directory/")
```

-----------------------------------------------------------------

For more detailed coding examples, check out the MIScnn wiki for
[tutorials](https://github.com/frankkramer-lab/MIScnn/wiki/Tutorials) or [examples](https://github.com/frankkramer-lab/MIScnn/wiki/Examples).
