# -*- coding: utf-8 -*-
"""
Created on Tue Sep 29 21:25:17 2020

@author: xiangguchn
"""

百度AI竞赛：问答摘要与推理

第一步：数据预处理：
    preprocess.py将原始数据"AutoMaster_TrainSet.csv"与"AutoMaster_Test.csv"处理成"train_set.seg_x.txt", "train_set.seg_y.txt"和"test_set.seg_x.txt"。其中涉及到清理异常值，删除特殊字符，将训练数据集分成输入x和输出y，测试集将只有输入x。
    处理完的数据会被保存在上述三个txt文件中。


第二步：构建词典：
    通过vocab_build.py构建词典并保存在vocab.txt中，单词以词频排序，词频大的排在前面，词频小的排在后面。
    没什么大问题



第三步：建立词向量：
    build_w2v.py将train_set.seg_x.txt, train_set.seg_y.txt以及test_set.seg_x.txt作为预料，训练得到词向量w2v.bin。
    词向量大小设置为256维，采用skip-gram，窗口为5 
    算法迭代次数默认是iter=5, 尝试了将迭代次数设置为10，得到的词向量相似度没有较大改变。
    得到的词向量中不包含“技师”等多个字组成的词，这不影响后续的训练。
    词向量的训练结果显示，‘技’与‘师’的相似度similarity为0.44。
    比较了CBOW与skip-gram，得到的‘技’与‘师’的相似度差别不大
    保存词向量时binary=False，保存的词向量不会是乱码
    解决了只有字向量的问题，预料要从文件中读取，而不是list向量：w2v = Word2Vec(corpus_file=sentence_path, size=256, window=5, sg=1, min_count=5)
    使用pickle.dump保存的词向量word2vec.txt打开时出现乱码，不知道原因。
    
第四步：训练










问题：
    1：测试结果大多是重复的字符，其中’，’、 ‘的’、 ’冒号‘的重复较多，可能是这些词出现的较多的缘故。
