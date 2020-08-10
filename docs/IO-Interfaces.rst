The Data IO class provides functionality for handling all input and output processes of the imaging data, as well as the temporary backup of batches to the disk.

Due to the large variety of different imaging formats, the user is required to provide an Data IO Interface for the correct format.  
The aim of the IO Interface is to handle the loading and saving process of images in their specific formats and file structure.

The Data IO Interface module enables MIScnn to handle a wide variety of imaging formats, as well as offers the possibility to integrate any user-required image format or data structure into the MIScnn pipeline.

## Usage of MIScnn Data IO Interfaces

The Data IO Interface can be passed to the Data IO class initialization. The Data IO class automatically uses the IO Interface for image loading and saving.

```python
# Import desired Data IO Interface
from data_loading.interfaces.nifti_io import NIFTI_interface

# Initialize Data IO Interface
interface = NIFTI_interface(pattern="case_00[0-9]+", channels=1,
                            classes=3)

# Pass Data IO Interface to Data IO class
from data_loading.data_io import Data_IO
data_path = "/home/mudomini/projects/KITS_challenge2019/kits19/data/"
data_io = Data_IO(interface, data_path)
```

## Available Data IO Interfaces provided by MIScnn

#### NIfTI IO Interface

Data I/O Interface for NIfTI files. The Neuroimaging Informatics Technology Initiative file format is designed to contain brain images from e.g. magnetic resonance tomography. Nevertheless, it is currently broadly used for any 3D medical image data.

```python
# Import NIfTI IO Interface and initialize it
from data_loading.interfaces.nifti_io import NIFTI_interface
interface = NIFTI_interface(pattern="case_00[0-9]+",
                            channels=1, classes=3)

# Pass Interface to Data IO class
data_path = "/home/mudomini/projects/KITS_challenge2019/kits19/data.interpolated/"
data_io = Data_IO(interface, data_path)

```

------------------

#### Image IO Interface

Data I/O Interface for JPEG, PNG and other common 2D image files.  
Images are read by calling the imread function from the Pillow module.

Expected file structure:
```
data/
     imgname001/imaging.png
                segmentation.png
     imgname002/imaging.png
                segmentation.png
     imgname003/imaging.png
                segmentation.png
     ...
```

**Args:**
- classes (int):          Number of classes of the segmentation
- img_type (string):      Type of imaging. Options: "grayscale", "rgb"
- img_format (string):    Imaging format: Popular formats: "png", "tif", "jpg"

**Example:**

```python
# Import Image IO Interface and initialize it
from data_loading.interfaces import Image_interface
interface = Image_interface(classes=2, img_type="grayscale", img_format="png")

# Pass Interface to Data IO class
data_path = "/home/mudomini/celltracking.MIScnn/data/"
data_io = Data_IO(interface, data_path)
```


------------------

#### DICOM IO Interface

Data I/O Interface for 3D imaging data encoded as DICOM.  
Volumes are read by utilizing the pydicom & SimpleITK module.

Expected file structure:
```
 data/
      imgname001/
              any_id/
                      000000.dcm
                      000001.dcm
                      ...
                      000139.dcm
              annotation_tag.any_id/
                      000000.dcm
      imgname002/
              0.000000-CT2p2b2 50 Ex-45938/
                      000000.dcm
                      000001.dcm
                      ...
                      000160.dcm
              annotation_tag.simplified-15042/
                      000000.dcm
      ...
```

**Args:**
- classes (int):                Number of classes of the segmentation.
- channels (int):               Number of channels of the input images.
- three_dim (boolean):          3D volume if True.
- mask_background (int):        Determines the value of the background pixels.
- structure_dict (dictionary):  Dictionary containg ROI names.
- annotation_tag (string):      String to identify annotation series.

**Example:**

```python
# Import DICOM IO Interface and initialize it
from data_loading.interfaces import DICOM_interface
interface = DICOM_interface(structure_dict={"Lung_L": 1, "Lung_R": 2},
                            classes=3, annotation_tag="annotation_tag")

# Pass Interface to Data IO class
data_path = "/home/mudomini/LCTSC/"
data_io = Data_IO(interface, data_path)
```

------------------

#### Dictionary IO Interface

Data I/O Interface for python dictionaries. This interface uses the basic-python dictionary to load and save data. Therefore the complete data management happens in the memory. Therefore, for common data set sizes this is NOT recommended!

It is advised to use already provided I/O interfaces of this package or to implement a custom I/O interface for perfect usability.

Dictionary structure:  
**Key:** sample_index  
**Value:** Tuple containing:    
> (0) image as numpy array  
> (1) optional segmentation as numpy array  
> (2) optional prediction as numpy array  
> (3) optional details


**Example:**  
```python
# Create dictionary containing the data set
dict = {"case_00001": (image1, segmentation1),
        "case_00002": (image2, segmentation2)}

# Import Data IO Interface and pass dictionary to it
from data_loading.interfaces import Dictionary_interface
interface = Dictionary_interface(dict, channels=1,
                                 classes=2, three_dim=True)

# Pass Interface to Data IO class
data_path = "/home/mudomini/MIScnn_tmp/"
data_io = Data_IO(interface, data_path)
```

