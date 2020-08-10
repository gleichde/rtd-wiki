This class provides functionality for handling all preprocessing methods. This includes diverse optional processing subfunctions like resampling, clipping, normalization or custom subfcuntions. This class processes the data into batches which are ready to be used for training, prediction and validation.

The user is only required to create an instance of the Preprocessor class with the desired specifications
and Data IO instance (optional also Data Augmentation instance).

## Methods

#### Initialization

```python
Preprocessor(data_io, batch_size, subfunctions=[],
             data_aug=Data_Augmentation(), prepare_subfunctions=False,
             prepare_batches=False, analysis="patchwise-crop",
             patch_shape=None)
```

Initialization function for creating a Preprocessor object.  

**Arguments:**  
- **data_io:** Data IO class instance which handles all I/O operations.
- **batch_size:** Number of samples inside a single batch.
- **subfunctions:** List of Subfunctions classes which will be SEQUENTIALLY executed on the data set.
- **data_aug:** Data Augmentation class instance which performs diverse data augmentation techniques. If no Data Augmentation is provided, an instance with default settings will be created. Use data_aug=None, if you want no data augmentation at all.
- **prepare_subfunctions:** Boolean, whether all subfunctions should be prepared and backup to disk before starting the batch generation (True), or should the subfunctions preprocessing be performed during runtime? (False).
- **prepare_batches:** Boolean, whether all batches should be prepared and backup to disk before starting the training (True), or should the batches be created during runtime? (False).
- **analysis:** String. Modus selection of analysis type. Options: ["fullimage", "patchwise-crop", "patchwise-grid"]
- **patch_shape:** Integer tuple. Size and shape of a patch.

**Returns:**  
A Preprocessor class object. Have to be passed to the Neural Network Model class.

**Additional Information:**  
Modus selection of analysis type. The analysis type will define how patches are created.
- "fullimage":      Analysis of complete image data
- "patchwise-crop": Analysis of random cropped patches from the image
- "patchwise-grid": Analysis of patches by splitting the image into a grid

**Example:**  
```python
from processing.preprocessor import Preprocessor
from processing.subfunctions import Clipping, Normalization, Resampling

sf = [Clipping(min=-100, max=500), Normalization(z_score=True), Resampling((3.22, 1.62, 1.62))]

pp = Preprocessor(data_io, data_aug=None, batch_size=2, subfunctions=sf,
                  prepare_subfunctions=True, prepare_batches=False,
                  analysis="patchwise-crop", patch_shape=(80,160,160))
```

--------------------------------------------------------
