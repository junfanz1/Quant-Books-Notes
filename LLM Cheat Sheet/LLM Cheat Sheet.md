ML/LLM Cheat Sheet
---

[Contents](https://bitdowntoc.derlin.ch/)

<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [1. Transformer如何设定learning rate?](#1-transformerlearning-rate)
- [2. Transformer: Why Positional Encoding?](#2-transformer-why-positional-encoding)
- [3. Deploy ML Applications?](#3-deploy-ml-applications)
- [4. MLOps：Model health in Prod?](#4-mlopsmodel-health-in-prod)
- [5. 如何优化RAG？](#5-rag)

<!-- TOC end -->



# 1. Transformer如何设定learning rate?

Learning rate是训练Transformer的超参数，决定了优化过程中每次迭代的步长大小，显著影响模型的收敛速度和最终性能。

设置策略

- 网络搜索Grid Search
  -   验证集上实验一系列学习率
  -   计算成本高，对于大模型
- 随机搜索
  -   指定范围内随机采样学习率
  -   比网络搜索更有效，对于高维超参数空间
- 学习率调度
  -   预热：从较低学习率开始，逐渐增到峰值。有助于模型在训练早期稳定。
  -   衰减：逐渐降低学习率，避免过拟合。线性衰减、指数衰减、余弦衰减。
- 技巧
  -   从保守的学习率开始，较小的学习率可以防止发散。
  -   监控训练损失，若损失增加，降低学习率。
  -   用学习率调度器，自动调整学习率。预热-线性衰减warm up-linear decay、余弦退火consine annealing（改善收敛的循环学习率调度）、polynomial decay（灵活调度，允许不同衰减率）
  -   用不同的优化器（Adam, AdamW）
  -   考虑批大小和模型大小，更大的批大小和模型需要更低的学习率。
  -   微调预训练模型，需要更低的学习率。

# 2. Transformer: Why Positional Encoding?

Transformer缺乏对序列顺序的理解。与RNN、CNN不同，Transformer不会逐个处理输入序列中的元素，而是同时处理所有元素。并行处理虽然高效，但丧失了对序列中元素位置信息的感知。

Positional Encoding为输入序列中的每个元素添加独特的表示，指示其相对或绝对位置，这对于机器翻译、文本摘要、问答至关重要。

Why

1. 保留序列顺序，理解序列和元素的关系。
2. 捕获长距离依赖，理解复杂语言结构。对于机器翻译，一个词的含义可能取决于远的词，这样可以帮助模型理解这种关联。
3. 提升模型性能，让模型理解上下文。

How

对于位置i和维度d

$PE(pos, 2i) = \sin(pos / 10000^{2i/d_{model}})$

$PE(pos, 2i + 1) = \cos(pos / 10000^{2i/d_{model}})$

# 3. Deploy ML Applications?

用Nginx部署机器学习应用

1. 准备ML应用，用Flask或FastAPI开发接口，把模型预测功能暴露出来
   
```python
@app.route('/predict', methods=['POST'])
def predict():
  data = request.json
  result = model.predict([data['input']])
  return jsonify({'prediction': result[0]})
```

用Gunicorn运行

```bash
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

2. 安装Nginx

```bash
sudo apt update && sudo apt install nginx
sudo systemctl start nginx
```

3. 配置Nginx reverse proxy
```bash
sudo nano /etc/nginx/sites-available/ml_app
```
写入：
```nginx
server {
  listen 80;
  location/{
    proxy_pass http://127.0.0.1:5000;
  }
}
```
启用配置：
```bash
sudo ln -s/etc/nginx/sites-available/ml_app/etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx
```

4. 配置HTTPS
用Certbot一键开启SSL：
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d <域名>
```

# 4. MLOps：Model health in Prod?

1. 监控输入数据
- 数据漂移：生产数据和训练数据分布是否一致，特征均值和方差
- 数据质量：用自动化工具看缺失值、异常值问题

2. 跟踪模型表现
- 预测漂移：模型输出是否变化 ，如分类概率分布偏移
- 性能指标：评估准确率，特别是有标签反馈时
- 响应时间：延迟报警，确保预测速度稳定

3. 资源监控

4. 配置告警和日志

5. 防止模型退化
- 检查漂移（输入输出关系变化），检测模型是否需要更新
- 构建自动化的数据-模型-系统全链路监控，可视化工具

# 5. 如何优化RAG？

1. 优化Retrieval Component
- 构建高质量知识库，确保知识库是最新高质量内容，通过筛选和过滤，提升检索相关性。
- 微调检索器，使其更能理解和优化排序内容
- 用领域特定的嵌入。预训练的嵌入模型通常缺乏专业领域特异性，可以对嵌入模型微调，用领域适配的语言模型，提供语义匹配精准度。
- 添加上下文过滤器，通过标签、元数据等过滤条件，将检索范围限定在子领域，提升准确度，确保只获取相关知识库内容。
2. 增强generation component
- 特定领域文本上微调生成器。生成模型微调，使其掌握语言风格、术语和表达方式，生成专业性更强、准确的回复。
- 结合知识基础的训练，用特定领域问答进行训练，让模型准确引用检索内容，避免依赖通用知识。
- 优化输出格式，如科学文献引用、分布指令。
3. 优化检索与生成的互动
- 实现重排序技术。检索后，用交叉编码器或相关性评分对检索到的段落重排序，确保生成器获取最丰富信息的上下文。
- 用查询扩展技术。用同义词或相关术语扩展查询，提高检索器捕捉重要内容的概率。
- 调整检索到的文档或token数量，确保生成器既能获取上下文，又不至于超载，提高效率。
4. 优化索引技术
- 分层索引策略。将知识库按领域分层索引，加快特定领域文档的检索速度，让检索器快速定位到子领域。
- 动态索引更新。对知识频繁变化的部分，用动态索引或定期更新索引，确保内容最新。
- 用向量索引加快语义搜索，如FAISS工具提高效率。
