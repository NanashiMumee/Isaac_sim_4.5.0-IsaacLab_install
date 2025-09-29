# Unitree_sim在Ubantu 20.04上的安装

```
(base)
lsb_release -a
```

#官方建议安装在最低20.04的系统上

## 准备pytorch和conda的环境

```
(base)
nvidia-smi
```

#查看CUDA version，安装对应的pytorch版本  
#参考链接：https://pytorch.org/get-started/locally/

## 安装conda，查看conda是否安装成功
运行
```
(base)
conda -V
```

## 准备虚拟环境

```
(base)
conda create -n unitree_sim_env python=3.10
conda activate unitree_sim_env
```

## 安装 Pytorch

```
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/cu128
```

## 验证环境

```
(base)
python
```

```
(python)
import torch
print(torch.__version__)
```

## 安装 Isaac Sim 4.5.0及资产
官方网站：https://docs.isaacsim.omniverse.nvidia.com/4.5.0/installation/download.html  
将三个文件解压到home/code/zyx/<自定义名字>

找到/home/code/<username>/isaacsim/apps/isaacsim.exp.base.kit，用记事本打开，

并在最后加入以下内容（注意要把所有<username>替换为自己的用户名）：
```
[settings]
persistent.isaac.asset_root.default = "/home/<username>/isaacsim_assets/Assets/Isaac/4.5"
exts."isaacsim.asset.browser".folders = [
    "/home/<username>/isaacsim_assets/Assets/Isaac/4.5/Isaac/Robots",
    "/home/<username>/isaacsim_assets/Assets/Isaac/4.5/Isaac/People",
    "/home/<username>/isaacsim_assets/Assets/Isaac/4.5/Isaac/IsaacLab",
    "/home/<username>/isaacsim_assets/Assets/Isaac/4.5/Isaac/Props",
    "/home/<username>/isaacsim_assets/Assets/Isaac/4.5/Isaac/Environments",
    "/home/<username>/isaacsim_assets/Assets/Isaac/4.5/Isaac/Materials",
    "/home/<username>/isaacsim_assets/Assets/Isaac/4.5/Isaac/Samples",
    "/home/<username>/isaacsim_assets/Assets/Isaac/4.5/Isaac/Sensors",
]
```

## 安装lsaac Lab

```
git clone git@github.com:isaac-sim/IsaacLab.git
sudo apt install cmake build-essential
cd IsaacLab
ln -s ${HOME}/tools/isaac-sim/ _isaac_sim     (Please replace with your own path)
./isaaclab.sh --conda unitree_sim_env
conda activate unitree_sim_env
./isaaclab.sh --install
```

## 链接IsaacLab与Isaac-sim

### 首先验证Isaac-sim安装

```
# Isaac Sim root directory
export ISAACSIM_PATH="${HOME}/code/zyx/isaac-sim-4.5.0"
# Isaac Sim python executable
export ISAACSIM_PYTHON_EXE="${ISAACSIM_PATH}/python.sh"
```

运行

```
${ISAACSIM_PATH}/isaac-sim.sh
```

能打开界面则isaac安装成功

```
cd IsaacLab

ln -s ${HOME}/code/zyx/isaac-sim-4.5.0 _isaac_sim
```

### 创建IsaacLab的虚拟环境

```
./isaaclab.sh --conda 
conda activate env_isaaclab 
```

安装依赖项

```
sudo apt install cmake build-essential
./isaaclab.sh --install
```

## 验证安装
直接创建

```
./isaaclab.sh -p scripts/tutorials/00_sim/create_empty.py
```

虚拟环境创建

```
python scripts/tutorials/00_sim/create_empty.py
```

## 测试训练一个机器人

```
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Ant-v0 --headless
```

## 参考网站：
https://github.com/unitreerobotics/unitree_sim_isaaclab/blob/main/doc/isaacsim4.5_install_zh.md  
https://docs.isaacsim.omniverse.nvidia.com/4.5.0/installation/install_faq.html  
https://blog.csdn.net/m0_65805744/article/details/150344985  
https://blog.csdn.net/qq_43309940/article/details/143839037  
https://www.cnblogs.com/myleaf/p/18843334
