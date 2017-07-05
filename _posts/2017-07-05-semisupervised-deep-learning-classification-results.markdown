---
layout: post
title:  Semi-supervised deep learning classification results
date:   2017-07-05 16:27:00 +0100
tags: [ml, dl, cv]
image_preview: http://sakishinoda.github.io/assets/mnist.png
short_description: "Comparison of published results on MNIST, SVHN, CIFAR-10, CIFAR-100 in semi-supervised setting."
---

Best self-reported performances on semi-supervised image classification benchmarks. Inspired by [Rodrigo Benenson's mini-site](http://rodrigob.github.io/are_we_there_yet/build/) collating published performance on key computer vision datasets.

Semi-supervised learning corresponds to the case where a large dataset may be available, but only a small subset of the data is labelled. Traditional semi-supervised learning includes graph-based algorithms like label propagation, which is included in ``scikit-learn``. Deep learning approaches tend to make use of novel architectures (e.g. ladder networks), regularization techniques (e.g. virtual adversarial training), or generative models (e.g. DGN, GAN). 

#### Permutation Invariant MNIST

| 100 labels  | 1000 labels   | All labels | Method   | Year  |
|---|---|---|---|---| 
| 0.93 (±0.065) | N/A | N/A | [Improved Techniques for Training GANs](https://arxiv.org/abs/1606.03498) | 2016 |
| 1.002 (±0.038)| 0.979 (±0.025) | 0.578 (±0.013) | [Deconstructing the Ladder Network Architecture](http://arxiv.org/abs/1511.06430) (Ladder w/ AMLP[2,2,2]) | 2015 |
| 1.072 (±0.015)| 0.974 (±0.021) | 0.598 (±0.014) | [Deconstructing the Ladder Network Architecture](http://arxiv.org/abs/1511.06430) (Ladder w/ AMLP[4]) | 2015 |
| 1.072 (±0.015)| 1.193 (±0.039) | 0.569 (±0.010) | [Deconstructing the Ladder Network Architecture](http://arxiv.org/abs/1511.06430) (Ladder w/ AMLP[2,2]) | 2015 |
| 1.06 (±0.37) | 0.84 (±0.08) | 0.57 (±0.02) | [Semi-Supervised Learning with Ladder Networks](http://arxiv.org/abs/1507.02672) | 2015 |
| 1.36 | 1.27 | 0.64 | [Virtual Adversarial Training: a Regularization Method for Supervised and Semi-supervised Learning](http://arxiv.org/abs/1704.03976) | 2017 |
| 2.33 | 1.36 | 0.637 (±0.046) | [Distributional Smoothing with Virtual Adversarial Training](https://arxiv.org/abs/1507.00677) | 2016 (ICLR) |


#### SVHN
_work in progress_

#### CIFAR-10
_work in progress_
 
#### CIFAR-100
_work in progress_