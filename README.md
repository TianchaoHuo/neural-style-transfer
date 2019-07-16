

## Overview
Neural Style Transfer (NST) refers to a class of software algorithms that manipulate digital images, or videos, to adopt the appearance or visual style of another image. NST algorithms are characterized by their use of deep neural networks in order to perform the image transformation. Common uses for NST are the creation of artificial artwork from photographs, for example by transferring the appearance of famous paintings to user supplied photographs. [1] In this project, we are going to implement this algorithm and it is also the assignment in the deep learning AI course delivered by Andrew Ng [2].

Repo:
<!--more-->

## Objective
In this example, we are going to generate an image of the Louvre museum in Paris (content image C), mixed with a painting by Claude Monet, a leader of the impressionist movement (style image S) [2]. This is an expected result of our implementataion.

<div align=center><img src = "Style-Transfer\objective.jpg"></div></br>

## Methodology

You can refer to this [blog](https://tianchaohuo.github.io/2019/07/16/Style-Transfer/), or directly refer to the [paper](https://arxiv.org/abs/1508.06576), and the course[2]

### Pre-trained neural networks
It is known that CNN can capture the high level feature of images [3]. As shown in the following, content image is passed through the CNN and then the image representation can be obtained, feature mapping in other words and then almost can obtain the image similar to the originial after reconstructing.In particular, the results obtained by reconstruction of the first few layers are closer to the original image, and it also indicates that more details of the images are retained in the first few layers, because there is the pooling layer behind, so some information of tuning will be naturally discarded.
<div align=center><img src = "Style-Transfer\flow.jpg"></div>
In the cost function, we utilize particular layer to compute the cost function. However, pratically if $l$ is small, for example, 1, then the generated image will be quite similar to your content image, but if it is too high, then the generated image will lose something contour or some interesting things of the content image. Hence, we usually selet a middel layer, rather than a too small or too deep layer. And then we also take advantage of the pre-trained neural network to implement the neural style transfer algorithm and VGG-19 neural network is used in this project. You can download the pre-trained neural model in: http://www.vlfeat.org/matconvnet/pretrained/. And the following figure is the architecture of the VGG-19 neural network [4].
<div align=center><img src = "https://www.researchgate.net/profile/Clifford_Yang/publication/325137356/figure/fig2/AS:670371271413777@1536840374533/llustration-of-the-network-architecture-of-VGG-19-model-conv-means-convolution-FC-means.jpg"></div>
We are not diving into the VGG-19 nerual network in this case.

## Result
<div align=left><img width=245, height=178, src = "Style-Transfer\style.jpg"> <img width=255, height=178, src = "Style-Transfer\test2.jpg"> <img width=255, height=178, src = "Style-Transfer\generated_image.jpg"></dir></br>
The left-most image is the style image: The Starry Night, from Van Gogh. And the middel one is "zhuhai" from [5]. The right-most one is the generated artical image from our nerual style transfer algorithm.



## Conclusion
This artical simply introduce the neural style transfer. We can learn that instead of updating the weight of the nerual network, it is target to update the values of the image while gradient descenting which is the same principle to minimize the cost function we defined above. In practica, the pre-trained nerual network is usually utilized to implement this project. The future work is to implement this algorithm in videos.




## References

[1] Neural style transfer, https://en.wikipedia.org/wiki/Neural_Style_Transfer</br>
[2] Andrew Ng, https://mooc.study.163.com/term/2001392030.htm</br>
[3] L. Gatys, A. Ecker and M. Bethge, "A Neural Algorithm of Artistic Style," Journal of Vision, vol. 16, (12), pp. 326, 2016.</br>
[4] VGG-19, https://www.researchgate.net/figure/llustration-of-the-network-architecture-of-VGG-19-model-conv-means-convolution-FC-means_fig2_325137356</br>
[5] zhuhai, https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1563285220753&di=bac7ad611650d54e5467b69c5db89a40&imgtype=0&src=http%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2F4oialo8ibP6okPtUN4Jj4DQFm8xenjibPHtYv6wVFaRicqorp3ticzqYShD64ibbIOBVuRdMzU3OnGMlgVLI89AXpHFA%2F640%3Fwx_fmt%3Djpeg
