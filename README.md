# 基于评论的评分修正
## 一·前期准备
### 1.项目背景
 * 电商网站评分存在分布过于集中
 * 为获得奖励导致的无用评论
 * 恶意刷单现象频生，平台的推荐系统产生了一定的不良影响
 * 用户个人习惯问题导致的评分标准不统一问题，即每个人对于分数的标准有自己的评价指标

 ### 2.数据集准备
 * 通过爬虫爬取美团网美食区餐厅下评论
 * 操作：
    * 繁体字转化
    * emoji去除
    * cookie池反爬虫
 * 参考博客：
    * https://blog.csdn.net/uvyoaa/article/details/80575503
    * https://www.datablog.top/2019/04/10/MTCommentsCrawler/
* 数据集：
    * data = {restaurantId:[[score, comment],...]}
    * 62家餐厅54770条数据， 6.1MB

### 3.语料库获取
* 停用词词典
    * 来自网络的 stoplist， stopword
    * 中文停用词表	cn_stopwords.txt
    * 哈工大停用词表	hit_stopwords.txt
    * 百度停用词表	baidu_stopwords.txt
    * 四川大学机器智能实验室停用词库 scu_stopwords.txt
## 二·过程实现
### 1. 数据集准备 
*  处理通过爬虫得到的初始数据
*  去除包括空数据、重复数据、长连续数据（例如非常非常非常->非常）
*  将数据中的繁体字转变为简体字
*  去除文本中的emoji表情
*  将数据划分为训练集和测试集
*  人工标注train ,人均4000+数据标注，最后标注18000条数据
### 2.正负向预料处理
* 统一数据集的格式为'UTF-8'，便于团队协作
* 对每一项利用根据jieba分词
* 根据停用词库stopwords.txt，给得到的分词去停用词
* 利用NLP进行情感分析，得到正向的分词和负向的分词 得到一个 motion.txt
* 将训练集同时划分为positive.csv 和 negative.csv
#### jieba分词（中文）
* 基于词典的中文分词

核心是首先建立统一的词典表，当需要对一个句子进行分词时，首先将句子拆分成多个部分，将每一个部分与字典一一对应，如果该词语在词典中，分词成功，否则继续拆分匹配直到成功。

* 基于统计的中文分词方法

统计学认为分词是一个概率最大化问题，即拆分句子，基于语料库，统计相邻的字组成的词语出现的概率，相邻的词出现的次数多，就出现的概率大，按照概率值进行分词，所以一个完整的语料库很重要。

#### 去停用词
建立停用词字典，停用词主要包括一些副词、形容词及其一些连接词。通过维护一个停用词表，实际上是一个特征提取的过程，本质上是特征选择的一部分。
本项目使用的停用词词典参考

    * 来源于网络上的常用停用词：stoplist.txt,stopword.txt 
    * 来源于各大官方的停用词：中文停用词表	cn_stopwords.txt、哈工大停用词表	hit_stopwords.txt、百度停用词表	baidu_stopwords.txt、四川大学机器智能实验室停用词库 scu_stopwords.txt
    
#### NLP模型
* 情感分析的主要流程图
![情感分析](https://user-images.githubusercontent.com/45160523/116832597-26675280-abe8-11eb-8ea4-fbb1bfbf11f0.png)
* 其中一些主要的情感分析库
![image](https://user-images.githubusercontent.com/45160523/116833499-06399280-abec-11eb-85f8-fa1192cf013d.png)
* 情感词典文本匹配算法
![image](https://user-images.githubusercontent.com/45160523/116833516-29fcd880-abec-11eb-833d-66681f999d45.png)
### 3.获取特征词向量
* 利用Word2Vec词向量模型将语料转换为词向量
* 提取特征词向量
* 特征降维

