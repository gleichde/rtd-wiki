# Welcome to the MIScnn wiki!

The open-source Python library MIScnn is an intuitive API allowing fast setup of medical image segmentation pipelines with state-of-the-art convolutional neural network and deep learning models in just a few lines of code.

MIScnn provides several core features:
- 2D/3D medical image segmentation for binary and multi-class problems
- Data I/O, preprocessing and data augmentation for biomedical images
- Patch-wise and full image analysis
- State-of-the-art deep learning model and metric library
- Intuitive and fast model utilization (training, prediction)
- Multiple automatic evaluation techniques (e.g. cross-validation)
- Custom model, data I/O, pre-/postprocessing and metric support
- Based on Keras with Tensorflow as backend

## Philosophy of MIScnn

**User friendliness:**  
MIScnn is an intuitive API designed for human beings, not machines. With a stronger growing interest in medical imaging processing, building medical image segmentation pipelines shouldn't be like inventing the wheel for every new user. MIScnn offers consistent & simple APIs for minimizing the number of user actions required for common use cases.

**Modularity:**  
The general steps in medical image processing are identical in nearly all projects. Nevertheless, switching to another neural network architecture or data set format break most public available medical image processing software, today. MIScnn changes this situation! In particular, Data I/O, pre-/postprocessing functions, metrics and model architectures are standalone interfaces that you can easily switch.

**Easy extensibility:**  
New interfaces are simple to integrate into the MIScnn pipeline. Existing interfaces provide sample examples. Additionally, MIScnn provide Abstract Base Classes for all interfaces which helps defining the structure and setup of custom interfaces. This results into easy integration of user architectures or adapting MIScnn to your data structure.

**Work with Python:**  
MIScnn can be used in Python code, which is compact, easier to debug, allow easier deployment and integration into workflows. The utilization of an intuitive framework guide to better understanding and customization instead of standalone blackbox software.

## Pipeline visualization

![Screenshot](https://raw.githubusercontent.com/frankkramer-lab/MIScnn/master/docs/MIScnn.pipeline.png)

## Support

For help on how to use MIScnn, check out the tutorials, examples and further documentation.

If you have any problem or identified a bug, please use the ISSUES system of GitHub. Share your problems and contribute to improve MIScnn.

## Why this name, MIScnn?

Maybe, you are asking yourself: What abbreviation MIScnn stands for?  
The answer is quite simple: **M**edical **I**mage **S**egmentation with **C**onvolutional **N**eural **N**etworks and deep learning.

Wondering what happened to the "deep learning" part and why it wasn't named "MIScnndl"?   
Also, a quite simple answer: No one would be able to pronounce it. ;)

And how should I pronounce it correctly?  
Answer: MIZ-C-N-N
