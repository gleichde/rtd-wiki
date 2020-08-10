The validation is one of the key steps in any medical image analysis. The aim of the validation is to utilize the fitted model in order to provide an unbiased performance evaluation of it. The resulting validation performance can be an indicator for overfitting to the training data or the need of hyperparameter adjustment.

MIScnn provides the following three popular validation techniques: Cross-Validation, Percentage-Split Validation and Leave-One-Out Validation.

## Methods

#### k-fold Cross-Validation

```python
cross_validation(sample_list, model, k_fold=3, epochs=20,
                 iterations=None, evaluation_path="evaluation",
                 draw_figures=True, run_detailed_evaluation=True,
                 callbacks=[], save_models=True, return_output=False)
```

Function for an automatic k-fold Cross-Validation of the Neural Network model by running the whole pipeline several times with different data set combinations.

**Arguments:**  
- **sample_list:** A list of sample indicies which will be used for validation.
- **model:** Instance of a Neural Network model class instance.
- **k_fold:** The number of k-folds for the Cross-Validation.
- **epochs:** Number of epochs. A single epoch is defined as one iteration through the complete data set.
- **iterations:** Number of iterations (batches) in a single epoch.
- **evaluation_path:** Path to the evaluation data directory. This directory will be created and used for storing all kinds of evaluation results during the validation processes.
- **draw_figures:** Boolean, whether evaluation figures should be automatically plotted in the evaluation directory.
- **run_detailed_evaluation:** Boolean, whether a detailed evaluation (additional prediction) should be performed.
- **callbacks:** A list of Callback classes for custom evaluation.
- **save_models:** Boolean, whether fitted models should be stored or thrown away.
- **return_output:** Boolean, whether computed evaluations will be output as the return of this function or if the evaluations will be saved on disk in the evaluation directory.

**Returns:**  
None or validation results if return_output is true. The variable validation_results is a list of Keras History objects containing all kinds of information. For each fold, a history object will created and appended to the list.

**Example:**  
```python
model = miscnn.Neural_Network(preprocessor=pp)

from miscnn.evaluation import cross_validation
cross_validation(sample_list, model, k_fold=3, epochs=50)
```

--------------------------------------------------------

#### Percentage-Split Validation

```python
split_validation(sample_list, model, percentage=0.2, epochs=20,
                 iterations=None, evaluation_path="evaluation",
                 draw_figures=True, run_detailed_evaluation=True,
                 callbacks=[], return_output=False)
```

Function for an automatic Percentage-Split Validation of the Neural Network model by running the whole pipeline once with a test and train data set.

**Arguments:**  
- **sample_list:** A list of sample indicies which will be used for validation.
- **model:** Instance of a Neural Network model class instance.
- **percentage:** Testing set percentage of the whole sample list size.
- **epochs:** Number of epochs. A single epoch is defined as one iteration through the complete data set.
- **iterations:** Number of iterations (batches) in a single epoch.
- **evaluation_path:** Path to the evaluation data directory. This directory will be created and used for storing all kinds of evaluation results during the validation processes.
- **draw_figures:** Boolean, whether evaluation figures should be automatically plotted in the evaluation directory.
- **run_detailed_evaluation:** Boolean, whether a detailed evaluation (additional prediction) should be performed.
- **callbacks:** A list of Callback classes for custom evaluation.
- **return_output:** Boolean, whether computed evaluations will be output as the return of this function or if the evaluations will be saved on disk in the evaluation directory.

**Returns:**  
None or a Keras History objects if return_output is true.

**Example:**  
```python
model = miscnn.Neural_Network(preprocessor=pp)

from miscnn.evaluation import split_validation
split_validation(sample_list, model, percentage=0.2, epochs=50)
```

--------------------------------------------------------

#### Leave-One-Out Validation

```python
leave_one_out(sample_list, model, epochs=20, iterations=None,
              callbacks=[], evaluation_path="evaluation")
```

Function for an automatic Leave-One-Out Validation of the Neural Network model by running the whole pipeline once by training on the complete data set except one sample and then predict the segmentation of the last remaining sample.

**Arguments:**  
- **sample_list:** A list of sample indicies which will be used for validation.
- **model:** Instance of a Neural Network model class instance.
- **epochs:** Number of epochs. A single epoch is defined as one iteration through the complete data set.
- **iterations:** Number of iterations (batches) in a single epoch.
- **callbacks:** A list of Callback classes for custom evaluation.
- **evaluation_path:** Path to the evaluation data directory. This directory will be created and used for storing all kinds of evaluation results during the validation processes.

**Returns:**  None.

**Example:**  
```python
model = miscnn.Neural_Network(preprocessor=pp)

from miscnn.evaluation import detailed_validation
detailed_validation(sample_list, model, evaluation_path="evaluation")
```

--------------------------------------------------------

#### Detailed Validation

```python
detailed_validation(validation_samples, model, evaluation_path)
```

Function for detailed validation of a validation sample data set. The segmentation of these samples will be predicted with an already fitted model and evaluated.  
Normally, this method will be called automatically inside one of the three validation techniques. Nevertheless, it is possible to use the detailed validation method manually.

Be aware, that this function only predicts and evaluates the given samples. The detailed validation does NOT include any training process.


**Arguments:**  
- **validation_samples:** A list of sample indicies which will be used for validation.
- **model:** Instance of a Neural Network model class instance.
- **evaluation_path:** Path to the evaluation data directory. This directory will be created and used for storing all kinds of evaluation results during the validation processes.

**Returns:**  None.

**Example:**  
```python
model = miscnn.Neural_Network(preprocessor=pp)

from miscnn.evaluation.detailed_validation import detailed_validation
detailed_validation(validation_samples, model, evaluation_path="evaluation")
```

--------------------------------------------------------
