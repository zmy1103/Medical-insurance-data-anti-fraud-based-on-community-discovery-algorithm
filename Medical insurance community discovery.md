# Medical insurance community discovery

总体思路阐述：

## 图数据的构建

![](https://tva3.sinaimg.cn/large/005IQUPRly1gjwrhvi0ddj31cc0r4acz.jpg)

- ### 第一类图：药店参保人之间时间关系（三套图)

  - 节点：19年有过药店报销行为得参保人

  - 边：每两个参保人之间在同一天买药的天数

  - 三个相似性行为考量：

    - **同时消费，但不在同家药店**（只考虑时间，不考虑地点，因为团伙可能特地分散开，可能在不同地点同时作案）全部数据：374667 节点数：135278  边数：629226114（边数太多，可以放弃这种方案）

    - **同家药店同时消费**（既考虑时间也考虑地点）

      - 节点数：135278  边数：7087001

    - **只在同一家药店作案（以每家药店为单位构建图去考量**）

      - 药店数（图数）： 184个

      - | Hospital_id | 节点数 | 边数   |
        | ----------- | ------ | ------ |
        | 9186        | 1046   | 15106  |
        | 9003        | 4223   | 223084 |
        | 9081        | 2369   | 147798 |

- ![](https://tvax1.sinaimg.cn/large/005IQUPRly1gjxxw8n456j31cy0rkduf.jpg)

  - 选择hospital_id为9100的药店，节点数为35，边数为38
  - ![](https://tvax4.sinaimg.cn/large/005IQUPRly1gjy5w1otwyj30df08ajrt.jpg)

  去掉度数为0得边，进行社区发现

- ### 第二张图：药店参保人之间的药品关系

## 多种社区发现算法介绍及评价指标

| 算法名称            | 简要介绍 | 参考文献                                                     | 优势 | 劣势 |
| ------------------- | -------- | ------------------------------------------------------------ | ---- | ---- |
| GN(Girvan&Newman)   |          | [《Community structure in social and biological networks》](https://arxiv.org/abs/cond-mat/0112110) |      |      |
| Spectral Clustering |          | [《A tutorial on spectral clustering》](https://arxiv.org/abs/0711.0189) |      |      |
| CPM                 |          |                                                              |      |      |
| Louvain             |          |                                                              |      |      |



- ### GN

  - 算法介绍

  两个重要概念的定义：

  - 边介数：所有节点对之间的最短路径中经过该边的最短路径数，边介数大的边往往是连接不同community之间的边。
  - 模块度：目前常用的一种衡量网络社区结构强度的方法，可以用来定量的衡量网络社区划分质量，其值越接近1，表示网络划分出的社区结构的强度越强，也就是划分质量越好。 因此可以通过最大化模块度Q来获得最优的网络社区划分。
  - GN算法伪代码如下：

  ```
   1) 计算当前网络的边介数和模块度Q值，并存储模块度Q值和当前网络中社团分割情况；
   2) 除去边介数最高的边；
   3) 计算当前网络的模块度Q值，如果此Q值比原来的大，则将现在的Q值和网络中社团分割情况存储更新，否则，进行下一次网络分割；
   4) 所有边分割完毕，返回当前的模块度Q值和community分割情况。
  ```

  GN算法复杂度较高，每次寻求最佳社区分块时，都需要计算每个点之间的最短路径。 不过GN算法也考虑了边的权重（根据模块度进行更新），所以当处理更为复杂网络的时候，GN算法比k-clique算法更为有效。

  - 实施方案
  - 代码实现

- ### Spectral Clustering

- ### CPM

- ### Louvain

  - 效果

  从实验效果中看，Louvain 算法比 clique 渗透算法性能要好，划分准确度更高，重要的原因是 Louvain 算法考虑进了 edge 的权重， 但是也有不足的地方，就是这些算法都没有考虑有向图的情况。 但是经过反思，虽然使用算法之前，都将有向图转换为了无向图，但是影响不会很大，因为网络B是根据相同的发送IP进行构建的，虽然是无向图， 但是这个发送IP的属性其实已经暗含了方向性，所以最后的结果也是不错的。

- 评价指标

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |



## 基于图神经网络的社区发现算法

## 基于图神经网络的社区发现算法实践

## 结果分析