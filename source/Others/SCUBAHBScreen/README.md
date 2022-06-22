# SCUBAHBScreen
## 脚本名称及用法
`SCUBAHBScreen.py`

## 应用场景
计算包埋的极性原子中未成氢键的比例

## 脚本注释
```
#!/bin/python3
# This program calculate the score of MC hygrogen bonds for a protein
# ori_cpp: /sampling/test/SCUBAHBSCreen.cpp
```
导入系统模块及SCUBA平台的路径和模块
```
import sys,os
path='/home/cyx/workspace/pySCUBA/pybind11/bin/'
sys.path.append(path)
import pb_geometry
import pb_proteinrep
import pb_screen
"""This program calculate the score of MC hygrogen bonds for a protein"""
```
读入PDB、设置溶剂可及阈值和氢键距离阈值
```
pdb = 'start.pdb'
expose_th = 0.14
HB_dist = 3.5
```
“非暴露极性原子”及“非暴露且未成氢键原子”计数归零
```
nexp_patm = 0.0
nexp_nhbatm = 0.0
```
读入PDB
```
AAC = pb_proteinrep.AAConformersInModel()
AAC.readpdbfile(pdb)
cons = AAC.conformers
```
统计“非暴露极性原子”及“非暴露且未成氢键原子”
```
for c in cons:
    for r in c:
        crds = r.globalcrd
        if not(pb_screen.SpherePoints.expose(r.globalcrd["N"], r.chainid_or, r.residueid_or,
 cons, expose_th, 'MC')):
            nexp_patm+=1
        if not(pb_screen.SpherePoints.formhb(r.globalcrd["N"], r.chainid_or, r.residueid_or,
 cons, HB_dist, 'MC')):
            nexp_nhbatm+=1
        if not(pb_screen.SpherePoints.expose(r.globalcrd["O"], r.chainid_or, r.residueid_or,
 cons, expose_th, 'MC')):                                                       nexp_patm+=1
        if not(pb_screen.SpherePoints.formhb(r.globalcrd["O"], r.chainid_or, r.residueid_or,
 cons, HB_dist, 'MC')):
            nexp_nhbatm+=1
```
输出未成氢键的包埋极性原子比例
```
nhb = nexp_nhbatm/nexp_patm
print("score: %f" %nhb)
# score for 40 native scaffolds is 0.08957
```

## 输入文件注释
无