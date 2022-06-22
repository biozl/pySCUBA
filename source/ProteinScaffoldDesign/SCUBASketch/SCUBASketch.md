# Protein Design

库文件名：pblib.py  保存实现各种功能的函数，以下SCUBA各类功能需要import这个库

1.脚本名：SCUBASketch.py
功能：对给定的二级结构草图（包括 二级结构类型，长度范围，构建方向（N端或C端），起始位置，终止方向）进行构建，得到一个主链结构

以下所有脚本都需要导入库文件和模块：
```
import sys
path='/home/XXX/pySCUBA/pybind11/bin/'
sys.path.append(path)
from pblib import *
```

输入文件：sketch文本（必须），sketchpar参数文件（可选）
脚本示例：
```
sketchpar='newsketchpar.txt'		#参数文件，如果没有会自动生成,用于备份你的信息。必须输入
skecthfile='3pvh.txt'			#sketch坐标文件  。必须输入
linkoption=1				#是否连接loop，0不连，1连，默认0
randomseed=15				#随机数种子
outputname='build'			#输出pdb的文件名,以此为前缀：build0.pdb,并会生成二级结构信息：build0_final_ss.txt 。 必须输入
gennumber=2				#生成的结构数目，默认1
Sketchbuildpdb(sketchpar,skecthfile,linkoption,randomseed,outputname,gennumber)	运行生成
```

输入sketch示例：3pvh.txt
```
H 10 10 N; 10 0 0;0 0 1
E 4 4 C;15 10 0; 0 0 -1
H 20 20 N; 16 20 0; 0 0 1
E 7 7 C; 10 10 0; 0 0 -1
H 14 14 N; 0 0 0; 0 0 1
E 7 7 C; 5 10 0; 0 0 -1
E 7 7 N; 0 10 0; 0 0 1
H 20 20 C; 0 20 0; 0 0 -1
H 20 20 N; 8 20 0; 0 0 1
```
每一行第一个分号前分别指明二级结构类型（E，H），长度范围（两个整数），构建方向（N或C，N代表从N端开始，C代表从C端开始）;第二个分号前表示第一个氨基酸CA坐标的起始位置；分号后表示该二级结构的终止方向（矢量）；

输出文件：输出用户所指定个数的pdb文件，和它们对应的二级结构信息文件。将生成信息保存到参数文件中newsketchpar.txt。