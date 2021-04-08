# stylegan-waifu-generator
Generate your waifu with styleGAN

StyleGAN老婆生成器 [中文](README_CN.md)

## Introduction
This is a pet project I've been working on, trained over months using the Danbooru 2019 dataset. It is able to produce some cool results.

## Dataset
I am using Danbooru 2019 dataset from [gwern](https://www.gwern.net/About)

```
Anonymous, The Danbooru Community, & Gwern Branwen; “Danbooru2020: A Large-Scale Crowdsourced and Tagged Anime Illustration Dataset”, 2020-01-12. Web. Accessed July, 2020 https://www.gwern.net/Danbooru2020
```
I cleaned the dataset a bit removing invalid pictures and cropped faces and only perserved high definition ones. The resulting training data is around 18GB for 512px square, later the samples are resized and converted to lmdb, the entire lmdb dataset is around 426GB

Example:

![Example data](res/2238231-0.jpg)

## Training
Training is done with the [stylegan2-pytorch](https://github.com/rosinality/stylegan2-pytorch) with training script modified and dataset loading modified. It took around 1.5 month to train this entire dataset, I forgot how many iteration it has done, but I vaguely remember it was around 800K-1M.

It was trained on Ubuntu 18.04 with RTX Titan.

## Result

![256px](res/000017.png)

## Control
One key difference for this project is you are able to control the generated output with a preset latent vector. The latent vector will be used in the network to generate styles and produce the final results. The same latent vector will product the same result given fixed random seed. To make the latent vector control manageable, I created the [rust-genome](https://github.com/r1cebank/genome) project that will handle the genome generation for the generator.

Sample genome:

```
c092c23a008000033f1fc413bdba659fbed62d56befa86f93e787d0a3f89028d3d4bf2673f8a221ebef5605bbda472dd3f678906be82855b3f9493b8c............020353ebf8c074ac02c3b9e3fc24ff03f49dbf0563f8bdf467743f8fca4dc0095918bf379b733e8382ec3ce89666bf2b9dbe3e08f23ebe8fe6a0bf3ab1b9bf7b6a743df1f45c
```

Generated image:

![genome generated image](res/000.png)

This also enables genome mixing (style mixing), merging of the generated results. A proof-of-concept generation script is made to generate an interpolation of the two genome.

![interpolation](res/out.gif)

## Trained model
Trained model for 256px and 512px is posted in release https://github.com/diva-eng/stylegan-waifu-generator/releases/

## Generation
Using the provided generate.py
### With CUDA
```python
device = 'cuda'
```
### With CPU
```python
device = 'cpu'
```

```
python generate.py --size 512 --ckpt checkpoint/790000.pt
```
*Note: you must provide the size in the parameter matching the size of the checkpoint.


## Special thanks
Danbooru 2019 dataset

Stylegan2-pytorch
