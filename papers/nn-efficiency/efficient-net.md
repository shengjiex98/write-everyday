# Efficient Net & Efficient Det

## Efficient Net

- Contribution
  - Proposed a **scaling method** that uniformly scales all dimensions of depth/width/resolution using *compound coefficients*: $2^N$ more computational resources means increasing depth, width, and image size by $\alpha^N$, $\beta^N$, and $\gamma^N$ respectively.
  - Designed a new baseline network, which is then scaled up to become a **family of classification models**.
- Related work
  - Previous work mostly only scale one of depth, width and image size.
  - Neural architecture search becomes popular in designing efficient (mobile-size) CNNs.
- Experimental setup
  - [ImageNet](https://www.image-net.org/). Here are some [examples](https://github.com/EliSchwartz/imagenet-sample-images).
  - **Transfer learning** datasets: [CIFAR-100](https://www.cs.toronto.edu/~kriz/cifar.html), [Flowers](https://www.robots.ox.ac.uk/~vgg/data/flowers/).

## Efficient Det

- Contribution
  - Proposed a weighted bi-directional feature pyramid network (**BiFPN**)
  - Proposed a **compound scaling method** for all backbone(?), feature network, and box/class prediction networks at the same time.
  - Designed a new **family of object detectors**.
- Experimental setup
  - [COCO 2017](https://cocodataset.org/#explore)
  - 