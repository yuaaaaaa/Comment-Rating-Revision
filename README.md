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
    * 本文使用中科院计算所中文自然语言处理开放平台发布的中文停用词表，包含了1208个停用词。下载地址：http://www.hicode.cc/download/view-software-13784.html
* 正负向语料库
    * 