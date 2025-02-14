# HIVE

### [HIVE: Harnessing Human Feedback for Instructional Visual Editing](https://arxiv.org/pdf/2303.09618.pdf)
Shu Zhang\*<sup>1</sup>, Xinyi Yang\*<sup>1</sup>, Yihao Feng\*<sup>1</sup>, Can Qin<sup>3</sup>, Chia-Chih Chen<sup>1</sup>, Ning Yu<sup>1</sup>, Zeyuan Chen<sup>1</sup>, Huan Wang<sup>1</sup>, Silvio Savarese<sup>1,2</sup>, Stefano Ermon<sup>2</sup>, Caiming Xiong<sup>1</sup>, and Ran Xu<sup>1</sup><br>
<sup>1</sup>Salesforce AI, <sup>2</sup>Stanford University, <sup>3</sup>Northeastern University<br>
 \*denotes equal contribution<br>
arXiv 2023

### [paper](https://arxiv.org/pdf/2303.09618.pdf) | [project page](https://shugerdou.github.io/hive/)

<img src='imgs/results.png' width=700></pre>

This is a PyTorch implementation of [HIVE: Harnessing Human Feedback for Instructional Visual Editing](https://arxiv.org/pdf/2303.09618.pdf). The major part of the code follows [InstructPix2Pix](https://github.com/timothybrooks/instruct-pix2pix). In this repo, we have implemented both [stable diffusion v1.5-base](https://huggingface.co/runwayml/stable-diffusion-v1-5) and [stable diffusion v2.1-base](https://huggingface.co/stabilityai/stable-diffusion-2-1-base) as the backbone.




## Usage

### Preparation
First set-up the ```hive``` enviroment and download the pretrianed model as below. This is only verified on CUDA 11.0 and CUDA 11.3 with NVIDIA A100 GPU.

```
conda env create -f environment.yaml
conda activate hive
bash scripts/download_checkpoints.sh
```

To fine-tune a stable diffusion model, you need to obtain the pre-trained stable diffusion models following their [instructions](https://github.com/runwayml/stable-diffusion). If you use SD-V1.5, you can download the huggingface weights [HuggingFace SD 1.5](https://huggingface.co/runwayml/stable-diffusion-v1-5/resolve/main/v1-5-pruned-emaonly.ckpt). If you use SD-V2.1, the weights can be downloaded on [HuggingFace SD 2.1](https://huggingface.co/stabilityai/stable-diffusion-2-1-base). You can decide which version of checkpoint to use. We use ```v2-1_512-ema-pruned.ckpt```. Download the model to checkpoints/.


### Data
We suggest to install Gcloud CLI following [Gcloud download](https://cloud.google.com/sdk/docs/install). Then run
```
bash scripts/download_hive_data.sh
```

An alternative method is to directly download the data through [Evaluation data](https://storage.cloud.google.com/sfr-hive-data-research/data/evaluation.zip) and [Evaluation instructions](https://storage.cloud.google.com/sfr-hive-data-research/data/test.jsonl).


### Inference
Samples can be obtained by running the command. 

For SD v2.1, if we use the conditional reward, we run

```
python edit_cli_rw_label.py --steps 100 --resolution 512 --seed 100 --cfg-text 7.5 --cfg-image 1.5 \
--input imgs/example1.jpg --output imgs/output.jpg --edit "move it to Mars" --ckpt checkpoints/hive_v2_rw_condition.ckpt \
--config configs/generate_v21_base.yaml
```


or run batch inference on our inference data:

```
python edit_cli_batch_rw_label.py --steps 100 --resolution 512 --seed 100 --cfg-text 7.5 --cfg-image 1.5 \
--jsonl_file data/test.jsonl --output_dir imgs/sdv21_rw_label/ --ckpt checkpoints/hive_v2_rw_condition.ckpt \
--config configs/generate_v21_base.yaml --image_dir data/evaluation/
```

For SD v2.1, if we use the weighted reward, we can run


```
python edit_cli.py --steps 100 --resolution 512 --seed 100 --cfg-text 7.5 --cfg-image 1.5 \
--input imgs/example1.jpg --output imgs/output.jpg --edit "move it to Mars" \
--ckpt checkpoints/hive_v2_rw.ckpt --config configs/generate_v21_base.yaml
```

or run batch inference on our inference data:

```
python edit_cli_batch.py --steps 100 --resolution 512 --seed 100 --cfg-text 7.5 --cfg-image 1.5 \
--jsonl_file data/test.jsonl --output_dir imgs/sdv21/ --ckpt checkpoints/hive_v2_rw.ckpt \
--config configs/generate_v21_base.yaml --image_dir data/evaluation/
```

For SD v1.5, if we use the conditional reward, we can run

```
python edit_cli_rw_label.py --steps 100 --resolution 512 --seed 100 --cfg-text 7.5 --cfg-image 1.5 \
--input imgs/example1.jpg --output imgs/output.jpg --edit "move it to Mars" \
--ckpt checkpoints/hive_rw_condition.ckpt --config configs/generate.yaml
```

or run batch inference on our inference data:

```
python edit_cli_batch_rw_label.py --steps 100 --resolution 512 --seed 100 --cfg-text 7.5 --cfg-image 1.5 \
--jsonl_file data/test.jsonl --output_dir imgs/sdv15_rw_label/ \
--ckpt checkpoints/hive_rw_condition.ckpt --config configs/generate.yaml \
--image_dir data/evaluation/
```

For SD v1.5, if we use the weighted reward, we run


```
python edit_cli.py --steps 100 --resolution 512 --seed 100 --cfg-text 7.5 --cfg-image 1.5 --input imgs/example1.jpg \
--output imgs/output.jpg --edit "move it to Mars" \
--ckpt checkpoints/hive_rw.ckpt --config configs/generate.yaml
```

or run batch inference on our inference data:

```
python edit_cli_batch.py --steps 100 --resolution 512 --seed 100 --cfg-text 7.5 --cfg-image 1.5 \
--jsonl_file data/test.jsonl --output_dir imgs/sdv15/ \
--ckpt checkpoints/hive_rw.ckpt --config configs/generate.yaml \
--image_dir data/evaluation/
```

## Citation
  ```
  @article{zhang2023hive,
  	title={HIVE: Harnessing Human Feedback for Instructional Visual Editing},
  	author={Zhang, Shu and Yang, Xinyi and Feng, Yihao and Qin, Can and Chen, Chia-Chih and Yu, Ning and Chen, Zeyuan and Wang, Huan and Savarese, Silvio and Ermon, Stefano and Xiong, Caiming and Xu, Ran},
  	journal={arXiv preprint arXiv:2303.09618},
  	year={2023}
	}
  ```


