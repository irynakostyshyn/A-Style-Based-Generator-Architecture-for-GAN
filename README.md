# Paper Review: A Style-Based Generator Architecture for Generative Adversarial Networks

<img src="./assets/photo5197499139783502838.jpg" width="250" align="right"/>

* **Title**: A Style-Based Generator Architecture for Generative Adversarial Networks
* **Authors**: Tero Karras, Samuli Laine, Timo Aila
* **[Link](https://arxiv.org/pdf/1812.04948.pdf)**
* **Tags**: Generative Adversarial Networks, Style-based generator, Faces Datasets
* **Year**: 2019

# Summary

* **What:**

  * The authors propose an alternative generator architecture for generative adversarial
   networks, borrowing from style transfer literature 
  * The new generator improves the state-of-the-art in terms
   of traditional distribution quality metrics
  * The authors propose two new, automated methods to quantify interpolation quality 
  and disentanglement, that are applicable to any generator architecture
  * The authors introduce a new, highly varied and high-quality dataset of
   human faces The (FFHQ, Flickr-Faces-HQ).

  * The authors proposed the way to control different features of the generated image and made
  the model more explainable. See theirs official video (https://www.youtube.com/watch?v=kSLJriaOumA&feature=youtu.be)
* **Method:**

The idea of creating Mapping Network with the transformed/inner latent space is used, instead of using input 
latent code. This mapping part consists of 8 MLP blocks(8 gave the best performance), which gives __new__ `W-space`
 latent code, that is passed to the __Generator__ as a style which consists of 18 conv layers(two for each resolution 4 ** 2 - 1024 ** 2).
 See [#Architecture] figure.
 
 Through the experimental process it was found out that starting image at the top of `g-Generator` can be learn 
 constant tensor instead of a real image. Following the architecture this image then in each block
  follows the next process. First conv layer. Then summed with _per channel scaled
 noise_ (scaling is learned as B-parameter). Then again conv. Then AdaIN block.
 
 AdaIN Block can be described the next way:
 ![AdaIN](assets/adain.png)
 where `x_i` is input per channel feature passed vertically from top and `(y_s_i, y_b_i)` are _style_ parameters
 got from transformation of `w-vector` from input `W` latent space. To get these style parameters this vector is 
 first passed through learned affine transformation. 
  
  **To sum it all up**, the main interesting points are: the input `4x4x512` tensor is always the same and we
  only get styles from our latent variable. Moreover they say that at each scale changing our _style_ inputs
  we can change different **very** localised parts of generated image (See video).
  
  > Copying the styles corresponding to coarse spatial resolutions (4 ** 2–8 ** 2) brings high-level aspects such as pose, general hair style, face shape, and eyeglasses from source B, while all colors(eyes, hair, lighting) and finer facial features resemble A. If we instead copy the styles of middle resolutions (16 ** 2–32 ** 2) from B, we inheritsmaller scale facial features, hair style, eyes open/closed from B, while the pose, general face shape, and eyeglasses from A are preserved.Finally, copying the fine styles (64 ** 2–1024 ** 2) 

By the way most of the architecture/training process is based on https://arxiv.org/pdf/1710.10196.pdf (Baseline configuration is the Progressive GAN). 
The changes made to architecture:
- The baseline using bilinear up/downsampling operations, longer training, and tuned hyperparameters
- Adding the mapping network and AdaIN operations
- Simplify the architecture by removing the traditional input layer and starting the image synthesis from a learned 4 × 4 × 512 constant tensor
- Adding the noise inputs
- Adding the mixing regularization(I didn't mention it. The idea is the following: in the learning process we don't 
take one latent variable, but take two. Then for one part of the network we feed as style first variable and then the other.
That way the model separate the influence and doesn't entangle it. At test time see [Mixing] figure below)

![Architecture](assets/photo5197353128075307948.jpg)

Figure _Architecture_
* **W** - an intermediate latent space
* **AdaIN** - adaptive *style* instance normalization
* **A** - learned affine transform
* **B** - learned per-channel scaling fac-tors to the noise input

![Mixture](assets/mixture.png)

_Figure Mixing_. Two sets of images were generated from their respective latent codes (sources A and B); the rest of the images were generated bycopying a specified subset of styles from source B and taking the rest from source A.


* **Proposed metrics:**
  
    - Perceptual path length:
    ![Path_length](assets/path_length.png)
    where `z1, z2` are latent variables; `t` is random value in `(0,1)` interval;
    `eps` = `1e-4`; `G` stands for generator; `slerp` - spherical 
    interpolation; `d(x1, x2)` - `l2(vgg(x1), vgg(x2))`;
    The idea is that for less entangled and informative latent code interpolation of latent-space 
    vectors should not yield surprisingly non-linear changes in the image. Quantative results in the Table 3.
    - Linear separability. Tries to find out the separability in the input
    latent code. Given the features of generated images as Male/Female, find out
    how informative for this info is input latent code.
    The idea is the next: given good dataset as CELEBA-HQ train good classifier of 
    a given feature; then generate images using our **Generator** and label using trained
    classifier so we have <latent variable, class of interest> mapping; then train SVM on this
    data and exponent of cross entropy of this model will be the score of Linear Separability.
    See table:
    ![Metrics](assets/metrics.png) 
* **Results:**

  * **Frechet inception distance (FID)**

![FID](assets/photo5195447889172737473.jpg)

 


