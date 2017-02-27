# TensorFlow + MNIST on Android

This is a demo app for Android with TensorFlow to detect handwritten digits.

## Introduction

This Android Demo is based on TensorFlow tutorial found here:
**MNIST For ML Beginners**
https://www.tensorflow.org/versions/r0.10/tutorials/mnist/beginners/index.html

**Deep MNIST for Experts**
https://www.tensorflow.org/versions/r0.10/tutorials/mnist/pros/index.html

## Usage

Just use the customized DrawView to doodle any digit from 0-9 on the screen, and using the trained data from MNIST, we will try to classify the doodle.

## Training the Model
Training Scripts for Neural Network Model are located at
https://github.com/AmruthPillai/TensorFlowAndroidMNIST/tree/master/trainer-script

To create a model by yourself, install TensorFlow and run these python scripts:

    $ python beginner.py

or

    $ python expert.py

depending on the complexity of the model you want to use.

Now, locate the exported .pb file and copy it to the assets directory of the Android App.

To export training model, I added some modification to original tutorial scripts.

Now, TensorFlow cannot export network graph and trained network weight variable at the same time,
so we need to create another graph to export and convert variable into constants.

After training is finished, converted trained variable to a numpy ndarray.

    _W = W.eval(sess)
    _b = b.eval(sess)

and then convert them into constant and re-create graph for exporting.

    W_2 = tf.constant(_W, name="constant_W")
    b_2 = tf.constant(_b, name="constant_b")

And then use tf.train.write_graph to export graph with trained weights.


## How to build JNI codes

Native .so files are already built in this project, but if you would like to
build it by yourself, please install and setup NDK.

First download, extract and place Android NDK.

http://developer.android.com/intl/ja/ndk/downloads/index.html

And then update your PATH environment variable. For example,

    export NDK_HOME="/Users/[your-username]/Development/android/android-ndk-r11b"
    export PATH=$PATH:$NDK_HOME

And build .so file in jni-build dir.

    $ cd jni-build
    $ make
    
and copy .so file into app/src/main/jniLibs/armeabi-v7a/ with

    $ make install

(Unlike original Android demo in TensorFlow, you don't need to install Bazel to build this demo.