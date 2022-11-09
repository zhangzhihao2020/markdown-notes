# ubuntu 上 anaconda虚拟环境配置

###  1.配置[虚拟环境](https://so.csdn.net/so/search?q=虚拟环境&spm=1001.2101.3001.7020)的终端命令

```
conda create --name +自己取的虚拟环境名称 +安装在环境中的包名
conda create --name DRL python=3.7.11
```

### 2.进入和退出虚拟环境

```
# 进入conda虚拟环境
conda activate name
conda activate DRL

# 推出conda虚拟环境
conda deactivate

#Ubuntu关闭anaconda自动进入base虚拟环境

conda config --set auto_activate_base false

```

### 3.显示安装过的虚拟环境

```
conda info --envs
# 或者
conda info -e
# 或者
conda env list
```

### 4.删除已安装的虚拟环境

```
conda remove --name 环境名 --all
conda remove --name DRL --all
```

### 5.查看安装的包

```
pip list
conda list
```

### 6.换为清华源

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --append channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/fastai/
conda config --append channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --append channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
 
# 搜索时显示通道地址
conda config --set show_channel_urls yes

# 查看
conda info

```
### 7.pytorch_gpu在线安装
```python
pip install torch==1.6.0+cu101 torchvision==0.7.0+cu101 -f https://download.pytorch.org/whl/torch_stable.html

# 进入pytorch环境
python
import torch
torch.cuda.is_available()
```

### 8.使用清华大学镜像源安装visdom包

```
pip install visdom -i https://pypi.tuna.tsinghua.edu.cn/simple
```

```
python main.py run --env=RLReachEnv --algo=DADDPG_MLP --vis_name=Reach_DADDPG
```

