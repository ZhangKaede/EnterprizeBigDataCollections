# UPLIFT研究数据集整理

> 来源：知乎文章 https://zhuanlan.zhihu.com/p/1955641216004765072  
> 作者：贝子涵（清华大学 电子信息硕士在读）  
> 整理时间：2026-06-11

---

# RCT数据集

## LENTA

- **链接**：https://www.kaggle.com/datasets/mrmorj/bigtarget/data
- **使用论文**：MIL（蚂蚁）
- **样本量**：约 687,029 名用户
- **Treatment**：表示用户是否收到了促销短信
- **Outcome**：表示用户是否在促销期间实际到店并产生购买行为
- **Features**：约 196 个特征

**下载方式**
- 需要先安装Kaggle API：`pip install kaggle`
- 配置Kaggle API密钥（从 https://kaggle.com/account 获取），将kaggle.json放入 ~/.kaggle/kaggle.json
- 下载数据集：`kaggle datasets download -d mrmorj/bigtarget`
- 解压：`unzip bigtarget.zip`

---

## Criteo Uplift Prediction Dataset (Criteo-Uplift-v2)

- **链接**: https://ailab.criteo.com/criteo-uplift-prediction-dataset/
- **论文**：A Large Scale Benchmark for Uplift Modeling (KDD 2018)
- **数据量**: 约 2500万 (25M) 条样本
- **特征数量**: 12个匿名化特征（f0-f11）
- **Y标签**: 两个二元标签
  - visit (是否访问)
  - conversion (是否转化)
- **Treatment**: 二元处理变量（是否被展示广告）

**下载方式**
- 需要注册Criteo Lab账号
- 访问 https://ailab.criteo.com/criteo-uplift-prediction-dataset/
- 填写请求表单后获取下载链接

---

## Hillstrom's MineThatData Email Analytics Dataset

- **链接**: https://blog.minethatdata.com/2008/03/minethatdata-e-mail-analytics-and-data.html
- **数据量**: 64,000 条样本
- **特征数量**: 8个特征
- **Y标签**:
  - 回归任务: visit (访问次数)
  - 分类任务: conversion (是否购买)
- **Treatment**: 二元处理变量（收到的邮件类型：无/男性主题/女性主题）

**下载方式**
- 方法1：直接wget下载
  ```bash
  wget http://www.minethatdata.com/Kevin_Hillstrom_MineThatData_E-MailAnalytics_DataMiningChallenge_2008.03.20.csv
  ```
- 方法2：使用Python
  ```python
  import requests
  r=requests.get('http://www.minethatdata.com/Kevin_Hillstrom_MineThatData_E-MailAnalytics_DataMiningChallenge_2008.03.20.csv')
  open('hillstrom.csv','wb').write(r.content)
  ```

---

# 非RCT数据集

## US Census Data

- **链接**：https://archive.ics.uci.edu/dataset/116/us+census+data+1990
- **数据来源**: 1990年美国人口普查公共使用微观数据样本（PUMS）的1%抽样
- **原始数据量**: 约 2,458,285 条样本
- **原始特征数量**: 68个特征（人口统计学信息）
- **在Uber论文中的Uplift数据集构造**：
  - 最终数据量: 225,814 个样本
  - **Treatment**: 定义为"此人的工作时间是否超过数据集中所有人平均工作时间"
  - **Outcome**: 个人的收入

**下载方式**
- 方法1：使用ucimlrepo包
  ```python
  pip install ucimlrepo
  from ucimlrepo import fetch_ucirepo
  us_census_data_1990 = fetch_ucirepo(id=116)
  X = us_census_data_1990.data.features
  y = us_census_data_1990.data.targets
  ```
- 方法2：直接下载
  ```bash
  wget https://archive.ics.uci.edu/static/public/116/data_501x69_original.csv
  ```

---

## Jobs

- **数据链接**：https://users.nber.org/~rdehejia/nswdata.html
- **核心目的**：评估国家支持的工作培训项目（NSW）对参与者就业与收入的因果效应
- **说明**：RCT+非RCT混合数据集

**下载方式**
- 下载RCT数据（实验组）：`wget https://users.nber.org/~rdehejia/nswdata/nsw_all.dta`
- 下载CPS观测数据：`wget https://users.nber.org/~rdehejia/nswdata/cps_all.dta`
- 下载PSID观测数据：`wget https://users.nber.org/~rdehejia/nswdata/psid_all.dta`
- 使用Python读取Stata格式：`python -c "import pandas as pd; df=pd.read_stata('nsw_all.dta'); print(df.head())"`

---

# 混合数据集

## EC-LIFT——阿里：Lazada数据集

- **链接**：https://github.com/kailiang-zhong/DESCN/blob/main/data/Lazada_dataset/README.md
- **特点**：
  - 数据集分为训练和测试数据集
  - 训练数据集是有偏的，测试集是RCT
  - 特征有83个（脱敏），以及label
  - treatment是binary的