------------------

## Creation of custom Data IO Interfaces

todo

```python
todo
```

## Abstract Base Class for Data IO Interfaces

MIScnn also offers a documented Abstract Base Class for simple creation of custom Data IO Interfaces for your specific needs.

A Data IO Interface inherits the abstract_io class with the following methods: initialize, load_image, load_segmentation, load_prediction, save_prediction, load_details.

```python
#-----------------------------------------------------#
#                   Library imports                   #
#-----------------------------------------------------#
# External libraries
from abc import ABC, abstractmethod

#-----------------------------------------------------#
#       Abstract Interface for the Data IO class      #
#-----------------------------------------------------#
""" An abstract base class for a Data_IO interface.

Methods:
    __init__                Object creation function
    initialize:             Prepare the data set and create indices list
    load_image:             Load an image
    load_segmentation:      Load a segmentation
    load_prediction:        Load a prediction from file
    load_details:           Load optional information
    save_prediction:        Save a prediction to file
"""
class Abstract_IO(ABC):
    #---------------------------------------------#
    #                   __init__                  #
    #---------------------------------------------#
    """ Functions which will be called during the I/O interface object creation.
        This function can be used to pass variables in the custom I/O interface.
        The only required passed variable is the number of channels in the images,
        the number of classes in the segmentation and the dimension of the data.

        Parameter:
            channels (integer):    Number of channels of the image (grayscale:1, RGB:3)
            classes (integer):     Number of classes in the segmentation (binary:2, multi-class:3+)
            three_dim (boolean):   Variable to express, if the data is two or three dimensional
        Return:
            None
    """
    @abstractmethod
    def __init__(self, channels=1, classes=2, three_dim=False):
        self.channels = channels
        self.classes = classes
        self.three_dim = three_dim
        pass
    #---------------------------------------------#
    #                  initialize                 #
    #---------------------------------------------#
    """ Initialize and prepare the image data set, return the number of samples in the data set

        Parameter:
            input_path (string):    Path to the input data directory, in which all imaging data have to be accessible
        Return:
            indices_list [list]:    List of indices. The Data_IO class will iterate over this list and
                                    call the load_image and load_segmentation functions providing the current index.
                                    This can be used to train/predict on just a subset of the data set.
                                    e.g. indices_list = [0,1,9]
                                    -> load_image(0) | load_image(1) | load_image(9)
    """
    @abstractmethod
    def initialize(self, input_path):
        pass
    #---------------------------------------------#
    #                  load_image                 #
    #---------------------------------------------#
    """ Load the image with the index i from the data set and return it as a numpy matrix.
        Be aware that MIScnn only supports a last_channel structure.
        2D: (x,y,channel) or (x,y)
        3D: (x,y,z,channel) or (x,y,z)

        Parameter:
            index (variable):       An index from the provided indices_list of the initialize function
        Return:
            image [numpy matrix]:   A numpy matrix/array containing the image
    """
    @abstractmethod
    def load_image(self, i):
        pass
    #---------------------------------------------#
    #              load_segmentation              #
    #---------------------------------------------#
    """ Load the segmentation of the image with the index i from the data set and return it as a numpy matrix.
        Be aware that MIScnn only supports a last_channel structure.
        2D: (x,y,channel) or (x,y)
        3D: (x,y,z,channel) or (x,y,z)

        Parameter:
            index (variable):       An index from the provided indices_list of the initialize function
        Return:
            seg [numpy matrix]:     A numpy matrix/array containing the segmentation
    """
    @abstractmethod
    def load_segmentation(self, i):
        pass
    #---------------------------------------------#
    #               load_prediction               #
    #---------------------------------------------#
    """ Load the prediction of the image with the index i from the output directory
        and return it as a numpy matrix.

        Parameter:
            index (variable):       An index from the provided indices_list of the initialize function
            output_path (string):   Path to the output directory in which MIScnn predictions are stored.
        Return:
            pred [numpy matrix]:    A numpy matrix/array containing the prediction
    """
    @abstractmethod
    def load_prediction(self, i, output_path):
        pass
    #---------------------------------------------#
    #                 load_details                #
    #---------------------------------------------#
    """ Load optional details during sample creation. This function can be used to parse whatever
        information you want into the sample object. This enables usage of these information in custom
        preprocessing subfunctions.
        Example: Slice thickness / voxel spacing

        Parameter:
            index (variable):       An index from the provided indices_list of the initialize function
        Return:
            dict [dictionary]:      A basic Python dictionary
    """
    @abstractmethod
    def load_details(self, i):
        pass
    #---------------------------------------------#
    #               save_prediction               #
    #---------------------------------------------#
    """ Backup the prediction of the image with the index i into the output directory.

        Parameter:
            pred (numpy matrix):    MIScnn computed prediction for the sample index
            index (variable):       An index from the provided indices_list of the initialize function
            output_path (string):   Path to the output directory in which MIScnn predictions are stored.
                                    This directory will be created if not existent
        Return:
            None
    """
    @abstractmethod
    def save_prediction(self, pred, i, output_path):
        pass
```
