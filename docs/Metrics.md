A loss function or metric (or objective function, or optimization score function) is a scoring function to evaluate the current predictive power and performance of a model.

A loss function is required for training and have to be decreasing, if the model performance is increasing. During the training process, the neural network model varies weights of its neurons and tries to optimize the loss function to be as small as possible. A model can only use one loss function.

Metrics are for user evaluation in order to give insights on the training process or the current performance status of the model. It is possible to pass multiple metrics to the model.

MIScnn loss functions and metrics are based on Keras loss/metric functions.  
Any loss or metric function defined in Keras, in miscnn.neural_network.metrics or any custom function, which follows the Keras metric guidelines, can be used in MIScnn.

## Usage of loss functions and metrics

The loss function and metrics can be defined at the neural network model class initialization.  
It is possible to pass the desired loss function with the 'loss' tag and the metrics within a standard python list with the 'metrics' tag.

```python
# Import desired loss functions or metrics
from miscnn.neural_network.metrics import tversky_loss, dice_soft
import keras

# Add the loss function & metrics to the neural network model
model = Neural_Network(preprocessor=pp,
                       loss=tversky_loss,
                       metrics=[dice_soft, keras.losses.categorical_crossentropy])
```

If no loss or metrics are provided, MIScnn uses the Tversky loss function and no metrics as default.

## Available loss functions and metrics provided by MIScnn

#### Tversky loss

A generalized loss function based on the Tversky index to address the issue of data imbalance and achieve much better trade-off between precision and recall in training 3D fully convo- lutional deep neural networks.  
Reference: https://arxiv.org/abs/1706.05721

```python
from miscnn.neural_network.metrics import tversky_loss
model = Neural_Network(preprocessor=pp, loss=tversky_loss)
```

------------------

#### Sum of Tversky loss and Cross-entropy

Based on the idea of Fabian Isensee et al. for using the sum of the Dice Similarity Coefficient and the Cross-Entropy, MIScnn implemented a loss function based on the sum of the Tversky loss & Cross-Entropy loss function.  
Reference Fabian Isensee et al: http://arxiv.org/abs/1809.10486

```python
from miscnn.neural_network.metrics import tversky_crossentropy
model = Neural_Network(preprocessor=pp, loss=tversky_crossentropy)
```

------------------

#### Dice Similarity Coefficient (DSC)

The Dice Similarity Coefficient is one the most popular loss functions for training deep learning models in medical image segmentation. It has the ability the handle strong class imbalance which is common in medical imaging. The standard Dice Similarity Coefficient is suited for binary medical image segmentation problems.  
Reference: ->  add wikipedia link

```python
from miscnn.neural_network.metrics import dice_coefficient_loss
model = Neural_Network(preprocessor=pp, loss=dice_coefficient_loss)
```

------------------

#### Soft Dice Similarity Coefficient

The Soft Dice Similarity Coefficient is an extension of the standard Dice Similarity Coeffcient. The only difference is, that the soft DSC is normalized on the number of classes. Therefore, the soft Dice Similarity Coefficient is more suited for multi-class (>2 classes) medical image segmentation problems.  
Reference: ->  add wikipedia link

```python
from miscnn.neural_network.metrics import dice_soft_loss
model = Neural_Network(preprocessor=pp, loss=dice_soft_loss)
```

------------------

#### Sum of Soft Dice Similarity Coefficient and Cross-entropy

In diverse medical image segmentation challenges, the winner Fabian Isensee et al. is always using the sum of the soft Dice Similarity Coeffcient and the Cross-Entropy in order to achieve the best performance. Additionally, Fabian Isensee et al. also uses this loss function in his powerful software nnUnet.  
Reference: http://arxiv.org/abs/1809.10486

```python
from miscnn.neural_network.metrics import dice_crossentropy
model = Neural_Network(preprocessor=pp, loss=dice_crossentropy)
```

------------------
