# RootFuse

面向 Solidity 智能合约漏洞检测的 RAG 实验项目。项目使用 CodeT5+ 检索相似漏洞样本，生成函数描述，再结合 CodeBERT 向量训练融合分类器。

## 环境准备

建议使用 Python 3.8，并根据本机 CUDA 版本安装 PyTorch（CPU 也可运行，但较慢）。在本目录执行：

```powershell
pip install -r requirements.txt
```

首次运行时，如本地模型目录不存在，项目会下载并缓存 `CodeT5+` 和 `CodeBERT`。

生成 RAG 描述需要配置 API Key：将密钥写入 `utils/api_key_gpt5.2.txt`，并按需修改 `generate_vul_desc_rag_new_top1.py` 中的接口地址与模型名称。

## 一键运行

```powershell
python train_with_rag_new_new.py --type Reentrancy --folds 5 --epochs 50 --batch-size 32
python train_with_rag_new_new.py --type "Integer Overflow" --folds 5 --epochs 50 --batch-size 32
python train_with_rag_new_new.py --type "Access Control" --folds 5 --epochs 50 --batch-size 32
```

流程依次执行：构建索引、检索 Top-K 样本、生成描述、提取向量和训练分类器。


## 主要目录

```text
generate_vul_reports/   RAG 样本库
data/<type>/            目标数据、描述、向量和实验结果
rag_index_new/          FAISS 检索索引
utils/                  模型加载、向量提取与工具脚本
```

```text
build_rag_index_new.py                构建 RAG 索引
export_rag_topk_new.py                导出检索结果
rag_search_new.py                     执行相似代码检索
generate_vul_desc_rag_new_top1.py     生成 RAG 辅助描述
utils/source_embedding.py             提取 CodeBERT 向量
train_with_rag_new_new.py             训练融合分类器
utils/model_loader.py                 加载并缓存模型
utils/pattern_utils.py                处理漏洞模式
```

