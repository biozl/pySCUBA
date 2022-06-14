DEPACTPocketMatck
=================

脚本名称及用法
--------------

``DEPACTPocketMatch.py input.par``

应用场景
--------

（批量）搜索口袋模型组装到蛋白骨架上的解

脚本注释
--------

::

   #!/bin/python3
   # This program match a pocket with a protein scaffold
   # ori_cpp: /noob/test/DEPACTPocketMatch.cpp

导入系统模块及SCUBA平台的路径和模块

::

   import sys,os
   path='/home/cyx/workspace/pySCUBA/pybind11/bin/'
   sys.path.append(path)
   import pb_geometry
   import pb_pocket
   """This program match a pocket with a protein scaffold."""

读入参数文件input.par

::

   # parameter file version
   kampar = "input.par"

进行组装

::

   pb_pocket.pacmatch(kampar)

输入文件注释 input.par：总参数文件 \|参数|说明\|
\|————————|————————————————\| \| PocketDir = Pocket # \|
口袋模型所在路径 \| \| ScaffoldDir = Scaffold \| 蛋白骨架所在路径 \| \|
Region = region.reg \| region.reg文件：描述候选位点 \| \| MovePocket =
ON \| 搜索时口袋可移动（默认ON） \| \| RMSDCutoff = 2.0 \|
判断某残基是否匹配成功的阈值（默认2.0Å） \| \| KeyResidues = kr.txt \|
kr.txt：关键残基的描述文件 \| \| AtomList = al.txt \|
al.txt：关键原子的描述文件 \| \| LigScaMCClash = 1.5 \|
Ligand原子和骨架主链原子clash阈值（默认1.5Å） \| \| PocSCClash = 1.5 \|
口袋残基侧链间clash阈值（默认1.5Å） \| \| LigPocSCClash = 1.0 \|
ligand原子和口袋残基侧链clash阈值（默认1.5Å） \| \| RotLibChoice = \|
Rotamer库的选择（为空时，默认使用模式1） \|

region.reg：控制候选位点的参数文件 \|参数|说明\|
\|————————|————————————————\| \| A 20 24A 26 26 \|
每一行描述骨架pdb中连续的一个片段作为候选位点。例如A 20
24表示A链上残基编号为20~24的残基（包括头尾）加入候选位点；A 26
26表示A链26号残基加入候选位点。\|

kr.txt：描述关键残基的参数文件 \|参数|说明\|
\|————————|————————————————\| \| RMSD 1.005 \|
（关键残基自己的）RMSD匹配阈值默认5.0Å，一般为了保证关键残基匹配更严格，可设为1.0Å。第二行及后续行指明哪些残基是关键残基（在口袋pdb文件中残基从上到下按从0开始编号）\|

al.txt：定义关键原子 \|参数|说明\| \|————————|————————————————\| \| A 1
LYS CD CE NZA 1 PHE CD1B 1 LEU CD2 \|
每一行描述一个口袋残基的链id、残基id、残基名、关键原子名称。例如A 1 LYS
CD CE
NZ表示口袋文件中A链1号残基为LYS，它的CD、CE和NZ原子定义为关键原子\|
DEPACTPocketMatck
=================

脚本名称及用法
--------------

``DEPACTPocketMatch.py input.par``

应用场景
--------

（批量）搜索口袋模型组装到蛋白骨架上的解

脚本注释
--------

::

   #!/bin/python3
   # This program match a pocket with a protein scaffold
   # ori_cpp: /noob/test/DEPACTPocketMatch.cpp

导入系统模块及SCUBA平台的路径和模块

::

   import sys,os
   path='/home/cyx/workspace/pySCUBA/pybind11/bin/'
   sys.path.append(path)
   import pb_geometry
   import pb_pocket
   """This program match a pocket with a protein scaffold."""

读入参数文件input.par

::

   # parameter file version
   kampar = "input.par"

进行组装

::

   pb_pocket.pacmatch(kampar)

输入文件注释 input.par：总参数文件 \|参数|说明\|
\|————————|————————————————\| \| PocketDir = Pocket # \|
口袋模型所在路径 \| \| ScaffoldDir = Scaffold \| 蛋白骨架所在路径 \| \|
Region = region.reg \| region.reg文件：描述候选位点 \| \| MovePocket =
ON \| 搜索时口袋可移动（默认ON） \| \| RMSDCutoff = 2.0 \|
判断某残基是否匹配成功的阈值（默认2.0Å） \| \| KeyResidues = kr.txt \|
kr.txt：关键残基的描述文件 \| \| AtomList = al.txt \|
al.txt：关键原子的描述文件 \| \| LigScaMCClash = 1.5 \|
Ligand原子和骨架主链原子clash阈值（默认1.5Å） \| \| PocSCClash = 1.5 \|
口袋残基侧链间clash阈值（默认1.5Å） \| \| LigPocSCClash = 1.0 \|
ligand原子和口袋残基侧链clash阈值（默认1.5Å） \| \| RotLibChoice = \|
Rotamer库的选择（为空时，默认使用模式1） \|

region.reg：控制候选位点的参数文件 \|参数|说明\|
\|————————|————————————————\| \| A 20 24A 26 26 \|
每一行描述骨架pdb中连续的一个片段作为候选位点。例如A 20
24表示A链上残基编号为20~24的残基（包括头尾）加入候选位点；A 26
26表示A链26号残基加入候选位点。\|

kr.txt：描述关键残基的参数文件 \|参数|说明\|
\|————————|————————————————\| \| RMSD 1.005 \|
（关键残基自己的）RMSD匹配阈值默认5.0Å，一般为了保证关键残基匹配更严格，可设为1.0Å。第二行及后续行指明哪些残基是关键残基（在口袋pdb文件中残基从上到下按从0开始编号）\|

al.txt：定义关键原子 \|参数|说明\| \|————————|————————————————\| \| A 1
LYS CD CE NZA 1 PHE CD1B 1 LEU CD2 \|
每一行描述一个口袋残基的链id、残基id、残基名、关键原子名称。例如A 1 LYS
CD CE
NZ表示口袋文件中A链1号残基为LYS，它的CD、CE和NZ原子定义为关键原子\|

rlc.txt：rotamer库的选择（使用时在input.par中设RotLibChoice = rlc.txt）
\|参数|说明\| \|————————|————————————————\| \| default 1 for 73 24 2 \|
第一行描述默认对于所有7个口袋残基使用RotLib为1型第二行开始强调哪些残基使用特殊库：3
2指第4个口袋残基（因为从0计数）使用RotLib2进行侧链采样。库的型号越大则构象数越多，程序越慢。经测试，一般不建议加此文件进行修改。\|
rlc.txt：rotamer库的选择（使用时在input.par中设RotLibChoice = rlc.txt）
\|参数|说明\| \|————————|————————————————\| \| default 1 for 73 24 2 \|
第一行描述默认对于所有7个口袋残基使用RotLib为1型第二行开始强调哪些残基使用特殊库：3
2指第4个口袋残基（因为从0计数）使用RotLib2进行侧链采样。库的型号越大则构象数越多，程序越慢。经测试，一般不建议加此文件进行修改。\|
