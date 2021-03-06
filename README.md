# mujoco-py [![Documentation](https://img.shields.io/badge/docs-latest-brightgreen.svg?style=flat)](https://openai.github.io/mujoco-py/build/html/index.html) [![Build Status](https://travis-ci.org/openai/mujoco-py.svg?branch=master)](https://travis-ci.org/openai/mujoco-py) [![Build status](https://ci.appveyor.com/api/projects/status/iw52c0198j87s76w?svg=true)](https://ci.appveyor.com/project/wojzaremba/mujoco-py)

[MuJoCo](http://mujoco.org/) is a physics engine for detailed, efficient rigid body simulations with contacts. `mujoco-py` allows using MuJoCo from Python 3.

## Synopsis

### Requirements

The following platforms are currently supported:

- Linux with Python 3.5.2. See [the `Dockerfile`](Dockerfile) for the canonical list of system dependencies. Support for Python 3.6 [is planned](https://github.com/openai/mujoco-py/issues/52).
- OS X with Python 3.5.2. Support for Python 3.6 [is planned](https://github.com/openai/mujoco-py/issues/52).
- Windows (experimental) with Python 3.5.2. See [the Appveyor file](https://github.com/openai/mujoco-py/blob/master/.appveyor.yml#L16-L32) for the canonical list of dependencies.

Python 2 has been desupported since [1.50.1.0](https://github.com/openai/mujoco-py/releases/tag/1.50.1.0). Python 2 users can stay on the [`0.5` branch](https://github.com/openai/mujoco-py/tree/0.5). The latest release there is [`0.5.7`](https://github.com/openai/mujoco-py/releases/tag/0.5.7) which can be installed with `pip install mujoco-py==0.5.7`.

### Install MuJoCo

1. Obtain a 30-day free trial on the [MuJoCo website](https://www.roboti.us/license.html)
   or free license if you are a student.
   The license key will arrive in an email with your username and password.
2. Download the MuJoCo version 1.50 binaries for
   [Linux](https://www.roboti.us/download/mjpro150_linux.zip),
   [OSX](https://www.roboti.us/download/mjpro150_osx.zip), or
   [Windows](https://www.roboti.us/download/mjpro150_win64.zip).
3. Unzip the downloaded `mjpro150` directory into `~/.mujoco/mjpro150`,
   and place your license key (the `mjkey.txt` file from your email)
   at `~/.mujoco/mjkey.txt`.

### Install and use `mujoco-py`
To include `mujoco-py` in your own package, add it to your requirements like so:
```
mujoco-py<1.50.2,>=1.50.1
```
To play with `mujoco-py` interactively, follow these steps:
```
$ pip3 install -U 'mujoco-py<1.50.2,>=1.50.1'
$ python3
import mujoco_py
from os.path import dirname
model = mujoco_py.load_model_from_path(dirname(dirname(mujoco_py.__file__))  +"/xmls/claw.xml")
sim = mujoco_py.MjSim(model)

print(sim.data.qpos)
# [ 0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.]

sim.step()
print(sim.data.qpos)
# [  2.09217903e-06  -1.82329050e-12  -1.16711384e-07  -4.69613872e-11
#   -1.43931860e-05   4.73350204e-10  -3.23749942e-05  -1.19854057e-13
#   -2.39251380e-08  -4.46750545e-07   1.78771599e-09  -1.04232280e-08]
```

See the [full documentation](https://openai.github.io/mujoco-py/build/html/index.html) for advanced usage.

### Missing GLFW

A common error when installing is:

    raise ImportError("Failed to load GLFW3 shared library.")

Which happens when the `glfw` python package fails to find a GLFW dynamic library.

MuJoCo ships with its own copy of this library, which can be used during installation.

Add the path to the mujoco bin directory to your dynamic loader:

    LD_LIBRARY_PATH=$HOME/.mujoco/mjpro150/bin pip install mujoco-py

This is particularly useful on Ubuntu 14.04, which does not have a GLFW package.

## Usage Examples

A number of examples demonstrating some advanced features of `mujoco-py` can be found in [`examples/`](/./examples/). These include:
- [`body_interaction.py`](./examples/body_interaction.py): shows interactions between colliding bodies
- [`disco_fetch.py`](./examples/disco_fetch.py): shows how `TextureModder` can be used to randomize object textures
- [`internal_functions.py`](./examples/internal_functions.py): shows how to call raw mujoco functions like `mjv_room2model`
- [`markers_demo.py`](./examples/markers_demo.py): shows how to add visualization-only geoms to the viewer
- [`serialize_model.py`](./examples/serialize_model.py): shows how to save and restore a model
- [`setting_state.py`](./examples/setting_state.py):  shows how to reset the simulation to a given state
- [`tosser.py`](./examples/tosser.py): shows a simple actuated object sorting robot application

See the [full documentation](https://openai.github.io/mujoco-py/build/html/index.html) for advanced usage.

## Development

To run the provided unit and integrations tests:

```
make test
```

To test GPU-backed rendering, run:

```
make test_gpu
```

This is somewhat dependent on internal OpenAI infrastructure at the moment, but it should run if you change the `Makefile` parameters for your own setup.

## Changelog

- 03/08/2018: We removed MjSimPool, because most of benefit one can get with multiple processes having single simulation.

## Credits

`mujoco-py` is maintained by the OpenAI Robotics team. Contributors include:

- Alex Ray
- Bob McGrew
- Jonas Schneider
- Jonathan Ho
- Peter Welinder
- Wojciech Zaremba

## Details in Intallation of Mujoco
#### by starimpact
1. ubuntu建议是16.04版本，或macos，其它系统我没试过。
1. 从mujoco官网下载mjpro150，放到当前用户目录下：`~/.mujoco`，没有这个目录就自己建一个。解压mjpro150。
1. 从mujoco官网申请一个30天的试用key。放到`~/.mujoco`下面。注：国内邮箱收不到验证邮箱，至少我的126是没收到。
1. 进入mjpro150中的sample文件夹，make一下，如果成功，说明没什么大问题了。可以到bin目录下运行：`LD_LIBRARY_PATH=./ ./simulate`，如果有窗口显示说明就正常了。注意将`.mujoco`下的mjkey.txt 复制一份到bin下。
1. 从starimpact的github下载`https://github.com/starimpact/mujoco_py.git`。从这里下载是因为这是一个坑已经填好的版本。
1. 安装一个virutalenv的工具，后面的所有python相关操作都是在虚拟环境中进行，防止把系统的搞坏了。这玩意也挺好用。
1. apt-get install一个python3.5或python3.6的版本，然后用`virtualenv --system-site-packages -p python3.5 [your virtual directory]`安装虚拟环境。
1. 用`source [your virtual directory]/bin/activate` 进入虚拟环境。
1. 建一个`~/.pip/pip.conf`文件夹，写入：
    ```
    [global]
    index-url = https://pypi.tuna.tsinghua.edu.cn/simple
    ```
    这是用pip清华源，速度会快很多，比官方源快几十倍的样子。
1. 进入clone下来的mujoco_py文件夹，运行`pip install -e .`  [注意后面有一个点]。
1. 一般情况下装到mujoco包时会等个十来分钟，如果超过这个时间，说明下载的速度出现了问题。这时候要用nload工具查看当前的下载速度，如果一直是在几kb，那么基本上永远都装不好了。这时候最好停掉，再重新安装，多试几次，直到看到下载速度在100kb左右时才有希望。
1. 上面一步完成后，进入python环境，然后运行导入命令`import mujoco_py`，这时个的mujoco会进行一些编译的工作。如果编译失败，说明缺少一些库文件，只需要按照提示安装好就行了。
1. mujoco_py导入成功后，那么后面问题就少了。进mujoco_py文件夹，会看到一个do.sh的文件，运行它，就可以看到效果。
1. 说一说do.sh里的变量的作用是什么：
    - `LD_LIBRARY_PATH=/home/mingzhang/.mujoco/mjpro150/bin` —> 将之前安装好的mjpro150的环境加到库搜索环境变量里。
    - `MUJOCO_PY_FORCE_CPU=1` —> 指定mujoco只在cpu上运行。这一步很重要。
1. 完成。

#### 注意：以上就是安装使用mujoco的关键步骤。中间会遇到的其它问题都是安装依赖库的问题，请自行google解决。不能google，自行翻墙，哈哈……
