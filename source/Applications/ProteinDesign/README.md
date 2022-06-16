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
|参数|说明|
|------------------------|------------------------------------------------|
|sketchpar='newsketchpar.txt'|#参数文件，如果没有会自动生成,用于备份你的信息。必须输入|
|skecthfile='3pvh.txt'|#sketch坐标文件  。必须输入|
|linkoption=1|#是否连接loop，0不连，1连，默认0|
|randomseed=15|#随机数种子|
|outputname='build'|#输出pdb的文件名,以此为前缀：build0.pdb,并会生成二级结构信息：build0_final_ss.txt 。 必须输入|
|gennumber=2|#生成的结构数目，默认1|
|Sketchbuildpdb(sketchpar,skecthfile,linkoption,randomseed,outputname,gennumber)|运行生成 |

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


2.脚本名：`SCUBALoopSampler.py&SCUBALoopReader.py`<br>
功能：对选定区域的loop采样和读取重采样loop的结构

`SCUBALoopSampler.py`<br>
输入文件：pdb文件（必须），参数文件（可选）
脚本示例：
```
parfile = 'ls.par'	# 设定参数文件，如果没有，会自动生成模板参数文件并命名为此，用于保存用户的参数
ipara, sdpara, lpspara = loopsamplerinit(parfile, jobname='auto')	#对参数初始化并返回三组参数：相互作用参数ipara，随机动力学sdpara，loop采样参数lpspara。可对这三组参数调用和修改，如下

#相互作用参数部分一般不需要改动
ipara.weight_coval=1.0          #共价能量项权重
ipara.weight_localhb=0.6        #主链氢键相互作用权重
ipara.weight_localstr=0.5       #主链连续二面角能量项权重
ipara.weight_steric=1.0         #主链原子空间排斥项权重
ipara.weight_phipsi=2.0         #主链单一位点二面角能量项权重
ipara.weight_rotamer=2.4        #侧链构象能量项权重
ipara.weight_scpacking=3.1      #侧链范德华相互作用能量项权重
ipara.weight_sitepair=0.32      #主链位点相互作用能量项权重

#相互作用参数部分一般不需要改动
ipara.weight_coval=1.0          #共价能量项权重
ipara.weight_localhb=0.6        #主链氢键相互作用权重
ipara.weight_localstr=0.5       #主链连续二面角能量项权重
ipara.weight_steric=1.0         #主链原子空间排斥项权重
ipara.weight_phipsi=2.0         #主链单一位点二面角能量项权重
ipara.weight_rotamer=2.4        #侧链构象能量项权重
ipara.weight_scpacking=3.1      #侧链范德华相互作用能量项权重
ipara.weight_sitepair=0.32      #主链位点相互作用能量项权重

#SD参数：用户可能要改动temperatures和gamma
sdpara.doshake=1                #shake failure检查
sdpara.printsteps=500           #输出当前温度和能量的间隔步数
sdpara.randomseed=512           #随机动力学模拟的随机数种子
sdpara.newnbliststeps=100       #重新计算邻居列表的间隔步数
sdpara.newsscodesteps=500       #重新计算二级结构类型的间隔步数
sdpara.temperatures=[0.2]       #Loop区采样温度，注意以列表形式输入[]
sdpara.gamma=5                  #摩擦系数
sdpara.timestep=0.002           #时间步长，单位是皮秒

#以下是loop采样参数	
lpspara.pdbstart = lspdb	    #输入的pdb文件 。必须
lpspara.maxsdsteps=2000	        #每轮采样的SD最大步数
lpspara.enedecay=0.98	        #计算平均能量的衰减系数
lpspara.sdenevarcut=10	        #停止SD的平均能量变化阈值
lpspara.helixinloop=0	        #生成带小helix的loop
lpspara.reconstructnum = 80	    #进行蒙特卡洛loop采样的轮数
lpspara.findlooptimes = 30	    #搜索每一段loop长度和始末位置的次数，建议30-50次
lpspara.loopsearchmode=
'auto'/’short’/’fixloop’/’fixshortloop’	#loop位置和长度的搜索模式。搜索Loop模式的4种模式：fixshortloop：在指定loop区域寻找最短loop；fixloop：在指定loop区域从最短loop的长度n，比较n+1，n+2长度的loop平局能量，取能量最低的loop长度；short：给定loop区的两端都可以往前或往后移动一位残基，然后各自寻找最短长度的loop，比较能量取最低能的长度和loop位置；auto：给定loop区的两端都可以往前或往后移动一位残基，然后寻找从最短长度到最短长度+2长度的最低平均能量的loop参数，使用该参数的loop长度和位置。
注：有可能找不到loop或者找到了但是连不上因为多个loop可能有位阻。
lpspara.readloopnum = 2	                #采样后，读取非冗余结构的数量
lpspara.readrmsd = '0.6'/’auto’/’min’	#非冗余的RMSD标准。可以直接输入数字（单位Å），或者使用以下公式：
0.3+0.2*(A/D-4.0) （单位Å）。其中A表示所有被采用Loop的总残基数，D的取值依赖于以下两种输入（1）auto: D= 1；（2）min：D=被采样的最短Loop的残基数。
lpspara.outloopfile = 'outloops.dat'	#用于保存loop坐标的文件
loops = "A 73 81 0 A 104 112 5 A 128 138 0 "	#需要采样的loop 。如果你没有给定已经定义好的参数文件，则是必须的。格式：每4个string为一组，中间以空格给开；每组第一个字符A表示所在链，第一个数字73表示loop起点，第二个数字81表示终点，第四个数字如果是0，表示需要搜索这个段loop的长度和起始终点位置，如果是其他数字如5，则按照这个长度连接loop并采样。
setloop(lpspara, loops)	                #设置loop
SCUBALoopSampler(parfile,lpspara,ipara,sdpara,loops)	# 进行loop采样,并修改保存lsparfile
SCUBALoopReader(parfile)	            #循环采样结束后读出采样的loop,可以再次修改读取参数使用SCUBALoopReader单独读取
```

