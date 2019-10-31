# Quantization-Flownet2-PyTorch

This is a quantized network for flownet which is implemented by https://github.com/NVIDIA/flownet2-pytorch.git

The algorithm I used for this quantization is similar to https://github.com/jiecaoyu/XNOR-Net-PyTorch

# flownet2-pytorch 
This is based on README.md from https://github.com/NVIDIA/flownet2-pytorch.git

Pytorch implementation of [FlowNet 2.0: Evolution of Optical Flow Estimation with Deep Networks](https://arxiv.org/abs/1612.01925). 

Multiple GPU training is supported, and the code provides examples for training or inference on [MPI-Sintel](http://sintel.is.tue.mpg.de/) clean and final datasets. The same commands can be used for training or inference with other datasets. See below for more detail.

Inference using fp16 (half-precision) is also supported.

For more help, type <br />
    
    python main.py --help

## Network architectures
Below are the different flownet neural network architectures that are provided. <br />
A batchnorm version for each network is available.

 - **FlowNet2S**
 - **FlowNet2C**
 - **FlowNet2CS**
 - **FlowNet2CSS**
 - **FlowNet2SD**
 - **FlowNet2**

## Custom layers

`FlowNet2` or `FlowNet2C*` achitectures rely on custom layers `Resample2d` or `Correlation`. <br />
A pytorch implementation of these layers with cuda kernels are available at [./networks](./networks). <br />
Note : Currently, half precision kernels are not available for these layers.

## Data Loaders

Dataloaders for FlyingChairs, FlyingThings, ChairsSDHom and ImagesFromFolder are available in [datasets.py](./datasets.py). <br />

## Loss Functions

L1 and L2 losses with multi-scale support are available in [losses.py](./losses.py). <br />

## Installation 

    # get flownet2-pytorch source
    git clone https://github.com/NVIDIA/flownet2-pytorch.git
    cd flownet2-pytorch

    # install custom layers
    bash install.sh
    
### Python requirements 
Currently, the code supports python 3
* numpy 
* PyTorch ( == 0.4.1, for <= 0.4.0 see branch [python36-PyTorch0.4](https://github.com/NVIDIA/flownet2-pytorch/tree/python36-PyTorch0.4))
* scipy 
* scikit-image
* tensorboardX
* colorama, tqdm, setproctitle 

## Converted Caffe Pre-trained Models
We've included caffe pre-trained models. Should you use these pre-trained weights, please adhere to the [license agreements](https://drive.google.com/file/d/1TVv0BnNFh3rpHZvD-easMb9jYrPE2Eqd/view?usp=sharing). 

* [FlowNet2](https://drive.google.com/file/d/1hF8vS6YeHkx3j2pfCeQqqZGwA_PJq_Da/view?usp=sharing)[620MB]
* [FlowNet2-C](https://drive.google.com/file/d/1BFT6b7KgKJC8rA59RmOVAXRM_S7aSfKE/view?usp=sharing)[149MB]
* [FlowNet2-CS](https://drive.google.com/file/d/1iBJ1_o7PloaINpa8m7u_7TsLCX0Dt_jS/view?usp=sharing)[297MB]
* [FlowNet2-CSS](https://drive.google.com/file/d/157zuzVf4YMN6ABAQgZc8rRmR5cgWzSu8/view?usp=sharing)[445MB]
* [FlowNet2-CSS-ft-sd](https://drive.google.com/file/d/1R5xafCIzJCXc8ia4TGfC65irmTNiMg6u/view?usp=sharing)[445MB]
* [FlowNet2-S](https://drive.google.com/file/d/1V61dZjFomwlynwlYklJHC-TLfdFom3Lg/view?usp=sharing)[148MB]
* [FlowNet2-SD](https://drive.google.com/file/d/1QW03eyYG_vD-dT-Mx4wopYvtPu_msTKn/view?usp=sharing)[173MB]
    
## Training and validation
    # Example on FlyingChairs for Training and MPISintel Clean  
    python main.py --total_epochs 100 --batch_size 8 --model FlowNet2S　--loss=L1Loss --optimizer=Adam --optimizer_lr=1e-4 --training_dataset　FlyingChairs --training_dataset_root /data/FlyingChairs/data　--validation_dataset MpiSintelClean --validation_dataset_root　/data/MPI-Sintel/training -ng 1
    
## Inference
    # Example on MPISintel Clean, with L1Loss on FlowNet2S model
    python main.py --inference --model FlowNet2S --save_flow --inference_datasetMpiSintelClean --inference_dataset_root /data/MPI-Sintel/training --resume ./work/FlowNet2S_model_best.pth.tar

## Visualization of the output flow

    ./flow-code/color_flow work/inference/run.epoch-0-flow-field/000000.floflownet2s.png 
 color_flow is the comand for visualizing the flow data.
 
 You can see the flow using eog comand.
 
    eog flownet2s.png
 You can find the correct flow from /data/MPI-Sintel/training/flow_viz.

## Results on MPI-Sintel
[![Predicted flows on MPI-Sintel](./image.png)](https://www.youtube.com/watch?v=HtBmabY8aeU "Predicted flows on MPI-Sintel")

## Reference 
If you find this implementation useful in your work, please acknowledge it appropriately and cite the paper:
````
@InProceedings{IMKDB17,
  author       = "E. Ilg and N. Mayer and T. Saikia and M. Keuper and A. Dosovitskiy and T. Brox",
  title        = "FlowNet 2.0: Evolution of Optical Flow Estimation with Deep Networks",
  booktitle    = "IEEE Conference on Computer Vision and Pattern Recognition (CVPR)",
  month        = "Jul",
  year         = "2017",
  url          = "http://lmb.informatik.uni-freiburg.de//Publications/2017/IMKDB17"
}
````
```
@misc{flownet2-pytorch,
  author = {Fitsum Reda and Robert Pottorff and Jon Barker and Bryan Catanzaro},
  title = {flownet2-pytorch: Pytorch implementation of FlowNet 2.0: Evolution of Optical Flow Estimation with Deep Networks},
  year = {2017},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/NVIDIA/flownet2-pytorch}}
}
```
## Related Optical Flow Work from Nvidia 
Code (in Caffe and Pytorch): [PWC-Net](https://github.com/NVlabs/PWC-Net) <br />
Paper : [PWC-Net: CNNs for Optical Flow Using Pyramid, Warping, and Cost Volume](https://arxiv.org/abs/1709.02371). 

## Acknowledgments
Parts of this code were derived, as noted in the code, from [ClementPinard/FlowNetPytorch](https://github.com/ClementPinard/FlowNetPytorch).
Majority of this code are from https://github.com/NVIDIA/flownet2-pytorch.git and https://github.com/jiecaoyu/XNOR-Net-PyTorch.
