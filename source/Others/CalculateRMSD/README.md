# CalculateRMSD
## 脚本名称及用法
`calcrmsd.py`

## 应用场景
计算一个蛋白的两个构象之间的RMSD（全原子、主链原子、Cα原子）

## 脚本注释
```
#!/bin/python3
# This program calculate rmsd between two pdbs for their ALL/MC/CA atoms.
# ori_cpp: no original cpp program.
```
导入系统模块及SCUBA平台的路径和模块
```
import sys,os
path='/home/cyx/workspace/pySCUBA/pybind11/bin/'
sys.path.append(path)
import pb_geometry
import pb_proteinrep
"""This program calculate rmsd between two pdbs for their ALL/MC/CA atoms."""
```
读入两个PDB文件，设置rmsd计算类型
```
pdb1 = 'output.pdb'
pdb2 = 'start.pdb'
rmsdtype = 'ALL'
# rmsdtype could be ALL MC CA
```
读入PDB
```
AAC1 = pb_proteinrep.AAConformersInModel()
AAC1.readpdbfile(pdb1)
AAC2 = pb_proteinrep.AAConformersInModel()
AAC2.readpdbfile(pdb2)
```
提取指定的坐标
```
crd1 = pb_proteinrep.AAConformersInModel.getcrdbyname(AAC1,rmsdtype)
crd2 = pb_proteinrep.AAConformersInModel.getcrdbyname(AAC2,rmsdtype)
```
计算rmsd
```
rmsd=pb_geometry.QuatFit.rmsd(crd1,crd2)
print("rmsd: %f" %rmsd)
```

## 输入文件注释
无
