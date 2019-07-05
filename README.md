# Transfer Learn Colab

Retraining one of Google's CNN image classification models to new categories using Transfer Learning.
This can be an much faster (in a few minutes) than training from scratch ([Inception V3](https://github.com/EN10/KerasInception) took Google, 2 weeks).

* Based on [Tensoflow Hub Retrain](https://github.com/tensorflow/hub/blob/master/docs/tutorials/image_retraining.md) (updated 10 Jul 2018)    
* Originally based on [Tensorflow for Poets](https://github.com/EN10/TensorFlowForPoets)

Colab - Runtime - Change runtime type - Hardware accelerator - GPU - SAVE

## Download Flowers
    !curl -LO http://download.tensorflow.org/example_images/flower_photos.tgz
    !tar xzf flower_photos.tgz

## Display Flower
    from IPython.display import Image
    Image(filename='flower_photos/roses/102501987_3cdb8e5394_n.jpg') 
Tab autocomplete can be used for image names

## Download Retrain
    !curl -LO https://github.com/tensorflow/hub/raw/master/examples/image_retraining/retrain.py

## Speedup Training 
reduce the number of images by ~70% : 3681 -> 1668

    !ls flower_photos/* | wc -l
    !rm flower_photos/*/[3-9]*
    !rm flower_photos/daisy/ flower_photos/dandelion/ flower_photos/tulips/ -r
    !ls flower_photos/* | wc -l
also only use 2 flowers e.g. roses and sunflowers : 1668 -> 591

## Retrain
    !python retrain.py --image_dir ./flower_photos --how_many_training_steps 500
    
692s : Colab - Python 3 - GPU - 3681 images - 4000 Steps - Test 90.4%    
135s : Colab - Python 3 - GPU - 3681 images - 500 Steps - Test 90.4%  
500s : Colab - Python 3 - GPU - 591 images - 4000 Steps - Test 96.9%    
131s : Colab - Python 3 - GPU - 591 images - 500 Steps - Test 96.9%

11m22 : PaizaCloud

* [Pre-trained Models ](https://github.com/tensorflow/models/blob/master/research/slim/README.md#pre-trained-models)
* [Comparision](https://1.bp.blogspot.com/-E1qM-CKq-BA/WfuGc22fPBI/AAAAAAAACIg/frpwbO5Jh-oL0cSObyJa29fXkBsuVl7CACLcBGAs/s1600/image3.jpg)

## Download Label Image
    !curl -LO https://github.com/tensorflow/tensorflow/raw/master/tensorflow/examples/label_image/label_image.py

## Download Test Image
    !wget https://5.imimg.com/data5/AA/KK/MY-6677193/red-rose-500x500.jpg

## Use the Retrained Model
    !python label_image.py \
    --graph=/tmp/output_graph.pb --labels=/tmp/output_labels.txt \
    --input_layer=Placeholder \
    --output_layer=final_result \
    --image=red-rose-500x500.jpg \
    | grep 'roses\|sunflowers'

## [Training on Your Own Categories](https://github.com/EN10/TensorFlowForPoets#training-on-your-own-categories)

images to colab: download images, rename folder, zip, upload, unzip, mkdir, mv   

#### Images
[Batch Image downloader](https://chrome.google.com/webstore/detail/fatkun-batch-download-ima/nnjjahlikiabnchcpehcpkdeckfgnohf?hl=en)    
Loads images on screen, in Google Images Scroll for more images.

Zip: right click - Send to - Compressed (zipped) folder

#### Colab Upload

    from google.colab import files

    uploaded = files.upload()

    for fn in uploaded.keys():
      print('User uploaded file "{name}" with length {length} bytes'.format(
          name=fn, length=len(uploaded[fn])))

#### Unzip

    !unzip foldername.zip

#### Folders

    mkdir images
    mv foldername images

moves `foldername` into `images` folder

## tmp

bottlenecks, graph & model in `/tmp`

### Label Image with Inception & Imagenet

    !curl -LO https://storage.googleapis.com/download.tensorflow.org/models/inception_v3_2016_08_28_frozen.pb.tar.gz
    !tar -xvzf inception_v3_2016_08_28_frozen.pb.tar.gz
    !curl -LO https://raw.githubusercontent.com/EN10/SimpleInception/master/5918348-image.jpg
        
    !python label_image.py \
    --graph=inception_v3_2016_08_28_frozen.pb --labels=imagenet_slim_labels.txt \
    --image=5918348-image.jpg