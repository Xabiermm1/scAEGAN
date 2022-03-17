scAEGAN- Unification of Single-Cell Genomics Data by Adversarial Learning of Latent Space Correspondences 
---------------------------------------------------------------------------------------------------------
This repository contains the online data and scAEGAN code to analyze and visualize multi-omics integration analysis, and it's downstream analysis outputs. Metrics are also available for quantifying outputs quality.

* [Summary](#Summary)
* [scAEGAN Workflow](#scAEGAN-Workflow)
* [Installation Requisites](#Installation-Requisites )
* [Datasets](#Datasets)
* [Usage](#Usage)
* [Running Example](#Running-Example)



 Summary
 -------
scAEGAN is a python based deep learning model that is designed for single-cell-omics and multi-omics integration. scAEGAN does this by using an Autoencoder which learns a low-dimensional embedding of each experiment independently, respecting each sample's uniqueness, protocol. Next, cycleGAN learns a non-linear mapping between these two Autoencoder representations, leveraging the observation that distributions in different latent spaces are similar.

scAEGAN Workflow
----------------
<img width="873" alt="scAEGAN" src="https://user-images.githubusercontent.com/70262340/150944062-c9c72e62-ee8b-41f2-8d97-8d7e8711529a.PNG">




Installation Requisites 
-----------------------

scAEGAN is simply installed by cloning the repository.

The required libraries are included in environment.yml file. In order to install these libraries, follow the following steps:

* Creating the conda environment with the following command. This will create and install the libraries included in the environment.yml file for training the scAEGAN.
```
conda env create --prefix ./env --file environment.yml --force
 ```

* The second step is to activate the conda envirnoment. 
```
conda activate ./env      
```


* To evaluate with the given scripts in the Analysis folder on the scAEGAN generated data following libraries are required: 
```
Seurat 4.1.0
clusteval 0.2.1
scclusteval 0.0.9
pheatmap 1.0.12
ggplot2 2.3.5
cowplot 1.1.1
pdfCluster 1.0.3
cluster 2.1.2
Rtsne 0.15.0
```
Datasets
---------

Simulated data : Two datasets containing 600 cells from 5 populations and with 3000 genes each were simulated using SymSim [paper](https://www.nature.com/articles/s41467-019-10500-w#code-availability)

Real data: The pre-processed mouse hematopoietic stem cell data-set of young and old individuals downloaded from   https://github.com/quon-titative-biology/scalign

Usage
------
There are two steps for the basic usage after activating the conda environment.
*  Training the autoencoder with the given parameters to get the latent representation by running. 
```bash
python AE.py --input_file <Specifies the input file (cell by gene matrix in csv format)> \
             --output_file <Specifies the low dimensional representation of the input from the autoencoder> \
             --batch_size <Specifies the batch size to train the autoencoder, default=16>  \
             --epochs <Specifies  the number of epochs for which autoencoder is trained, default=200> \
             --dropout <Specifies the dropout rate used to train the autoencoder, default=0.2> \
             --learning_rate <Specifies the larning rate.Default=0.0001>
```


*  Training the cyclegan with the given parameters on latent representations obtained from the Autoencoder by running.

```bash
python cGANtrain.py --data_path <Specifies the folder path to the training and testing data> \
                    --train_file <Specifies the training files for training the cGAN for both domains (A and B) that are to be integrated. 
                    For instance --train_file train_A.csv train_B.csv \
                    --test_file <Specifies the testing files. For instance --test_file test_A.csv test_B.csv> \
                    --save_path <Specifies the folder path where the output from the cGAN in the csv format will be saved> \
                    --input_shape <Specifies the shape of the input, default=50> \
                    --batch_size <Specifies the batch size, default=4> \
                    --epochs <Specifies the number of epochs for training cGAN, default=200>
```

 Running Example
 ---------------
*   In this tutorial we show how to run scAEGAN on the Simulated Data. We have 
prepared the required input dataset which you can find in the Simulated_Data folder
*   We created a command-line interface for scAEGAN that allows it to be run in a high-performance computing environment. Because scAEGAN is built with tensorflow/keras, we recommend running it on GPUs to significantly reduce run-time.
 * For model training and analysis, a [vignette](https://github.com/sumeer1/scAEGAN/blob/main/Example/scAEGAN_Analysis.ipynb) is given as an example how to run the scAEGAN and carry out the analysis with the scripts in Analysis folder. 
 
 


