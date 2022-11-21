# GMAA-Pytorch Implementation

## Introduction

The repo provides code of **GMAA** with [Lightning-Hydra-Template](https://github.com/ashleve/lightning-hydra-template).

## Usage

The training and testing experiments are conducted using with a single NVIDIA Tesla V100 32GB.

1. Prerequisites:

   Note that GMAA is only tested on Ubuntu OS with the following environments. It may work on other operating systems (i.e., Windows) as well but we do not guarantee that it will.
    + Creating a virtual environment in terminal: `conda create -n GMAA python=3.8`.
    + Installing necessary packages: `pip install -r requirements.txt`.

2. Prepare the data/pretrained weights:

    + downloading CelebA-HQ dataset from [Google Drive](https://drive.google.com/open?id=1badu11NqxGf6qM3PTTooQDJvQbejgbTv) or [Baidu Drive](https://pan.baidu.com/s/1wN1E-B1bJ7mE1mrn9loj5g) (From [CelebAMask-HQ](https://github.com/switchablenorms/CelebAMask-HQ)).

      Assigning your costumed path `--src_hq_path` to run `data/CelebAHQ/process.py` in order to filter original CelebA-HQ dataset by valid AUs (the confidence from [OpenFace](https://github.com/TadasBaltrusaitis/OpenFace) need be greater or equal to 0.95).

    + downloading pretrained face recognition models from [Google Drive](https://drive.google.com/drive/folders/1G_2R_7XQhzzMQdEhph0ZI7dV4sGYjjzu?usp=sharing) (From [Adv-Makeup](https://github.com/TencentYoutuResearch/Adv-Makeup)).
      Move the pretrained models to `pretrained/FRmodels`.

3. Training Configuration:

    + Just enjoy it via running `bash script/train.sh` in your terminal. The trainning results is in `logs/train/gmaa/runs/%Y-%m-%d_%H-%M-%S`.

4. Testing Configuration: 

    + After step.3, replace your trained model path (`--ckpt_path`) in `script/eval.sh`. The trained model path is `logs/train/gmaa/runs/%Y-%m-%d_%H-%M-%S/checkpoints/epoch_019.ckpt`(`max_epoch` is setted to `20`).
    + Just enjoy it via running `bash script/eval.sh` in your terminal. The **evaluation results directory** is `logs/eval/gmaa/runs/%Y-%m-%d_%H-%M-%S`. The generated adversarial examples of test dataset is in `test_vis` of  **evaluation results directory**.

5. Evaluation Configuration:

    + Replace your testing adversarial examples directory (`--res_root`) in `metric/test_asr.py` & `metric/test_faceplusplus.py`. The testing adversarial examples directory of step.4 is `test_vis` of  **evaluation results directory**.
    
    + Just enjoy it via running `python metric/test_asr.py` to get the attack success rate. The result is saved in `test_asr` of  **evaluation results directory**.
    
    + Just enjoy it via running `python metric/test_faceplusplus.py` to get the Face++ confidence score. The result is saved in `test_faceplusplus` of  **evaluation results directory**. 
    
      **Please note:** Need fill your own api `--key` and `--secret` getted from [Face++](https://www.faceplusplus.com.cn/).
```shell
        logs/eval/gmaa/runs/%Y-%m-%d_%H-%M-%S
           └- test_asr                         # Attack Success Rate
           └- test_faceplusplus                # Face++ confidence score
           └- test_vis                         # generated adversarial examples of test dataset
           └- ...
```

**The final project should be like this:**


```shell
    GMAA
    └- data
       └- CelebAHQ
       └- CelebA-pairs
       └- typical_au.txt
    └- log
       └- eval
       └- train
    └- pretrained
       └- exper_edit
          └- ...
       └- FRmodels
          └- facenet.pth
          └- ir152.pth
          └- irse50.pth
          └- mobile_face.pth
    └- ...
```

## Acknowledge

Some of the codes are built upon [AMT](https://github.com/CGCL-codes/AMT-GAN), pretrained face recognition models are from [Adv-Makeup](https://github.com/TencentYoutuResearch/Adv-Makeup), CelebA-HQ dataset is from [CelebAMask-HQ](https://github.com/switchablenorms/CelebAMask-HQ).

**Anonymity rules:** All the above links are from third parties!