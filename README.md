# Matrix Computation
# Background
In this project, we used urban morphology type data. <br><br>
The term ‘urban morphology’ refers to **the patterns of building structures in the urban settlements**.<br>
Referring to the definition of Local climate zones (LCZs) by Stewart and Oke (2012), in this study we define a UMT as **a built environment with a similar size and density of buildings**.<br><br>
The UMT data has five categories. **1 indicates Compact High-rise, 2 Compact Low-rise, 3 Open Mid-rise, 4 Sparsely Built, and 5 Non-urban Area.**<br><br>
We differentiate the urban morphology type into pixel scales. A resolution of 300 × 300 m was set to calculate the properties for UMT delineation.<br>
We aim to calculate **the distance matrix** between different pixels, as well as **the UMT type relationship matrix** between different pixels.<br>
# Preparation
- Create fishnet points for Urban Morphology Type (UMT) data.<br>
- Extract fishnet x, y coordinates and UMT values.
- Export property data (txt file).

  *In this project, the Distance text file does not contain the table header, while the Relationhip text file contains the table header.*
  
# UMTData Introduction
1.The UMT data: UMT2017.tif;
2.The boundary data: Boundary.shp;
3.The 300 × 300 m fishnet point data: Fishnet_point.shp.

# Compute the first matrix: the distance matrix.
- Import
 ```
import numpy as np
import pandas as pd
from scipy.spatial.distance import pdist, squareform
```
- Read the txt file
**(Modify the file path here)**
 ```
df=pd.read_csv('/public/home/ac73auhrcp/UMT/Distance.txt',delimiter=',',header='infer')
 ```
- Read the x and y coordinates
 ```
dist=df[['x','y']]
 ```
- The distance matrix is calculated using the pdist function
 ```
distances = pdist(dist)
 ```
- Convert the distance matrix to a square matrix using the squareform function
 ```
distances = pdist(dist)
 ```
- Store the distance matrix
 ```
np.savetxt('/public/home/ac73auhrcp/UMT/Result_Distance.csv',distance_matrix, delimiter=',')
print("Complete！")
 ```

# Compute the second matrix: the relationship matrix

The relationship matrix is used to express the attribute relationships between fishnet points. That is, to express **the relationships between a point and itself and all other points**.<br>

As mentioned above, the UMT data has 5 categories. When building the relationship matrix, **a total of 25 cases will occur**, as shown in the following table.

![](/relationship.jpg) 

**Calculation relationship matrix code is as follows**
- Import
 ```
import numpy as np
import os 
```
- Read the txt file
**(Modify the file path here)**
 ```
with open(r"/public/home/ac73auhrcp/UMT/Relationship.txt","r",encoding='UTF-8')as f:
    point = f.readlines()
 ```
- Iterates each property value into an empty array
 ```
points=[]
for p in point:
    data = p.strip().split(',')
    points.append(data)
 ```
- Creates an empty matrix with n (the array length) dimensions
 ```
n=len(points)
rr=[[0]*n for z in range(n)]
 ```
- The relational values are computed and stored in an empty matrix
 ```
for i in range(len(points)):
    r1=int(points[i][4])
    for j in range(len(points)):
        r2=int(points[j][4])
        relationship = r1*10+r2
        rr[i][j]=relationship
 ```
- Store the relationship matrix
 ```
np.savetxt('/public/home/ac73auhrcp/UMT/Result_relationship.csv', rr, delimiter=',')
 ```
