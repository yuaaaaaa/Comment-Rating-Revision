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
* 利用NLP进行情感分析，得到正向的分词和负向的分词 pos.txt 和 nag.txt
* 将训练集同时划分为positive.csv 和 negative.csv
#### NLP模型
![情感分析](https://user-images.githubusercontent.com/45160523/116832597-26675280-abe8-11eb-8ea4-fbb1bfbf11f0.png)
### 3.获取特征词向量
* 利用Word2Vec词向量模型将语料转换为词向量
* 提取特征词向量
* 特征降维

