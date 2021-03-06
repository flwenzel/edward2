# Wide ResNet 28-10 on CIFAR

## CIFAR-10

| Method | Train/Test NLL | Train/Test Accuracy | Train/Test Cal. Error | cNLL/cA/cCE | Train Runtime (hours) | # Parameters |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| Deterministic | 1e-3 / 0.159 | 99.9% / 96.0% | 1e-3 / 0.0231 | 1.29 / 69.8% / 0.173 | 1.2 (8 TPUv2 cores) | 36.5M |
| BatchEnsemble (size=4) | 0.08 / 0.143 | 99.9% / 96.2% |  5e-5 / 0.0206 | 1.24 / 69.4% / 0.143 | 5.4 (8 TPUv2 cores) | 36.6M |
| Dropout | 2e-3 / 0.160 | 99.9% / 95.9% | 2e-3 / 0.0241 | 1.35 / 67.8% / 0.178 | 1.2 (8 TPUv2 cores) | 36.5M |
| Ensemble (size=4) | 2e-3 / 0.114 | 99.9% / 96.6% |  - | - | 1.2 (32 TPUv2 cores) | 146M |
| Variational inference | 1e-3 / 0.214 | 99.9% / 94.7% | 1e-3 / 0.029 | 1.53 / 70.6% / 0.190 | 5.5 (8 TPUv2 cores) | 73M |

## CIFAR-100

| Method | Train/Test NLL | Train/Test Accuracy | Train/Test Cal. Error | cNLL/cA/cCE | Train Runtime (hours) | # Parameters |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| Deterministic<sup>10</sup> | 1e-3 / 0.875 | 99.9% / 79.8% | 2e-3 / 0.0857 | - | 1.1 (8 TPUv2 cores) | 36.5M |
| BatchEnsemble (size=4) | 4e-3 / 0.734 | 99.7% / 81.5% | 3e-3 / 0.0338 | - | 5.5 (8 TPUv2 cores) | 36.6M |
| Dropout | 1e-2 / 0.830 | 99.9% / 79.6% | 9e-3 / 0.0501 | - | 1.1 (8 TPUv2 cores) | 36.5M |
| Ensemble (size=4) | 0.003 / 0.666 | 99.9% / 82.7% | - | - | 1.1 (32 TPUv2 cores) | 146M |
| Variational inference | 2e-3 / 1.03 | 99.9% / 77.3% | 2e-3 / 0.111 | - | 5.5 (8 TPUv2 cores) | 73M |

## Metrics

