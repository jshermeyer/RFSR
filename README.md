# Random Forest Super-Resolution (RFSR)
![Alt text](/Examples/RFSR/Plane/60cm_Native.png | width=250)![Alt text](/Examples/RFSR/Plane/30cm_2x_RFSR.png | width=250)

RFSR is an adaptation and major twist on other random-forest super-resolution techniques such as SRRF by [Schulter et al.](https://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Schulter_Fast_and_Accurate_2015_CVPR_paper.pdf)  Our method uses a random forest regressor with a few simple standard parameters.  All parameters were finely tweaked using empirical testing to maximize PSNR scores while maintaining minimal training time (4 hours or less per level of enhancement using 200 million pixels on a 64GB RAM CPU).  The technique is trained only using the luminance component from a YCbCr converted image.

HR images are degraded to create LR and HR image pairs. The degraded LR image is then shifted by a set dimension in each direction versus the HR image. Each unique shift is stored and then compressed into a 3-dimensional array with a stack of uniquely shifted LR images.  All images are then padded with zeros using a two-pixel width to minimize edge effects.  The original up-sampled LR image is then subtracted from the 3-dimensional LR stack, and from the HR image for a residual training schema.  This normalizes the LR stack and HR image pair and also removes homogenous areas, emphasizing important edge effects.  After training and inference the interpolated LR image is then added back to the output image from the model to create the super-resolved output.  

After the LR image stack and corresponding HR images are created they are randomly sampled at a 10% rate for training.  Based on empirical observation, this sampling removes a significant amount of redundant data and enhances training speed with little to no recognizable affect on performance.  RFSR can only produce one level of enhancement at a time and is not built for concurrent training across multiple levels of enhancement. Average training time per level of enhancement on ~200 million pixel examples (10% sampled) is 3.6 hours.  Average inference speed on a 544x544 pixel image is 0.7 seconds. 


### For more information, see:
1. Initial blog on Super-Resolution: TBA November 2019

2. arXiv paper: TBA November 2019


____
## Running RFSR

____

### 0. Installation
Python 2.7+, GDAL, scikit-learn, scipy, numpy, opencv, and Jupyter Notebooks are the baseline requirements. All are listed in Requirements.txt and are easily installable via pip or conda.


    pip install -r Requirements.txt
    
### 1. Train, Test, and Infer
Simply launch a jupyter notebook instance and open the notebook.  Follow the instructions cell by cell and initialize all functions.  RFSR features the ability to train new models, test performance on various datasets, and run inference to enhance coarser imagery based on a set user scale. Sample data from the [xView dataset](https://arxiv.org/abs/1802.07856) is provided.

### 2. Expand
Feel free to modify the code, work with new data, and try to improve your performance.

### 3. Other Resources

Check out our 8-bit conversion code. -TBA
[SpaceNet Utilities](https://github.com/SpaceNetChallenge/utilities) is a recommended toolkit for working with geospatial data and deep learning
[Very Deep Super Resolution (VDSR) 4 Geospatial (VDSR4GEO)](github.com/jshermeyer/VDSR4Geo) is a partner repository and also used in the the arXiv paper listed above.
