
# 推荐系统算法-矩阵分解（MF）

# Main Idea

每个user或item表示为K维向量，
分别组成user矩阵**P**和item矩阵**Q**，
令**M**为user与item交互矩阵。
使用FunkSVD方法进行矩阵分解：`M = PQ`。
目标函数采用均方误差（MSE），使用梯度下降法优化。

参考：https://blog.csdn.net/qq_19446965/article/details/82079367

# Environments
+ python 3.8
+ pytorch 1.70

# Dataset

1. Amazon(2014) http://jmcauley.ucsd.edu/data/amazon/links.html
2. Yelp(2020) https://www.yelp.com/dataset

For example:
`data/ratings_Digital_Music.csv` (Amazon Digital Music: rating only)

注：本实验将数据集按0.8/0.1/0.1的比例划分为训练集、验证集、测试集。
数据集前三列分别为用户id、产品id、评分（1~5）。
若使用json格式的amazon（或yelp）数据集，
可使用`data_preprocess.py`预处理。

For example:
```shell script
python data_preprocess.py --data_path Digital_Music_5.json --data_source amazon --save_file amazon_music_ratings.csv
```

# Running
Train and evaluate the model
```
python main.py --dataset_file data/ratings_Digital_Music.csv
```

# Experiment
<table align="center">
    <tr>
        <th>Dataset</th>
        <th>number of users</th>
        <th>number of items</th>
        <th>MSE</th>
        <th>MSE of balanced test</th>
    </tr>
    <tr>
        <td><a href="http://files.grouplens.org/datasets/movielens/ml-latest-small.zip">movielens-small</a> (100,836)</td>
        <td>610</td>
        <td>9724</td>
        <td>0.804585</td>
        <td>1.879120</td>
    </tr>
    <tr>
        <td>Amazon Digital Music small (64,706)</td>
        <td>5541</td>
        <td>3568</td>
        <td>0.900899</td>
        <td>2.436231</td>
    </tr>
    <tr>
        <td>Amazon Digital Music (836,006)</td>
        <td>478235</td>
        <td>266414</td>
        <td>0.875224</td>
        <td>3.566706</td>
    </tr>
    <tr>
        <td>Amazon Clothing, Shoes and Jewelry (5,748,919)</td>
        <td>3117268</td>
        <td>1136004</td>
        <td>1.512551</td>
        <td>3.266632</td>
    </tr>
    <tr>
        <td>Yelp (8,021,121)</td>
        <td>1968703</td>
        <td>209393</td>
        <td>2.171064</td>
        <td>2.429343</td>
    </tr>
</table>

*MSE of balanced test: 数据集划分时，验证集与测试集中，每个分值对应的样本数相等。

# Analysis of Dataset

对数据集的评分分布进行了统计：
## movielens
<img src="data/image/movielens.png" align="middle">

## Amazon digital music small
<img src="data/image/amazon_music_5.png" align="middle">

## Amazon digital music
<img src="data/image/amazon_digital_music.png" align="middle">

## Amazon Clothing, Shoes and Jewelry
<img src="data/image/amazon_CSJ.png" align="middle">

## Yelp
<img src="data/image/yelp_rating.png" align="middle">