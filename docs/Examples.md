The examples for MIScnn are implemented in Jupyter Notebooks. Jupyter Notebooks offer reproducibility by including the output of each coding block, but can also integrate commentary blocks with Markdown. Also, Jupyter Notebooks can be directly displayed in GitHub without any additional software.

## Overview

| Data Set | Task | Link  |
|-------------|:--------------------:|:------:|
| [KiTS19](#KiTS19) | 3-fold cross-validation on 3D images | [KiTS19.ipynb](https://github.com/frankkramer-lab/MIScnn/blob/master/examples/KiTS19.ipynb) |
| [ISBI-CTC15](#ISBI-CTC15) | Leave-One-Out Validation on 2D images | [ISBI-CTC15.ipynb](https://github.com/frankkramer-lab/MIScnn/blob/master/examples/CellTracking.ipynb) |
| [COVID-19](#COVID-19) | 5-fold cross-validation with 3D limited data | [Git Repository](https://github.com/frankkramer-lab/covid19.MIScnn) |
| [LCTSC](#LCTSC) | Split-Validation with DICOM data | [LCTSC_DICOMInterface.ipynb](https://github.com/frankkramer-lab/MIScnn/blob/master/examples/LCTSC_DICOMInterface.ipynb) |

## <a name="KiTS19"></a>Kidney Tumor Segmentation Challenge 2019 (KiTS19)

With more than 400 000 kidney cancer diagnoses worldwide in 2018, kidney cancer is under the top 10 most common cancer types in men and under the top 15 in woman.

The goal of the KiTS19 challenge is the development of reliable and unbiased kidney and kidney tumor semantic segmentation methods. Therefore, the challenge built a data set for arterial phase abdominal CT scan of 300 kidney cancer patients. The original scans have an image resolution of 512x512 and on average 216 slices (highest slice number is 1059).

For all CT scans, a ground truth semantic segmentation was created by experts. This semantic segmentation labeled each pixel with one of three classes: Background, kidney or tumor. 210 of these CT scans with the ground truth segmentation were published during the training phase of the challenge, whereas 90 CT scans without published ground truth were released afterwards in the submission phase.

The CT scans were provided in NIfTI format in original resolution and also in interpolated resolution with slice thickness normalization.

The data for the KITS19 challenge can be found here: https://github.com/neheller/kits19

## <a name="ISBI-CTC15"></a>ISBI Cell Tracking Challenge 2015 (ISBI-CTC15)

Segmenting and tracking moving cells in time-lapse video sequences is a challenging task, required for many applications in both scientific and industrial settings. Properly characterizing how cells change shapes and move as they interact with their surrounding environment is key to understanding the mechanobiology of cell migration and its multiple implications in both normal tissue development and many diseases. In this challenge, we objectively compare and evaluate state-of-the-art whole-cell and nucleus segmentation and tracking methods using both real (2D and 3D) time-lapse microscopy videos of cells and nuclei, along with computer-generated (2D and 3D) video sequences simulating whole cells and nuclei moving in realistic environments.

Cell Tracking dataset: PhC-C2DH-U373  
Source: http://celltrackingchallenge.net/2d-datasets/  
Reference: Dr. S. Kumar. Department of Bioengineering, University of California at Berkeley, Berkeley CA (USA)

The cell tracking data set consist of 34 2D images in TIFF format.  
The images are glioblastoma-astrocytoma U373 cells on a polyacrylamide substrate.

## <a name="COVID-19"></a>COVID-19 CT imaging data

We used the public dataset from Ma et al. which consists of 20 annotated COVID-19 chest CT volumes⁠. Currently, this dataset is the only publicly available 3D volume set with annotated COVID-19 infection segmentation⁠.

Each CT volume was first labeled by junior annotators, then refined by two radiologists with 5 years of experience and afterwards the annotations verified by senior radiologists with more than 10 years of experience⁠.

The CT images were labeled into four classes: Background, lung left, lung right and COVID-19 infection.

Reference: https://zenodo.org/record/3757476#.XqhRp_lS-5D

## <a name="LCTSC"></a>Lung CT Segmentation Challenge (LCTSC)

This data set was provided in association with a challenge competition and related conference session conducted at the AAPM 2017 Annual Meeting.
 
The dataset consists of 60 cases and includes computed tomography (CT) images and Radiation Therapy (RT) structure sets in the DICOM format. The structure files contain the following delineations:
- Esophagus
- Heart
- Left Lung
- Right Lung
- Spinal Cord

Source: https://wiki.cancerimagingarchive.net/display/Public/Lung+CT+Segmentation+Challenge+2017
