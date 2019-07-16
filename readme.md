

## Overview
Neural Style Transfer (NST) refers to a class of software algorithms that manipulate digital images, or videos, to adopt the appearance or visual style of another image. NST algorithms are characterized by their use of deep neural networks in order to perform the image transformation. Common uses for NST are the creation of artificial artwork from photographs, for example by transferring the appearance of famous paintings to user supplied photographs. [1] In this project, we are going to implement this algorithm and it is also the assignment in the deep learning AI course delivered by Andrew Ng [2].

Repo:
<!--more-->

## Objective
In this example, we are going to generate an image of the Louvre museum in Paris (content image C), mixed with a painting by Claude Monet, a leader of the impressionist movement (style image S) [2]. This is an expected result of our implementataion.

<div align=center><img src = "Style-Transfer\objective.jpg"></div></br>

## Methodology
For any neural model, it definitely has the cost function. Therefore, we define the cost function at the begining.
### Cost function
We have two main parts of the cost function[3]:
- $J_{content}(C,G)$: this one is so-called content cost, which is related to the originial image(content image) and the generated image and is used to measure the similarity between the generated image and the content image.
- $J_{style}(S,G)$: this one is named as style cost, realting to the style image and the generated image, which is target to value the similarity between the style image and the generated image.

And then finally we put them together, so that we have:
$$J(G)=\alpha J_{content}(C,G) + \beta J_{content}(S,G)$$
$\alpha$ and $\beta$ are the hyperparameters.

In order to generate a new image, we firstly need to randomly inital an image $G$ and then using gradient descent to reduce above cost function so that we can updating the
$$
G := G-\frac{\alpha}{\alpha G}J(G)
$$
This step is acutally updating values of image G, rather than the weight by convention in classification we did before. So the above is an abstract of neural style transfer. Now we dive more into the cost function and elaborate the hidden principle.

#### Content cost function
Now we want to test the similarity between the content image and the generated image. To achieve that, we suppose we have two activation values:$a^{[l][C]}$ and $a^{[l][G]}$. We would say if these two values are close to each other, then the image is similar to each other. So we define:
$$
J_{content}=\frac{1}{2}||a^{[l][C]}-a^{[l][G]}||^2
$$
It means that L2 norm of the difference of the values of particular $l$ layer of different images can represent the content similarity of two images.

#### Style cost function
We suppose that $a_{i,j,k}^{[l]}$ is  the activation value of the $l$ layer and $i,j,k$ represent height, width, and the number of channels. Now we are going to construct a style matrix, $G_{kk^{'}}^{[l][S]}$, which is the $n_c$ by $ n_c$ matrix for style image.
Gram matrix (style matrix):
$$
G_{kk^{'}}^{[l][S]}=\sum_{i=1}^{n_H^{[l]}}\sum_{j=1}^{n_W^{[l]}}a_{i,j,k}^{[l][S]}a_{i,j,k^{'}}^{[l][S]}$$
$k,k^{'}$ here denote the coefficient of association between $k$ channel and the $k{'}$ channel. So we just multiply these two activation value of the same position in corresponding channels and then $i$ and $j$ increase by the height and width respectively, which are $n_H^{[l]}$ and $n_W^{[l]}$
Similarly, we do the same operation for the generated image, so that we have:
$$
G_{kk^{'}}^{[l][G]}=\sum_{i=1}^{n_H^{[l]}}\sum_{j=1}^{n_W^{[l]}}a_{i,j,k}^{[l][G]}a_{i,j,k^{'}}^{[l][G]}
$$
Then  we put them together to set up the style cost function:
$$
J_{style}(S,G)=\frac{1}{(2*n_H^l n_W^l n_c^l)^2}\sum_k \sum_{k^{'}}{||G_{kk^{'}}^{[l][S]}-G_{kk^{'}}^{[l][G]}||}_{F}^2
$$
So far we have captured the style from only one layer. We'll get better results if we "merge" style costs from several different layers. So we can have style weight matrix and then combine the style costs for different layers as follows:
$$
J_{style}(S,G) = \sum_{l} \lambda^{[l]} J^{[l]}_{style}(S,G)
$$

Right now we can compute the total cost function by using gradient descent method.

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

[1] Neural style transfer, https://en.wikipedia.org/wiki/Neural_Style_Transfer
[2] Andrew Ng, https://mooc.study.163.com/term/2001392030.htm
[3] L. Gatys, A. Ecker and M. Bethge, "A Neural Algorithm of Artistic Style," Journal of Vision, vol. 16, (12), pp. 326, 2016.
[4] VGG-19, https://www.researchgate.net/figure/llustration-of-the-network-architecture-of-VGG-19-model-conv-means-convolution-FC-means_fig2_325137356
[5] zhuhai, https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1563285220753&di=bac7ad611650d54e5467b69c5db89a40&imgtype=0&src=http%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2F4oialo8ibP6okPtUN4Jj4DQFm8xenjibPHtYv6wVFaRicqorp3ticzqYShD64ibbIOBVuRdMzU3OnGMlgVLI89AXpHFA%2F640%3Fwx_fmt%3Djpeg
