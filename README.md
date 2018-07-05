# TFHubRetrain

Retraining one of Google's CNN image classification models to new categories using Transfer Learning.
This can be an much faster (in a few minutes) than training from scratch ([Inception V3](https://github.com/EN10/KerasInception) took Google, 2 weeks).

* Based on [Tensoflow Hub Retrain](https://github.com/tensorflow/hub/blob/master/docs/tutorials/image_retraining.md) (updated 21 June)    
* Originally based on [Tensorflow for Poets](https://github.com/EN10/TensorFlowForPoets)


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

519s on	Tesla K80 and python2.7

## Faster Retrain
    python retrain.py \
    --image_dir ~/flower_photos \
    --tfhub_module https://tfhub.dev/google/imagenet/mobilenet_v2_100_224/feature_vector/2

* [MobileNetV2](https://ai.googleblog.com/2018/04/mobilenetv2-next-generation-of-on.html)
* [Speedup Training](https://github.com/EN10/TensorFlowForPoets#speedup-training)
* [Compare Models](https://github.com/tensorflow/models/blob/master/research/slim/README.md)
 
## Classify
    curl -LO https://github.com/tensorflow/tensorflow/raw/master/tensorflow/examples/label_image/label_image.py
    
    python label_image.py \
    --graph=/tmp/output_graph.pb  \
    --image=image.jpg