
# 技术背景


PySAGES是一款可以使用GPU加速的增强采样插件，它可以直接对接到[OpenMM](https://github.com)上进行增强采样分子动力学模拟，这里我们测试一下相关的安装，并尝试跑一个简单的增强采样示例。



![](https://img2024.cnblogs.com/blog/2277440/202411/2277440-20241126112510850-23739988.png)

# 安装PySAGES


PySAGES本身可以使用pip进行安装：



 python3 \-m pip install git\+https://github.com/SSAGESLabs/PySAGES.git

```
$ python3 -m pip install git+https://github.com/SSAGESLabs/PySAGES.git
Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
Collecting git+https://github.com/SSAGESLabs/PySAGES.git
  Cloning https://github.com/SSAGESLabs/PySAGES.git to /tmp/pip-req-build-1fcvtmpb
  Running command git clone --filter=blob:none --quiet https://github.com/SSAGESLabs/PySAGES.git /tmp/pip-req-build-1fcvtmpb
  Resolved https://github.com/SSAGESLabs/PySAGES.git to commit 5f5bfc7ab97c8027bb60eedd65cdcd66b5556b57
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Requirement already satisfied: cython in /home/dechin/.local/lib/python3.10/site-packages (from pysages==0.5.0) (3.0.11)
Collecting dill (from pysages==0.5.0)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/46/d1/e73b6ad76f0b1fb7f23c35c6d95dbc506a9c8804f43dda8cb5b0fa6331fd/dill-0.3.9-py3-none-any.whl (119 kB)
Requirement already satisfied: jax>=0.3.5 in /home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages (from pysages==0.5.0) (0.3.25)
Collecting plum-dispatch!=2.0.0,!=2.0.1,>=1.5.4 (from pysages==0.5.0)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/56/48/253352df240f5f1d4226f757e4107344bc7f49a4f84ba7d1affb5916d622/plum_dispatch-2.5.3-py3-none-any.whl (42 kB)
Collecting numba (from pysages==0.5.0)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/79/58/cb4ac5b8f7ec64200460aef1fed88258fb872ceef504ab1f989d2ff0f684/numba-0.60.0-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (3.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.7/3.7 MB 1.3 MB/s eta 0:00:00
Requirement already satisfied: numpy>=1.20 in /home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages (from jax>=0.3.5->pysages==0.5.0) (1.24.3)
Requirement already satisfied: opt-einsum in /home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages (from jax>=0.3.5->pysages==0.5.0) (3.3.0)
Requirement already satisfied: scipy>=1.5 in /home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages (from jax>=0.3.5->pysages==0.5.0) (1.10.0)
Requirement already satisfied: typing-extensions in /home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages (from jax>=0.3.5->pysages==0.5.0) (4.11.0)
Collecting beartype>=0.16.2 (from plum-dispatch!=2.0.0,!=2.0.1,>=1.5.4->pysages==0.5.0)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/64/69/f6db6e4cb2fe2f887dead40b76caa91af4844cb647dd2c7223bb010aa416/beartype-0.19.0-py3-none-any.whl (1.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.0/1.0 MB 1.4 MB/s eta 0:00:00
Collecting rich>=10.0 (from plum-dispatch!=2.0.0,!=2.0.1,>=1.5.4->pysages==0.5.0)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl (242 kB)
Collecting llvmlite<0.44,>=0.43.0dev0 (from numba->pysages==0.5.0)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/c6/21/2ffbab5714e72f2483207b4a1de79b2eecd9debbf666ff4e7067bcc5c134/llvmlite-0.43.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (43.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 43.9/43.9 MB 1.6 MB/s eta 0:00:00
Collecting markdown-it-py>=2.2.0 (from rich>=10.0->plum-dispatch!=2.0.0,!=2.0.1,>=1.5.4->pysages==0.5.0)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl (87 kB)
Requirement already satisfied: pygments<3.0.0,>=2.13.0 in /home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages (from rich>=10.0->plum-dispatch!=2.0.0,!=2.0.1,>=1.5.4->pysages==0.5.0) (2.15.1)
Collecting mdurl~=0.1 (from markdown-it-py>=2.2.0->rich>=10.0->plum-dispatch!=2.0.0,!=2.0.1,>=1.5.4->pysages==0.5.0)
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl (10.0 kB)
Building wheels for collected packages: pysages
  Building wheel for pysages (pyproject.toml) ... done
  Created wheel for pysages: filename=pysages-0.5.0-py3-none-any.whl size=117796 sha256=d9d97db55522297ba4ec17a6680790e0c13e87d92ea35e92a00b4055b7bc47b7
  Stored in directory: /tmp/pip-ephem-wheel-cache-zec6nip9/wheels/85/08/28/c73436bba0d28b37f2bbcf4081cdaa187aa06eef51e869c8a1
Successfully built pysages
Installing collected packages: mdurl, llvmlite, dill, beartype, numba, markdown-it-py, rich, plum-dispatch, pysages
Successfully installed beartype-0.19.0 dill-0.3.9 llvmlite-0.43.0 markdown-it-py-3.0.0 mdurl-0.1.2 numba-0.60.0 plum-dispatch-2.5.3 pysages-0.5.0 rich-13.9.4

```


# 安装测试


看起来是安装成功了，跑一个简单的用例试一试。先准备一个简单的pdb文件：



 input.pdb

```
CRYST1    0.000    0.000    0.000  90.00  90.00  90.00 P 1           1
ATOM      1  H1  ACE A   1      -1.838  -6.570  -0.492  0.00  0.00
ATOM      2  CH3 ACE A   1      -0.764  -6.587  -0.283  0.00  0.00
ATOM      3  H2  ACE A   1      -0.392  -7.533  -0.746  0.00  0.00
ATOM      4  H3  ACE A   1      -0.592  -6.446   0.740  0.00  0.00
ATOM      5  C   ACE A   1      -0.006  -5.404  -0.828  0.00  0.00
ATOM      6  O   ACE A   1      -0.544  -4.619  -1.673  0.00  0.00
ATOM      7  N   ALA A   2       1.278  -5.323  -0.423  0.00  0.00
ATOM      8  H   ALA A   2       1.622  -5.845   0.368  0.00  0.00
ATOM      9  CA  ALA A   2       2.284  -4.164  -0.399  0.00  0.00
ATOM     10  HA  ALA A   2       2.098  -3.653   0.505  0.00  0.00
ATOM     11  CB  ALA A   2       3.651  -4.787  -0.566  0.00  0.00
ATOM     12  HB1 ALA A   2       4.274  -4.031  -0.972  0.00  0.00
ATOM     13  HB2 ALA A   2       3.977  -5.106   0.419  0.00  0.00
ATOM     14  HB3 ALA A   2       3.697  -5.612  -1.274  0.00  0.00
ATOM     15  C   ALA A   2       1.995  -3.152  -1.576  0.00  0.00
ATOM     16  O   ALA A   2       1.544  -2.065  -1.221  0.00  0.00
ATOM     17  N   NME A   3       2.255  -3.614  -2.845  0.00  0.00
ATOM     18  H   NME A   3       2.788  -4.485  -2.929  0.00  0.00
ATOM     19  CH3 NME A   3       1.991  -2.802  -4.055  0.00  0.00
ATOM     20 HH31 NME A   3       2.561  -1.891  -3.988  0.00  0.00
ATOM     21 HH32 NME A   3       1.897  -3.419  -4.937  0.00  0.00
ATOM     22 HH33 NME A   3       0.985  -2.388  -3.930  0.00  0.00
END

```


然后在上一篇文章中介绍的[OpenMM基础案例](https://github.com)的基础上增加一个PySAGES的MetaDynamics案例：



```
from openmm.app import PDBFile, ForceField, Simulation, PDBReporter, StateDataReporter, HBonds
from openmm import LangevinMiddleIntegrator
from openmm.unit import nanometer, kelvin, picoseconds, picosecond, BOLTZMANN_CONSTANT_kB, AVOGADRO_CONSTANT_NA, kilojoules_per_mole

import pysages
from pysages.colvars import DihedralAngle
from numpy import pi
from pysages.methods import Metadynamics, MetaDLogger

kB = BOLTZMANN_CONSTANT_kB * AVOGADRO_CONSTANT_NA
kB = kB.value_in_unit(kilojoules_per_mole / kelvin)

def NVT(pdb_name='input.pdb', pdb_out='output.pdb', ff='amber14-all.xml', log_file='log.dat'):
    pdb = PDBFile(pdb_name)
    forcefield = ForceField(ff)
    system = forcefield.createSystem(pdb.topology, nonbondedCutoff=1*nanometer, constraints=HBonds)

    integrator = LangevinMiddleIntegrator(300*kelvin, 1/picosecond, 0.004*picoseconds)
    simulation = Simulation(pdb.topology, system, integrator)
    simulation.context.setPositions(pdb.positions)

    simulation.minimizeEnergy()
    simulation.reporters.append(PDBReporter(pdb_out, 1000))
    simulation.reporters.append(StateDataReporter(log_file, 1000, step=True, potentialEnergy=True, temperature=True, volume=True))
    return simulation

def MetaD(hills_file="hills.dat", time_steps=10000):
    cvs = [DihedralAngle([4, 6, 8, 14]), DihedralAngle([6, 8, 14, 16])]
    height = 1.2  # kJ/mol
    sigma = [0.35, 0.35]  # radians
    deltaT = 5000
    stride = 500
    ngauss = time_steps // stride + 1
    grid = pysages.Grid(lower=(-pi, -pi), upper=(pi, pi), shape=(50, 50), periodic=True)
    method = Metadynamics(cvs, height, sigma, stride, ngauss, deltaT=deltaT, kB=kB, grid=grid)
    callback = MetaDLogger(hills_file, stride)
    run_result = pysages.run(method, NVT, time_steps, callback)
    result = pysages.analyze(run_result)
    metapotential = result["metapotential"]
    return metapotential

if __name__ == '__main__':
    potential = MetaD()
    print (potential)

```

发生了一个报错：



```
Traceback (most recent call last):
  File "/home/dechin/projects/gitee/dechin/tests/test_openmm.py", line 43, in <module>
    potential = MetaD()
  File "/home/dechin/projects/gitee/dechin/tests/test_openmm.py", line 37, in MetaD
    run_result = pysages.run(method, NVT, time_steps, callback)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/plum/function.py", line 383, in __call__
    return _convert(method(*args, **kw_args), return_type)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 230, in run
    futures = [submit_work(ex, method, callback) for _ in range(config.copies)]
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 230, in <listcomp>
    futures = [submit_work(ex, method, callback) for _ in range(config.copies)]
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 218, in submit_work
    return executor.submit(
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/utils.py", line 33, in submit
    future.set_result(fn(*args, **kwargs))
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 324, in _run_replica
    return run(method, *args, **kwargs)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/plum/function.py", line 383, in __call__
    return _convert(method(*args, **kw_args), return_type)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 371, in _run
    sampling_context = SamplingContext(method, context_generator, callback, context_args)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/backends/core.py", line 101, in __init__
    backend = import_module("." + self._backend_name, package="pysages.backends")
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "", line 1050, in _gcd_import
  File "", line 1027, in _find_and_load
  File "", line 1006, in _find_and_load_unlocked
  File "", line 688, in _load_unlocked
  File "", line 883, in exec_module
  File "", line 241, in _call_with_frames_removed
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/backends/openmm.py", line 8, in <module>
    import openmm_dlext as dlext
ModuleNotFoundError: No module named 'openmm_dlext'

```

提示是需要安装一个`openmm_dlext`的插件。因为这个插件只有一个Github仓库，没有太多的文档，也没有介绍怎么安装的。我测试过下载源码下来，`cmake&&make install`去编译构建，但是又会有很多其他的报错提示要处理，最终我采取的方案是使用conda安装：



```
$ conda install conda-forge::openmm-dlext

```

安装完成后再次运行上面的案例，又有一个新的报错：



```
$ python3 test_openmm.py 
Traceback (most recent call last):
  File "/home/dechin/projects/gitee/dechin/tests/test_openmm.py", line 43, in <module>
    potential = MetaD()
  File "/home/dechin/projects/gitee/dechin/tests/test_openmm.py", line 37, in MetaD
    run_result = pysages.run(method, NVT, time_steps, callback)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/plum/function.py", line 383, in __call__
    return _convert(method(*args, **kw_args), return_type)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 230, in run
    futures = [submit_work(ex, method, callback) for _ in range(config.copies)]
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 230, in <listcomp>
    futures = [submit_work(ex, method, callback) for _ in range(config.copies)]
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 218, in submit_work
    return executor.submit(
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/utils.py", line 33, in submit
    future.set_result(fn(*args, **kwargs))
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 324, in _run_replica
    return run(method, *args, **kwargs)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/plum/function.py", line 383, in __call__
    return _convert(method(*args, **kw_args), return_type)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 371, in _run
    sampling_context = SamplingContext(method, context_generator, callback, context_args)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/backends/core.py", line 102, in __init__
    self.sampler = backend.bind(self, callback, **kwargs)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/backends/openmm.py", line 194, in bind
    force.add_to(context)  # OpenMM will handle the lifetime of the force
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/openmm_dlext/__init__.py", line 29, in add_to
    self.__alt__.add_to(_to_capsule(context), _to_capsule(system))
RuntimeError: Unsupported platform

```

关于这个报错，我在[openmm\_dlext的Issue](https://github.com)里面找到了相应的解决方案，说是要手动配置一下CUDA Platform，于是修改一下代码：



```
from openmm.app import PDBFile, ForceField, Simulation, PDBReporter, StateDataReporter, HBonds
from openmm import LangevinMiddleIntegrator, Platform
from openmm.unit import nanometer, kelvin, picoseconds, picosecond, BOLTZMANN_CONSTANT_kB, AVOGADRO_CONSTANT_NA, kilojoules_per_mole

import pysages
from pysages.colvars import DihedralAngle
from numpy import pi
from pysages.methods import Metadynamics, MetaDLogger

openmm_platform = Platform.getPlatformByName('CUDA')
kB = BOLTZMANN_CONSTANT_kB * AVOGADRO_CONSTANT_NA
kB = kB.value_in_unit(kilojoules_per_mole / kelvin)

def NVT(pdb_name='input.pdb', pdb_out='output.pdb', ff='amber14-all.xml', log_file='log.dat', platform=openmm_platform):
    pdb = PDBFile(pdb_name)
    forcefield = ForceField(ff)
    system = forcefield.createSystem(pdb.topology, nonbondedCutoff=1*nanometer, constraints=HBonds)

    integrator = LangevinMiddleIntegrator(300*kelvin, 1/picosecond, 0.004*picoseconds)
    simulation = Simulation(pdb.topology, system, integrator, platform=platform)
    simulation.context.setPositions(pdb.positions)

    simulation.minimizeEnergy()
    simulation.reporters.append(PDBReporter(pdb_out, 1000))
    simulation.reporters.append(StateDataReporter(log_file, 1000, step=True, potentialEnergy=True, temperature=True, volume=True))
    return simulation

def MetaD(hills_file="hills.dat", time_steps=10000):
    cvs = [DihedralAngle([4, 6, 8, 14]), DihedralAngle([6, 8, 14, 16])]
    height = 1.2  # kJ/mol
    sigma = [0.35, 0.35]  # radians
    deltaT = 5000
    stride = 500
    ngauss = time_steps // stride + 1
    grid = pysages.Grid(lower=(-pi, -pi), upper=(pi, pi), shape=(50, 50), periodic=True)
    method = Metadynamics(cvs, height, sigma, stride, ngauss, deltaT=deltaT, kB=kB, grid=grid)
    callback = MetaDLogger(hills_file, stride)
    run_result = pysages.run(method, NVT, time_steps, callback)
    result = pysages.analyze(run_result)
    metapotential = result["metapotential"]
    return metapotential

if __name__ == '__main__':
    potential = MetaD()
    print (potential)

```

再次运行，又出现一个新的报错：



```
Traceback (most recent call last):
  File "/home/dechin/projects/gitee/dechin/tests/test_openmm.py", line 44, in <module>
    potential = MetaD()
  File "/home/dechin/projects/gitee/dechin/tests/test_openmm.py", line 38, in MetaD
    run_result = pysages.run(method, NVT, time_steps, callback)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/plum/function.py", line 383, in __call__
    return _convert(method(*args, **kw_args), return_type)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 230, in run
    futures = [submit_work(ex, method, callback) for _ in range(config.copies)]
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 230, in <listcomp>
    futures = [submit_work(ex, method, callback) for _ in range(config.copies)]
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 218, in submit_work
    return executor.submit(
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/utils.py", line 33, in submit
    future.set_result(fn(*args, **kwargs))
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 324, in _run_replica
    return run(method, *args, **kwargs)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/plum/function.py", line 383, in __call__
    return _convert(method(*args, **kw_args), return_type)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 371, in _run
    sampling_context = SamplingContext(method, context_generator, callback, context_args)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/backends/core.py", line 78, in __init__
    context = context_generator(**context_args)
  File "/home/dechin/projects/gitee/dechin/tests/test_openmm.py", line 20, in NVT
    simulation = Simulation(pdb.topology, system, integrator, platform=platform)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/openmm/app/simulation.py", line 104, in __init__
    self.context = mm.Context(self.system, self.integrator, platform)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/openmm/openmm.py", line 19511, in __init__
    _openmm.Context_swiginit(self, _openmm.new_Context(*args))
openmm.OpenMMException: Error loading CUDA module: CUDA_ERROR_UNSUPPORTED_PTX_VERSION (222)

```

好，这次是CUDA版本号不支持，类似的问题在一条[openmm的issue](https://github.com)中有讨论过，需要检查一下自己本地cudatoolkit的配置信息：



```
$ conda list | grep cudatoolkit
$

```

这里发现在这个虚拟环境里面没有配置cudatoolkit，需要再装一个：



```
$ conda install -c conda-forge cudatoolkit=11.6
Collecting package metadata (current_repodata.json): done
Solving environment: failed with initial frozen solve. Retrying with flexible solve.
Collecting package metadata (repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 23.1.0
  latest version: 24.11.0

Please update conda by running

    $ conda update -n base -c defaults conda

Or to minimize the number of packages updated during conda update use

     conda install conda=24.11.0



## Package Plan ##

  environment location: /home/dechin/anaconda3/envs/jax

  added / updated specs:
    - cudatoolkit=11.6


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    cudatoolkit-11.6.2         |      hfc3e2af_13       598.8 MB  conda-forge
    openmm-8.1.1               |  py310h358ce72_1        10.8 MB  conda-forge
    openmm-dlext-0.2.1         |  py310h552f1b7_8         115 KB  conda-forge
    ------------------------------------------------------------
                                           Total:       609.8 MB

The following NEW packages will be INSTALLED:

  cudatoolkit        conda-forge/linux-64::cudatoolkit-11.6.2-hfc3e2af_13 

The following packages will be REMOVED:

  cuda-nvrtc-12.4.127-h99ab3db_1
  cuda-version-12.4-hbda6634_3
  libcufft-11.2.1.3-h99ab3db_1

The following packages will be UPDATED:

  openssl            anaconda/pkgs/main::openssl-3.0.15-h5~ --> conda-forge::openssl-3.4.0-hb9d3cd8_0 

The following packages will be SUPERSEDED by a higher-priority channel:

  ca-certificates    anaconda/pkgs/main::ca-certificates-2~ --> conda-forge::ca-certificates-2024.8.30-hbcca054_0 
  openmm             anaconda/cloud/conda-forge::openmm-8.~ --> conda-forge::openmm-8.1.1-py310h358ce72_1 

The following packages will be DOWNGRADED:

  openmm-dlext                        0.2.1-py310hcb41016_8 --> 0.2.1-py310h552f1b7_8 


Proceed ([y]/n)? y


Downloading and Extracting Packages
                                                                                                                                                                        
Preparing transaction: done                                                                                                                                             
Verifying transaction: done                                                                                                                                             
Executing transaction: | By downloading and using the CUDA Toolkit conda packages, you accept the terms and conditions of the CUDA End User License Agreement (EULA): https://docs.nvidia.com/cuda/eula/index.html

done

```

再次执行上面的程序，报错\+1：



```
$ python3 test_openmm.py 
Traceback (most recent call last):
  File "/home/dechin/projects/gitee/dechin/tests/test_openmm.py", line 44, in <module>
    potential = MetaD()
  File "/home/dechin/projects/gitee/dechin/tests/test_openmm.py", line 38, in MetaD
    run_result = pysages.run(method, NVT, time_steps, callback)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/plum/function.py", line 383, in __call__
    return _convert(method(*args, **kw_args), return_type)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 230, in run
    futures = [submit_work(ex, method, callback) for _ in range(config.copies)]
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 230, in <listcomp>
    futures = [submit_work(ex, method, callback) for _ in range(config.copies)]
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 218, in submit_work
    return executor.submit(
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/utils.py", line 33, in submit
    future.set_result(fn(*args, **kwargs))
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 324, in _run_replica
    return run(method, *args, **kwargs)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/plum/function.py", line 383, in __call__
    return _convert(method(*args, **kw_args), return_type)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/methods/core.py", line 371, in _run
    sampling_context = SamplingContext(method, context_generator, callback, context_args)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/backends/core.py", line 102, in __init__
    self.sampler = backend.bind(self, callback, **kwargs)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/backends/openmm.py", line 197, in bind
    helpers, restore, bias = build_helpers(sampling_context.view, sampling_method)
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/backends/openmm.py", line 135, in build_helpers
    sync_forces, view = utils.cupy_helpers()
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/site-packages/pysages/backends/utils.py", line 21, in cupy_helpers
    cupy = importlib.import_module("cupy")
  File "/home/dechin/anaconda3/envs/jax/lib/python3.10/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "", line 1050, in _gcd_import
  File "", line 1027, in _find_and_load
  File "", line 1004, in _find_and_load_unlocked
ModuleNotFoundError: No module named 'cupy'

```

不过这个看起来好处理，就是少装了一个cupy的依赖，稳妥起见，我们还是选择使用conda来安装cupy：



```
$ conda install -c conda-forge cupy -y
Collecting package metadata (current_repodata.json): done
Solving environment: failed with initial frozen solve. Retrying with flexible solve.
Solving environment: - 
failed with repodata from current_repodata.json, will retry with next repodata source.
Collecting package metadata (repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 23.1.0
  latest version: 24.11.0

Please update conda by running

    $ conda update -n base -c defaults conda

Or to minimize the number of packages updated during conda update use

     conda install conda=24.11.0



## Package Plan ##

  environment location: /home/dechin/anaconda3/envs/jax

  added / updated specs:
    - cupy


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    cuda-version-11.6          |       hca96458_3          21 KB  conda-forge
    cupy-13.3.0                |  py310h189a05f_2         347 KB  conda-forge
    cupy-core-13.3.0           |  py310h5da974a_2        42.9 MB  conda-forge
    fastrlock-0.8.2            |  py310hc6cd4ac_2          37 KB  conda-forge
    ------------------------------------------------------------
                                           Total:        43.3 MB

The following NEW packages will be INSTALLED:

  cuda-version       conda-forge/noarch::cuda-version-11.6-hca96458_3 
  cupy               conda-forge/linux-64::cupy-13.3.0-py310h189a05f_2 
  cupy-core          conda-forge/linux-64::cupy-core-13.3.0-py310h5da974a_2 
  fastrlock          conda-forge/linux-64::fastrlock-0.8.2-py310hc6cd4ac_2 



Downloading and Extracting Packages
                                                                                                                                                                        
Preparing transaction: done                                                                                                                                             
Verifying transaction: done                                                                                                                                             
Executing transaction: done                 

```

然后再执行测试程序：



```
$ python3 test_openmm.py 
<CompiledFunction of <function analyze.<locals>.<lambda> at 0x7fc1a04aba30>>

```

同时会在执行路径下生成`log.dat`文件和`hills.dat`文件如下：



 log.dat

```
#"Step","Potential Energy (kJ/mole)","Temperature (K)","Box Volume (nm^3)"
1000,-39.254608154296875,284.2437497410935,8.0
2000,-18.68511962890625,318.9596355900776,8.0
3000,-30.86761474609375,289.998240351891,8.0
4000,-21.921295166015625,338.6328004320157,8.0
5000,-31.451812744140625,245.66209355694957,8.0
6000,-32.077880859375,182.1505555238515,8.0
7000,-56.050750732421875,252.68200201473644,8.0
8000,-27.819427490234375,289.4957194622587,8.0
9000,-36.86553955078125,271.0313362334861,8.0
10000,-12.531005859375,254.41902934626566,8.0

```



 hills.dat

```
500	-1.3592318296432495	1.5952203273773193	0.35	0.35	1.2
1000	-1.1732323169708252	0.8145138621330261	0.35	0.35	1.1973907824735082
1500	-1.433311104774475	1.7097197771072388	0.35	0.35	1.1671557288100634
2000	-1.114228367805481	2.042632579803467	0.35	0.35	1.179103510697602
2500	-1.1403875350952148	0.9936402440071106	0.35	0.35	1.1603072043059584
3000	-1.2672390937805176	0.40286365151405334	0.35	0.35	1.173835519022326
3500	-1.302258014678955	1.4455255270004272	0.35	0.35	1.1211946752964734
4000	-1.4070658683776855	1.013744592666626	0.35	0.35	1.121531633082655
4500	-2.6735711097717285	2.6055266857147217	0.35	0.35	1.1999969285502206
5000	-2.8140780925750732	2.9895386695861816	0.35	0.35	1.1809462925907876
5500	-2.6453146934509277	2.5226593017578125	0.35	0.35	1.1505253387717271
6000	-2.476658344268799	2.7877397537231445	0.35	0.35	1.1410532890823843
6500	-2.7321791648864746	-2.84220814704895	0.35	0.35	1.1797609616409976
7000	-1.596192479133606	1.0611979961395264	0.35	0.35	1.1126547940366505
7500	-1.3219820261001587	0.2645364999771118	0.35	0.35	1.1460450294138773
8000	-1.5232703685760498	1.4845924377441406	0.35	0.35	1.0865578670844307
8500	-1.3037762641906738	0.6937571167945862	0.35	0.35	1.076390175717164
9000	-1.3598891496658325	2.0672760009765625	0.35	0.35	1.1276490649082718
9500	-2.420367479324341	2.7348878383636475	0.35	0.35	1.1032026693365438

```


喜大普奔，PySAGES环境部署完毕！


# 案例测试


还是沿用上面的`input.pdb`案例，这里我们测试一个MetaDynamics的FES，增加了一个analyse的plot模块：



```
from openmm.app import PDBFile, ForceField, Simulation, PDBReporter, StateDataReporter, HBonds
from openmm import LangevinMiddleIntegrator, Platform
from openmm.unit import nanometer, kelvin, picoseconds, picosecond, BOLTZMANN_CONSTANT_kB, AVOGADRO_CONSTANT_NA, kilojoules_per_mole
from sys import stdout

import pysages
from pysages.colvars import DihedralAngle
from numpy import pi
from pysages.methods import Metadynamics, MetaDLogger
from pysages.approxfun import compute_mesh

import matplotlib.pyplot as plt

openmm_platform = Platform.getPlatformByName('CUDA')
kB = BOLTZMANN_CONSTANT_kB * AVOGADRO_CONSTANT_NA
kB = kB.value_in_unit(kilojoules_per_mole / kelvin)
T = 300*kelvin
dt = 0.004*picoseconds

def NVT(pdb_name='input.pdb', pdb_out='output.pdb', ff='amber14-all.xml', log_file='log.dat', platform=openmm_platform):
    pdb = PDBFile(pdb_name)
    forcefield = ForceField(ff)
    system = forcefield.createSystem(pdb.topology, nonbondedCutoff=1*nanometer, constraints=HBonds)

    integrator = LangevinMiddleIntegrator(T, 1/picosecond, dt)
    simulation = Simulation(pdb.topology, system, integrator, platform=platform)
    simulation.context.setPositions(pdb.positions)

    simulation.minimizeEnergy()
    simulation.reporters.append(PDBReporter(pdb_out, 1000))
    simulation.reporters.append(StateDataReporter(stdout, 1000, step=True, potentialEnergy=True, temperature=True, volume=True))
    simulation.reporters.append(StateDataReporter(log_file, 1000, step=True, potentialEnergy=True, temperature=True, volume=True))
    return simulation

def plot_grid(metapotential, method):
    plot_grid = pysages.Grid(lower=(-pi, -pi), upper=(pi, pi), shape=(64, 64), periodic=True)
    xi = (compute_mesh(plot_grid) + 1) / 2 * plot_grid.size + plot_grid.lower
    alpha = (
        1
        if method.deltaT is None
        else (T.value_in_unit(kelvin) + method.deltaT) / method.deltaT
    )
    kT = kB * T.value_in_unit(kelvin)
    A = metapotential(xi) * -alpha / kT
    A = A - A.min()
    A = A.reshape(plot_grid.shape)
    # plot and save free energy to a PNG file
    fig, ax = plt.subplots(dpi=120)

    im = ax.imshow(A, interpolation="bicubic", origin="lower", extent=[-pi, pi, -pi, pi])
    ax.contour(A, levels=12, linewidths=0.75, colors="k", extent=[-pi, pi, -pi, pi])
    ax.set_xlabel(r"$\phi$")
    ax.set_ylabel(r"$\psi$")

    cbar = plt.colorbar(im)
    cbar.ax.set_ylabel(r"$A~[k_{B}T]$", rotation=270, labelpad=20)

    fig.savefig("Figure.png", dpi=fig.dpi)

def MetaD(hills_file="hills.dat", time_steps=50000):
    cvs = [DihedralAngle([4, 6, 8, 14]), DihedralAngle([6, 8, 14, 16])]
    height = 2.0  # kJ/mol
    sigma = [0.2, 0.2]  # radians
    deltaT = 5000
    stride = 50
    ngauss = time_steps // stride + 1
    grid = pysages.Grid(lower=(-pi, -pi), upper=(pi, pi), shape=(50, 50), periodic=True)
    method = Metadynamics(cvs, height, sigma, stride, ngauss, deltaT=deltaT, kB=kB, grid=grid)
    callback = MetaDLogger(hills_file, stride)
    run_result = pysages.run(method, NVT, time_steps, callback)
    result = pysages.analyze(run_result)
    metapotential = result["metapotential"]
    plot_grid(metapotential, method)
    return result

if __name__ == '__main__':
    res = MetaD()

```

因为在OpenMM的Simulation的Report中我们增加了一个`stdout`，因此会同时在屏幕上输出结果，也会在相应的`log.dat`文件中保存结果，运行输出如下：



```
#"Step","Potential Energy (kJ/mole)","Temperature (K)","Box Volume (nm^3)"
1000,9.73681640625,377.6993364201515,8.0
2000,-23.971282958984375,298.44721588786604,8.0
3000,-15.113677978515625,213.76058649837066,8.0
4000,-9.906219482421875,444.7447921758242,8.0
5000,-35.83831787109375,299.89809403902336,8.0
6000,-33.826202392578125,313.4731859605099,8.0
7000,-19.394073486328125,337.10699269365875,8.0
8000,-45.882415771484375,250.66251735991736,8.0
9000,-14.17413330078125,358.22016011687015,8.0
10000,-29.421051025390625,246.90072858113598,8.0
11000,-19.7567138671875,301.9975514069083,8.0
12000,-32.948822021484375,367.195135668361,8.0
13000,-9.27825927734375,289.7929305791173,8.0
14000,-30.180389404296875,309.66557282887885,8.0
15000,-2.736083984375,302.5309003205113,8.0
16000,-32.576629638671875,291.86829747083937,8.0
17000,-14.334503173828125,231.850481046364,8.0
18000,-20.755645751953125,298.57497669296873,8.0
19000,-43.75299072265625,306.65343794873587,8.0
20000,33.35467529296875,258.82936068957366,8.0
21000,-1.04156494140625,339.65646518408494,8.0
22000,0.01190185546875,197.8572390770094,8.0
23000,5.273040771484375,289.2310517046787,8.0
24000,-14.901947021484375,383.9835521646287,8.0
25000,-0.839019775390625,268.7104144595147,8.0
26000,-23.747772216796875,222.84395037451839,8.0
27000,-27.284759521484375,285.9093985100245,8.0
28000,-23.12164306640625,248.21416090812198,8.0
29000,10.6822509765625,319.4106894537426,8.0
30000,-16.64678955078125,304.24748131130184,8.0
31000,-6.5423583984375,329.8362141299685,8.0
32000,-3.944793701171875,333.3584976075751,8.0
33000,-29.894744873046875,355.53462625307105,8.0
34000,-22.54876708984375,366.93298893561547,8.0
35000,-14.81097412109375,330.12835522481674,8.0
36000,-45.39825439453125,363.9047710837139,8.0
37000,-5.33160400390625,355.4129749973852,8.0
38000,-19.806365966796875,361.73243838698073,8.0
39000,-13.85650634765625,411.9526625662002,8.0
40000,1.711639404296875,225.61063301965956,8.0
41000,-34.70196533203125,389.73863301467037,8.0
42000,-33.63153076171875,307.1604571229406,8.0
43000,-37.86602783203125,277.5274210978626,8.0
44000,-3.5263671875,248.0723224072663,8.0
45000,21.574676513671875,294.5427172838524,8.0
46000,-31.097808837890625,302.0992611330069,8.0
47000,-23.125152587890625,307.7661366778404,8.0
48000,8.406402587890625,174.53798831979185,8.0
49000,-19.694549560546875,380.88297517197196,8.0
50000,12.754608154296875,380.40854584876394,8.0

```

这里输出的FES被保存成了一个图片，内容为：



![](https://img2024.cnblogs.com/blog/2277440/202411/2277440-20241126105458588-570859847.png)

这就是PySAGES的Well\-Tempered MetaDynamics输出的FES结果。


# 工作流


PySAGES的工作流是这样的：



![](https://img2024.cnblogs.com/blog/2277440/202411/2277440-20241126143848810-1461777648.png)

这里我们的backend使用的就是OpenMM了，大致的流程是，通过PySAGES来构建对应backend的Simulation对象，然后启动Simulation。每一次需要`update bias force`的时候，从backend传回来一个force，在PySAGES层面加入bias force然后传回backend。循环迭代，直至time step截止。


PySAGES自带了一些增强采样的方法和一些定义好的CV，当然，因为其基于Jax\-Python开发，因此自定义一个新的CV在形式上也非常的简洁：



![](https://img2024.cnblogs.com/blog/2277440/202411/2277440-20241126144529097-1004416694.png)

最关键的，一般这种外接的增强采样软件会很大程度上影响到整体分子模拟的性能，甚至很可能成为Bottleneck。而根据PySAGES官方给出的profile结果来看：



![](https://img2024.cnblogs.com/blog/2277440/202411/2277440-20241126145006649-2128505507.png)

MetaDynamics部分的时间占比并没有成为Bottleneck，从时长比例上来说，这个性能表现是非常突出的。


# 总结概要


本文主要介绍了增强采样外接软件PySAGES的基本安装和使用方法，重点是安装过程中没有写清楚的一些环境依赖和可能出现的问题介绍，以及相应的解决方案。并简单的梳理了一下PySAGES软件的工作流机制，其能够做到Zero Copy，并使得Enhanced Sampling不再成为很多模拟的Bottleneck，这是一个相当出色的结果。


# 版权声明


本文首发链接为：[https://github.com/dechinphy/p/pysages.html](https://github.com):[westworld加速](https://xbsj9.com)


作者ID：DechinPhy


更多原著文章：[https://github.com/dechinphy/](https://github.com)


请博主喝咖啡：[https://github.com/dechinphy/gallery/image/379634\.html](https://github.com)


# 参考链接


1. [https://pysages.readthedocs.io/en/latest/installation.html](https://github.com)


