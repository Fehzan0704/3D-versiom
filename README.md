<h1 align="center"> Tire groove depth estimation </h1>

This repository contains a 3D computer vision task of estimating the depth of each groove in a tire given a point cloud of that tire, available in the file `data/depth-mono.xyz`.

<h1 align="center"> Tools used </h1>
<p align="center"> <a href="https://www.python.org" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" alt="python" width="40" height="40"/> </a> <a href="https://pandas.pydata.org/" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/2ae2a900d2f041da66e950e4d48052658d850630/icons/pandas/pandas-original.svg" alt="pandas" width="40" height="40"/> </a> <a href="https://scikit-learn.org/" target="_blank" rel="noreferrer"> <img src="https://upload.wikimedia.org/wikipedia/commons/0/05/Scikit_learn_logo_small.svg" alt="scikit_learn" width="40" height="40"/> </a> <a href="https://seaborn.pydata.org/" target="_blank" rel="noreferrer"> <img src="https://seaborn.pydata.org/_images/logo-mark-lightbg.svg" alt="seaborn" width="40" height="40"/> </a> </p>
<p align="center"> <a href="http://www.open3d.org/" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/isl-org/Open3D/master/docs/_static/open3d_logo_horizontal.png" alt="open3d" width="160" height="40"/> </a> </p>

The main code is scattered across four notebooks:

<h1 align="center"> 1 - Point cloud visualization </h1>

In the notebook `1-visualization.ipynb`, two simple plots are made to visualize the point cloud:
- Scatter plot
- Heatmap (given depth estimation data)

<h1 align="center"> 2 - Point cloud clustering </h1>

This notebook, `2-clustering.ipynb`, deals with clustering the tire along different segments. The goal of the clustering task is to separate the tread and each individual groove of the tire.

The following approach is used:
- Calculating the normal vectors of the tire surface
- Filter the sides of the grooves by selecting only datapoints with normals that are close to perpendicular to the tire's rotation axis
- Cluster the filtered point cloud, which is now well separated, using the DBSCAN clustering algorithm

<h1 align="center"> 3 - Shifted depth estimation </h1>

Notebook `3-tread-depth.ipynb` uses the clustered data to estimate a depth for each datapoint. This depth is not relative to the surface of the tire: it is relative to the point further away from the rotation axis in the point cloud.

It involves the following steps:
- Performing constrained optimization to estimate the tire's rotation axis vector
- Projecting datapoints to the rotation plane defined by the rotation axis vector
- Fitting a circle to that data to get the coordinates for the tire's rotation axis center
- Calculating the distance between the rotation center and each datapoint
- Subtracting the maximum distance by the distance of each datapoint

<h1 align="center"> 4 - Goove depth estimation </h1>

Notebook `4-grove-depth.ipynb` estimates the depth of each groove using the depth estimated from the previous notebook:

- Visualize the histogram of depths for the tread and each groove: they have peaks at different depths
- Estimate the peak different distribution by calculating the peak difference on different samples of the whole tread and groove data population
- Estimate the depth as the mean of the peak difference distribution and use quantile values according to a given signifance to estimate the uncertainty in the depth calculated