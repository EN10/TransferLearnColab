# TFHubRetrain

Based on Tensoflow [Retrain](https://www.tensorflow.org/versions/master/tutorials/image_retraining)

## Install 
    pip install "tensorflow>=1.7.0"
    pip install tensorflow-hub

## Download Flowers
    cd ~
    curl -LO http://download.tensorflow.org/example_images/flower_photos.tgz
    tar xzf flower_photos.tgz

## Download Code
    mkdir ~/example_code
    cd ~/example_code
    curl -LO https://github.com/tensorflow/hub/raw/master/examples/image_retraining/retrain.py

## Retrain
    python retrain.py --image_dir ~/flower_photos