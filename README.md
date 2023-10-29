# Classification

# Preparation
- Create fishnet points for UMT data.
- Extract fishnet x, y coordinates and UMT values.
- Export property data (txt file).

  *In this project, the Distance text file does not contain the table header, while the Relationhip text file contains the table header.*

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
np.savetxt('/public/home/ac73auhrcp/UMT/result300_4.csv',distance_matrix, delimiter=',')
print("Complete！")
 ```

# Compute the second matrix: the relationship matrix
In the property table, the UMT column has a total of five property values.

**1 indicates Compact High-rise,2 Compact Low-rise, 3 Open Mid-rise, 4 Sparsely Built, and 5 Non-urban Area.**

When building the relationship matrix, a total of 25 cases will occur, as shown in the following table.

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
np.savetxt('/public/home/ac73auhrcp/UMT/relationship_3004.csv', rr, delimiter=',')
 ```
