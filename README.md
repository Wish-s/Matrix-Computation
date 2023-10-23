# classification

# 准备
- 对UMT数据创建渔网点
- 提取渔网点x、y坐标和UMT值
- 导出属性数据（txt文件）

# 计算第一个矩阵：距离矩阵
- 导入
 ```
import numpy as np
import pandas as pd
from scipy.spatial.distance import pdist, squareform
```
- 读取txt文件
 ```
df=pd.read_csv('/public/home/ac73auhrcp/UMT/fishnet300_4.txt',delimiter=',',header='infer')
 ```
- 读取x、y坐标
 ```
dist=df[['x','y']]
 ```
- 使用pdist函数计算距离矩阵
 ```
distances = pdist(dist)
 ```
- 使用squareform函数将距离矩阵转换为方阵
 ```
distances = pdist(dist)
 ```
- 存储距离方阵
 ```
np.savetxt('/public/home/ac73auhrcp/UMT/result300_4.csv',distance_matrix, delimiter=',')
print("存储完成")
 ```

# 计算第二个矩阵：关系矩阵
在属性表中，UMT共有5个属性值：1表示紧凑高层建筑，2表示紧凑低层建筑，3表示开敞中层建筑，4表示稀疏建筑，5表示非城市区域。

在建立关系矩阵时，一共会出现25种情况，如下表所示：

![](https://github.com/Wish-s/classification/raw/master/relationship.jpg) 


**计算矩阵代码如下：**
- 导入
 ```
import numpy as np
import os 
```
- 读取txt文件
 ```
with open(r"/public/home/ac73auhrcp/UMT/fishnet300_41.txt","r",encoding='UTF-8')as f:
    point = f.readlines()
 ```
- 遍历将每一个属性值其存入空数组
 ```
with open(r"/public/home/ac73auhrcp/UMT/fishnet300_41.txt","r",encoding='UTF-8')as f:
    point = f.readlines()
points=[]
 ```
- 创建n（数组长度）维空矩阵
 ```
n=len(points)
rr=[[0]*n for z in range(n)]
 ```
- 计算关系值并存入刚刚创立的空矩阵
 ```
for i in range(len(points)):
    r1=int(points[i][4])
    for j in range(len(points)):
        r2=int(points[j][4])
        relationship = r1*10+r2
        rr[i][j]=relationship
 ```
- 计算关系值并存入刚刚创立的空矩阵
 ```
np.savetxt('/public/home/ac73auhrcp/UMT/relationship_3004.csv', rr, delimiter=',')
 ```
