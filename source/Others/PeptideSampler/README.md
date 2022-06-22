# Peptidesampler
内含三类：Protein类，PeptideContainer类，PepBuilder类，PepSampler类
## Protein
`__init__`根据给定pdb文件，实现pdb结构读入,`pdbfile`：string，pdb文件所在的路径	
`nchains`获取链个数
`nresidues`获取某条链的氨基酸个数,`chainidx`：int,某链的index，从0开始，如0为第一条链
`getcacrd`获取某条链上某个氨基酸Cα坐标,`chainidx`：int,某链的index，从0开始，如0为第一条链.`residueidx`: int,该氨基酸在某条链上的位置，从0开始.如`getcacrd(0，0)`即为第一条链上的第一个氨基酸
`getcrds`，`getcrds_double`获取protein所有原子坐标	
`changecrds`，`changecrds_double`更改protein所有原子坐标,`crds`：更改为新的所有原子的坐标,`changecrds`需要提供的坐标类型为包含`pb_geometry.XYZ()`的list，`changecrds_double`需要提供的坐标类型为包含float的list
`addchain`添加链,`blcks`：添加的新的链，由包含pb_iblock.IntrctBlck()类的list组成
`writepdb`导出至pdb文件,`savedfile`：string, pdb结构保存的文件的路径
`specifyactiveblks`SD中指定可活动区域,`mainchainselectedblks`：指定活动的主链区域,`sidechainselectedblks`：指定活动的侧链区域,这两个参数类型相同，如`[ [chainid,set(resid…)], [] ,…, [] ]`, chainid与resid都为int类型，每个list由一个chainid与一个由多个resid组成的set组成，一个list指定了一条链上活动的区域，两个参数都是由多个这样的list构成，因此该函数可指定多条链多个区域
`setupintrctpara`SD所需的能量项权重,`ipara`：pb_iblock.IntrctPara()
`getenergy`获取protein的SCUBA能量	

## PeptideContainer
`__init__`包含peptide的容器,`receptor`: `Protein`类，包含的peptide的同一个receptor,`peplength`: int, 形成peptide的长度,`rmsdcutoff`: float, rmsd的阈值，判断peptide是否冗余的标准
`savenonredundant`判断并保存新产生的peptide,`oneconfig`：list, 由四项构成，list[0]为包含8个float的list类型，8个分别为SD的能量项对应的能量，list[1]为float类型的总能量，list[2]为float类型的contact约束的能量,list[3]为包含float的list类型的peptide原子坐标
`writepdb`导出某个peptide的pdb文件,`oneconfig`:同savenonredundant函数的参数
`rmsd`计算两个peptide的rmsd,`config1`,`config2`：类型相同，分别是某个peptide所有原子坐标，即包含float的list类型
`writepdb_topn`以某个设定的rmsd阈值的标准，将n个结构导出至pdb文件	`n`: int,`rmsdcutoff`: float, rmsd的阈值，判断peptide是否冗余的标准

## PepBuilder
`__init__`基于receptor构建底物peptide,`receptor`:`Protein`类,`peptidelen`:int, peptide长度,`ifresiduesidx`:指定口袋位置，如'chain0 71 72 101 102 62 100'，则指定口袋位置在第一条链上，第72，73，102，103，63，101个氨基酸的位置上
`genpepcrds`产生单个peptide原子坐标	
`genpepcrd`将产生的peptide原子坐标添加至receptor对象中,`receptor`:`Protein`类
`addpeptoreceptor`产生peptide，并将产生的peptide原子坐标添加至receptor对象中（对外使用该函数即可）,`receptor`: `Protein`类,`gencrd`:bool，是否将产生的peptide原子坐标添加到receptor中
`randompepcenter`随机产生peptide中心的坐标和方向	

## PepSampler
`__init__`构建并优化底物peptide,`receptor`:`Protein`类,`pepsamplerpar`: namedtuple类的参数文件，主要需修改PDBStart，InterfaceResidues，分别为包含receptor的pdb文件路径，和指定口袋位置，如'chain0 71 72 101 102 62 100'，则指定口袋位置在第一条链上，第72，73，102，103，63，101个氨基酸的位置上,`sdpar`: `pb_sampling.SDRunPara()`,`ipara`: `pb_iblock.IntrctPara()`
`buildandoptimize`构建peptide并执行优化peptide功能	
`saveconfig`导出至pdb文件,`savefile`：string, pdb结构保存的文件的路径
`writeContainers`以某个设定的rmsd阈值的标准，将n个结构导出至pdb文件,`n`: int, `rmsdcutoff`: float, rmsd的阈值，判断peptide是否冗余的标准
`setupsdrun`构建优化peptide的SD	
`rebuildpeptide`重新产生peptide初始化结构

## 主程序
导入所需模块
```
import argparse
from copy import deepcopy
import os,sys
from collections import namedtuple
import random
from math import sqrt
sys.path.append("/home/zhanglu/workspace/pyscuba/pySCUBA/pybind11/bin")
import pb_iblock
import pb_proteinrep
import pb_backbone
import pb_geometry
import pb_sampling
```
初始化Protein类
```
pdbfile='/home/zhanglu/workspace/new/pybind11/backbone/test/1awr_preprocessed.pdb'
protein=Protein(pdbfile)
```
pdbfile所需修改的pdb结构的文件,实例化Protein类

设定参数
```
interfaceresidues='chain0 71 72 101 102 62 100'
pepsamplerpar=PepSamplerPar()
pepsamplerpar=pepsamplerpar._replace(PDBStart=pdbfile)
pepsamplerpar=pepsamplerpar._replace(InterfaceResidues=interfaceresidues)

sdpar=SDRunPar()
ipar=InteractionPar()
setseed(int(sdpar.RandomSeed))
```
初始化pepsampler之前，需要对receptor的pdb文件和口袋的位置进行设置，其它默认参数可依据情况进行更改

初始化pepsampler并产生peptide进行优化
```
pepsampler=PepSampler(protein,pepsamplerpar,sdpar,ipar)
savefile='/home/zhanglu/workspace/new/pybind11/backbone/test/pepsavedconfigs'
for n in range(para['nconfig']):
    pepsampler.buildandoptimize()
    pepsampler.saveconfig(savefile)
```
`para[‘nconfig’]`为产生peptide轮数，一轮产生初始骨架并优化最终生成一个，可设置多轮

将产生的peptide连同receptor一起导出
```
pepsampler.writeContainers(para['outputnum'],para['rmsdcutoff'])
```
`para[‘outputnum’]`为输出pdb的个数,`para[‘rmsdcutoff’]`为以rmsd为去冗余的阈值，即输出的pdb中peptide的之间rmsd大于该设定阈值