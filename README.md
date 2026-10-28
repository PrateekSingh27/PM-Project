# Image Segmentation Using K-Means Clustering with Optimal Cluster Selection

A Python-based image segmentation project that applies **K-Means
clustering** to group image pixels by their RGB colour values. The
notebook evaluates candidate cluster counts and selects one using a
combined score based on the **Silhouette Score** and
**Calinski--Harabasz Index**.

## Project Overview

Images can contain thousands of colours, which can make visual analysis
and processing more complex. This project simplifies an image by
replacing each pixel with the representative colour of its assigned
cluster.

The workflow: 1. Upload an image and convert it to RGB format. 2. Resize
the image proportionally when its longest dimension exceeds 500 pixels.
3. Reshape pixels into RGB feature vectors and normalize values to the
range `0–1`. 4. Evaluate cluster counts from `K = 2` through `K = 10`.
5. Calculate inertia (WCSS), Silhouette Score, and Calinski--Harabasz
Index for each candidate. 6. Normalize the Silhouette and
Calinski--Harabasz scores and combine them with equal weights. 7. Run
K-Means with the selected cluster count. 8. Display the original and
segmented images, show individual cluster masks, and save the segmented
image.

## Features

-   Image upload through Google Colab.
-   RGB image preprocessing with Pillow.
-   Proportional resizing to a maximum dimension of 500 pixels.
-   K-Means clustering using Scikit-learn.
-   Evaluation of nine candidate cluster counts (`2–10`).
-   Elbow, Silhouette, and Calinski--Harabasz evaluation plots.
-   Combined-score-based cluster selection.
-   Side-by-side original and segmented image visualization.
-   Grayscale masks for individual clusters.
-   Export of the segmented image as `segmented_image.png`.
-   Display of the RGB centroid colour for every final cluster.

## Technology Stack

  Technology / Library   Purpose
  ---------------------- ------------------------------------------------------
  Python                 Main programming language
  NumPy                  Pixel arrays and numerical operations
  Matplotlib             Evaluation graphs and image visualization
  Pillow (PIL)           Image loading, RGB conversion, resizing, and saving
  Scikit-learn           K-Means clustering and clustering evaluation metrics
  Google Colab           Notebook execution and image upload/download

## Getting Started

### Option 1: Run in Google Colab

The notebook uses `google.colab.files` to upload the input image and
download the generated result, so **Google Colab is the simplest way to
run it**.

1.  Open `PM_PROJECT.ipynb` in Google Colab.
2.  Run the notebook cells in order.
3.  When prompted, upload an image file.
4.  Wait for the evaluation metrics and plots to be generated.
5.  View the original-versus-segmented comparison and cluster masks.
6.  Download `segmented_image.png` when prompted.

### Option 2: Install the dependencies

In a Python environment, install the core libraries:

``` bash
pip install numpy matplotlib pillow scikit-learn
```

The current notebook also imports `google.colab.files`. If you run it
outside Google Colab, replace the upload and download calls with local
file paths and ordinary file saving.

## How Cluster Selection Works

For each candidate `K` from 2 to 10, the notebook records:

-   **Inertia / WCSS:** Measures within-cluster squared distances. It
    generally decreases as `K` increases and is visualized using the
    Elbow Method.
-   **Silhouette Score:** Evaluates how well-separated the clusters are.
    Higher values generally indicate better separation.
-   **Calinski--Harabasz Index:** Compares between-cluster dispersion
    with within-cluster dispersion. Higher values generally indicate
    more compact and separated clusters.

The Silhouette and Calinski--Harabasz scores are each min--max
normalized across the tested values of `K`. The notebook calculates:

``` text
combined_score = 0.5 * silhouette_norm + 0.5 * calinski_norm
```

The candidate with the highest combined score is selected as `best_k`.
The Elbow plot is used for visual interpretation and does not directly
contribute to the combined score.

