This class provides functionality for handling all input and output processes of the imaging data, as well as the temporary backup of batches to the disk. The Data I/O class is the gateway into the MIScnn pipeline and therefore one of the four core classes in MIScnn.

The user is only required to create an instance of the Data IO class with the desired specifications and IO interface for the correct format. It is possible to create a custom IO interface for handling special data structures or formats.

## Methods

#### Initialization

```python
Data_IO(interface, input_path, output_path="predictions",
        batch_path="batches", delete_batchDir=True)
```

Initialization function for creating an object of the Data IO class.

**Arguments:**  
- **interface:** Data I/O interface object.
- **input_path:** Path to the input image data directory (passed to Data I/O interface object).
- **output_path:** Path to the output directory for predictions (passed to Data I/O interface object).
- **batch_path:** Path to the directory for temporary files like batches or preprocessing files.
- **delete_batchDir:** Boolean, whether the temporary batch directory should be deleted, afterwards.

**Returns:**  
A Data_IO class object. Have to be passed to the Preprocessor class and can be used to access the sample list.

**Example:**  
```python
# Create a Data I/O interface for kidney tumor CT scans in NIfTI format
from miscnn.data_loading.interfaces import NIFTI_interface
interface = NIFTI_interface(pattern="case_000[0-9]*", channels=1, classes=3)

# Initialize data path and create the Data I/O instance
data_path = "/home/mudomini/projects/KITS_challenge2019/kits19/data.original/"
data_io = miscnn.Data_IO(interface, data_path)
```

--------------------------------------------------------

#### get_indiceslist

```python
get_indiceslist()
```
Return a list of indices for all available samples

**Arguments:** None

**Returns:**  
List of indices (Strings)

**Example:**  
```python
sample_list = data_io.get_indiceslist()

print(sample_list[0:3])
# ['case_00000', 'case_00001', 'case_00002']
```

--------------------------------------------------------

#### sample_loader

```python
sample_loader(index, load_seg=True, load_pred=False, backup=False)
```

Load a sample class object given a valid sample index.

**Arguments:**
- **index:** Index (String) of a sample.
- **load_seg:** Boolean, whether the segmentation should be loaded, as well.
- **load_pred:** Boolean, whether the prediction should be loaded, as well.
- **backup:** Boolean, whether the sample file should be created from an original image or loaded from a temporary backup file.

**Returns:**  
A sample class object.

**Example:**  
```python
index = "case_00000"
sample = data_io.sample_loader(index, load_seg=True)
```

--------------------------------------------------------
#### save_prediction

```python
save_prediction(pred, index)
```

Save a segmentation prediction into a file.

**Arguments:**
- **pred:** NumPy array containing a prediction.
- **index:** Index (String) of a sample.

**Returns:**  
None

**Example:**  
```python
index = "case_00000"
prediction = model.predict([index], return_output=True)[0]

data_io.save_prediction(prediction, index)
```

--------------------------------------------------------
