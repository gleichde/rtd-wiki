The Sample class is provides objects to store all kinds of information for a single sample.  

It mandatory variables of a Samples are its index (id), an image and some information of the image like its shape and the number of channels.
The Sample class can also store a optional segmentation with its number of classes, as well as a predicted segmentation from the model.

It is also possible to add additional custom information in the details dictionary. This feature can be exploited in later custom interfaces like in a Subfunction.

The Data IO class will automatically create Sample objects during a Pipeline run. It is also possible to obtain all created Sample objects by the following call:

```python
sample_list = data_io.get_indiceslist()
```

## Methods

#### Initialization

```python
Sample(index, image, channels, classes)
```

Initialization function for creating a Sample object.  

**Arguments:**  
- **index:** Index (String) of a sample.
- **image:** NumPy array containing the image.
- **channels:** Number of channels of the image (dimension of last layer).
- **classes:** Number of classes of the segmentation.

**Returns:**  
A Sample class object.

**Example:**  
```python
sample = data_io.sample_loader(index="case_00001", load_seg=True)

img = sample.img_data
```

--------------------------------------------------------
