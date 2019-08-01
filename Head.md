## Introduction

作为虚拟现实技术的热门应用，虚拟试衣通常要求拍摄者借助虚拟影像设备，任意切换着装，并得到立体可视化预览。基于 3D 扫描的人体动态重建技术越发成熟[xxx]，高精度的采集设备也日益普及。[xxx][xxx]中介绍的的布料动态仿真技术，能够模拟服装在动态人体中的物理特性，使得这项应用的展示效果更加接近真实。同时，[xxx]中介绍的人体体型还原算法，能精准的预测衣服下方人体体型，从而辅助尺寸选择和增强衣物仿真效果。 但衣服模型的重建与分离却少有研究与进步。目前服装模型的生成主要依赖于手工制作，但这种方法效率较低，同时制作细节教差，难以匹配人体重建、布料动态仿真等技术的真实性。此外，在一些人体动态重建场景中，拍摄者被要求穿着较为紧身单薄的衣服进行拍摄，后期加入制作好的衣服模型，进行布料仿真，还原衣物的动态真实性。此类方法流程并不直接，其次后期处理难度较大。因此，在人体模型采集、生成的同时，分离出服装模型的技术尤为重要，它能简化拍摄工序，结合人体体型估计算法[xxx]和布料动态仿真技术[xxx][xxx]，一次拍摄后，即产生高真实度的动态人体模型。它也能应用于服装模型的采集与生成中，对穿好待采集服装的假人模特进行衣着面片的抠取，批量获取衣服模型。此外，[xxx]介绍的衣物 fitting 方法能够复用分割得到的服装模型，使得采集得到的大量服装模型能够得到更广泛的应用。

现有的算法鲜有直接讨论将三维人体与衣服进行分割。较为接近的有[xxx]中介绍的对衣服模型进行上下衣及其相关部位(如裙摆、长袖)的分割，但不足以解决上述提到的问题。此外，[xxx]介绍了用于 fashion 图片数据集的时装抠取算法，但未扩展至 3D 模型的分割。现存的大量 3D 分割算法，一般应用于 mesh 分割和点云分割。Mesh 分割方法，如[xxx][xxx][xxx][xxx]，涵盖面过于宽泛，并不专注于人体与服装的切割，对细节、噪音较多，衣着复杂、人体姿态变化较大的模型难以达到理想效果；点云切割算法，如[xxx][xxx]，能够较好的进行细节识别，全局优化，但多应用于室内点云图的分割与分类。

我们更多地参考了 mesh 分割方法，发现其对模型各部位的分割通常基于对几何、拓扑、基数等特征的识别。衣物与肢体在几何、拓扑甚至是纹理等方面均存在大小差异，因此我们采用了数据驱动的学习算法，融入更多人体、服装特有的特征量，拟合这种差异，实现服装与衣物的分离，即在已有的基于学习的 mesh 分割算法上，加入纹理、骨骼等人体特有特征值。我们所介绍的算法与 [xxx] 有着相似之处，但我们利用 Dome 系统采集得到了大量的人体服装数据集，加入 并应用 Graph-Net 网络作为学习模型。相比 [xxx] 数据集的

## Related Work

关于三维分割的方法较多，早期 Rashad et al. 阐述的分割方法结合基数(Cardinality)、几何(Geometrical)、拓扑(Topological)等限制条件，和平坦度(Planarity)、曲率(Curvature)、测地距离(Geodesic distances)等特征值，通过区域增长(Region Growing)、聚类(Clustering)和图割(Graph Cut)等优化算法得到分割结果[xxx]。Kalogerakis 利用条件随机场(CRF)，将几何、拓扑作为特征进行JointBoost监督学习[xxx]。该方法的学习/预测过程分两步进行，第一步利用上述单元特征(Unary feature)获得初次分类结果，第二步利用第一步的分类结果计算二元特征(Pairwise feature)，并与单元特征(Unary feature)结合，再次得到分类结果。