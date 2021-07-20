+++
title = "image filtering"
author = ["felladog"]
date = 2021-05-28T17:47:00+05:45
lastmod = 2021-07-13T21:55:10+05:45
tags = ["convolution", "correlation", "filters", "gaussian", "median", "non-local", "bilateral", "BM3D"]
categories = ["image processing"]
draft = false
+++

---

-   References :
    -   Done Reading
    -   Reading
        -   <https://vincmazet.github.io/ftip/convolution.html>
        -   <https://nptel.ac.in/courses/106/106/106106224/> [IIT Madras]
        -   [Cornell CV Lecture on Filter](https://www.cs.cornell.edu/courses/cs5670/2020sp/lectures/lec01%5Ffilter.pdf)
        -   <https://szeliski.org/Book/> [Book Chapter 3]
        -   [Non-local means denoising for preserving textures](https://scikit-image.org/docs/dev/auto%5Fexamples/filters/plot%5Fnonlocal%5Fmeans.html)
    -   To read:
        -   <https://www.cs.tut.fi/~foi/papers/ICIP2019%5FYmir.pdf> [BM3D]
        -   <https://stackoverflow.com/questions/40596026/what-does-entropy-mean-in-this-context>
-   Questions :

---

Modify image pixels based on some function of a local neighbourhood of each pixel.


## Why filter a image {#why-filter-a-image}

-   To sharpen,
-   smooth,
-   intensify or
-   enhance a imag


## Linear filters {#linear-filters}

-   Output pixel’s value is determined as a weighted sum of input pixel values within a small neighborhood N,
    -   \\(g(i, j) =  \sum\_{k,l}{f(i + k, j + l)h(k, l)}\\)

-   The entries in the weight kernel or mask h(k, l) are often called the filter coefficients.
-   The above **correlation** operator can be more compactly notated as
    -   \\(g = f \otimes h\\)
-   Correlation can be used for [Template Matching]({{<relref "2021-07-05--14-03-49Z--object_detection.md" >}})

-   Variant of above formula is
    -   \\(g(i, j) =  \sum\_{k,l}{f(i - k, j - l)h(k, l)}\\)

-   where the sign of the offsets in f has been reversed, This is called the **convolution** operator,
    -   \\(g = f \ast h\\)
    -   Equivalent to flip the filter in both directions (bottom to top, right to left) and apply cross-correlation

-   h is called the impulse response function.

-   Difference between convolution and correlation operation
    -   h, convolved with an impulse signal, δ(i, j) _(an image that is 0 everywhere except at the origin)_ reproduces itself, h \* δ = h, whereas correlation produces the reflected signal.


### Gaussian Filter/Blur {#gaussian-filter-blur}

-   good for [denoising]({{<relref "2021-04-10--04-25-11Z--image_processing.md" >}}) normally/gaussian distributed noise

{{< figure src="/ox-hugo/2021-05-30_13-23-25_screenshot.png" caption="Figure 1: from cornell CV lecture" width="600" height="350" target="/blogs" >}}

```python
# img should be float so that operation works properly
g_img1 = cv2.GaussianBlur(img, (3,3), 0, borderType=cv2.BORDER_CONSTANT)

g_img2 = skimage.filters.gaussian(img, sigma=1, mode='constant', cval=0.0)
```


## Non linear filters {#non-linear-filters}


### [median]({{<relref "2021-05-29--04-28-46Z--statistics.md" >}}) filter {#median--2021-05-29-04-28-46z-statistics-dot-md--filter}

-   for denoising the salt and pepper noise
-   Replace each pixel with MEDIAN value of all pixels in neighbourhood
-   median calculation over the given filter size
-   Properties:
    -   Non-linear
    -   Does not spread noise
    -   Can remove spike noise
    -   Robust to outliers, but not good for Gaussian noise

{{< figure src="/ox-hugo/2021-05-29_18-01-48_screenshot.png" caption="Figure 2: from nptel [IIT Madras] CV lecture slide" width="450" height="250" target="/blogs" >}}

```python
from skimage.filters import median
from skimage.morphology import disk

median_cv = cv2.medianBlur(img, 3)

median_skimage = median(img, disk(3), mode='constant', cval=0.0)
```


### [entropy]({{<relref "2021-03-06--03-56-06Z--entropy.md" >}}) filter {#entropy--2021-03-06-03-56-06z-entropy-dot-md--filter}

The entropy filter can detect subtle variations in the local gray level distribution. It is usually used to classify textures, a certain texture might have a certain entropy as certain patterns repeat themselves in approximately certain ways.

```python
from skimage.filters.rank import entropy
from skimage.morphology import disk
entropy_img = entropy(img, disk(3))
```


### Bilateral filter {#bilateral-filter}

-   Edge perserving denoising filter.
-   Noise removal comes at expense of image blurring at edges.
-   Simple, non-linear edge-preserving smoothing.
-   Reject(in a soft manner) pixels whose values differ too much from the central pixel value(outlier rejection).
-   Output pixel value is weighted combination of neighboring pixel values:
    \\(g(i,j) = \frac{\sum\_{k,l}f(k,l) w(i,j,k,l)}{\sum\_{k,l}w(i,j,k,l)}\\)

{{< figure src="/ox-hugo/2021-05-29_17-56-31_screenshot.png" caption="Figure 3: from szeliski book, chapter 3" width="600" height="300" target="/blogs" >}}

-   \\(\sigma\_{d}\\) controls influence of the distant pixels.
-   \\(\sigma\_{r}\\) controls the influence of pixels with intensity value different from center pixel intensity.

{{< figure src="/ox-hugo/2021-05-29_17-57-56_screenshot.png" caption="Figure 4: from szeliski book, chapter 3" width="500" height="350" target="/blogs" >}}

```python
bilateral_img_cv = cv2.bilateralFilter(image,
				    d=5,
				    sigmaColor=20,
				    sigmaSpace=100,
				    borderType=cv2.BORDER_CONSTANT)

from skimage.restoration import denoise_bilateral
bilateral_img_sk  = denoise_bilateral(image,
				      win_size,
				      sigma_color=0.05,
				      sigma_spatial=15,
				      mode='constant',
				      cval=0,
				      multichannel=False)
```


### Non-local Means filter {#non-local-means-filter}

-   Estimated value is the weighted average of all pixels in the image but, the family of weights depend on the similarity between the pixels i and j.
-   Similar pixel neighborhoods give larger weights.
-   _The non-local means algorithm replaces the value of a pixel by an average of a selection of other pixels values: small patches centered on the other pixels are compared to the patch centered on the pixel of interest, and the average is performed only for pixels that have patches close to the current patch. As a result, this algorithm can restore well textures, that would be blurred by other denoising algorithm._

<!--listend-->

```python
"""
https://scikit-image.org/docs/dev/auto_examples/filters/plot_nonlocal_means.html
"""
from skimage.restoration import denoise_nl_means, estimate_sigma

sigma_est = np.mean(estimate_sigma(img, multichannel=False))
denoise_img = denoise_nl_means(img,
			       h=1.0*sigma_est,
			       fast_mode=True,
			       patch_size=5,
			       patch_distance=3,
			       multichannel=False)
```


### Total Variation filter {#total-variation-filter}

-   Signal with excessive spurious(counterfeit) detail have high total variation
-   Reducing the total variation of the signal, removes unwanted detail while preserving important details such as edges.
-   The result of this filter is an image that has a minimal total variation norm, while being as close to the initial image as possible.
-   The total variation is the L1 norm of the gradient of the image.
-   good for normally/gaussian distributed noise

    ```python
    from skimage.restoration import denoise_tv_chambolle
    denoise_img_tv = denoise_tv_chambolle(img,
    				  weight=0.1,
    				  eps=0.0002,
    				  n_iter_max=200,
    				  multichannel=True)
    ```


### BM3D (Block Matching and 3D filtering) {#bm3d--block-matching-and-3d-filtering}

-   Collaborative filtering process
-   Group of similar blocks extracted from the image.
-   A block is grouped if its dissimilarity with a reference fragment falls below a specified threshold - block matching.
-   All blocks in a group are then stacked together to form 3D cylinder- like shapes.
-   Filtering is done on every block group.
-   Linear transform is applied followed by wiener filtering, then transform is inverted to reproduce all filtered blocks.
-   Image transformed back to its 2D form.

<!--listend-->

```python
import bm3d
"""
 bm3d library is not well documented yet, but looking into source code....
 sigma_psd - noise standard deviation
 stage_arg: Determines whether to perform hard-thresholding or Wiener filtering.
 stage_arg = BM3DStages.HARD_THRESHOLDING or BM3DStages.ALL_STAGES (slow but powerful)
 All stages performs both hard thresholding and Wiener filtering.
"""
bm3d_denoised = dm3d.bm3d(noisy_img,
			  sigma_psd=0.2,
			  stage_arg=bm3d.BM3DStages.ALL_STAGES)
```


### anisotropic diffusion {#anisotropic-diffusion}


## Unsharp Mask {#unsharp-mask}

-   Sharped = Original Image + (Orginial - Gaussian Blurred Image) \* Amount

{{< figure src="/ox-hugo/2021-05-30_12-56-17_screenshot.png" caption="Figure 5: from cornell CV lecture" width="500" height="400" target="/blogs" >}}

{{< figure src="/ox-hugo/2021-05-30_13-01-41_screenshot.png" caption="Figure 6: from cornell CV lecture" width="650" height="300" target="/blogs" >}}

{{< figure src="/ox-hugo/2021-05-30_13-13-43_screenshot.png" caption="Figure 7: from cornell CV lecture" width="650" height="350" target="/blogs" >}}

```python
from skimage.filters import unsharp_mask
unsharped_img = unsharp_mask(img, radius=3, amount=1.0)
```


## [Edge Filters]({{<relref "2021-05-31--03-54-38Z--edge_filters.md" >}}) {#edge-filters--2021-05-31-03-54-38z-edge-filters-dot-md}


## Ridge filters {#ridge-filters}

-   for detecting ridges in the image
-   In skimage

    -   meijering
    -   sato
    -   frangi
    -   hessian

    <!--listend-->

    ```python
    from skimage.filters import meijering, sato, frangi, hessian
    meijering_img = meijering(img)
    sato_img  = sato(meijering)
    frangi_img= frangi(img)
    hessian_img = hessian(img)
    ```


## Low and High Pass filters {#low-and-high-pass-filters}


### Low-Pass Filters: {#low-pass-filters}

-   Filters that allow low frequencies to pass through (block high frequencies).
-   Example: Gaussian filter


### High-Pass Filters: {#high-pass-filters}

-   Filters that allow high frequencies to pass through (block low frequencies).
-   Example: Edge filter


## Discrete Fourier Transform {#discrete-fourier-transform}

-   [dft\_in\_image]({{<relref "2021-07-04--14-06-19Z--dft_in_image.md" >}})


## In python {#in-python}

```python
import cv2
# img dtype should be checked so that operation works properly
# applying filter using opencv
convolved_image = cv2.filter2D(img, -1, kernel, borderType=cv2.BORDER_CONSTANT)
```