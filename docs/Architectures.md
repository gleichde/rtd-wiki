The selection of a deep learning or convolutional neural network model is the most important step in a medical image segmentation pipeline. But there is a variety of model architectures and each has different strengths and weaknesses. MIScnn features an open model interface to load and switch between provided state-of-the-art convolutional neural network models like the popular U-Net model.

Models are represented with the open-source neural network library Keras which provides an user-friendly API for commonly used neural-network building blocks on top of TensorFlow.

The already implemented models are highly configurable by definable number of neurons, custom input sizes, optional dropout and batch normalization layers or enhanced architecture versions like the Optimized High Resolution Dense-U-Net model. Additionally, MIScnn offers architectures for 3D, as well as 2D medical image segmentation.

Besides the flexibility in switching between already implemented models, the open model interface enables the ability for custom deep learning model implementations and simple integrating these custom models into the MIScnn pipeline.

[[https://github.com/frankkramer-lab/MIScnn/blob/master/docs/Model_Creation.png|alt=architectures]]

## Usage of MIScnn Architectures

An Architecture can be passed to the Neural Network class initialization. The Neural Network class automatically uses provided Architecture as model.

```python
# Import desired Architecture
from miscnn.neural_network.architecture.unet.residual import Architecture

# Initialize Architecture
unet_residual = Architecture(activation="softmax")

# Pass Architecture to Neural Network Class
model = Neural_Network(preprocessor=pp, architecture=unet_residual)
```

## Available Architectures provided by MIScnn

#### Standard U-Net

The popular and state-of-the-art architecture of medical image segmentation is the standard U-Net. The architecture consists of a contracting path to capture context and a symmetric expanding path that enables precise localization.  
Reference:  
Olaf Ronneberger, Philipp Fischer, Thomas Brox. 18 May 2015. U-Net: Convolutional Networks for Biomedical Image Segmentation. MICCAI 2015

**Arguments:**  
- **n_filters**: Number of filters in the first layer. Default 32.
- **depth**: Number of layers of the U-Net structure. Default 4.
- **activation**: Activation function for the final output layer. Default 'sigmoid'.
- **batch_normalization**: Boolean option, whether batch normalization should be applied or not. Default True.

**Example:**

```python
from miscnn.neural_network.architecture.unet.standard import Architecture
unet_standard = Architecture(n_filters=24, batch_normalization=False)

model = Neural_Network(preprocessor=pp, architecture=unet_standard)
```

------------------

#### Plain U-Net

A plain variant of the popular U-Net architecture based on the winning model architecture of Fabian Isensee at the Kidney Tumor Segmentation Challenge 2019.  
Reference: http://arxiv.org/abs/1908.02182

**Arguments:**  
- **activation**: Activation function for the final output layer. Default 'softmax'.
- **batch_normalization**: Boolean option, whether batch normalization should be applied or not. Default True.

**Example:**

```python
from miscnn.neural_network.architecture.unet.plain import Architecture
unet_plain = Architecture()

model = Neural_Network(preprocessor=pp, architecture=unet_plain)
```

------------------

#### Residual U-Net

The Residual variant of the popular U-Net architecture. It is an improved version of the standard U-Net and uses an additional add layer after each convolutional block (2x conv layers). Be aware, that a residual architecture requires additional GPU RAM.  
Reference:  
Zhang Zhengxin, Liu Qingjie, Wang Yunhong. 2018. Road Extraction by Deep Residual U-Net. IEEE Geoscience and Remote Sensing Letters.

**Arguments:**  
- **n_filters**: Number of filters in the first layer. Default 32.
- **depth**: Number of layers of the U-Net structure. Default 4.
- **activation**: Activation function for the final output layer. Default 'sigmoid'.
- **batch_normalization**: Boolean option, whether batch normalization should be applied or not. Default True.

**Example:**

```python
from miscnn.neural_network.architecture.unet.residual import Architecture
unet_residual = Architecture()

model = Neural_Network(preprocessor=pp, architecture=unet_residual)
```

------------------

#### Compact U-Net

The Compact variant of the popular U-Net architecture. It is an improved version of the standard U-Net and uses an additional concatenate layer after each convolutional block (2x conv layers). Be aware, that a compact architecture requires additional GPU RAM.  
Reference: https://arxiv.org/pdf/1512.03385.pdf

**Arguments:**  
- **n_filters**: Number of filters in the first layer. Default 32.
- **depth**: Number of layers of the U-Net structure. Default 4.
- **activation**: Activation function for the final output layer. Default 'sigmoid'.
- **batch_normalization**: Boolean option, whether batch normalization should be applied or not. Default True.

**Example:**

```python
from miscnn.neural_network.architecture.unet.compact import Architecture
unet_compact = Architecture()

model = Neural_Network(preprocessor=pp, architecture=unet_compact)
```

------------------

#### Dense U-Net

The Dense variant of the popular U-Net architecture. It is an improved version of the standard U-Net and uses multiple concatenate layers in each convolutional block (2x conv layers). Be aware, that a dense architecture requires additional GPU RAM.  
Reference: https://arxiv.org/pdf/1512.03385.pdf

**Arguments:**  
- **n_filters**: Number of filters in the first layer. Default 32.
- **depth**: Number of layers of the U-Net structure. Default 4.
- **activation**: Activation function for the final output layer. Default 'sigmoid'.
- **batch_normalization**: Boolean option, whether batch normalization should be applied or not. Default True.

**Example:**

```python
from miscnn.neural_network.architecture.unet.dense import Architecture
unet_dense = Architecture()

model = Neural_Network(preprocessor=pp, architecture=unet_dense)
```

------------------

#### MultiRes U-Net

The MultiRes variant of the popular U-Net architecture. It is an improved version of the standard U-Net and contains some small modifications to improve upon the already state-of-the-art U-Net model.  
Reference:  
Nabil Ibtehaz and M. Sohel Rahman. February 12, 2019. MultiResUNet : Rethinking the U-Net Architecture for Multimodal Biomedical Image Segmentation. Neural Networks: Volume 121, January 2020, Pages 74-87   

**Arguments:**  
- **activation**: Activation function for the final output layer. Default 'sigmoid'.

**Example:**

```python
from miscnn.neural_network.architecture.unet.MultiRes import Architecture
unet_multires = Architecture()

model = Neural_Network(preprocessor=pp, architecture=unet_multires)
```

------------------

## Creation of custom Architectures

todo

```python
todo
```

## Abstract Base Class for Architectures

MIScnn also offers a documented Abstract Base Class for simple creation of custom Architectures for your specific needs.

An Architecture inherits the abstract_architecture class with the following methods: create_model_2D, create_model_3D.

```python
#-----------------------------------------------------#
#                   Library imports                   #
#-----------------------------------------------------#
# External libraries
from abc import ABC, abstractmethod

#-----------------------------------------------------#
#     Abstract Interface for an Architecture class    #
#-----------------------------------------------------#
""" An abstract base class for a Architecture class.

Methods:
    __init__                Object creation function
    create_model_2D:        Creating a 2D Keras model
    create_model_3D:        Creating a 3D Keras model
"""
class Abstract_Architecture(ABC):
    #---------------------------------------------#
    #                   __init__                  #
    #---------------------------------------------#
    """ Functions which will be called during the Architecture object creation.
        This function can be used to pass variables and options in the Architecture instance.
        The are no mandatory required parameters for the initialization.

        Parameter:
            None
        Return:
            None
    """
    @abstractmethod
    def __init__(self):
        pass
    #---------------------------------------------#
    #               Create 2D Model               #
    #---------------------------------------------#
    """ Create the 2D version of a deep learning or convolutional neural network model.
        This function will be called inside the pipeline and have to return a functional
        Keras model for 2D images. The model itself should be created here or in a subfunction
        called by this function.
        It is possible to pass configurations through the initialization function of this class.

        Parameter:
            input_shape (Tuple):        Input shape of the image data for the first model layer
            n_labels (Integer):         Number of classes/labels of the segmentation (by default binary problem)
        Return:
            model (Keras model):        A Keras model
    """
    @abstractmethod
    def create_model_2D(self, input_shape, n_labels=2):
        pass
    #---------------------------------------------#
    #               Create 3D Model               #
    #---------------------------------------------#
    """ Create the 3D version of a deep learning or convolutional neural network model.
        This function will be called inside the pipeline and have to return a functional
        Keras model for 3D images. The model itself should be created here or in a subfunction
        called by this function.
        It is possible to pass configurations through the initialization function of this class.

        Parameter:
            input_shape (Tuple):        Input shape of the image data for the first model layer
            n_labels (Integer):         Number of classes/labels of the segmentation (by default binary problem)
        Return:
            model (Keras model):        A Keras model
    """
    @abstractmethod
    def create_model_3D(self, input_shape, n_labels=2):
        pass
```
