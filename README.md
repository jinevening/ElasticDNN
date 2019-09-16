ElasticDNN
-----
This repo includes the implementation of **IONN (Incremental Offloading of Neural Network)**, which was proposed by the paper titled "IONN: Incremental Offloading of Neural Network Computations From Mobile Devices to Edge Servers" published in SoCC (ACM Symposium on Cloud Computing) 2018.

The repo merely consists of two submodules: *IONN-client* and *IONN-server*. Each submodule runs on the mobile device (ARM) and the server (x86), respectively.

Installation
-----
Client-side
1. Clone our project (IONN-client)

```bash
$ git clone https://github.com/jinevening/IONN-client.git
```

2. Build our project according to the [caffe installation instructions](https://caffe.berkeleyvision.org/installation.html).

Server-side
1. Clone our project (IONN-server)

```bash
$ git clone https://github.com/jinevening/IONN-server.git
```

2. Build our project according to the [caffe installation instructions](https://caffe.berkeleyvision.org/installation.html).

Execution
-----
First, we have to run IONN-server on the server by running the following command at the repository directory

```
./build/examples/partitioning_server/classification.bin
```

Then, logs like following should be printed on the shell.

```
GPU mode
Upload Server Started on Port 7675
Execution Server Started on Port 7676
```

While IONN-server is running on the server, IONN-client can offload DNN execution to the server by running the following command at the client device

```
./build/examples/partitioning/classification.bin \
  *path to the prototxt file* \
  *path to the caffemodel file* \
  *path to the mean file* \
  *path to the label file* \
  *profile of the execution time in the client* \
  *prediction function for the execution time in the server* \
  *network speed in Mbps* \
  *path to the image file* \
  *value of K in the IONN paper* \
  incremental \
  *optimization target*
```

Example
```
./build/examples/partitioning/classification.bin \
  models/bvlc_alexnet/deploy.prototxt \
  models/bvlc_alexnet/bvlc_alexnet.caffemodel \
  data/ilsvrc12/imagenet_mean.binaryproto \
  data/ilsvrc12/synset_words.txt \
  models/bvlc_alexnet/prediction_model.txt \
  server_prediction_model.txt \
  80 \
  cat.jpg \
  0.8 \
  incremental \
  time
```

Acknowledgements
-----
We implemented IONN based on *caffe*, an open source project to train and test DNN. We extended caffe to support the collaborative execution (inference) between a client and a server. To transmit the activation data between the client and the server, we used boost.asio library.

* [caffe](https://github.com/BVLC/caffe) by [Berkeley Vision and Learning Center](https://github.com/BVLC)
* [boost.asio](https://github.com/boostorg/asio) by [Boost.org](https://github.com/boostorg)

License and Citation
-----
IONN is released under the MIT license, so anyone can use this source code in any purpose.
Please cite following papers in your publications if IONN helps your research:

```
@inproceedings{jeong2018ionn,
  title={Ionn: Incremental offloading of neural network computations from mobile devices to edge servers},
  author={Jeong, Hyuk-Jin and Lee, Hyeon-Jae and Shin, Chang Hyun and Moon, Soo-Mook},
  booktitle={Proceedings of the ACM Symposium on Cloud Computing},
  pages={401--411},
  year={2018},
  organization={ACM}
}

@inproceedings{shin2019enhanced,
  title={Enhanced Partitioning of DNN Layers for Uploading from Mobile Devices to Edge Servers},
  author={Shin, Kwang Yong and Jeong, Hyuk-Jin and Moon, Soo-Mook},
  booktitle={The 3rd International Workshop on Deep Learning for Mobile Systems and Applications},
  pages={35--40},
  year={2019},
  organization={ACM}
}
```

Project Team Members
-----
Hyuk Jin Jeong (jinevening@snu.ac.kr) and Kwang Yong Shin (kwangshin@altair.snu.ac.kr)