We define metrics specific to CIFAR below. For general metrics, see [`baselines/`](https://github.com/google/edward2/tree/master/baselines).

1. __cNLL/cA/cCE__. Negative-log-likelihood, accuracy, and calibration error on [CIFAR-10-C](https://arxiv.org/abs/1903.12261); we apply the same corruptions to produce a CIFAR-100-C. `c` stands for corrupted. Results take the mean across corruption intensities and corruption types.

## CIFAR-10 Related Results

We note results in the literature below. Note there are differences in the setup
(sometimes major), so take any comparisons with a grain of salt.

| Source | Method | Train/Test NLL | Train/Test Accuracy | Train Runtime (hours) | # Parameters |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| [`meliketoy/wide-resnet.pytorch`](https://github.com/meliketoy/wide-resnet.pytorch) | Deterministic | - | - / 96.21% | 6.8 (1 GPU) | 36.5M |
| | Non-MC Dropout | - | - / 96.27% | 6.8 (1 GPU)  | 36.5M |
| [He et al. (2015)](https://arxiv.org/abs/1512.03385)<sup>1</sup> | Deterministic (ResNet-56) | - | - / 93.03% | - | 850K |
| | Deterministic (ResNet-110) | - | - / 93.39% | - | 1.7M |
| [Zagoruyko and Komodakis (2016)](https://github.com/szagoruyko/wide-residual-networks) | Deterministic | - | - / 96.00% | - | 36.5M |
| | Non-MC Dropout | - | - / 96.11% | - | 36.5M |
| [Louizos et al. (2017)](https://arxiv.org/abs/1705.08665)<sup>2</sup> | Group-normal Jeffreys | - | - / 91.2% | - | 998K |
| | Group-Horseshoe | - | - / 91.0% | - | 820K |
| [Molchanov et al. (2017)](https://arxiv.org/abs/1701.05369)<sup>2</sup> | Variational dropout | - | - / 92.7% | - | 304K |
| [Louizos et al. (2018)](https://arxiv.org/abs/1712.01312) | L0 regularization | - | - / 96.17% | 200 epochs | - |
| [Havasi et al. (2019)](https://openreview.net/forum?id=rkglZyHtvH)<sup>3</sup> | Refined VI (no batchnorm) | - / 0.696 | - / 75.5% | 5.5 (1 P100 GPU) | - |
| | Refined VI (batchnorm) | - / 0.593 | - / 79.7% | 5.5 (1 P100 GPU) | - |
| | Refined VI hybrid (no batchnorm) | - / 0.432 | - / 85.8% | 4.5 (1 P100 GPU) | - |
| | Refined VI hybrid (batchnorm) | - / 0.423 | - / 85.6% | 4.5 (1 P100 GPU) | - |
| [Heek and Kalchbrenner (2019)](https://arxiv.org/abs/1908.03491)<sup>5</sup> | Deterministic (ResNet-56) | - / 0.243 | - / 94.4% | 1000 epochs (1 V100 GPU) | 850K |
| | Adaptive Thermostat Monte Carlo (single sample) | - / 0.303 | - / 92.4% | 1000 epochs (1 V100 GPU) | - |
| | Adaptive Thermostat Monte Carlo (multi-sample) | - / 0.194 | - / 93.9% | 1000 epochs (1 V100 GPU) | - |
| | Sampler-based Nose-Hoover Thermostat (single sample) | - / 0.343 | - / 91.7% | 1000 epochs (1 V100 GPU) | - |
| | Sampler-based Nose-Hoover Thermostat (multi-sample) | - / 0.211 | - / 93.5% | 1000 epochs (1 V100 GPU) | - |
| [Izmailov et al. (2019)](https://arxiv.org/abs/1907.07504)<sup>6</sup> | Deterministic | - / 0.1294 | - / 96.41% | 300 epochs | 36.5M |
| | SWA | - / 0.1075 | - / 96.46% | 300 epochs | 36.5M |
| | SWAG | - / 0.1122 | - / 96.41% | 300 epochs | 803M |
| | Subspace Inference (PCA+VI) | - / 0.1081 | - / 96.32% | >300 epochs | 219M |
| [Osawa et al. (2019)](https://arxiv.org/abs/1906.02506)<sup>7</sup>  | Variational Online Gauss-Newton | - / 0.48 | 91.6% / 84.3% | 2.38 (128 P100 GPUs) | - |
| [Ovadia et al. (2019)](https://arxiv.org/abs/1906.02530)<sup>8</sup> | Deterministic (ResNet-20) | - / 1.120 | - / 91% | - | 274K |
| | Dropout | - / 0.771 | - / 91% | - | 274K |
| | Ensemble | - / 0.653 | - | - / 93.5% | - |
| | Variational inference | - / 0.823 | 88% | - | 630K |
| [Wen et al. (2019)](https://openreview.net/forum?id=Sklf1yrYDr)<sup>4</sup> | Deterministic (ResNet-32x4) | - | - / 95.31% | 250 epochs | 7.43M |
| | BatchEnsemble | - | - / 95.94% | 375 epochs | 7.47M |
| | Ensemble | - | - / 96.30% | 250 epochs each | 29.7M |
| | Monte Carlo Dropout | - | - / 95.72% | 375 epochs | 7.43M |
| [Zhang et al. (2019)](https://arxiv.org/abs/1902.03932)<sup>9</sup> | Deterministic (ResNet-18) | - | - / 94.71% | 200 epochs | 11.7M |
| | cSGHMC | - | - / 95.73% | 200 epochs | 140.4M |
| | Ensemble of cSGHMC (size=4) | - | - / 96.05% | 800 epochs | 561.6M |

## CIFAR-100 Related Results

We note results in the literature below. Note there are differences in the setup
(sometimes major), so take any comparisons with a grain of salt.

| Source | Method | Train/Test NLL | Train/Test Accuracy | Train Runtime (hours) | # Parameters |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| [`meliketoy/wide-resnet.pytorch`](https://github.com/meliketoy/wide-resnet.pytorch) | Deterministic | - | - / 81.02% | 6.8 (1 GPU) | 36.5M |
| | Non-MC Dropout | - | - / 81.49% | 6.8 (1 GPU)  | 36.5M |
| [Zagoruyko and Komodakis (2016)](https://github.com/szagoruyko/wide-residual-networks) | Deterministic | - | - / 80.75% | - | 36.5M |
| | Non-MC Dropout | - | - / 81.15% | - | 36.5M |
| [Izmailov et al. (2019)](https://arxiv.org/abs/1907.07504)<sup>7</sup> | Deterministic | - / 0.7958 | - / 80.76% | 300 epochs | 36.5M |
| | SWA | - / 0.6684 | - / 82.40% | 300 epochs | 36.5M |
| | SWAG | - / 0.6078 | - / 82.23% | 300 epochs | 803M |
| | Subspace Inference (PCA+VI) | - / 0.6052 | - / 82.63% | >300 epochs | 36.5M |
| [Zhang et al. (2019)](https://arxiv.org/abs/1902.03932)<sup>10</sup> | Deterministic (ResNet-18) | - | - / 77.40% | 200 epochs | 11.7M |
| | cSGHMC | - | - / 79.50% | 200 epochs | 140.4M |
| | Ensemble of cSGHMC (size=4) | - | - / 80.81% | 800 epochs | 561.6M |

1. Trains on 45k examples.
2. Not a ResNet (VGG). Parameter count is guestimated from counting number of parameters in [original model](http://torch.ch/blog/2015/07/30/cifar.html) to be 14.9M multiplied by the compression rate.
3. Uses ResNet-20. Does not use data augmentation.
4. Uses ResNet-32 with 4x number of typical filters. Ensembles uses 4 members.
5. Uses ResNet-56 and modifies architecture. Cyclical learning rate.
6. SWAG with rank 20 requires 20 + 2 copies of the model parameters and uses 30 samples at test time. Deterministic baseline is reported in [Maddox et al. (2019)](https://arxiv.org/abs/1902.02476). Subspace inference with a rank 5 projection requires a total of 5 + 1 copies of the model parameters at test time (the posterior inference parameters are negligible). Subspace inference also uses a fixed temperature.
7. ResNet-20. Scales KL by an additional factor of 10.
8. ResNet-20. Trains on 40k examples. Performs variational inference over only first convolutional layer of every residual block and final output layer. Has free parameter on normal prior's location. Uses scale hyperprior (and with a fixed scale parameter). NLL results are medians, not means; accuracies are guestimated from Figure 2's plot.
9. Uses ResNet-18. cSGHMC uses a total of 12 copies of the full size of weights for prediction. Ensembles use 4 times cSGHMC's number. The authors use a T=1/200 temperature scaling on the log-posterior (see the newly added appendix I at https://openreview.net/forum?id=rkeS1RVtPS).
10. Results are slightly worse than open-source implementations such as the [original paper](https://github.com/szagoruyko/wide-residual-networks)'s 80.75%. Our experiments only tuned over l2, so there may be more work to be done.

TODO(trandustin): Add column for Test runtime.

TODO(trandustin): Add column for Checkpoints.

TODO(trandustin): Rename directory to `cifar/`.
