# Generative Adversarial Network Based Abnormal Behavior Detection in Massive Crowd Videos: A Hajj Case Study



In this repository, we introduce the [Hajj v1 dataset](https://github.com/KAU-Smart-Crowd/Hajj_abnormal_behavior_detection/releases/tag/1). In addition, the official implementation of the proposed method at [Generative Adversarial Network Based Abnormal Behavior Detection in Massive Crowd Videos: A Hajj Case Study](https://link.springer.com/article/10.1007/s12652-021-03323-5). 



The proposed method uses [FlowNet](https://github.com/sampepose/flownet2-tf) to extract the optical flow as a pre-processing step. Then, it trains a GAN to detect the abnormal behaviors. 



<!-- This repository is the official implementation of [Generative Adversarial Network Based Abnormal Behavior Detection in Massive Crowd Videos: A Hajj Case Study](https://link.springer.com/article/10.1007/s12652-021-03323-5). In this paper we introduce  -->



![scalars_tensorboard](https://github.com/KAU-Smart-Crowd/Hajj_abnormal_behavior_detection/blob/main/Assets/architecture.JPG?raw=true)





The implementation has been done using TensorFlow. Please follow the instructions to run the code.

<!-- It is implemented in tensorflow. Please follow the instructions to run the code. -->



## 1. Installation (Anaconda with python3.6 installation is recommended)

* Install 3rd-package dependencies of python (listed in requirements.txt)

```

numpy==1.14.1

scipy==1.0.0

matplotlib==2.1.2

tensorflow-gpu==1.4

tensorflow==1.4

Pillow==5.0.0

pypng==0.0.18

scikit_learn==0.19.1

opencv-python==3.2.0.6

```

* Or run the following command to in one step:

```shell

pip install -r Codes/requirements.txt

```

* Other libraries



```code

CUDA 8.0

Cudnn 6.0

Ubuntu 14.04 or 16.04, Centos 7 and other distributions.

```



## 2. Testing on saved models

* Download the trained models (There are the pretrained FlowNet and the trained models of the papers, such as ped1 and ped2).

Please manually download pretrained models from [pretrains.tar.gz, ped1, ped2, flownet](https://onedrive.live.com/?authkey=%21AMqh2fTSemfrokE&id=3705E349C336415F%215109&cid=3705E349C336415F)

and tar -xvf pretrains.tar.gz, and move pretrains into **Codes/checkpoints** folder. **[ShanghaiTech pre-trained models](https://onedrive.live.com/?authkey=%21AMlRwbaoQ0sAgqU&id=303FB25922AAD438%217383&cid=303FB25922AAD438)**



* Running the sript (as ped2 datasets for example). cd into **Codes** folder at first.

```shell

python inference.py  --dataset  ped2    \

                    --test_folder  ../Data/ped2/testing/frames      \

                    --gpu  1    \

                    --snapshot_dir    checkpoints/pretrains/ped2

```



<!-- ```shell

python inference.py  --dataset  avenue    \

                    --test_folder  ../Data/avenue/testing/frames      \

                    --gpu  1    \

                    --snapshot_dir    checkpoints/pretrains/avenue

``` -->



## 3. get optical flow images



```python

python flow.py

```

![optical_flow](https://github.com/KAU-Smart-Crowd/Hajj_abnormal_behavior_detection/blob/main/Assets/opticalflow.jpg?raw=true)



## 4. Training from scratch

* Download the pretrained FlowNet at first and see above mentioned step 2

* Set hyper-parameters

The default hyper-parameters, such as <img src="https://render.githubusercontent.com/render/math?math=\lambda_{init}" >, <img src="https://render.githubusercontent.com/render/math?math=\lambda_{gd}">, <img src="https://render.githubusercontent.com/render/math?math=\lambda_{op}">, <img src="https://render.githubusercontent.com/render/math?math=\lambda_{adv}"> and the learning rate of G, as well as D, are all initialized in **training_hyper_params/hyper_params.ini**. 

* Running script (as ped2 for instances) and cd into **Codes** folder at first.

```shell

python train.py  --dataset  ped2    \

                 --train_folder  ../Data/ped2/training/frames     \

                 --test_folder  ../Data/ped2/testing/frames       \

                 --gpu  0       \

                 --iters    80000

```

* Model selection while training

In order to do model selection, a popular way is to testing the saved models after a number of iterations or epochs (Since there are no validation set provided on above all datasets, and in order to compare the performance with other methods, we just choose the best model on testing set). Here, we can use another GPU to listen the **snapshot_dir** folder. When a new model.cpkt.xxx has arrived, then load the model and test. Finnaly, we choose the best model. Following is the script.

```shell

python inference.py  --dataset  ped2    \

                     --test_folder  ../Data/ped2/testing/frames       \

                     --gpu  1

```

Run **python train.py -h** to know more about the flag options or see the detials in **constant.py**.

```shell

Options to run the network.



optional arguments:

  -h, --help            show this help message and exit

  -g GPU, --gpu GPU    the device id of gpu.

  -i ITERS, --iters ITERS

                        set the number of iterations, default is 1

  -b BATCH, --batch BATCH

                        set the batch size, default is 4.

  --num_his NUM_HIS    set the time steps, default is 4.

  -d DATASET, --dataset DATASET

                        the name of dataset.

  --train_folder TRAIN_FOLDER

                        set the training folder path.

  --test_folder TEST_FOLDER

                        set the testing folder path.

  --config CONFIG      the path of training_hyper_params, default is

                        training_hyper_params/hyper_params.ini

  --snapshot_dir SNAPSHOT_DIR

                        if it is folder, then it is the directory to save

                        models, if it is a specific model.ckpt-xxx, then the

                        system will load it for testing.

  --summary_dir SUMMARY_DIR

                        the directory to save summaries.

  --psnr_dir PSNR_DIR  the directory to save psnrs results in testing.

  --evaluate EVALUATE  the evaluation metric, default is compute_auc

```

<!-- * (Option) Tensorboard visualization

```shell

tensorboard    --logdir=./summary    --port=10086

```

Open the browser and type **https://ip:10086**. Following is the screen shot of avenue on tensorboard.

![scalars_tensorboard](assets/scalars.JPG)



![images_tensorboard](assets/images.JPG)

Since the models are trained in BGR image color channels, the visualized images in tensorboard look different from RGB channels.

In the demo, we change the output images from BGR to RGB. -->



## 5.Experiment Result

<!-- ![result_1](https://github.com/KAU-Smart-Crowd/Hajj_abnormal_behavior_detection/blob/main/Assets/result_1.jpg?raw=true)

![result_2](https://github.com/KAU-Smart-Crowd/Hajj_abnormal_behavior_detection/blob/main/Assets/result_2.jpg?raw=true)

![result_3](https://github.com/KAU-Smart-Crowd/Hajj_abnormal_behavior_detection/blob/main/Assets/result_3.jpg?raw=true) -->



The results of the Abnormal Behaviors Hajj dataset: 



<table width="100%">

 <tr>

  <th>Performance Matric</th><th>Result</th>

 </tr>

 <tr>

  <td>AUC</td><td>79.63%</td>

 </tr>

 <tr>

  <td>Accuracy</td><td>65.10%</td>

 </tr>

  <tr>

  <td>Precision</td><td>61.48%</td>

 </tr>

   <tr>

  <td>Recall</td><td>80.30%</td>

 </tr>

</table>



<!-- ## Notes

The flow loss (temporal loss) module is based on [a TensorFlow implementation of FlowNet2](https://github.com/sampepose/flownet2-tf). Thanks for their nice work. -->



# Citation

Please refer to this reference when using the scripts or dataset:

```code

@article{alafif2021generative,

  title={Generative adversarial network based abnormal behavior detection in massive crowd videos: a Hajj case study},

  author={Alafif, Tarik and Alzahrani, Bander and Cao, Yong and Alotaibi, Reem and Barnawi, Ahmed and Chen, Min},

  journal={Journal of Ambient Intelligence and Humanized Computing},

  pages={1--12},

  year={2021},

  publisher={Springer}

}

```

