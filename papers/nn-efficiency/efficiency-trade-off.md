# Efficiency and Accuracy Trade-off in Neural Networks

- Model compression: [Han et al. (2016)](https://arxiv.org/abs/1510.00149); [He et al. (2018)](https://arxiv.org/abs/1802.03494); [Yang et al. (2018)](https://arxiv.org/abs/1804.03230).
  - Pruning: hard to convert reduction in model parameters to speedup due to lack of exploitable regularity in the resulting sparse structure.
  - Tucker decomposition.
- Implementation optimization
- Neural architecture search: [Tan et al. (2019)](https://arxiv.org/abs/1807.11626); [Cai et al. (2019](https://arxiv.org/abs/1812.00332)).
  - However it is difficult to apply it for larger models with much larger design space and much more expensive tuning cost.
- Handcraft mobile-size CNNs: [SqueezeNet](https://arxiv.org/abs/1602.07360); [MobileNet](https://arxiv.org/abs/1704.04861); [ShuffleNet](https://arxiv.org/abs/1707.01083).
- Model scaling
  - ways to scale
    - depth (# layers) [ResNet](https://arxiv.org/abs/1512.03385)
    - width (# channels) [WideResNet](https://arxiv.org/abs/1605.07146), [MobileNet](https://arxiv.org/abs/1704.04861);
    - resolutions
  - Relationship between network depth and width
    - Theoretical: [Raghu et al. (2017)](https://arxiv.org/abs/1606.05336); [Lu et al. (2018)](https://arxiv.org/abs/1709.02540).
    - Empirical: [WideResNet](https://arxiv.org/abs/1605.07146).
- Expressive power?
  - [Raghu et al. (2017)](https://arxiv.org/abs/1606.05336); [Lin & Jegelka (2018)](https://arxiv.org/abs/1806.10909); [Sharir & Shashua (2018)](https://arxiv.org/abs/1703.02065); [Lu et al. (2018)](https://arxiv.org/abs/1709.02540).
