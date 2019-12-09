# Airbnb-
主要针对Airbnb平台新用户的预定结果进行预测

项目实现步骤：  

1.加载数据集，对数据集进行数据清洗（train和test数据结合处理），对部分离散数据进行one-hot-encoding，对部分连续数据进行标准化；  
对于session1（用户行为数据集），除了正常的数据清洗等，还进行了一些隐藏特征的挖掘，比如用户在airbnb平台活跃到正式注册所花的时间，从date_account_created中提取年月日等等；  
后期再根据user_id对数据集进行合并，得到更多的特征来拟合模型。  

2.筛选出重要特征用于训练模型；github上的开源库feature_selector  
项目地址：   
https://github.com/WillKoehrsen/feature-selector  
有关使用方法，请参阅Feature Selector的使用指南  
https://github.com/WillKoehrsen/feature-selector/blob/master/Feature%20Selector%20Usage.ipynb  

3.交叉验证数据集并用多个模型进行训练，得到结果比较及参数调优：LR,DT,RF,XGBoost等等  

结果 -> 未调参：DT          ->     train: 0.8114  test:0.8112  time:747s                                                                   
	       XGB         ->     train:0.8373   test:0.8259  time:598s  
	       LR          ->     train:0.8250   test: 0.8    time:48313s  
	       KNN         ->     train:0.8277   test:0.7762  time:5996s  
	       SVM-rbf     ->     train:0.8107   test:0.8068  time:53501s  
	       SVM-ploy    ->     train:0.8093   test:0.8068  time:48070s  
	       SVM-linear  ->     train:0.8070   test:0.8066  time:40195s  
	       RF          ->     train:0.8255   test: 0.8093 time:255s  
	       AdaBoost    ->     train: 0.8040  test: 0.8036 time:475s  
	       Bagging     ->     train:0.9953   test:0.8049  time:2187s  
	       ExtraTree   ->     train:0.8187   test:0.8056  time:267s  
	       GraBoost    ->     train:0.9493   test:0.8104  time:23726s  
	       xgboost.Sklearn -> train:0.8993   test:0.8048  time:5971s  
	       
	   
结果分析：1.由于此项目样本数据集较大，固KNN算法不适用于此建模，在这里使用KNN只单纯做出比较，实际里，KNN不适用于较大数据集（可分批次训练）；  
2.Bgging，GraBoost在训练集和测试集上面表现差异较大，固判断出现了过拟合，需样本数或降维来重新训练；  
3.总体来看原生XGBoost表现最佳，这也是竞赛中最常用的集成算法。  
