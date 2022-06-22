4脚本名：`SCUBASD.py`<br>
功能：随机动力学模拟采样
输入：pdb文件；参数文件（可选）；约束参数文件（可选）
脚本示例1：有用户自定义的参数文件sd.par
```
sdpar='sd_pdbbuild.par'	#必须
SCUBASD(sdpar, 50000)	#指定参数文件，运行50000步SD
```
脚本示例2：使用预设的SD策略，修改其中的参数并运行
```
usr_strategy,usr_para=chooseSDstrategy('default_strategy')	#选择SD策略，返还用户参数usr_para和用户策略usr_strategy
usr_para.MolSystmPar.PDBStart='build0.pdb'	                #指定pdb文件
usr_para.SDRunPar.RestraintsFile='restraints1.txt'	        #选择约束参数文件
usr_para.SDRunPar.AnnealingScheme='1.4 0.9 10000 2000 2000'	#模拟退火温度选项
usr_para.MolSystmPar.FixedResidues='chain0 0-50'	        #选择固定残基部分
runSDstrategy(usr_strategy, 50000)	                        #运行选择的策略
```
输出：sdpar保存<mark>用户参数</mark>；低能的<mark>非冗余pdb</mark>：enetop_n.pdb；实时输出的结构sdout.pdb；

SD参数：
调用方式：usr_para.SDRunPar.
```
#分子体系参数MolSystmPar.	
MolSystmPar. PrintParameters	=	0/1	        #打印此部分参数
MolSystmPar. FixedResidues	= ‘chain0 0-5,11-20; chain1 55-62’	#使特定残基的所有原子被固定.形式字符串，以某一条链开始：chainX 指定残基范围，中间以逗号分隔。不同链以分号隔开。形式以下4个参数相同
MolSystmPar. ActiveResidues	=	                #使特定残基的所有原子可以运动
MolSystmPar. MainChainFixedResidues	=	        #使特定残基主链被固定
MolSystmPar. SideChainActiveResidues	=	    #使特定残基的侧链原子可以运动
MolSystmPar. SoftSideChainResidues	=	        #残基使用软侧链堆积相互作用
MolSystmPar. JobName	=  ‘ sd0’	            #任务名
MolSystmPar. PDBStart	=  ‘start.pdb’	        #输入PDB文件名

#相互作用参数InteractionPar	
InteractionPar. PrintParameters	=	0	#打印此部分参数
InteractionPar. WriteEneDetails = 0	    #是否输出详细能量项，1表示输出
InteractionPar. CovalentWeight = 1	    #共价键权重
InteractionPar. LocalHBWeight = 0.6	    #局部氢键权重
InteractionPar. LocalStrWeight = 0.5	#局部空间位阻权重
InteractionPar. MCStericWeight = 1	    #主链位阻权重（比较小时，主链可以交叉穿过）
InteractionPar. PhiPsiWeight = 2.0	    #主链二面角能量项权重
InteractionPar. RotamerWeight = 2.4	    #侧链Rotamer构象能量权重
InteractionPar. SCPackingWeight = 3.1	#侧链范德华相互作用堆权重
InteractionPar. SitePairWeight = 0.32	#主链位点相互作用权重

#随机动力学参数SDRunPar	
SDRunPar. DOAnnealing	=	1/0	        #模拟退火选项1做/0无
SDRunPar. DOShake = 1	#shake failure判断
SDRunPar. PrintParameters	=	0	    #输出此部分参数
SDRunPar. PrintSteps = 500	            #定义输出当前结果的步数
SDRunPar. RandomSeed = 513	            #随机数种子
SDRunPar. StoreTopConfig= ’50 2 0.5 -1000’	#保存文件参数，4个数代表：保存能量前50低的非冗余结构，要求RMSD相差>2埃，计算当前能量的滑动平均值衰减参数0.5，保存能量低于-1000结果
SDRunPar. RecalcNeighborListSteps = 50	#每50步重新计算邻居数
SDRunPar. RecalcSSSteps = 500	        #每500步重新计算二级结构
SDRunPar. SavePDBSteps = 100	        #每100步保存一次结果
SDRunPar. GAMMA = 1.0	                #摩擦系数 
SDRunPar. TimeStep = 0.002	            #步长0.002皮秒
SDRunPar. AnnealingScheme	=’3 0.5 20000 8000 2000’	#模拟退火5个参数1：高温温度2低温温度3每一轮总步数4高温步数5温度下降步数
SDRunPar. OutPDBFile = ‘finall_out1.pdb’	#实时输出pdb文件名
SDRunPar. JobName	=	#任务名
SDRunPar. RestraintsFile = ‘restraints.txt’	#约束文件

#以下三个参数设置分组模拟需同时设置	
SDRunPar. TemperatureGroups = 'SELECTIONS chain0 0-30, chain0 120-154 ; chain0 31-80 ; chain0 81-119'	#为分子分组，设置温度ALL（全部）/MC+SC（主链和侧链）/SELECTIONS（选择部分残基）。ALL时，应设置AnnealingGroup	=-1. SELECTIONS后面不同的组以分号分隔，对应AnnealingGroup的0/1/2等组。
SDRunPar. GroupTemperatures =’ 1.0 2.0 3.0’	#设置三组的初始温度
SDRunPar. AnnealingGroup = 2	            #分组模拟退火选项:-1不使用,0/1/2等选择模拟退火的组,选择的组会模拟退火，使用的是AnnealingScheme参数，其他组按照GroupTemperatures温度。
```

