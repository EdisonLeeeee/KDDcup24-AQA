# KDD cup 2024 AQA 赛题解决方案
+ 参数量：7B
+ 显存使用：单卡Nvidia RTX 3090ti 24G.由于显存不足无法微调训练，因此该解决方案仅使用开源模型参数进行推理
+ B榜Rank: 10


思路：
+ 直接利用[MTEB榜单](https://huggingface.co/spaces/mteb/leaderboard)上的开源模型进行推理得到文本embedding
+ 采用额外的[DBLP论文库](https://open.aminer.cn/open/article?id=655db2202ab17a072284bc0c)进行论文的关键词提取，大约有12%左右的论文包含在该数据库中
+ question+body作为检索query，对所有论文的title+abstrack+keywords(如果有)进行相似度检索
+ 采用faiss进行相似度检索得到top20


# Installation

```bash
pip uninstall -y transformer-engine
pip install torch==2.2.0
pip install transformers --upgrade
pip install flash-attn==2.2.0
pip install sentence-transformers==2.7.0
conda install -c pytorch -c nvidia faiss-gpu=1.8.0
```

# 结果复现
+ 安装上述所需依赖
+ 下载[B榜数据](https://www.biendata.xyz/competition/aqa_kdd_2024/data/)到 `data/`
+ 下载[NV-Embed-v1](https://huggingface.co/nvidia/NV-Embed-v1)模型参数到`models/`
+ 下载额外的[DBLP-Citation-network-V15论文库](https://opendata.aminer.cn/dataset/DBLP-Citation-network-V15.zip)到`data/`
+ 运行 `solution.ipynb`

# 文件说明
+ results: 存放最终预测结果文件夹
+ data: 赛题数据和其他数据文件夹
+ models: huggingface模型权重文件夹
+ `solution.ipynb` 解决方案代码文件