Your First Image Classifier
===========================

KNN 算法的核心思想是用 **距离** 最近的 **K** 个样本数据的分来来代表目标数据的分类。

KNN 算法流程
------------

* 收集数据：确定训练样本集和测试集
* 计算测试集数据和训练样本中每个样本数据的距离
  
	常用距离计算公式(n 表示维度)：

	欧式距离
	
	.. math::
		d(p,q) = \sqrt{\sum_{i=1}^{N} (q_{i} - p_{i})^{2}}

	曼哈顿距离

	.. math::

		d(p,q) = \sum_{i=1}^{N} \vert q_{i} - p_{i} \vert

* 按照距离递增的顺序排列
* 选取距离最近的K个点
* 确定这k个点中分类信息的频率
* 返回前K个点中出现频率最高的分类，作为当前测试数据的分类

KNN 超参数
----------

k 
	k越小，分类边界曲线越光滑，偏差越小，方差越大，k小效率越高，但是会受到噪声和异常数据点的影响。k越大，分类边界曲线越平坦，偏差越大，方差越小。实际过程中，应该通过交叉验证的方法来确定k值。

distance
	可以选择欧式距离或曼哈顿距离等

KNN 实现
---------

	.. code:: python

		import torch
		import torchivision.transforms as transforms
		import argparse
		import time

		ap = argparse.ArgumentPaser()
		ap.add_argument("-d", "--dataset", required=True, 
			help="path to input dataset")
		ap.add_argument("-k", "--neighbors", required=True, 
			help="of nearest neighbors for classification")

		# transform
		transform = transforms.Compose(
			[transforms.Resize(32),
			transforms.ToTensor()])
		
		# load dataset
		dataset = torchvision.datasets.ImageFolder(args["dataset"], transform=transform)
		labels_dict = dataset.class_to_idx

		# split dataset
		train_size, test_size = int(len(dataset)*0.7), int(len(dataset)*0.3)
		trainset, testset = torch.utils.data.random_split(dataset, [train_size, valid_size])


		def KNN_test(test, train_x, train_y, k):
			
			since = time.time()

			# 将测试数据扩展成和训练集一样大小，以矩阵来计算
			m = len(train_x.size(0))
			test_x = torch.stack([test] * m)
			
			# 计算距离
			dists = torch.sum(((test_x-train_x)**2)**0.5, dim=1)

			# 按距离升序排序后选取前K个

			idxs = dists.argsort()[:k]
			# 获取对应的k个labels
			labels = list(train_y[idx] for idx in idxs)

			# 根据字典的value获取key
			new_dict = {v:k for k,v in labels_dict.items()}
			label = new_dict[max(labels, key=labels.count)]

			time_elapsed = time.time() -since

			print("Detected {} in {:.6f}s".format(label, time_elapsed))
			
		
		if __name__ == "__main__":
			train_x = []
			train_y = []

			for i in range(len(trainset)):
				img, target = trainset[i]
				train_x.append(img.view(-1))
				train_y.append(target)

			test_x = []
			test_y = []
			
			for i in range(len(testset)):
				img, target = testset[i]
				test_x.append(img.view(-1))
				test_y.append(target)

			KNN_test(test_x[0], torch.stack(train_x), train_y, args["neighbors"])


KNN 优缺点
----------

**优点**

* 简单易理解，无需训练，无需参数拟合

* 在样本本身区分度较高的时候效果会很不错

* 对异常值不明感

* 适合于多分类问题（对象具有多个标签）

**缺点**

* 在样本量大的时候，找出K个邻近点的计算代价大，内存开销大

* 可解释性差，无法告诉哪个变量更重要

* 样本不平衡问题，此时预测偏差会比较大

* 消极学习算法，懒惰算法




