# Paper Review: A Style-Based Generator Architecture for Generative Adversarial Networks



* **Title**: A Style-Based Generator Architecture for Generative Adversarial Networks
* **Authors**: Tero Karras, Samuli Laine, Timo Aila
* **[Link](https://arxiv.org/pdf/1812.04948.pdf)**
* **Tags**: Generative Adversarial Networks, Style-based generator, Faces Datasets
* **Year**: 2019

# Summary

* **What:**

  * The authors propose an alternative generator architecture for generative adversarial networks, borrowing from style transfer literature 
  * The new generator improves the state-of-the-art in terms of traditional distribution quality
metrics, leads to demonstrably better interpolation properties, and also better disentangles the latent factors of variation. To quantify interpolation quality and disentanglement,
we propose two new, automated methods that are applicable to any generator architecture. Finally, we 
  * The authors introduce a new, highly varied and high-quality dataset of human faces The (FFHQ, Flickr-Faces-HQ).

* **How:**

  
  * compute the spatially invariant style y from vector w instead of an example image



* **Results:**

  * **Frechet inception distance (FID)**

![FID](assets/methods)

 



## Architecture
![Architecture](assets/architecture.jpg)
the mapping network f consists of 8 fully convolutional layers
the synthesis network g consists of 18 layers— two foreach resolution (4**2 − 1024**2).
The output of the last layer is converted to RGB using a separate 1 × 1 convolution

## Blocks description
* **W** - an intermediate latent space
* **AdaIN** - adaptive instance normalization (AdaIN(xi , y) = ys,i xi − µ(xi) σ(xi) + yb,i,)
* **A** - learned affine transform
* **B** - an uncorrelated Gaussian noise
