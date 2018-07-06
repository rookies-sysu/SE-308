### 2.x 软件设计技术

#### 2.x.1 Objected-Oriented Programming

我们项目后台逻辑层部分，将数据库模型中的每一张表的操作分别封装成一个类，实现面向对象，使得逻辑层对表进行操作时，无需考虑 sql 语句，只需调用类中所提供的增删改查操作，即可完成对于数据库的操作。

详见下图：
```shell
dbOperators.py
├─ restaurantOperator
├─ dishTypeOperator
├─ tableOperator
├─ QRlinkOperator
├─ dishOperator
├─ dishCommentOperator
├─ orderListOperator
├─ RecommendationOperator
└─ RecommendationDetailsOperators
```

**Code:** `https://github.com/rookies-sysu/Order-System-Backend/blob/dbpool/app/dbTools/dbOperators.py`

![](https://ws1.sinaimg.cn/large/3d32c419gy1fsz0ize8rbj20rt0p0jtj.jpg)



#### 2.x.2 Database Connection Pool

在后台与前端对接之后，发现频繁地打开和关闭连接会对性能造成很大的影响，而且之前假设的情况是接受的请求都是同步的，但是前端可能发送异步请求，当两个请求同时到达时，甚至会导致数据库故障。 基于以上原因，就不得不使得我们去考虑使用数据库连接池的方法。 

数据库连接池的基本原理：最初，打开一定数量的数据库连接**(最小连接数)**，当收到调用者的调用请求之后，分配给调用者，在调用完毕之后将连接返回到连接池（返回到连接池的连接不会关闭，而是为下一次的调用而分配）。 

数据库连接池**参数设置**：

```shell
DB_MIN_CACHED=3
DB_MAX_CACHED=3
DB_MAX_SHARED=10
DB_MAX_CONNECYIONS=20
DB_BLOCKING=True
DB_MAX_USAGE=0
DB_SET_SESSION=None
```
**Code:** `https://github.com/rookies-sysu/Order-System-Backend/blob/dbpool/app/dbTools/dbConfig.py`

![](https://ws1.sinaimg.cn/large/3d32c419gy1fsz0xmfcl0j20tw0dd74x.jpg)

将数据库连接池封装成类 `DBPool`:

**Code:** `https://github.com/rookies-sysu/Order-System-Backend/blob/dbpool/app/dbTools/dbPool.py`

![](https://ws1.sinaimg.cn/large/3d32c419gy1fsz1bt82hxj20sb0jbjs7.jpg)

关于数据库连接池的使用，我们将其作为`Tools`类的函数：

**Code:** `https://github.com/rookies-sysu/Order-System-Backend/blob/dbpool/app/dbTools/tools.py`

![](https://ws1.sinaimg.cn/large/3d32c419gy1fsz1dhghj2j20ry0jvmxu.jpg) 