输出文件：
SCUBALoopSampler(parfile,lpspara,ipara,sdpara,loops)输出：保存的<mark>参数文件ls.par</mark>;<mark>loop坐标文件</mark>outloops.dat；<mark>新loop文件</mark>newloop；<mark>初始loop坐标文件</mark>startloops.dat，如果实际loop采样的长度和初始pdb的loop不同，则这个文件保存的loop坐标是随机生成的，用于计算非冗余rmsd。
SCUBALoopReader(parfile)函数输出：用户指定数量的<mark>非冗余结构top_n.pdb</mark>以及它们的非冗余loop坐标文件和非冗余rmsd信息文件。

`SCUBALoopReader.py`<br>
功能：读取loop采样后的pdb结构
输入文件：需要SCUBALoopSampler产生参数文件ls.par  ; 新loop文件newloop；loop坐标文件outloops.dat；初始loop坐标文件startloops.dat。
脚本示例：
```
lpspara, sl = loopreaderinit(‘ls.par’)	# 读取参数，返回loop采样参数lpspara和loop保存参数sl
lpspara.readloopnum = 4	                # 读取非冗余结构的数量
lpspara.readrmsd = '0.4'	            # 非冗余的RMSD标准
LoopReader(lpspara,sl)	                # 读loop构象替换到初始结构上，产生top_n.pdb和相关文件
```

另，此时用户也可以直接修改ls.par文件中的
```
ReadLoopNum = 2
ReadRMSD = 0.6
```
两个值，在python中运行：SCUBALoopReader(parfile)
输出文件：同上

3.脚本名：`ABACUS2Designseq.py`<br>
功能：设计序列
输入：pdb文件
```
pdbname='top_0.pdb'	            #输入文件
outname ='SCUBAdesign'	        #输出文件名：SCUBAdesign-00n.pdb
logfile='design.log'	        #log文件
n= 2	#设计序列产生数
resfile='resfile_A'	            #resfile
Vdw=1	                        #范德华系数
Designseq(pdbname,outname,logfile,n,vdw,designresfile=resfile)	#运行设计序列
# Designseq全部参数：Designseq(pdbname, outname,logfile, n=1,vdw=1,div=1,nat=-100, resfile='', parafile='',aapropfile='')	#div:设计序列之间的最大相似度，1表示没有相似性限制；
#nat：与输入的pdb序列相似性（0-1之间，值越大相似性越高，默认-100不开启不用改）
```
输出：设计的n个<mark>序列结构pdb</mark>：SCUBAdesign-001.pdb；<mark>序列信息</mark>和ABACUS2<mark>能量信息</mark>：design.log


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