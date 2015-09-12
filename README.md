Code for [Unsupervised Learning of Video Representations using LSTMs](http://arxiv.org/pdf/1502.04681.pdf)
taken from http://www.cs.toronto.edu/~nitish/unsupervised_video/

INSTALL
=======

```bash
export CUDA_ROOT=...
make
mkdir mnist
cd mnist
wget www.cs.toronto.edu/~nitish/data/mnist.h5
mkdir models
cd ..
```

RUN
===
Training
--------
```bash
python lstm_composite.py lstm_combo_1layer_mnist.pbtxt bouncing_mnist.pbtxt
```
eventually I got `nan` on the scores while running
```
Step 17000 Dec 213.17694 Fut 345.98576 Writing model to mnist/models/lstm_autoencoder_20150911163609.h5
Step 17100 Dec 213.55469 Fut 347.48344
Step 17200 Dec 213.18424 Fut 346.63582
Step 17300 Dec 213.91465 Fut 347.33368
Step 17400 Dec 212.99746 Fut 347.44646
Step 17500 Dec 212.81396 Fut 346.90646
Step 17600 Dec 212.92453 Fut 346.50480
Step 17700 Dec 212.61709 Fut 345.45132
Step 17800 Dec nan Fut nan
Step 17900 Dec nan Fut nan
```
The files created at Step 17000, just before the `nan`, are
[mnist/models/lstm_autoencoder_20150911163609.h5](https://s3.amazonaws.com/udivideo/lstm_autoencoder_20150911163609.h5) and
[mnist/models/lstm_autoencoder_20150911163609.pbtxt](https://s3.amazonaws.com/udivideo/lstm_autoencoder_20150911163609.pbtxt)


if you receive an exception such as 
```
cudamat.CUDAMatException: CUBLAS error.
```
then your GPU does not have enough memory and you should switch to a different board
(e.g. Mac internal GPU vs. [GTX980](http://udibr.github.io/using-external-gtx-980-with-macbook-pro.html))
or run a smaller model:
```bash
python lstm_composite.py lstm_combo_1layer_mnist_512.pbtxt bouncing_mnist.pbtxt
```

Continue Training
-----------------
continue a terminated train run by supplying the `pbtxt` file it created instead of initial `pbtxt` file.
For example:
```bash
python lstm_composite.py mnist/models/lstm_autoencoder_20150911163609.pbtxt bouncing_mnist.pbtxt
```

Running a trained model
-----------------------
Use the `pbtxt`, created in training, and add `runandshow` at the end.
You can optionally add the maximal number of examples you want to show.
For example:
```bash
python lstm_composite.py mnist/models/lstm_autoencoder_20150911163609.pbtxt bouncing_mnist.pbtxt runandshow 128
```
you will then see two frames animating images.
The first one will show an original sequence of images and the second will show you 
a a sequence made from decoded images, from the autoencoder, followed by predicted future images.

![original](images/figure_1.png "original sequence")

![decoded/predicted](images/figure_2.png "decoded and predicted sequence")

