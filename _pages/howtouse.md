---
layout: page
permalink: /howtouse/
title: Howtouse
description: We will kindly guide you.
nav: False
nav_order: 1
---

***
### Installation

##### pip
The MGS can be install via pip install: 

```
pip install Mulguisin
```

##### git clone

The latest version of MGS source code can be download with: 

```
git clone https://github.com/Mulguisin/Mulguisin.git
```

***

### Basic usage

Public MGS is fully written by python. You can use it as normal python package.

At first, you must import MGS as `import Mulguisin`. Then, you have to create MGS class. At this time, we provide three options.

* MGS finds cluster/group based on density, the density order is important when MGS make cluster/group. If your data is already sorted by your criteria, you have to state as `mgs_init = Mulguisin.mulguisin_type('weight')`. If your data is not sorted by density or you want to follow our crieria, you have to state as `mgs_init = Mulguisin.mulguisin_type('voronoi')` (or 'local'). The 'voronoi' means that MGS calculate density based on 'Voronoi Tessellation'. If data point has small voronoi volume, it has high density. The 'local' means that MGS calculate density based on spherical method. In this case, you need another parameter 'radius'. This method get number of data points within 'radius'. Then, it calculate number density with defined sphere.

Second, you define 'cut length' (`Rcut`). This terminology is known as 'linking length' in astronomy. That is, MGS connects data points if distance between two points is lesser than 'cut length'.

Third, MGS finds cluster/group and it return following values, `Nmgs, imgs, clg, clm, cng`. We will explain these values in next section.

Below code shows how to use MGS with voronoi volume. This case is for 2D data, so that you have to define 'boundaries', because it need boundaries to calculate 'voronoi area' in 2D data.

```python
>>> import Mulguisin
>>> import numpy as np
>>> data = np.random.random((10,2))
>>> boundaries = [0,1,0,1]
>>> Rcut = 0.1
>>> mgs_init = Mulguisin.mulguisin_type('voronoi')
>>> MGS = mgs_init(Rcut,data[:,0],data[:,1],boundaries=boundaries)
>>> Nmgs, imgs, clg, clm, cng = MGS.get_mgs()
>>> Nmgs
9
```

***

### MGS values

Basically, MGS returns five values as Nmgs, imgs, clg, clm, cng. These are essential values generated by MGS, it also important values to estimate cluster/group information. Briefly, **Nmgs** is the number of clusters/groups, **imgs** is a label which have clustering information for each elements. The clg, clm and cng have clustering information which data point belongs to which MGS cluter/group. The **clg** have data point which compose each group. The **clm** have data point for mother, the mother means the ancestor of each data point in MGS structure. The **cng** have the number of member for each group.

These value are useful, but sometimes it can bother you. So, we make simple variable you can check clustering easily. The **label** contains information about which data belongs to the corresponding MGS. The label is (len(data), ) array and each value in the label indicates the each MGS group.  
e.g.) label = [0, 0, 0, 1, 2]. The first data point is belongs to the first largest MGS group. 

We made simple example how to use label for plotting. Below one plot MGS groups with different colors.

```python
>>> import matplotlib.pyplot as plt
>>> label = MGS.get_label()
>>> for i in range(Nmgs):
...   ids = np.where(label==i)[0]
...   plt.scatter(data[:,0][ids],data[:,1][ids],c='C%s'%i)
...
>>> plt.show()
```


##### *More examples are in the [Mulguisin github](https://github.com/Mulguisin/Mulguisin/tree/main/test) and [colab](https://colab.research.google.com/drive/1nbGBaHeSzA7o-qYLeXtMSfElz2zALRvx?usp=sharing ). We provide jupyter notebook in the link.*
###### Please make sure that you need to upload **rand_gal_data_210218_sig10.hdf5** to your colab file repository.