<mark>SD约束形式：restraints.txt</mark>
包含5种：RMSDRestraints/ RgRestraint/ HelixRestraints/ ContactRestraints/ ScaleInteraction
<mark>1 RMSD约束：</mark>
RMSDRestraints 0 
RMSDterms 500 4.0 6.0  sel_sel allatoms build0.pdb
refresidues chain0 0-9,15-18,24-43,49-55,61-74,82-88,96-102,111-130,135-154,
restrainedresidues chain0 0-9,15-18,24-43,49-55,61-74,82-88,96-102,111-130,135-154,
ENDRMSDrestraints 
第一个字符串表示是RMSD约束：RMSDrestraint；1：在模拟退火高温阶段关闭此约束，0：始终约束，默认选项
500约束力常数
3.0 5.0表示RMSD范围从3埃到5埃范围逐渐增加。(请勿设置为同一数值)
sel_sel 对于选中的残基有约束（另all_all表示对所有残基有约束）
allatoms表示选中残基中的所有原子，不包含氢原子（另mcatoms主链原子，caatoms 碳α原子）
第7个参数：start.pdb表示参考结构
第2n行：参考残基
	参数1：参考结构的片段
	参数2、3、4、、、：参考残基
第2n+1行：约束残基，即sd模拟的残基
	参数参数1：约束结构的片段
	参数2、3、4、、、：约束残基

<mark>2.回旋半径参数：</mark>约束分子选定部分的回旋半径，模拟退火过程中在仅在低温有效
RgRestraint 1 
50 8 Rg_residues chain0 0-9,15-18,24-43,49-55,61-74,82-88,96-102,111-130,135-154,
ENDRgRestraint
第一行：表示读取RgRestraint参数 1：在模拟退火高温阶段关闭此约束，0：始终约束，默认选项
第二行：三部分参数：约束力常数50；打开回转抑制半径的启动值（埃）；Rg_residues chain0 0-100 约束的链的残基

3. HelixRestraints参数：约束序列片段形成αhelix
HelixRestraints 
0 0 9 1000,0 24 43 1000,0 61 74 1000,0 111 130 1000,0 135 154 1000,
ENDHelixRestraints 
第一行：表示读取HelixRestraints参数
第二行：每四个参数为一组，分别表示：链id，约束的起始位置，约束的终止位置，约束力常数

<mark>4.ContactRestraints参数：</mark>约束两个片段或残基上主链原子的距离
ContactRestraints 1
GroupAResidues chain0 15-18 
GroupBResidues chain0 49-55 
3 25 5 8 20
GroupAResidues chain0 49-55
GroupBResidues chain0 96-102
3 25 5 8 20
GroupAResidues chain0 82-88
GroupBResidues chain0 96-102
3 25 5 8 20
ENDContactRestraints

首行和末行为起始和结束行
第一行：ContactRestraints  1：在模拟退火高温阶段关闭此约束，0：终约束，默认选项
每三行为一组约束，3n-1行定义约束的第一段残基，3n行定义第二段残基，3n+1行定义约束的残基数和约束能量、距离约束的gdmin、gdsmall、gdoff参数，gdmin、gdsmall、gdoff的缺省值为5 9 20,单位为埃,当约束位置上指定数目的原子对距离小于5埃时满足约束，距离在5-9埃时会有线性增加的能量惩罚，当距离达到或大于20埃时能量惩罚达到最大（此处约束力常数为25）。

<mark>5.ScaleInteraction 参数：</mark>对选定的残基削弱其相互作用
ScaleInteraction 
0.1 chain0 10-14,19-23,44-48,56-60,75-81,89-95,103-110,131-134,
ENDScaleInteraction 
第一行：表示读取ScaleInteraction参数， 
第二行：参数0.1表示相互作用的倍数；指定链chain0；指定残基位点和片段1 10-14,19-23,44-48,56-60,75-81,89-95,103-110,131-134
#可以注释掉该行内容。