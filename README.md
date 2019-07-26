# U-Net: Convolutional Network for Biomedical Image Segmentation
Implemantation of [U-Net](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/) architecture using [Keras](https://keras.io/) framework

## Brief info

U-Net is a convolutional network architecture for fast and precise segmentation of images. 
Input is a grey scale 512x512 image in jpeg format, output - a 512x512 mask in png format. 
Below is the implemented model's architecture
<img src="https://pp.userapi.com/c854420/v854420774/9732f/7JeFZnZa-Xo.jpg" width=640 height=450>

## Project scheme
    .
    ├── model
    │   ├── unet.py             # defines U-Net class
    │   └── utils.py            # layers for U-Net class
    ├── tools                     
    │   ├── data.py             # generates data 
    │   └── image.py            # image-related functions  
    ├── images                     
    │   ├── img                 # image examples for readme
    │   └── mask                # generated by nn masks
    ├── main.py                 # main script to run
    └── ...

## Requirements

* Python 3.7
* Anaconda 3

## Installation

After installing the Python 3.7 version of [Anaconda](https://www.anaconda.com/distribution/), clone the project
```sh
git clone https://github.com/sevakon/unet-keras.git
```
Create and activate a tensorflow virtual environment (no GPU support):
```sh
conda create -n tensorflow_env tensorflow
conda avtivate tensorflow_env
```

Alternatively, create a tensorflow virtual environment with GPU support:
```sh
conda create --name tf_gpu tensorflow-gpu 
conda avtivate tf_gpu
```

Install other packages to the virtual environment:
```sh
conda install -c conda-forge keras   # installs Keras
```
```sh
pip install matplotlib               # installs matpoltlib
```
```sh
pip install scikit-image             # installs skimage
```

## Instructions
All the data preparation happens is data.py file. 
Data augmentation can be added to ImageDataGenerator parameters

```py
    image_datagen = ImageDataGenerator(rescale=1. / 255)      # add data augmentation here
    mask_datagen = ImageDataGenerator(rescale=1. / 255)       # as function parameter
```

Note that your data should be structured like this:

    .
    ├── train_path
    │   ├── img                 # training images
    │   │   ├── 1.jpg                       
    │   │   ├── 2.jpg                 
    │   │   └── ...    
    │   └── mask                # training masks
    │       ├── 1.png    
    │       ├── 2.png     
    │       └── ...    
    ├── test_path               # folder with images fot test
    │       ├── 1.jpg      
    │       ├── 2.jpg
    │       └── ...    
    ├── save_path               # where results of tests will be saved
    └── ...

  
Define your config in main.py file
```py
img_height =          # set your image resolution (512x512 or 256X256 recommended)
img_width =           # every image from your dataset will be resacled
img_size = (img_height, img_width)
train_path = ''       # full path to the folder with training data
test_path = ''        # full path to the folder with testing data
save_path = ''        # full path to the folder to save results on testing data
model_name = 'unet_model.hdf5'     # how to name your model save
model_weights_name = 'unet_weight_model.hdf5'
```

## Model Training
To start training your model, run main.py script. 
All the samples in the dataset will be first made square with black paddings added at random.
Then the samples are pushed into training/testing generators, the pretrained weights (if defined) are loaded into the model, and the fitting process begins. At the end of the training, testing samples are predicted to see how well your model behaves on real data. If further training is required, run main.py script again, commenting prepare_dataset function since the dataset was already prepared during the first training.


## Model Predict
To predict results with the already trained model, run predict.py script:

```py
python predict.py n
```
where n is the number of samples that await prediction. Please note that testing samples must be structured like so:

    .
    ├── data
    │   ├── test               # testing images
    │   │   ├── 1.jpg          # must have names of             
    │   │   ├── 2.jpg          # ascending numbers from 1-to-n     
    │   │   └── ...    
    │   └── results            # predicted masks 
    ├── predict.py             # script
    ├── weight.hdf5            # file with the weights     
    └── ...


## Results
After 10 epochs, accuracy of 98% was reached on a training set of 528 bone images. 
After 95 epochs, reached accuracy was 99.7%.
Please check [images](https://github.com/sevakon/unet-keras/tree/master/images) folder to see all the sample-prediction pairs, below are only a few 

Sample | Prediction
:--------------:|:--------------:
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/11.jpg" width="203" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/11_predict.png"  width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/1.jpg" width="256" height="203">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/1_predict.png"  width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/13.jpg" width="203" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/13_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/5.jpg" width="203" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/5_predict.png"  width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/16.jpg" width="175" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/16_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/17.jpg" width="164" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/17_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/2.jpg" width="256" height="195">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/2_predict.png"  width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/18.jpg" width="220" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/18_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/19.jpg" width="203" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/19_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/20.jpg" width="184" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/20_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/21.jpg" width="204" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/21_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/3.jpg" width="203" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/3_predict.png"  width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/22.jpg" width="164" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/22_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/24.jpg" width="203" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/24_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/25.jpg" width="204" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/25_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/26.jpg" width="167" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/26_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/29.jpg" width="124" height="256">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/29_predict.png" width="256" height="256">
<img src="https://github.com/sevakon/unet-keras/blob/master/images/img/7.jpg" width="256" height="212">|<img src="https://github.com/sevakon/unet-keras/blob/master/images/mask/7_predict.png" width="256" height="256">

## About Keras

Keras is a minimalist, highly modular neural networks library, written in Python and capable of running on top of either TensorFlow or Theano. It was developed with a focus on enabling fast experimentation. Being able to go from idea to result with the least possible delay is key to doing good research.

Use Keras if you need a deep learning library that:

allows for easy and fast prototyping (through total modularity, minimalism, and extensibility).
supports both convolutional networks and recurrent networks, as well as combinations of the two.
supports arbitrary connectivity schemes (including multi-input and multi-output training).
runs seamlessly on CPU and GPU.
Read the documentation [Keras.io](http://keras.io/)

Keras is compatible with: Python 2.7-3.5.