**下载方式**
```bash
git clone https://github.com/kailiang-zhong/DESCN.git
# 数据位于 DESCN/data/Lazada_dataset/
```

---

## MT-LIFT——美团数据集

- **链接**：https://github.com/MTDJDSP/MT-LIFT
- **特点**：
  - 数据集是RCT数据
  - 共102个字段，其中99个为特征（f0~f98）
  - 剩余字段为CTR、CVR、干预和特征
  - 干预特征取值在[0,4]之间

**下载方式**
```bash
git clone https://github.com/MTDJDSP/MT-LIFT.git
```

---

## IHDP（Infant Health and Development Program）半合成数据集

- **数据链接**：https://github.com/AMLab-Amsterdam/CEVAE/tree/master/datasets/IHDP
- **原始IHDP研究**：1980年代进行的随机对照试验
- **半合成版本**：由 Jennifer Hill (2011) 构建
- **原始数据**：747个样本 和 25个特征
- **Treatment**：是否接受早期干预（二元变量）
- **Outcome**：婴儿在36个月大时的认知测试得分

**下载方式**
- 方法1：克隆CEVAE仓库
  ```bash
  git clone https://github.com/AMLab-Amsterdam/CEVAE.git
  # 数据位于 CEVAE/datasets/IHDP/
  ```
- 方法2：直接下载数据文件
  ```bash
  wget https://raw.githubusercontent.com/AMLab-Amsterdam/CEVAE/master/datasets/IHDP/ihdp_npci_1.csv
  ```

---

## Twins（观测数据、半合成）

- **数据链接**：https://github.com/AMLab-Amsterdam/CEVAE/blob/master/datasets/TWINS/ReadmeTwins
- **来源**：美国国家卫生统计中心（NCHS）的 1989–1991 年双胞胎出生记录
- **样本量**：约 11,984 对双胞胎（共 23,968 个个体）
- **特征**：50多个特征
- **Treatment 定义**：
  - T = 1：出生体重 < 2000 克
  - T = 0：出生体重 ≥ 2000 克
- **Outcome 定义**：
  - Y = 1：1 岁内死亡
  - Y = 0：存活

**下载方式**
```bash
# 克隆CEVAE仓库
git clone https://github.com/AMLab-Amsterdam/CEVAE.git
# 数据位于 CEVAE/datasets/TWINS/

# 读取说明文档
cat CEVAE/datasets/TWINS/ReadmeTwins
```

---

## IBM ACIC Challenge Datasets

- **链接**：https://academic.oup.com/gigascience/article/doi/10.1093/gigascience/giaf016/8089999
- **描述**：基于2019年ACIC数据挑战赛的公开代码生成的合成数据集
- **数据生成**：使用从高维癫痫发作识别数据集（Epilepsy）收集的协变量生成40k观察样本

**下载方式**
```bash
# 克隆ACIC Challenge 2019仓库
git clone https://github.com/vdorie/aciccomp.git

# 运行R代码生成数据集
cd aciccomp
Rscript scripts/generate_data.R
```

---

## Software Usage Promotion Campaign Uplift Model

- **链接**：https://www.kaggle.com/datasets/hwwang98/software-usage-promotion-campaign-uplift-model/data
- **场景**：软件公司开展推广活动，向部分用户发送促销信息
- **样本量**：2000 条记录
- **特征数量**：11 个特征
- **干预变量**：由Tech Support和Discount构成
- **Outcome**: Revenue，激励后一年内购买的产品数量

**下载方式**
```bash
# 使用Kaggle API下载
kaggle datasets download -d hwwang98/software-usage-promotion-campaign-uplift-model
unzip software-usage-promotion-campaign-uplift-model.zip
```

---

## 论文使用的数据集统计

| 论文 | 会议/期刊 | 使用的数据集 |
|------|----------|-------------|
| ECUP | WWW'24 | Criteo-Uplift, MT-LIFT |
| UMCL | KDD'25 | 合成数据, 模拟数据 |
| ERUM | KDD'24 | Hillstrom-Men, Hillstrom-Women |
| EFIN | KDD'23 | Criteo, EC-LIFT |
| DESCN | KDD'22 | ACIC (Epilepsy), EC-LIFT |
| EEUEN | ICDM'21 | Criteo, EC-LIFT |
| DragonNet | - | IHDP, ACIC |
| MIL | 蚂蚁 | Lenta, Criteo |
| Multi-treatment DML | - | Twins, Jobs, Lazada |

---

## 参考资源

- uplift-modeling官方文档：https://www.uplift-modeling.com/
- IHDP数据集：https://github.com/AMLab-Amsterdam/CEVAE/tree/master/datasets/IHDP
- Twins数据集：https://github.com/AMLab-Amsterdam/CEVAE/blob/master/datasets/TWINS/ReadmeTwins
- Kaggle API文档：https://www.kaggle.com/docs/api
- UCI ML Repository：https://archive.ics.uci.edu/

---

*本文档由AI助手根据知乎文章自动整理生成（已补充下载命令）*
