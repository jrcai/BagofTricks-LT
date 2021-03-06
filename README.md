## Bag of tricks for long-tailed visual recognition with deep convolutional neural networks

This repository is the official PyTorch implementation of AAAI-21 paper [Bag of Tricks for Long-Tailed Visual Recognition with Deep Convolutional Neural Networks](http://www.lamda.nju.edu.cn/zhangys/papers/AAAI_tricks.pdf), which provides practical and effective tricks used in long-tailed image classification. 



#### Trick gallery:  ***[trick_gallery.md](https://github.com/zhangyongshun/BagofTricks-LT/blob/main/documents/trick_gallery.md)***

#### Trick combinations: *[trick_combination.md](https://github.com/zhangyongshun/BagofTricks-LT/blob/main/documents/trick_combination.md)*




* The tricks will be **constantly updated**. If you have or need any long-tail related trick newly proposed, 
  please to [open an issue](https://github.com/zhangyongshun/BagofTricks-LT/issues) or [pull requests](https://github.com/zhangyongshun/BagofTricks-LT/pulls). Make sure to attach the results in corresponding md files if you pull a request with a new trick.
* Codes are based on [Megvii-Nanjing/BBN](https://github.com/Megvii-Nanjing/BBN). Welcome to [find a job or an intern position](https://github.com/zhangyongshun/BagofTricks-LT/blob/ace03362ff7041e6ef4e3bbe8728acfe76fac96e/documents/MRN_jobs.jpg) in Megvii Research Nanjing.
* For any problem, such as bugs, feel free to [open an issue](https://github.com/zhangyongshun/BagofTricks-LT/issues).

<!--<><summary> <b>TODO LIST</b> </summary></details>-->

## Development log

- [x] `2020-12-26` - Reorignize all the codes, according to [Megvii-Nanjing/BBN.](https://github.com/Megvii-Nanjing/BBN)
- [x] `2020-12-30` - Add codes of torch.nn.parallel.DistributedDataParallel. Support apex in both torch.nn.DataParallel and torch.nn.parallel.DistributedDataParallel.
- [x] `2021-01-02` - Add [LDAMLoss, NeurIPS 2019](https://arxiv.org/abs/1906.07413), and a regularization method: [label smooth cross-entropy, CVPR 2016](https://arxiv.org/abs/1512.00567).
- [x] `2021-01-05` - Add [SEQL (softmax equalization loss), CVPR 2020](https://arxiv.org/abs/2003.05176).
- [x] `2021-01-10` - Add [CDT (class-dependent temparature), arXiv 2020](https://arxiv.org/abs/2001.01385), [BSCE (balanced-softmax cross-entropy), NeurIPS 2020](https://papers.nips.cc/paper/2020/file/2ba61cc3a8f44143e1f2f13b2b729ab3-Paper.pdf), and support a smooth version of cost-sensitive cross-entropy (smooth CS_CE), which add a hyper-parameter $ \gamma$ to vanilla CS_CE. In smooth CS_CE, the loss weight of class i is defined as: $(\frac{N_{min}}{N_i})^\gamma$, where $\gamma \in [0, 1]$, $N_i$ is the number of images in class i. We can set $\gamma = 0.5$ to get a square-root version of CS_CE.
- [x] `2021-01-11` - Add a mixup related method: [Remix, ECCV 2020 workshop](https://arxiv.org/abs/2007.03943).
- [ ] Test and add the results of two-stage training in trick_gallery.md
- [ ] Add the results of trick combinations.
- [ ] Add the results of best bag of tricks on all long-tailed datasets.
- [ ] Add more backbones in each long-tailed benchmark to exlpore the influence of network capacity.
- [ ] Add trick family: `post-processing` and corresponding experiments, such as [$\tau$-normalization, ICLR 2020](https://openreview.net/forum?id=r1gRTCVFvB).
- [ ] Update the version of PyTorch to >=1.6, and use torch.cuda.amp instead of apex provided by Nividia.

## Trick gallery and combinations

#### Brief  inroduction

We divided the long-tail realted tricks into four families: re-weighting, re-sampling, mixup training, and two-stage training. For more details of the above four trick families, see the [original paper](https://cs.nju.edu.cn/wujx/paper/AAAI2021_Tricks.pdf).

<!--For re-weighting, re-sampling, and mixup training, we implement these methods in  [loss](https://github.com/zhangyongshun/BagofTricks-LT/blob/main/lib/loss/loss.py), [dataset](https://github.com/zhangyongshun/BagofTricks-LT/tree/main/lib/dataset), and [combiner](https://github.com/zhangyongshun/BagofTricks-LT/blob/main/lib/core/combiner.py), respectively. For two-stage training, which contains DRW and DRS, we implement them in  [loss](https://github.com/zhangyongshun/BagofTricks-LT/blob/main/lib/loss/loss.py) and [dataset](https://github.com/zhangyongshun/BagofTricks-LT/tree/main/lib/dataset).-->

#### Detailed information :

- Trick gallery:

  ##### Tricks, corresponding results, experimental settings, and running commands are listed in ***[trick_gallery.md](https://github.com/zhangyongshun/BagofTricks-LT/blob/main/documents/trick_gallery.md)***.

- Trick combinations:

  ##### Combinations of different tricks, corresponding results, experimental settings, and running commands are listed in *[trick_combination.md](https://github.com/zhangyongshun/BagofTricks-LT/blob/main/documents/trick_combination.md)*.
  
- These tricks and trick combinations, which provide the corresponding results in this repo, have been reorgnized and tested. We are trying our best to deal with the rest, which will be constantly updated.


## Main requirements
```bash
torch >= 1.4.0
torchvision >= 0.5.0
tensorboardX >= 2.1
tensorflow >= 1.14.0 #convert long-tailed cifar datasets from tfrecords to jpgs
Python 3
apex
```
- We provide the detailed requirements in [environment.yaml](https://github.com/zhangyongshun/BagofTricks-LT/blob/ace03362ff7041e6ef4e3bbe8728acfe76fac96e/documents/environment.yaml). You can run `conda env create -f environment.yaml` to create the same running environment as ours.
- The [apex](https://github.com/NVIDIA/apex) **must be installed**:
```bash 
pip3 install -U pip3
git clone https://github.com/NVIDIA/apex
cd apex
pip3 install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./
```

## Prepare datasets

We provide three datasets in this repo: long-tailed CIFAR (CIFAR-LT), long-tailed ImageNet (ImageNet-LT), and iNaturalist 2018 (iNat18). 

The detailed information of these datasets are shown as follows:

<table>
<thead>
  <tr>
     <th align="center" rowspan="3">Datasets</th>
     <th align="center" colspan="2">CIFAR-10-LT</th>
     <th align="center" colspan="2">CIFAR-100-LT</th>
     <th align="center" rowspan="3">ImageNet-LT</th>
     <th align="center" rowspan="3">iNat18</th>
  </tr>
  <tr>
    <td align="center" colspan="4"><b>Imbalance factor</b></td>
  </tr>
  <tr>
     <td align="center" ><b>100</b></td>
     <td align="center" ><b>50</b></td>
     <td align="center" ><b>100</b></td>
     <td align="center" ><b>50</b></td>
  </tr>
</thead>
<tbody>
  <tr>
     <td align="center" style="font-weight:normal">    Training images</td>
     <td align="center" style="font-weight:normal">  12,406 </td>
     <td align="center" style="font-weight:normal">  13,996  </td>
     <td align="center" style="font-weight:normal">  10,847  </td>
     <td align="center" style="font-weight:normal"> 12,608 </td>
     <td align="center" style="font-weight:normal">11,5846</td>
     <td align="center" style="font-weight:normal">437,513</td>
  </tr>
  <tr>
     <td align="center" style="font-weight:normal">    Classes</td>
     <td align="center" style="font-weight:normal">  50  </td>
     <td align="center" style="font-weight:normal">   50 </td>
     <td align="center" style="font-weight:normal">   100 </td>
     <td align="center" style="font-weight:normal">  100  </td>
     <td align="center" style="font-weight:normal"> 1,000 </td>
     <td align="center" style="font-weight:normal">8,142</td>
  </tr>
  <tr>
     <td align="center" style="font-weight:normal">Max images</td>
     <td align="center" style="font-weight:normal">5,000</td>
     <td align="center" style="font-weight:normal">5,000</td>
     <td align="center" style="font-weight:normal">500</td>
     <td align="center" style="font-weight:normal">500</td>
     <td align="center" style="font-weight:normal">1,280</td>
     <td align="center" style="font-weight:normal">1,000</td>
  </tr>
  <tr>
     <td align="center" style="font-weight:normal" >Min images</td>
     <td align="center" style="font-weight:normal">50</td>
     <td align="center" style="font-weight:normal">100</td>
     <td align="center" style="font-weight:normal">5</td>
     <td align="center" style="font-weight:normal">10</td>
     <td align="center" style="font-weight:normal">5</td>
     <td align="center" style="font-weight:normal">2</td>
  </tr>
  <tr>
     <td align="center" style="font-weight:normal">Imbalance factor</td>
     <td align="center" style="font-weight:normal">100</td>
     <td align="center" style="font-weight:normal">50</td>
     <td align="center" style="font-weight:normal">100</td>
     <td align="center" style="font-weight:normal">50</td>
     <td align="center" style="font-weight:normal">256</td>
     <td align="center" style="font-weight:normal">500</td>
  </tr>
</tbody>
</table>
<font size=2> -  `Max images` and `Min images` represents the number of training images in the largest and smallest classes, respectively.</font>

<font size=2> -  `CIFAR-10-LT-100` means the long-tailed CIFAR-10 dataset with the imbalance factor $\beta = 100$.</font>

<font size=2> -  `Imbalance factor` is defined as $\beta = \frac{\text{Max images}}{\text{Min images}}$.</font>

- #### Data format

The annotation of a dataset is a dict consisting of two field: `annotations` and `num_classes`.
The field `annotations` is a list of dict with
`image_id`, `fpath`, `im_height`, `im_width` and `category_id`.

Here is an example.
```
{
    'annotations': [
                    {
                        'image_id': 1,
                        'fpath': '/data/iNat18/images/train_val2018/Plantae/7477/3b60c9486db1d2ee875f11a669fbde4a.jpg',
                        'im_height': 600,
                        'im_width': 800,
                        'category_id': 7477
                    },
                    ...
                   ]
    'num_classes': 8142
}
```
- #### CIFAR-LT

  There are **two versions of CIFAR-LT**. 

  1. [Cui et al., CVPR 2019](https://arxiv.org/abs/1901.05555) firstly proposed the CIFAR-LT. They provided the [download link](https://github.com/richardaecn/class-balanced-loss/blob/master/README.md#datasets) of CIFAR-LT, and also the [codes](https://github.com/richardaecn/class-balanced-loss/blob/master/README.md#datasets) to generate the data, which are in TensorFlow. 

     You can follow the steps below to get this version of  CIFAR-LT:

     1. Download the Cui's CIFAR-LT in [GoogleDrive](https://drive.google.com/file/d/1NY3lWYRfsTWfsjFPxJUlPumy-WFeD7zK/edit) or [Baidu Netdisk ](https://pan.baidu.com/s/1rhTPUawY3Sky6obDM4Tczg) (password: 5rsq). Suppose you download the data and unzip them at path `/downloaded/data/`.
     2. Run tools/convert_from_tfrecords, and the converted CIFAR-LT and corresponding jsons will be generated at `/downloaded/converted/`.

  ```bash
  # Convert from the original format of iNaturalist
  python tools/convert_from_tfrecords.py  --input_path /downloaded/data/ --out_path /downloaded/converted/
  ```

  2. [Cao et al., ICLR 2020](https://arxiv.org/abs/1910.09217) followed  [Cui et al., CVPR 2019](https://arxiv.org/abs/1901.05555)'s method to generate the CIFAR-LT randomly. They modify the CIFAR datasets provided by PyTorch as [this file](https://github.com/zhangyongshun/BagofTricks-LT/blob/main/lib/dataset/cao_cifar.py) shows.

- #### ImageNet-LT

  You can use the following steps to convert from the original images of ImageNet-LT.

  1. Download the original [ILSVRC-2012](http://www.image-net.org/). Suppose you have downloaded and reorgnized them at path `/downloaded/ImageNet/`, which should contain two sub-directories: `/downloaded/ImageNet/train` and `/downloaded/ImageNet/val`.
  2. Download the train/test splitting files (`ImageNet_LT_train.txt` and `ImageNet_LT_test.txt`) in [GoogleDrive](https://drive.google.com/drive/u/0/folders/19cl6GK5B3p5CxzVBy5i4cWSmBy9-rT_-) or [Baidu Netdisk](https://pan.baidu.com/s/17alnFve8l-oMZFZQLxHrzQ) (password: cj0g).  Suppose you have downloaded them at path `/downloaded/ImageNet-LT/`. 
  3. Run tools/convert_from_ImageNet.py, and you will get two jsons: `ImageNet_LT_train.json` and `ImageNet_LT_val.json`.

  ```bash
  # Convert from the original format of iNaturalist
  python tools/convert_from_ImageNet.py --input_path /downloaded/ImageNet-LT/ --image_path /downloaed/ImageNet/ --output_path ./
  ```

- #### iNat18
  
  You can use the following steps to convert from the original format of iNaturalist 2018. 
  
  1. The images and annotations should be downloaded at [iNaturalist 2018](https://github.com/visipedia/inat_comp/blob/master/2018/README.md) firstly. Suppose you have downloaded them at  path `/downloaded/iNat18/`.
  2. Run tools/convert_from_iNat.py, and use the generated `iNat18_train.json` and `iNat18_val.json` to train.
  
  ```bash
  # Convert from the original format of iNaturalist
  # See tools/convert_from_iNat.py for more details of args 
  python tools/convert_from_iNat.py --input_json_file /downloaded/iNat18/train2018.json --image_path /downloaded/iNat18/images --output_json_file ./iNat18_train.json
  
  python tools/convert_from_iNat.py --input_json_file /downloaded/iNat18/val2018.json --image_path /downloaded/iNat18/images --output_json_file ./iNat18_val.json 
  ```
  

## Usage

In this repo:

- The results of CIFAR-LT (ResNet-32) and ImageNet-LT (ResNet-10), which need only one GPU to train, are gotten by DataParallel training with apex.

- The results of iNat18 (ResNet-50), which need more than one GPU to train, are gotten by DistributedDataParallel  training with apex. 

- If more than one GPU is used, DistributedDataParallel training is efficient than DataParallel training, especially when the CPU calculation forces are limited.

  

#### Parallel training with DataParallel 

```bash
1, Change the NCCL_SOCKET_IFNAME in run_with_distributed_parallel.sh to your own socket name. 

2, To train/valid
# To train long-tailed CIFAR-10 with imbalanced ratio of 50. 
# `GPUs` are the GPUs you want to use, such as `0,4`.
bash data_parallel_train.sh configs/test/data_parallel.yaml GPUs
```

#### Distributed training with DistributedDataParallel 
```bash
1, Change the NCCL_SOCKET_IFNAME in run_with_distributed_parallel.sh to your own socket name. 
export NCCL_SOCKET_IFNAME = your own socket name

2, To train/valid
# To train long-tailed CIFAR-10 with imbalanced ratio of 50. 
# `GPUs` are the GPUs you want to use, such as `0,1,4`.
# `NUM_GPUs` are the number of GPUs you want to use. If you set `GPUs` to `0,1,4`, then `NUM_GPUs` should be `3`.
bash distributed_data_parallel_train.sh configs/test/distributed_data_parallel.yaml NUM_GPUs GPUs
```

## Baseline results

- We use **Top-1 error rates** as our evaluation metric.

<!--ImageNet_LT baseline acc, baseline_noc 35.01 baseline 36.26 baseline2 34.18 baseline2_noc 33.71 -->

- From the results of two CIFAR-LT, we can see that the **CIFAR-LT provided by [Cao](https://arxiv.org/abs/1910.09217) has much lower Top-1 error rates on CIFAR-10-LT**, compared with the baseline results reported in his paper. So, **in our experiments, we use the CIFAR-LT of [Cui](https://arxiv.org/abs/1901.05555) for fairness.**
- **For the ImageNet-LT, we find that  the color_jitter augmentation was not included in our experiments, which, however, is adopted by other methods. So, in this repo, we add the color_jitter augmentation on ImageNet-LT. The old baseline without color_jitter is 64.89, which is +1.15 points higher than the new baseline.**
- You can click the `Baseline` in the table below to see the experimental settings and corresponding running commands. 

<table>
<thead>
  <tr>
    <th rowspan="4" align="center">Datasets</th>
    <th align="center" colspan="4" align="center"><a href="https://arxiv.org/abs/1901.05555">Cui et al., 2019</a></th>
    <th align="center" colspan="4"><a href="https://arxiv.org/abs/1910.09217">Cao et al., 2020</a></th>
    <th align="center" rowspan="4">ImageNet-LT</th>
    <th align="center" rowspan="4">iNat18</th>
  </tr>
  <tr>
    <th align="center"  colspan="2">CIFAR-10-LT</td>
    <th align="center"  colspan="2">CIFAR-100-LT</td>
    <th align="center"  colspan="2">CIFAR-10-LT</td>
    <th align="center"  colspan="2">CIFAR-100-LT</td>
  </tr>
  <tr>
    <th align="center"  colspan="4" align="center">Imbalance factor</td>
    <th align="center"  colspan="4">Imbalance factor</td>
  </tr>
  <tr>
    <th align="center" >100</td>
    <th align="center" >50</td>
    <th align="center" >100</td>
    <th align="center" >50</td>
    <th align="center" >100</td>
    <th align="center" >50</td>
    <th align="center" >100</td>
    <th align="center" >50</td>
  </tr>
</thead>
<tbody>
  <tr>
    <th align="center" style="font-weight:normal">Backbones</td>
    <th align="center"  colspan="4" style="font-weight:normal">ResNet-32</td>
    <th align="center"  colspan="4" style="font-weight:normal">ResNet-32</td>
    <th align="center" style="font-weight:normal">ResNet-10</td>
    <th align="center" style="font-weight:normal" >ResNet-50</td>
  </tr>
  <tr>
    <th align="left" style="font-weight:normal"><details><summary>Baseline</summary>
      <ol>
      <li>CONFIG (from left to right): 
        <ul>
          <li>configs/cui_cifar/baseline/{cifar10_im100.yaml, cifar10_im50.yaml, cifar100_im100.yaml, cifar100_im50.yaml}</li> 
          <li>configs/cao_cifar/baseline/{cifar10_im100.yaml, cifar10_im50.yaml, cifar100_im100.yaml, cifar100_im50.yaml}</li>
          <li>configs/ImageNet_LT/imagenetlt_baseline.yaml</li>
          <li>configs/iNat18/iNat18_baseline.yaml</li>
        </ul>
        </li><br/>
      <li>Running commands:
        <ul>
          <li>For CIFAR-LT and ImageNet-LT: bash data_parallel_train.sh CONFIG GPU</li>
          <li>For iNat18: bash distributed_data_parallel_train.sh configs/iNat18/iNat18_baseline.yaml NUM_GPUs GPUs</li>
        </ul>
      </li>
      </ol>
      </details></td>
    <th align="center" style="font-weight:normal">30.12</td>
    <th align="center" style="font-weight:normal">24.81</td>
    <th align="center" style="font-weight:normal">61.76</td>
    <th align="center" style="font-weight:normal">57.65</td>
    <th align="center" style="font-weight:normal">28.05</td>
    <th align="center" style="font-weight:normal">23.55</td>
    <th align="center" style="font-weight:normal">62.27</td>
    <th align="center" style="font-weight:normal">56.22</td>
    <th align="center" style="font-weight:normal">63.74</td>
    <th align="center" style="font-weight:normal">40.55</td>
  </tr>
  <tr>
    <th align="left" style="font-weight:normal">Reference (<a href="https://arxiv.org/abs/1901.05555">Cui</a>;<a href="https://arxiv.org/abs/1910.09217">Cao</a>; <a href="https://arxiv.org/abs/1904.05160">Liu</a.>)</td>
    <th align="center" style="font-weight:normal">29.64</td>
    <th align="center" style="font-weight:normal">25.19</td>
    <th align="center" style="font-weight:normal">61.68</td>
    <th align="center" style="font-weight:normal">56.15</td>
    <th align="center" style="font-weight:normal">29.64</td>
    <th align="center" style="font-weight:normal">25.19</td>
    <th align="center" style="font-weight:normal">61.68</td>
    <th align="center" style="font-weight:normal">56.15</td>
    <th align="center" style="font-weight:normal">64.40</td>
    <th align="center" style="font-weight:normal">42.86</td>
  </tr>
</tbody>
</table>



## Citation

```
@inproceedings{zhang2020tricks,
  author    = {Yongshun Zhang and Xiu{-}Shen Wei and Boyan Zhou and Jianxin Wu},
  title     = {Bag of Tricks for Long-Tailed Visual Recognition with Deep Convolutional Neural Networks},
  booktitle = {AAAI},
  year      = {2021},
}
```

- *pages need to be added.*

## Contacts

If you have any question about our work, please do not hesitate to contact us by emails provided in the [paper](http://www.lamda.nju.edu.cn/zhangys/papers/AAAI_tricks.pdf).
