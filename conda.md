- Miniconda 安裝(Linux)
  - 下載 Miniconda script: https://docs.conda.io/en/latest/miniconda.html
  - wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
  - bash 該 script
  - ```source ~/.bashrc```

- 新增環境

```
conda create --name XXX python=3.6
```

- 刪除環境

```
conda env remove --name XXX
```

- 列出所有環境

```
conda env list
```

