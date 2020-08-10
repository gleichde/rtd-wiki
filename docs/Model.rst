This class provides functionality for handling all model methods. This class runs the whole pipeline and uses a Preprocessor instance to obtain batches.

With an initialized Neural Network model instance, it is possible to run training, prediction and evaluations.

## Methods

#### Initialization

```python
Neural_Network(preprocessor, architecture=Architecture(),
               loss=tversky_loss, metrics=[dice_soft],
               learninig_rate=0.0001, batch_queue_size=2,
               workers=1, gpu_number=1)
```

Initialization function for creating a Neural Network (model) object.

**Arguments:**  
- **preprocessor:** Preprocessor class instance which provides the Neural Network with batches.
- **architecture:** Instance of a neural network model Architecture class instance. By default, a standard U-Net is used as Architecture.
- **loss:** The Metric function which is used as loss for training.
- **metrics:** List of one or multiple Metric Functions, which will be shown during training.
- **learninig_rate:** Learning rate in which weights of the neural network will be updated.
- **batch_queue_size:** The batch queue size is the number of previously prepared batches in the cache during runtime.
- **workers:** Number of workers/threads which preprocess batches during runtime.
- **gpu_number:** Number of GPUs, which will be used for training.

**Returns:**  
A Neural Network Model class object.

**Example:**  
```python
model = miscnn.Neural_Network(preprocessor=pp)
model.train(sample_list[0:100], epochs=50)
```

--------------------------------------------------------

#### train

```python
train(sample_list, epochs=20, iterations=None, callbacks=[])
```

Fitting function for the Neural Network model using the provided list of sample indices.

**Arguments:**
- **sample_list:**  A list of sample indicies which will be used for training.
- **epochs:** Number of epochs. A single epoch is defined as one iteration through the complete data set.
- **iterations:** Number of iterations (batches) in a single epoch.
- **callbacks:** A list of Keras Callback classes for custom evaluation.

**Returns:**  
None.

**Example:**  
```python
model.train(sample_list[0:100], epochs=50)
```

--------------------------------------------------------

#### predict

```python
predict(sample_list, return_output=False)
```

Prediction function for the Neural Network model. The fitted model will predict a segmentation for the provided list of sample indices.

**Arguments:**
- **sample_list:**  A list of sample indicies for which a segmentation prediction will be computed.
- **return_output:** Boolean, whether computed predictions will be output as the return of this function or if the predictions will be saved with the save_prediction method defined in the provided Data I/O interface.
- **activation_output:** Parameter which decides, if model output (activation function, normally softmax) will be saved/outputed (if FALSE) or if the resulting class label (argmax) should be outputed.

**Returns:**  
None or a list of NumPy arrays containing predictions.

**Example:**  
```python
# Passing output of predictions into Data I/O interface - (intended for disk storage)
model.predict(sample_list[100:120])

# Direct output of predictions into variable (memory)
predictions = model.predict(sample_list[100:120], return_output=True)
```

--------------------------------------------------------

#### evaluate

```python
evaluate(training_samples, validation_samples, epochs=20,
         iterations=None, callbacks=[]):
```

Evaluation function for the Neural Network model using the provided lists of sample indices for training and validation. It is also possible to pass custom Callback classes in order to obtain more information.

**Arguments:**
- **training_samples:**  A list of sample indicies which will be used for training.
- **validation_samples:** A list of sample indicies which will be used for validation.
- **epochs:** Number of epochs. A single epoch is defined as one iteration through the complete data set.
- **iterations:** Number of iterations (batches) in a single epoch.
- **callbacks:** A list of Keras Callback classes for custom evaluation.

**Returns:**  
Keras history object (gathered fitting information and evaluation results of the validation).

**Example:**  
```python
history = model.evaluate(training_samples=sample_list[0:80],
                         validation_samples=sample_list[80:100],
                         iterations=10)
```

--------------------------------------------------------

#### reset_weights

```python
reset_weights()
```

Re-initialize weights of the neural network model.

**Arguments:** None.

**Returns:**  None.

**Example:**  
```python
# Fit weights of model
model.train(sample_list[0:100], epochs=50)
# Throw fitted weights away to default weights
model.reset_weights()
```

--------------------------------------------------------

#### dump

```python
dump(file_path)
```

Dump neural network model and its weights to file.

**Arguments:**
- **file_path:** Output path, at which the model will be stored.

**Returns:**  None.

**Example:**  
```python
# Fit weights of model
model.train(sample_list[0:100], epochs=50)
# Save fitted model for reusability
model.dump("my_model.hdf5")
```

--------------------------------------------------------

#### load

```python
load(file_path, custom_objects={}):
```

Load neural network model and its weights from a file. After loading, the model will be compiled.

**Arguments:**
- **file_path:** Input path, from which the model will be loaded.
- **custom_objects:** Dictionary for custom objects for compiling (e.g. custom-defined metrics).

**Returns:**  None.

**Example:**  
```python
# Save fitted model for reusability
model.load("my_model.hdf5")
# Predict segmentation of some samples
model.predict(sample_list[100:120])
```

--------------------------------------------------------