> **Important:** The selected value is the best candidate according to
> this scoring rule and the tested range, not a guarantee of globally
> optimal segmentation. The current normalization also assumes each
> metric varies across the tested values; if a metric is constant, its
> normalization denominator becomes zero and should be handled.

## Output

When the notebook runs successfully, it produces:

-   Printed image dimensions.
-   Inertia, Silhouette Score, and Calinski--Harabasz Index for every
    tested `K`.
-   Three evaluation plots:
    -   Elbow Method
    -   Silhouette Score vs. K
    -   Calinski--Harabasz Index vs. K
-   The selected cluster count (`best_k`).
-   A visualization comparing the original and segmented images.
-   Individual cluster masks.
-   RGB centroid colours for the final clusters.
-   `segmented_image.png` --- the saved segmented image.

The exact metrics, selected `K`, and appearance of the output depend on
the uploaded image and should be taken from the notebook's actual run.

## Repository Structure

``` text
.
├── PM_PROJECT.ipynb          # Main implementation notebook
├── PATTERN MATCHING REPORT.pdf  # Project report supplied with the project
└── README.md                 # Project documentation
```

The PDF report title and notebook contents describe **image segmentation
using K-Means clustering**. The filename `PATTERN MATCHING REPORT.pdf`
is retained as supplied.

## Limitations

-   Clustering uses RGB colour values only; pixel position, texture, and
    semantic meaning are not included.
-   Similar colours in different parts of the image may be assigned to
    the same cluster.
-   Candidate cluster counts are limited to `K = 2–10`.
-   Testing several values of `K` can be computationally expensive.
-   The Silhouette Score uses a sample of up to 10,000 pixels, while
    K-Means and the Calinski--Harabasz calculation process the full
    pixel dataset.
-   Colour simplification can remove subtle details or boundaries.
-   The combined score gives equal weight to the normalized Silhouette
    and Calinski--Harabasz scores; this is a project-level heuristic.

## Future Enhancements

-   Add pixel coordinates to encourage spatially coherent segmentation.
-   Experiment with HSV or CIELAB colour spaces.
-   Compare K-Means with algorithms such as Gaussian Mixture Models,
    Mean Shift, DBSCAN, or spectral clustering.
-   Improve cluster-count selection and handle normalization edge cases.
-   Use sampling or MiniBatchKMeans for larger images.
-   Build an interactive interface for uploading images and adjusting
    parameters.
-   Evaluate segmentation against ground-truth masks when available,
    using metrics such as Intersection over Union (IoU) and Dice
    coefficient.

## References

1.  J. MacQueen, "Some methods for classification and analysis of
    multivariate observations," *Proceedings of the Fifth Berkeley
    Symposium on Mathematical Statistics and Probability*, 1967.
2.  P. J. Rousseeuw, "Silhouettes: A graphical aid to the interpretation
    and validation of cluster analysis," *Journal of Computational and
    Applied Mathematics*, 1987.
3.  T. Caliński and J. Harabasz, "A dendrite method for cluster
    analysis," *Communications in Statistics*, 1974.
4.  D. Arthur and S. Vassilvitskii, "k-means++: The advantages of
    careful seeding," *Proceedings of the Eighteenth Annual ACM-SIAM
    Symposium on Discrete Algorithms*, 2007.
5.  F. Pedregosa et al., "Scikit-learn: Machine Learning in Python,"
    *Journal of Machine Learning Research*, 2011.
6.  NumPy documentation: https://numpy.org/doc/
7.  Matplotlib documentation: https://matplotlib.org/
8.  Pillow documentation: https://pillow.readthedocs.io/
9.  Scikit-learn clustering documentation:
    https://scikit-learn.org/stable/modules/clustering.html

------------------------------------------------------------------------

**Project:** Image Segmentation Using K-Means Clustering with Optimal
Cluster Selection\
**Authors listed in the supplied report:** Prawahan Katyal and Prateek
Singh\
**Institution listed in the supplied report:** Manav Rachna
International Institute of Research and Studies, Faridabad
